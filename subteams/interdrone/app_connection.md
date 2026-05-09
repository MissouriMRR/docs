---
permalink: /interdrone/app_connection/
---

Back to index: [Index](/docs/interdrone/index/)

---
# Flutter App Connection

The Flutter app communicates with Pi 1 over the WiFi access point (`DroneCommandNet`). Pi 1 then relays commands to the other drones over the BATMAN-adv mesh.

## Architecture

The app connects **to** Pi 1 on port `5001` and Pi 1 connects back **to** the app on port `5100` to send data.



## Configuration

### Pi 1 — `mission_config.json`

```json
{
  "app_opperable": true,
  "drone_info": [
    { "id": 1, "IP": "169.254.97.1", "port": 5001 },
    { "id": 2, "IP": "169.254.97.2", "port": 5002 },
    { "id": 3, "IP": "169.254.97.3", "port": 5003 },
    { "id": 4, "IP": "169.254.97.4", "port": 5004 }
  ],
  "app_info": {
    "ip": "192.168.50.X",
    "port": 5100
  }
}
```

Replace `192.168.50.X` with the phone's IP address. To find it:
```bash
cat /var/lib/misc/dnsmasq.leases
```

Note: `mission_config.json` is automatically updated on boot by the startup script to set the correct mesh IPs. The `app_info` IP may need to be updated manually if the phone's IP changes.

### Pi 1 — `server.py`

The server binds to `0.0.0.0` so it accepts connections from both the app (via `wlan1`) and other drones (via `bat0`):

```python
server = await asyncio.start_server(
    self.handle_client,
    "0.0.0.0",
    self.networkConfig.get_drone_port(self.droneId),
)
```

### Flutter App — `networking.dart`

```dart
final InternetAddress externalIp = InternetAddress('192.168.50.1');
final int externalPort = 5001;
final int listenPort = 5100;
```

---

## Testing the Connection

### 1. Verify Pi 1 is listening
```bash
sudo ss -tulnp | grep 5001
```
Should show `main.py` listening on `0.0.0.0:5001`.

### 2. Connect phone to DroneCommandNet
The phone should get a `192.168.50.x` IP address.

### 3. Ping the phone from Pi 1
```bash
ping -c 4 192.168.50.X
```

### 4. Open the Flutter app
The app will automatically attempt to connect to `192.168.50.1:5001`.

### 5. Monitor Pi 1 for incoming connections
Stop the background service and run `main.py` manually to see real-time output:
```bash
sudo systemctl stop batman-net.service
sudo bash -c 'cd /home/mrrdt-1/IARC-10 && uv run main.py -i 1'
```

You should see connection and message activity in the terminal when the app connects.



## Message Protocol

The app and Pi 1 communicate using JSON messages over raw TCP sockets. Each message has the following structure:

```json
{
  "id": 401,
  "dronesToSendData": [1],
  "data": {}
}
```

## Troubleshooting

### App can't connect to Pi 1
- Verify phone is connected to `DroneCommandNet` and has a `192.168.50.x` IP
- Verify Pi 1 is listening: `sudo ss -tulnp | grep 5001`
- Check `app_opperable` is `true` in `mission_config.json`
- Check `main.py` is running: `ps aux | grep main.py`

### App connects but no data received
- Check `app_info` IP in `mission_config.json` matches the phone's actual IP
- The phone's IP can change between connections — check with `cat /var/lib/misc/dnsmasq.leases`

### Pi 1 can't send data back to app
- Pi 1 uses `app_info.ip` and `app_info.port` to connect back to the app
- Make sure `app_info.port` matches `listenPort` in the Flutter app (`5100`)
- Make sure `app_info.ip` matches the phone's current IP on `DroneCommandNet`
