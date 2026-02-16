---
permalink: /simulation/installation/windows/
---

# Simulation Installation and Environment Setup (Windows)

[Back to Simulation Docs](/docs/simulation/)

**Note:** This page applies only to Windows users.

## Table of Contents

It is recommended you follow this tutorial in the order listed.

- [Installing Unreal Engine](#installing-unreal-engine)
- [Installing Git](#installing-git)
- [Installing Docker](#installing-docker)
- [Installing Project AirSim](#installing-project-airsim)
    - [Visual Studio](#visual-studio)
    - [Project AirSim](#project-airsim)
- [Initial Environment Setup](#environment-setup)
    - [Simulation Git Repo](#simulation-git-repository)
- [Next Steps](#next-steps)

## Installing Unreal Engine

We will be using Unreal Engine for simulating virtual drones. If you have the Epic Games Launcher installed already (e.g., if you own *Fortnite*), you can download Unreal Engine from the "Unreal Engine" tab. If you don't have the Epic Games Launcher, then you will have to [download](https://store.epicgames.com/en-US/download) it to install Unreal Engine.

The Unreal Engine version you will download is **5.2.1**. Project AirSim, which allows us to simulate multirotors in Unreal, will only work for this version.

Once Unreal Engine has finished downloading, you **will need to run it once before doing anything else**. You can continue on to the following steps until told otherwise.

## Installing Git

Go to the following link and download [Git](https://git-scm.com). The installer should prompt you to set up your credentials needed for downloading GitHub repositories.

## Installing Docker

Head to our Docker download page and follow the instructions [there](/docs/simulation/installation/docker/).

## Installing Project AirSim

### Visual Studio

To install Project AirSim, you must first install Microsoft's [Visual Studio Community 2022](https://visualstudio.microsoft.com). This will also be the IDE you will use for programming C++ code for Unreal.

When installing, you must select the following under the **Individual Components** tab:
- `C++ Development Pack`
- `Windows SDK 10`
- `.NET 8.0 Runtime (Long Term Support)`
- `.NET Framework 4.8 SDK`
- `MSVC v143 - VS 2022 C++ x64/x86 build tools (v14.37-17.7)(Out of support)`

You will need to go to `C:\Users\<YOUR_USER>\AppData\Roaming\Unreal Engine\UnrealBuildTool\BuildConfiguration.xml`
- If you cannot find the AppData folder, the easiest way to get there is to press the `Windows` and `R` keys at the same time. This should open a prompt in the bottom left of your screen called `Run`. In the search box type `%appdata%` and press enter. This should put you in your `C:\Users\<YOUR_USER>\AppData\Roaming` folder.

Now replace the contents of BuildConfiguration.xml with the following:
```
<?xml version="1.0" encoding="utf-8" ?>
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
    <WindowsPlatform>
        <CompilerVersion>14.37.32822</CompilerVersion>
    </WindowsPlatform>
</Configuration>
```

### Project AirSim

Project AirSim is an Unreal Engine plugin for simulating multirotors and cars that is maintained by IAMAISIM; it is the backbone of simulation.

To install Project AirSim, follow these steps:

1. clone the AirSim GitHub repository: `git clone https://github.com/iamaisim/ProjectAirSim.git`
    - the location of your local copy does not matter, as long as you know where it is.
2. Make sure to close out of all terminals opened prior to this step, if you don't environment variables can and will break.
3. Launch `x64 Native Tools Command Prompt for VS 2022` (using Windows search)
    - Navigate to where you cloned Project AirSim with the `cd` command
    - Run `set UE_ROOT=C:\path\to\UE_5.2`
    - Run the command `build.cmd all`
    - Wait for Project AirSim to build
        - You can move on to the next step while waiting, this will take a while.

## Environment Setup

### Simulation Git Repository

The simulation git repository contains many useful files that streamline running code. Open a terminal (by typing `Terminal` in the Windows search), and `cd C:/path/to/where/you/want/the/repo/at`.

Now run `git clone https://github.com/MissouriMRR/Simulation-2023.git`. This will create a new directory called `\Simulation-2023\`



## Next Steps

If you've run all the above steps, you're environment should be set up! 

Next, you should go to the [flying page](/docs/simulation/flying) to see how to fly the virtual drone with Python code.
