---
permalink: /simulation/flying/
---

# Flying the Virtual Drone with Code

[Back to Simulation Docs](/docs/simulation/)

This page assumes you've followed the steps in [Environment Setup (Windows)](/docs/simulation/install/windows) or [Environment Setup (Linux)](/docs/simulation/install/linux)

1. Open your AirSim Unreal Engine project
    - you can run AirSim's default project, called *Blocks*, located in your AirSim folder at `\AirSim\Unreal\Environments\Blocks\Blocks.uproject`
        - open the uproject directly in unreal; trying to run blocks from the Visual Studio Solution probably won't work
2. Open a terminal and navigate to the root of your cloned repository. Then, run `poetry shell`
    - This will activate a virtual environment outfitted with the necessary dependencies need to interact with the simulation with Python.
3. Start the Unreal Engine simulation using the editor's `Play` button
4. Start the PX4 and MavSDK servers by running `\scripts\servers.ps1`
    - if you encounter any issues with PX4 and MavSDK connecting, check out the [environment debugging page](/docs/simulation/environment-debug)
5. Run your code!

Whenever you want to rerun code, you must
- stop the Unreal Simulation
- close the PX4 and MavSDK PowerShell instances
- repeat steps 3-5 in the above guide

There might be a better way to do this, but we haven't discovered it.

When finished running code, run `exit` to exit your Poetry virtual environment.

