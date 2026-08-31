---
permalink: /simulation/flying/
---

# Flying the Virtual Drone with Code

[Back to Simulation Docs](/docs/simulation/)

This page assumes you have completed [Environment Setup (Windows)](/docs/simulation/install/windows) and can start the containers as described on the [container setup page](/docs/simulation/containers/).

## Table of Contents

- [How the pieces fit together](#how-the-pieces-fit-together)
- [Start order matters](#start-order-matters)
- [Running the interface script](#running-the-interface-script)
- [Ports](#ports)
- [Flying more than one drone](#flying-more-than-one-drone)
- [Shutting down](#shutting-down)
- [Troubleshooting](#troubleshooting)
- [Production](#production)
- [Legacy: configuring AirSim with settings.json](#legacy-configuring-airsim-with-settingsjson)

## How the pieces fit together

Three programs run at once, split across two systems:

| Where | What it does |
| --- | --- |
| Windows | Unreal Engine (Project AirSim as a plugin) includes: the scene, the physics, and the simulated sensors |
| WSL, `env` container | The flight code, plus the interface script that stands the scene up and drives it |
| WSL, `sim` container | ArduPilot SITL — the actual autopilot firmware, one instance per drone |

Nothing here runs Unreal inside a container. Unreal runs natively on Windows, and the containers talk to it over the network.

Three connections have to come up, and they depend on each other:

1. The interface script connects to Unreal over the Project AirSim API.
2. Unreal sends sensor data to the SITL over UDP; the SITL sends servo output back.
3. Your flight code connects to the SITL over MAVLink on TCP port `5762` (and `5772`, `5782`, ... for additional drones — the SITL adds 10 per instance).

## Start order matters

**Start these in order.** The interface script loads an *empty* scene first and then waits for you, on purpose: the SITL needs *a* scene to pull data from when it starts, but a drone that spawns before its SITL exists will lock up, and only a full restart recovers it. The prompt in the script is the gap between those two requirements.

### 1. Unreal

Open the Project AirSim Unreal project, open the level you want, and press **Play**.

- The Play button is above the viewport to the far right. If it is not immediately visible, press the double-arrow button, then `Play`.
- The Unreal Editor may appear to freeze once the simulation is running. This is normal.

This must be running first — the interface script connects to Unreal as its very first action, and will simply fail if Unreal is not in Play mode.

### 2. The `env` container

In a WSL terminal:

```bash
./simulation/run_container.sh env
```

This drops you into a shell inside the container, with your competition repository mounted at `/workspace`. From there, start the interface script (see [Running the interface script](#running-the-interface-script) below).

It will connect to Unreal, load the empty scene, and then stop at a prompt reading:

```
Start your sim container now. Press enter to continue (add drones to scene)
```

**Do not press Enter yet.**

### 3. The `sim` container

Leave the first terminal sitting at that prompt. In a **second** WSL terminal:

```bash
./simulation/run_container.sh sim
```

This starts the ArduPilot SITL. Wait until it reports that it is waiting for a heartbeat, which means the autopilot is up and listening.

The first time you run this it may need to download the container image; after that it starts immediately.

### 4. Back to the first terminal

Press Enter. The interface script spawns the drones into the scene, waits for MAVLink to come up, and then launches your flight code.

## Running the interface script

The interface scripts live in `simulation/interfaces/`. They use imports relative to the `simulation` package, so they must be run **as a module from the repository root**, not as a bare file path:

```bash
cd /workspace && python -m simulation.interfaces.suas
```

Running `python simulation/interfaces/suas.py` instead will fail with an import error.

The script loads its scene from `simulation/sim_config/`, which is why it has to be started from `/workspace`.

## Ports

For drone index `i`, counting from 0:

| Port | Direction | Set by |
| --- | --- | --- |
| `9003 + 10i` | Unreal to SITL — sensor data | `ardupilot-udp-port` in `sim_config/robot_ardu_quadrotor.jsonc` |
| `9002 + 10i` | SITL to Unreal — servo output | `local-host-udp-port` in the same file |
| `5762 + 10i` | Flight code to the SITL — MAVLink over TCP | the SITL's `--instance` argument |
| `5760 + 10i` | SITL serial0, claimed by MAVProxy | the SITL's `--instance` argument |

The SITL side of all of these comes from `--instance` alone: the ArduPilot binary adds `10 * instance` to *every* port it uses. The Project AirSim side has to be configured to match.

> Under Project AirSim these ports live in the `sim_config/*.jsonc` scene and robot configs. The older AirSim `settings.json` mechanism described at the [bottom of this page](#legacy-configuring-airsim-with-settingsjson) is no longer how this is configured.

## Flying more than one drone

The `sim` container's default command in `compose.yml` starts a **single** SITL:

```bash
python /ardupilot/Tools/autotest/sim_vehicle.py -v ArduCopter -f airsim-copter --out=127.0.0.1:14550 -A "--sim-port-in=9002 --sim-port-out=9003"
```

To run several drones, override that command in `simulation/compose.override.yml` so the container runs the multi-drone launcher instead, and set `NUM_DRONES` to match:

```yaml
version: "3"
services:
  sim:
    command: bash /ardupilot/Tools/autotest/sim_start_drones.sh
    environment:
      - NUM_DRONES=4
```

`sim_start_drones.sh` starts one SITL per drone in its own tmux window, giving each its own port set using the arithmetic in the table above. Cycle through the windows with `Ctrl-b n` and wait for every one of them to come up before continuing.

> The drone count has to agree on **both** sides. The interface script and the `sim` container each work out their own port assignments from the number of drones, and nothing checks that the two match. A mismatch shows up as the flight code waiting forever for MAVLink that never arrives.

## Shutting down

Stop your flight code, then shut the containers down:

```bash
./simulation/run_container.sh shutdown
```

Then stop **Play** in Unreal.

To run again, repeat from step 1. The order matters every time — a drone that spawns without its SITL will lock up, and there is no partial recovery.

## Troubleshooting

| Symptom | Likely cause |
| --- | --- |
| Connecting to Unreal hangs or is refused | Unreal is not in Play mode, or the wrong host address is set |
| Drones spawn but never move, and the SITL prints `No sensor message received in last 1s, resending servos` | The SITL is not receiving sensor packets from Unreal — check that the two agree on the host address |
| A drone locks up immediately after spawning | Enter was pressed before the SITL was ready. Restart everything; there is no partial recovery |
| `PreArm: Need Position Estimate` for a while | Normal. The EKF is waiting for GPS lock; it clears on its own |
| No MAVLink from the SITL | The number of drones the interface script expects does not match the number of SITLs the `sim` container started |

If you are running WSL without mirrored networking, Windows and WSL do not share `127.0.0.1`, and you will need to pass the correct host addresses to both sides. Setting `networkingMode=mirrored` in `.wslconfig` avoids this entirely and is strongly recommended.

## Production

The final rendition of the simulation is a work in progress. Use the developer workflow above for now.

## Legacy: configuring AirSim with `settings.json`

> **This section applies to the older AirSim, not Project AirSim.** Project AirSim is configured through the `.jsonc` scene and robot files in `simulation/sim_config/`, and does not read `settings.json` at all. This is kept for reference and for anyone working with the legacy setup.

`update_airsim_settings.ps1`, in the root of the simulation repository, copies a settings file to where AirSim expects `settings.json`. Templates live in `simulation/templates/`. See the [Project AirSim settings documentation](https://github.com/iamaisim/ProjectAirSim/blob/main/docs/config.md) or the [AirSim settings documentation](https://microsoft.github.io/AirSim/settings/) for the fields themselves.

### Single drone

```powershell
.\update_airsim_settings.ps1 .\templates\legacy-airsim-settings-ardupilot.json
```

Or with your own file:

```powershell
.\update_airsim_settings.ps1 .\path\to\your\settings.json
```

### Multiple drones

Writing multi-drone settings by hand is tedious, so the script can generate them. For a 2x2 grid of four drones:

```powershell
.\update_airsim_settings.ps1 .\templates\legacy-airsim-settings-multidrone.json -NumDrones 2,2
```

The count is given as a grid (`rows,columns`), so the total is `rows * columns`.

The file you pass in is used as a *reference*, not copied verbatim. Global settings are copied as-is, and the single vehicle in the reference's `"Vehicles"` section becomes the template for every generated vehicle. Everything in that vehicle is copied unchanged except the ports and the X, Y, Z offsets: ports increment by `10` per drone, and the offsets come from the separation arguments (3 metres by default).

Given this reference vehicle:

```json
{
    "VehicleType": "ArduCopter",
    "UseSerial": false,
    "LocalHostIp": "127.0.0.1",
    "UdpIp": "127.0.0.1",
    "UdpPort": 9003,
    "ControlPort": 9002
}
```

the script generates:

```json
{
    "VehicleType": "ArduCopter",
    "UseSerial": false,
    "LocalHostIp": "127.0.0.1",
    "UdpIp": "127.0.0.1",
    "UdpPort": 9003,
    "ControlPort": 9002,
    "X": 0, "Y": 0, "Z": -1
},
{
    "VehicleType": "ArduCopter",
    "UseSerial": false,
    "LocalHostIp": "127.0.0.1",
    "UdpIp": "127.0.0.1",
    "UdpPort": 9013,
    "ControlPort": 9012,
    "X": 3, "Y": 0, "Z": -1
},
{
    "VehicleType": "ArduCopter",
    "UseSerial": false,
    "LocalHostIp": "127.0.0.1",
    "UdpIp": "127.0.0.1",
    "UdpPort": 9023,
    "ControlPort": 9022,
    "X": 0, "Y": 3, "Z": -1
},
{
    "VehicleType": "ArduCopter",
    "UseSerial": false,
    "LocalHostIp": "127.0.0.1",
    "UdpIp": "127.0.0.1",
    "UdpPort": 9033,
    "ControlPort": 9032,
    "X": 3, "Y": 3, "Z": -1
}
```

Camera settings and anything else on the reference vehicle carry over to all of them.

### Script parameters

- `File`: The file to copy to AirSim Settings, or use as a reference for generating multi-drone settings. Can be named explicitly; otherwise it is the first positional argument.
- `OutDir`: The directory to copy the file into, under the name `settings.json`. Defaults to the directory AirSim looks in globally. If you are using a compiled build of the simulation you may want to point this at the executable's directory instead, since AirSim looks there first.
- `NumDrones`: Generates drone entries compatible with what the simulation container expects. Each drone's settings are copied from the vehicle in `File`, with `ControlPort`, `UdpPort`, `X`, and `Y` adjusted automatically. Expects a grid size, `ROW,COL`.
- `XSep`: With `NumDrones`, the x-separation between drones in metres. Defaults to 3.
- `YSep`: With `NumDrones`, the y-separation between drones in metres. Defaults to 3.
- `ZOffset`: With `NumDrones`, the Z offset applied to every drone. Negative values are higher altitudes. Defaults to 0.
- `StartControlPort`: With `NumDrones`, the control port of the first drone; each successive drone is ten higher. Defaults to 9002, giving {9002, 9012, 9022, 9032, ...}.
- `StartUdpPort`: With `NumDrones`, the UDP port of the first drone; each successive drone is ten higher. Defaults to 9003, giving {9003, 9013, 9023, 9033, ...}.