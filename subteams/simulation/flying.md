---
permalink: /simulation/flying/
---

# Flying the Virtual Drone with Code

[Back to Simulation Docs](/docs/simulation/)

This page assumes you have completed [Environment Setup (Windows)](/docs/simulation/install/windows) and can start the containers as described on the [container setup page](/docs/simulation/containers/).

## How the pieces fit together

Three programs run at once, split across two systems:

| Where | What it does                                                                                           |
| --- |--------------------------------------------------------------------------------------------------------|
| Windows | Unreal Engine (Project AirSim as a plugin) includes: the scene, the physics, and the simulated sensors |
| WSL, `env` container | The flight code, plus the interface script that stands the scene up and drives it                      |
| WSL, `sim` container | ArduPilot SITL — the actual autopilot firmware, one instance per drone                                 |

Nothing here runs Unreal inside a container. Unreal runs natively on Windows, and the containers talk to it over the network.

Three connections have to come up, and they depend on each other:

1. The interface script connects to Unreal over the Project AirSim API.
2. Unreal sends sensor data to the SITL over UDP; the SITL sends servo output back.
3. Your flight code connects to the SITL over MAVLink on TCP port `5762` (and `5772`, `5782`, ... for additional drones — the SITL adds 10 per instance).

## Start order matters

**Start these in order.** The interface script loads an *empty* scene first and then waits for you, on purpose: the SITL needs *a* scene to pull data from when it starts, but a drone that spawns before its SITL exists will lock up, and only a full restart recovers it. The prompt in the script is the gap between those two requirements.

### 1. Unreal

Open the Project AirSim Unreal project, open the level you want, and press **Play**.

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
