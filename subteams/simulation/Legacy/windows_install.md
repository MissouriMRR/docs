---
permalink: /simulation/legacy/install/windows
---

# Simulation Installation and Environment Setup (Windows)

[Back to Simulation Docs](/docs/simulation/)

**Note:** This page applies only to Windows users.

There are several pieces of software needed to get up and running with the simulator, namely PX4, Unreal Engine, and MavSDK. You will also need the software ubiquitous to the software team:
- [git](https://git-scm.com)

## Table of Contents

It is recommended you follow this tutorial in the order listed.

- [Installing Unreal Engine](#installing-unreal-engine)
- [Installing AirSim](#installing-airsim)
    - [Visual Studio](#visual-studio)
    - [AirSim](#airsim)
- [Initial Environment Setup](#environment-setup)
    - [Simulation Git Repo](#simulation-git-repository)
    - [Configuration File Setup](#configuration-file-setup)
- [Next Steps](#next-steps)
- 
We will be using Unreal Engine for simulating virtual drones. If you have the Epic Games Launcher installed already (e.g., if you own *Fortnite*), you can download Unreal Engine from the "Unreal Engine" tab. If you don't have the Epic Games Launcher, then you will have to [download](https://store.epicgames.com/en-US/download) it to install Unreal Engine.

The Unreal Engine version you will download is **4.27.2**. AirSim, which allows us to simulate multirotors in Unreal, does *not* work with Unreal Engine 5 or greater.

Finally, run Unreal Engine at least once before continuing to the next steps.

## Installing AirSim

### Visual Studio

To install AirSim, you must first install Microsoft's [Visual Studio Community 2022](https://visualstudio.microsoft.com). This will also be the IDE you will use for programming C++ code for Unreal.

When installing, you must select the following:
When installing, you must select the following under the **Individual Components** tab:
- `C++ Development Pack`
- `Windows SDK 10`
- `.NET 8.0 Runtime (Long Term Support)`
- `.NET Framework 4.8 SDK`
- `MSVC v143 - VS 2022 C++ x64/x86 build tools (v14.37-17.7)(Out of support)`

### AirSim
AirSim is an Unreal Engine plugin for simulating multirotors and cars that was previously maintained by Microsoft; it is the backbone of simulation.

To install AirSim, follow these steps:

1. clone the AirSim GitHub repository: `git clone https://github.com/Microsoft/AirSim`
    - the location of your local copy does not matter
2. Launch `x64 Native Tools Command Prompt for VS 2022` (using Windows search)
    - navigate to where you cloned AirSim with the `cd` command
    - run the command `.\build.cmd`
    - wait for AirSim to build
        - you can move on to the next step while waiting

#### Incompatible Execution Policy

If you encountered no errors completing the above, you may skip this.

Sometimes, Windows may not allow you to run PowerShell scripts due to your Execution Policy. To fix this, start a PowerShell/Terminal as Administrator (right-click the application as click "Run as Administrator"). Then, run the following command:
```ps1
Set-ExecutionPolicy RemoteSigned
```
Afterward, restart all Terminal/PowerShell instances to apply the change.
## Next Steps

If you've run all the above steps, you're environment should be set up! 