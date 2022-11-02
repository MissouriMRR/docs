---
permalink: /simulation/install/
---

# Simulation Installations

## Table of Contents

- [Installing Unreal Engine and Airsim](#installing-unreal-engine-and-airsim)
    - [Installing Unreal Engine and Airsim for Windows](#installing-unreal-engine-and-airsim-for-windows)
    - [Installing Unreal Engine and Airsim for Ubuntu 18.04](#installing-unreal-engine-and-airsim-for-ubuntu-1804)
- [Installing PX4](#installing-px4)


## Installing Unreal Engine and AirSim

[Back to Simulation Docs](/docs/simulation/)

Below is a guide for getting AirSim and Unreal Engine set up on your machine.

**Windows**: Follow the [guide below](#installing-unreal-engine-for-windows) or follow the guide here: [https://microsoft.github.io/AirSim/build_windows/](https://microsoft.github.io/AirSim/build_windows/)

**MacOS**: [https://microsoft.github.io/AirSim/build_macos/](https://microsoft.github.io/AirSim/build_macos/)

**Linux**: Follow the [guide below](#installing-unreal-engine-and-airsim-for-ubuntu) or follow the guide here: [https://microsoft.github.io/AirSim/build_linux/](https://microsoft.github.io/AirSim/build_linux/)

After following the guide, you should have Unreal Engine and AirSim set up on your machine.

### Installing Unreal Engine and Airsim for Windows

__Download the following files:__

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

### Installing Unreal Engine and Airsim for Ubuntu 18.04

**Prerequisites:**

1. Make an account with Epic Games (https://www.unrealengine.com/id/login).
2. Once logged in to Epic Games, go to the “Connected Accounts” tab.
3. Connect your Epic Games account with your GitHub account.
4. Log into GitHub. You should see an invitation from Epic Games for the Unreal Engine repository. Accept the invitation. You now have access to the private UnrealEngine repository.

**Download and install Unreal Engine:**

1. Open a terminal and navigate to where you want to install Unreal Engine.
2. Run the commands (make will take ~50 minutes):\
`git clone -b 4.24 https://github.com/EpicGames/UnrealEngine.git`\
`cd UnrealEngine`\
`./Setup.sh`\
`./GenerateProjectFiles.sh`\
`make`

**Download and install AirSim:**

1. Navigate to where you want to install AirSim.
2. Run the commands:\
`git clone https://github.com/Microsoft/AirSim.git`\
`cd AirSim`\
`./setup.sh`\
`./build.sh`
3. Navigate to where you install Unreal Engine and run Engine/Binaries/Linux/UE4Editor which will start Unreal Editor.
4. On first start you might not see any projects in UE4 editor. Click on Projects tab, Browse button and then navigate to AirSim/Unreal/Environments/Blocks/Blocks.uproject. 
5. (If you get prompted for incompatible version and conversion, select In-place conversion which is usually under "More" options. If you get prompted for missing modules, make sure to select No so you don't exit.)
6. You can now press the Play button in Unreal Editor to run AirSim. Any time you want to run AirSim you must navigate to the Unreal Engine repository and run Engine/Binaries/Linux/UE4Editor (consider making a shortcut for this).

If you make any changes to AirLib code or Unreal/Plugins folder, then you must run `./build.sh` (located in the AirSim repository) and repeat all of the previous steps.

Troubleshooting tip: if your mouse disappears upon pressing play in the Unreal Engine Editor, press shift + f1 to get it back.

You now have everything you need to start exploring the AirSim APIs [https://microsoft.github.io/AirSim/docs/apis/](https://microsoft.github.io/AirSim/docs/apis/). If you are on the Vision Team, this will probably be enough. But if you are on the Flight Team or want to be able to run the flight code produced by the team, you will also need to download and build PX4 (which is the flight controller software we will be using in competition).


## Installing PX4

