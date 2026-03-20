---
permalink: /simulation/flying/
---

# Flying the Virtual Drone with Code

[Back to Simulation Docs](/docs/simulation/)

This page assumes you've followed the steps in [Environment Setup (Windows)](/docs/simulation/install/windows) or [Environment Setup (Linux)](/docs/simulation/install/linux).

## Table of Contents

- [Flying the Virtual Drone with Code](#flying-the-virtual-drone-with-code)
  - [Table of Contents](#table-of-contents)
  - [During Development (Unreal Editor and running code manually)](#during-development-unreal-editor-and-running-code-manually)
    - [Configuring the Environment](#configuring-the-environment)
      - [Single-Drone](#single-drone)
      - [Multi-Drone Settings](#multi-drone-settings)
      - [More on `.\update_airsim_settings.ps1`](#more-on-update_airsim_settingsps1)
    - [Flying](#flying)
  - [Production](#production)

## During Development (Unreal Editor and running code manually)

### Configuring the Environment

Ensure that your AirSim settings are up to date using the `./update_airsim_settings.ps1` command located in the root of the `SUAS` repository. The essence of this script is copying a file to where AirSim expects a `settings.json` file. We have a few pre-made templates available, but you can refer to the [Project AirSim Settings Documenation](https://github.com/iamaisim/ProjectAirSim/blob/main/docs/config.md) or [AirSim Settings Documentation](https://microsoft.github.io/AirSim/settings/) for more details.

#### Single-Drone

Flying a single drone is easiest. For a quick start, you can run the following command from the SUAS repository root to set up your settings:

```powershell
.\update_airsim_settings.ps1 .\templates\airsim-settigns-ardupilot.json
```

If you have a custom settings file:

```ps1
.\update_airsim_settings.ps1 .\path\to\your\settings.json
```

#### Multi-Drone Settings

If you wish to fly multiple drones, that must be reflected in your settings. Manually doing this is tedious, so our script provides functionality to automate this. For a quick start with 4 drones, you can use the following:

```ps1
.\update_airsim_settings.ps1 .\templates\airsim-settings-multidrone.json -NumDrones 2,2
```

The number of drones is given as a grid (`rows,columns`), so the total number of drones is `rows * columns`.

It is important to note that the file provided to the script is used as a *reference* by which a new settigns file is generated. The global settings of the reference is copied as is; the vehicle provided in the reference's `"Vehicles"` section is used as a base for the generated vehicle entries. All settings within the reference vehicle are copied verbatim except for the ports and X, Y, and Z offsets. The port are incremented by `10` from the reference droens for each drone, and the X, Y, and Z are set based on command arguments (which default to 3-meter separation).

For instance, given this reference vehicle:

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

The following drones will be generated:

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

Any camera settings and such will be carried over for all drones.

#### More on `.\update_airsim_settings.ps1`

**Parameters**:

- `File`: The file to copy to AirSim Settings or use as a reference for generating multidrone settings. Can be explicitly named; otherwise is the first positional argument.
- `OutDir`: The directory to copy the given file to, under the name "settings.json". By default, this is the directory AirSim looks for settigns globally. If using a compiled version of the simulation, you may want to change this to the directory of the executable, since AirSim looks there first.
- `NumDrones`: This is part of a ease-of-use functionality that automatically creates drones compatible with what our simulation Docker container expects. The settings of each drone is copied from the drone settings provided by the given File, but the properties "ControlPort", "UdpPort", "X", and "Y" are adjusted automatically and may be overridden. This parameter expects a grid size input, or ROW,COL.
- `XSep`: Given that NumDrones is provided, determines the x-separation between drones in meters. This parameter is 3 by default.
- `YSep`: Given that NumDrones is provided, determines the y-separation between drones in meters. This parameter is 3 by default.
- `ZOffset`: Given the NumDrones is provided, sets the Z offset for all drones. All drones have the same offset. Negative values correspond to higher altitudes. This parameter is 0 by default.
- `StartControlPort`: Given that NumDrones is provided, determines the control port of the first drone. Each successive drone has a port ten higher than the previous. It is 9002 by default.
  - e.g., drone ports will be {9002, 9012, 9022, 9032, ...} if this value is 9002
- `StartUdpPort`: Given that NumDrones is provided, determines the UDP port of the first drone. Each successive drone has a port ten higher than the previous. It is 9003 by default.
  - e.g., drone ports will be {9003, 9013, 9023, 9033, ...} if this value is 9003

### Flying

1. Open your AirSim Unreal Engine project
    - you can run AirSim's default project, called *Blocks*, located in your AirSim folder at `\AirSim\Unreal\Environments\Blocks\Blocks.uproject`
        - open the uproject directly in Unreal; trying to run blocks from the Visual Studio Solution probably won't work
        - if it says that the project was built with a different Unreal version, click "Yes" to rebuild with the version you have
2. Open a terminal and navigate to the root of the `SUAS` repository
    - start WSL using the `wsl` command as well
3. Start the Unreal Engine simulation using the editor's `Play` button
    - located above the viewport to the far right
    - if not immediately visible, press the double-arrow ($>\!\!>$) button, then `Play`
    - the Unreal Editor should freeze (this is normal)
4. Start the `sim` and `env` containers using `./run_container.sh`
    - **for multi-drone**, you must specify the number of drones using the `NUM_DRONES` environment variable: `NUM_DRONES=4 ./run_container.sh`
      - you only need to provide the environment variable when running a command that starts the `sim` container
5. run your code:
    - if using the `env` container, attach to the `env` container: `./run_container.sh attach env`
    - run your code now

Whenever you want to rerun code, you must

- stop the Unreal Simulation
- shutdown the `sim` container
  - you can use `./run_container.sh shutdown` to shutdown `env` and `sim`
- repeat steps 3-5 in the above guide

## Production

The final rendition of the simulation is a work in progress! Use the developer stuff for now.
