> **Dev only** — This is likely too much detail to read through unless you are actively developing this app or migrating it to a future challenge.

---

## main.dart

Entry point and UI layer of the app.

### `main()`
Calls `runApp(Root())`, which starts the Flutter widget tree.

### `Root` (StatelessWidget)
Creates long-lived singletons and registers them as Providers so every widget in the tree can access them without manual threading:
- `iarc_state.State` — GPS position and map coordinate data
- `Networking` — TCP connection to the server
- `SpeechRecognition` — microphone / voice command handler
- `app_logger.Logger` — in-app log store

### `HomePage` / `_HomePageState` (StatefulWidget)
The one and only screen. On `initState()` it:
1. Calls `checkrgbFile()` — if `RGBmap.dat` does not exist on disk, creates a blank 1000×1000 black RGBA file so the map widget has something to show.
2. Calls `initFileWatcher()` — sets up a `dart:io` file watcher on `RGBmap.dat`. Whenever the file changes on disk, `loadImage()` re-reads it and calls `setState()` to redraw the map.
3. Calls `SpeechRecognition.initSpeech()` to load the Vosk model.
4. Calls `Networking.initializeSocket()` to connect to the server.

### `loadImage()`
Reads the raw RGBA bytes from `RGBmap.dat` into a `ui.Image` using `ui.decodeImageFromPixels`. Guards against partial writes by checking the file is exactly `width * height * 4` bytes (4 MB) before decoding.

### Normal UI (`debugModeOn = false`)
- **AppBar:** green background when connected, red when not.
- **Map:** a `RawImage` widget displaying the decoded `ui.Image`, overlaid with a green `CircleAvatar` dot that tracks the phone's map coordinates from `State`.
- **Text below map:** shows "Awaiting input..." while mic is active, otherwise the last command response from `SpeechRecognition`.
- **SpeedDial** (top button, green):
  - Arm — sends `MessageTypes.ARM`
  - Emergency Land — sends `MessageTypes.EMERGENCY_LAND`
  - Takeoff — sends `MessageTypes.START_TAKEOFF`
  - Debug — toggles debug view
- **Mic / Connect FAB** (bottom): if connected, toggles listening; if not connected, retries `initializeSocket()`.

### Debug UI (`debugModeOn = true`)
Scrollable list of all Logger entries (info = black, debug = blue, error = red). SpeedDial with Return (back to normal view) and Clear (wipe the log).

---

## networking.dart

All TCP communication between the app and the IARC-10 server.

### `MessageTypes`
Namespace of integer constants shared between the app and the server. Each constant is an agreed-upon channel number for a specific action:

| Direction | Constants |
|---|---|
| App-side | `APP_CONFIG(401)`, `SEND_PHONE_LOCATION(999)`, `REQUEST_DRONE_LOCATIONS(415)`, `SET_SCAN_STATUS(410)`, … |
| Drone-side | `ARM(520)`, `DISARM(523)`, `START_TAKEOFF(530)`, `START_DEMO(535)`, `START_MISSION(540)`, `EMERGENCY_LAND(555)`, `LAND(556)`, … |
| Server→App | `SEND_PATHS_TO_APP(420)`, `SEND_DRONE_LOCATIONS(425)`, `ARM_ACK(521)`, `ARM_NACK(522)`, `START_TAKEOFF_ACK(531)`, … |

### `Networking` (ChangeNotifier)
**Fields:**
- `localIp` / `listenPort(5100)` — the phone's own server address
- `externalIp(10.106.93.77)` / `externalPort(6002)` — the IARC-10 server
- `hasConnection` / `attemptingConnection` — drive the UI button state

#### `getIp()`
Scans the device's network interfaces for one named `"wlan"` with an IPv4 address. Returns the first match (the phone's Wi-Fi IP).

#### `initializeSocket()`
1. Gets the local IP.
2. Calls `attemptConnection()` to make a test TCP connection to the server.
3. On success: closes the test socket, starts a listening `ServerSocket` on `listenPort` via `startServer()`, sets `hasConnection = true`, then sends:
   - `APP_CONFIG` with the phone's IP and port so the server knows where to call back.
   - `REQUEST_DRONE_LOCATIONS` to get reference points for the position tracker.
   - `sendPhoneLocation()` with the current GPS fix.

#### `sendData(dataToSend)`
Opens a fresh TCP connection for every outbound message (connect → write JSON line → flush → close). If the connection attempt fails, sets `hasConnection = false` and notifies the UI.

#### `handleServerMessage(client)`
The callback registered on the listening `ServerSocket`. Reads incoming bytes, UTF-8 decodes and JSON-parses them into a `Data` object, then switches on `recievedData.id`:
- `ARM_ACK/NACK`, `PING_ACK/NACK`, `TAKEOFF_ACK`, `DEMO_ACK/DONE`, `MISSION_ACK`, `NEW_WAYPOINTS_ACK`, `REACHED_WAYPOINT`, `SCAN_ERROR` → update `SpeechRecognition.commandResponse` text.
- `SEND_PATHS_TO_APP` → calls `map.processMapFromJson()` to redraw the map file.
- `SEND_DRONE_LOCATIONS` → destructures drone1/drone2 lat-long + x/y pairs and calls `state.launchPhoneTracker()` to initialize the coordinate transform.

#### `sendPhoneLocation()`
Sends a `SEND_PHONE_LOCATION` message with `state.latitude` / `state.longitude`.

#### `constructDebugMessage(embeddedMessage)`
Wraps any `Data` object inside an `APP_DEBUG` message so the server echoes it back to the app — used only during development to test the inbound handler.

---

## speech.dart

Offline voice recognition using the Vosk speech engine.

### `SpeechRecognition` (ChangeNotifier)
**Fields:**
- `isListening` — drives the mic button icon in the UI
- `commandResponse` — the text displayed below the map after a command
- `_lastProcessedText` — deduplicates repeated Vosk results

#### `initSpeech()`
Loads the bundled Vosk model from assets (`vosk-model-small-en-us-0.15.zip`), creates a `Recognizer` restricted to a fixed grammar:
```
'arm drones', 'take off', 'start scanning', 'start demo',
'start mission', 'stop', 'send location', 'hover', '[unk]'
```
Registers `_onResult` as the result callback. Sets `networking.setSpeechRecog` so the `Networking` class can update `commandResponse` on incoming ACKs.

#### `startListening()`
Starts the Vosk `SpeechService`, sets `isListening = true`, then sleeps 3 seconds and calls `stopListening()`. Recognition is therefore always a fixed 3-second window.

#### `stopListening()`
Calls `_speechService.stop()` and clears `isListening`.

#### `_onResult(result)`
JSON-decodes the Vosk result, extracts the `"text"` field, skips empty or duplicate strings, then passes the text to `messaging.setAppState()`. Stores the return value in `commandResponse` and notifies the UI.

---

## messaging.dart

Message serialization and voice command parsing.

### `Data`
The single message format used by every app↔server exchange:
```json
{
  "id":               int,
  "dronesToSendData": [int, ...],
  "data":             { ... },
  "senderId":         0
}
```
`id` is a `MessageTypes` constant. `dronesToSendData` specifies which drones to target. `senderId` is always `0` for the app. `Data.fromJson` / `toJson` handle serialization.

> Note: `MessageTypes` constants are also defined here, duplicated from `networking.dart` for use by messaging code.

### `setAppState(state, networking, lastWords)`
Voice command dispatch table.

**Steps:**
1. Splits `lastWords` into tokens, lowercases all.
2. Rejects if fewer than 2 tokens.
3. Matches `wordList[0]` against a fixed set of control words, then `wordList[1]` for sub-commands.

**Supported commands:**

| Voice command | Message sent |
|---|---|
| `"arm drones"` | `ARM(520)` |
| `"take off"` | `START_TAKEOFF(530)` |
| `"start scanning"` | `SET_SCAN_STATUS(410)` `{Message: 'Start scan'}` |
| `"start demo"` | `START_DEMO(535)` |
| `"start mission"` | `START_MISSION(540)` |
| `"stop"` | `SET_SCAN_STATUS(410)` `{Message: 'Stop scanning'}` |
| `"send location"` | `updateLocation()` then `sendPhoneLocation()` |
| `"hover <n> [m\|meters]"` | `SET_HOVER_STATUS(412)` with height in feet (auto-converts meters × 3.821 if unit given) |
| `"debug map"` | Calls `processMapFromJson()` locally with hard-coded test data; no network involved |
| `"debug message"` | Sends an `APP_DEBUG` message to the server which echoes it back to exercise `handleServerMessage()` |

---

## state.dart

GPS position tracking and lat/long → map-coordinate conversion.

### `State` (ChangeNotifier)
**Fields:**
- `_position` — the current GPS `Position` from geolocator
- `latitude` / `longitude` — getters that unwrap `_position` safely
- `_phoneCord [x, y]` — the phone's position in the drone map's coordinate system; read by `main.dart` to place the green dot overlay
- `phoneTrackerInitialized` — guards against drawing before calibration
- `transformMatrix`, `latLongMatrix`, `localCordMatrix` — 2×2 matrices used for the coordinate transform (from `ml_linalg`)
- `_refLatLong`, `_refLocal` — the reference point (drone 1's known position in both coordinate systems)

#### `launchPhoneTracker(state, lat1, long1, x1, y1, lat2, long2, x2, y2)`
Called by `Networking` when `SEND_DRONE_LOCATIONS` arrives. Uses the GPS and map coordinates of two drones as calibration points, then:
1. Calls `initTransformMatrix()` to compute the transform.
2. Gets an initial GPS fix.
3. Calls `updatePhonePosition()` once immediately.
4. Starts a continuous GPS stream via `initGeolocationStreamListener()`.

#### `initTransformMatrix(...)`
Builds a 2×2 affine transform from two control points. The matrix maps Δlat/Δlong offsets to Δx/Δy map offsets. Computed as:
```
localCordMatrix × inverse(latLongMatrix)
```
where each matrix encodes the rotation/scale of its respective space.

#### `updatePhonePosition()`
Converts the current GPS position to map coordinates:
```
phoneCoords = transformMatrix × (phoneLatLong − _refLatLong) + _refLocal
```
Stores the result in `_phoneCord` and notifies listeners. Also re-renders the map by calling `map.processMapFromJson(prevJson, this)` from the GPS stream.

#### `updateLocation()`
One-shot GPS fetch via `Geolocator.getCurrentPosition()`. Called by the `"send location"` voice command before reporting position to the server.

---

## map.dart

Renders the 1000×1000 mission map image and writes it to disk.

### `processMapFromJson(jsonIn, state)`
Rebuilds the full map from scratch on every call. Input is a JSON map with:
- `"paths"` — list of waypoint objects `{x, y, straight, lastInPath}`
- `"mines"` — list of mine objects `{x, y, radius}`

**Drawing pipeline** (using the `image` dart package):
1. Creates a blank 1000×1000 RGBA image with a black background.
2. Iterates over the path list:
   - If `path[i].lastInPath == 1`: skip (end-of-segment marker).
   - If `path[i].straight == 0`: treat `path[i]`, `path[i+1]`, `path[i+2]` as start/midpoint/end of a quadratic Bézier arc; calls `_drawArc()`. Advances `i` by 2.
   - Otherwise: draws a straight line from `path[i]` to `path[i+1]`. Advances `i` by 1.
   - All path lines are green `(0, 255, 0)`, thickness 3.
3. For each mine, draws a solid red outline circle and a semi-transparent red filled circle (alpha 80) at its `(x, y)` with the given radius.
4. Converts the image to raw RGBA bytes (`Uint8List`).
5. Writes bytes to `RGBmap.dat.tmp`, then atomically renames to `RGBmap.dat`. The atomic rename prevents the file watcher in `main.dart` from reading a partially written file.

### `_drawArc(img, start, control, end, color, steps)`
Approximates a quadratic Bézier curve by stepping `t` from 0→1 in `steps` increments, computing the on-curve point at each `t`, and drawing a straight line segment between consecutive points. The control point is derived from the on-curve midpoint using the standard inverse Bézier formula:
```
ctrl = 2*mid − 0.5*start − 0.5*end
```

### `prevJson`
Module-level variable that caches the last JSON received. Used by `state.dart`'s GPS stream to re-render the map with the latest path data whenever the phone's position changes.

---

## logger.dart

In-app logging that feeds the debug UI in `main.dart`.

### `Logger` (ChangeNotifier)
Stores log entries as a `List<RichText>` (Flutter rich-text widgets). The debug UI in `_HomePageState` watches this list directly and rebuilds when it changes.

| Method | Level | Style | Prefix |
|---|---|---|---|
| `i(str)` | info | black | `I ` |
| `d(str)` | debug | blue italic | `D ` |
| `e(str)` | error | red bold | `E ` |

`clear()` empties the list and notifies listeners.

### `logger`
Module-level singleton (`Logger logger = Logger()`) imported by all other files so every class writes to the same log store.
