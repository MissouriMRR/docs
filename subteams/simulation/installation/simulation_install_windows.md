---
permalink: /simulation/install/windows/
---

# Simulation Installation and Environment Setup (Windows)

[Back to Simulation Docs](/docs/simulation/)

There are several pieces of software needed to get up and running with the simulator, namely PX4, Unreal Engine, and MavSDK. You will also need the software ubiquitous to the software team:
- [git](https://git-scm.com)
- [poetry](https://python-poetry.org/docs/)
    - *if you are on **Windows 11***, Poetry will not automatically be added to your path
    - find the folder where poetry was installed (likely `C:\Users\<your user>\AppData\Roaming\pypoetry`) in a file explorer or terminal
        - copy the *full path* of the `\venv\Scripts` subdirectory
        - type "environment variables" into the Windows search bar and selected the "Edit system environment variables" option 
            - if this doesn't work, go to your Windows Settings > About > Advanced System Properties > Environment variables...
        - in the top list of variables (the User variables), click the `PATH` option, then click "Edit..."
        - click the "New" button on the right; paste the full path of the Poetry scripts directory into the next PATH entry
            - this path will probably look something like `C:\Users\<your username>\AppData\Roaming\pypoetry\venv\Scripts`
        - click "Ok" on all the Windows you've pull up to the point
        - restart any terminals you have open
        - now you should have access to the `poetry` command!

**Note:** This page applies only to Windows users.

## Table of Contents

It is recommended you follow this tutorial in the order listed.

- [Installing Unreal Engine](#installing-unreal-engine)
- [Installing AirSim](#installing-airsim)
    - [Visual Studio](#visual-studio)
    - [AirSim](#airsim)
- [Installing PX4 and MavSDK](#installing-px4-and-mavsdk)
    - [PX4](#px4)
    - [MavSDK](#mavsdk)
- [Initial Environment Setup](#environment-setup)
    - [Simulation Git Repo](#simulation-git-repository)
    - [Configuration File Setup](#configuration-file-setup)
- [Next Steps](#next-steps)

## Installing Unreal Engine

We will be using Unreal Engine for simulating virtual drones. If you have the Epic Games Launcher, you can download Unreal Engine from the "Unreal Engine" tab. If you don't have the Epic Games Launcher, then you will have to [download](https://store.epicgames.com/en-US/download) it to install Unreal Engine.

The Unreal Engine version you will download is **4.27.2**. AirSim, which allows us to simulate multirotors in Unreal, does *not* work with Unreal Engine 5 or greater.

Finally, run Unreal Engine at least once before continuing to the next steps.

## Installing AirSim

### Visual Studio

To install AirSim, you must first install Microsoft's [Visual Studio Community 2022](https://visualstudio.microsoft.com). This will also be the IDE you will use for programming C++ code for Unreal.

When installing, you must select the following:
- `C++ Development Pack`
- `Windows SDK 10`

### AirSim

AirSim is an Unreal Engine plugin for simulating multirotors and cars that was previously maintained by Microsoft; it is the backbone of simulation.

To install AirSim, follow these steps:

1. clone the AirSim GitHub repository: `git clone https://github.com/Microsoft/AirSim`
    - the location of your local copy does not matter
2. Launch `x64 Native Tools Command Prompt for VS 2022`
    - navigate to where you cloned AirSim with the `cd` command
    - run the command `.\build.cmd`
    - wait for AirSim to build
        - you can move on to the next step while waiting

## Installing PX4 and MavSDK

These two softwares act as the bridge between your code and the virtual drone.

### PX4

1. download the [PX4 0.9 installer](https://github.com/PX4/PX4-windows-toolchain/releases/download/v0.9/PX4.Windows.Cygwin.Toolchain.0.9.msi) and run the `.msi` file that is downloaded.
    - if Windows attempts to prevent you from running the installer, click on "more info" and click "run anyway"
    - you may install PX4 wherever you please (default location is `C:\PX4\`)
    - **Do NOT check the box at the end that says “Clone PX4 Repository and Start Simulation"**
2. Open a command line prompt/open file explorer and navigate to where you installed PX4
3. Run/click on `run-console.bat` to start the Cygwin bash console
4. Clone the PX4 Firmware repository from within the opened bash console: `git clone --recursive -j8 https://github.com/PX4/Firmware.git`
5. go to the newly-cloned Firmware repository and checkout this "known good" branch: `git checkout v1.11.3`
6. run `make px4_sitl_default none_iris`. You will encounter serveral red text messages: in response to each, type 'u' and press enter.

### MavSDK

The MavSDK server is what your Python code will directly interface with; then, the MavSDK server will relay instructions to PX4, which in turn communicates with the virtual drone.

1. [install MavSDK](https://github.com/mavlink/MAVSDK/releases/download/v1.4.16/mavsdk-windows-x64-release.zip)
2. unzip the downloaded ZIP file wherever you please
    - keep track of where you unzipped MavSDK; you will need it for Environment Setup

## Environment Setup

### Simulation Git Repository

The simulation git repository contains many useful files that streamline running code. Open a terminal instance and clone it to your desired location using `git clone https://github.com/MissouriMRR/Simulation-2023.git`. This creates a new directory called `\Simulation-2023\` in the current working directory of your terminal.

Next, you should navigate to your local copy of the repository (just run `cd .\Simulation-2023\` immediately after running `git clone`) and run `poetry install`. This should create a virtual environment for you to run code.

### Configuration File Setup

After cloning our repository, navigate to the `\scripts\` directory within it. In that directory, run `.\setup.ps1`. (If this command results in an error that mentions Execution Policy, follow [these steps](#incompatible-execution-policy).) This command will create two files: `\scripts\server-config.json` and `\scripts\airsim-settings.json`. Open `server-config.json` in a text editor of your choice and do the following:
1. replace `mav_sdk_server_path` with the absolute path to the `\bin` directory located in your MavSDK server install (the zip you downloaded previously)
2. replace `px4_path` with the absolute path of your PX4 directory (default is `C:\PX4\`)
3. `drone_port` should ideally be correct already; if you encounter any issues with connecting to the virtual drone at later steps, check the [debugging page](/docs/simulation/environment-debug/windows)

Your final `server-config.json` should look similar to this:

```json
{
    "drone_port": 14030,
    "mavsdk_server_path": "C:\\Dev\\school\\multirotor\\MavSDK\\bin",
    "px4_path": "C:\\PX4"
}
```

#### Incompatible Execution Policy

If you encountered no errors completing the above, you may skip this.

Sometimes, Windows may not allow you to run PowerShell scripts due to your Execution Policy. To fix this, start a PowerShell/Terminal as Administrator (right-click the application as click "Run as Administrator"). Then, run the following command:
```ps1
Set-ExecutionPolicy RemoteSigned
```
Afterward, restart all Terminal/PowerShell instances to apply the change.
## Next Steps

If you've run all the above steps, you're environment should be set up! 

Next, you should go the the [flying page](/docs/simulation/flying) to see how to fly the virtual drone with Python code.

Also, if you run into problems while attempting to fly the drone, check out the [environment debugging](/docs/simulation/environment-debug/windows) page.

