---
permalink: /simulation/install/
---

# Simulation Installations

## Installing Unreal Engine and AirSim

[Back to Simulation Docs](/docs/simulation/)

Below is a guide for getting AirSim and Unreal Engine set up on your machine.

**Windows**: Follow the [guide below](#installing-unreal-engine-for-windows) or follow the guide here: [https://microsoft.github.io/AirSim/build_windows/](https://microsoft.github.io/AirSim/build_windows/)

**MacOS**: [https://microsoft.github.io/AirSim/build_macos/](https://microsoft.github.io/AirSim/build_macos/)

**Linux**: [https://microsoft.github.io/AirSim/build_linux/](https://microsoft.github.io/AirSim/build_linux/)

After following the guide, you should have Unreal Engine and AirSim set up on your machine.

### Installing Unreal Engine for Windows

__**Download the following files:**__

**Epic Games Launcher (~1 GB):** [https://www.unrealengine.com/download](https://www.unrealengine.com/download) (You will need to make/link an account)

**Visual Studio 2019 (~3 GB):** [https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16) (You will need a Microsoft account) Make sure select the C++ development pack and check windows sdk 10 while installing!

**Once you finish either of these downloads, start taking the following steps:**

__After you get Visual Studio 2019:__

1. Start x64 Native Tools Command Prompt for VS 2019 (just hit Windows button and search for it - it should pop up). Use this terminal for all future commands in this guide.
2. Navigate to wherever you store git repositories (ask someone if you need help).
3. Clone the AirSim repo: git clone https://github.com/Microsoft/AirSim.git and go to the AirSim directory (~6 GB).
4. Run build.cmd from the command line. This builds AirSim, which is actually just a plugin for Unreal Engine.

__After you get Epic Games Launcher:__

1. Open the launcher, go to the Library tab, and download Unreal 4.24.3 (~20 GB).

__Once everything above is finished:__

1. Navigate to folder AirSim\Unreal\Environments\Blocks and run update_from_git.bat.
2. Open the generated .sln file in Visual Studio 2019.
3. Make sure Blocks project is the startup project, build configuration is set to DebugGame_Editor and Win64. Hit F5 to run.
4. Press the Play button in Unreal Editor to launch the world (the 3D world should open upon doing this - note: world assets will begin to download, so you may not be able to see everything right away).

You now have everything you need to start exploring the AirSim APIs [https://microsoft.github.io/AirSim/docs/apis/](https://microsoft.github.io/AirSim/docs/apis/). If you are on the Vision Team, this will probably be enough. But if you are on the Flight Team or want to be able to run the flight code produced by the team, you will also need to download and build PX4 (which is the flight controller software we will be using in competition). (Simulation team will obviously need to do further steps.)

## Installing PX4

