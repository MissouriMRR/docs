---
permalink: /simulation/install/linux/
---

# Simulation Installation and Environment Setup (Linux)

[Back to Simulation Docs](/docs/simulation/)

There are several pieces of software needed to get up and running with the simulator, namely PX4, Unreal Engine, and MavSDK. You will also need the software ubiquitous to the software team ([git](https://git-scm.com), [poetry](https://python-poetry.org)).

This page applies only to Linux (and MacOS) users.

## Table of Contents

- [Installing Unreal Engine and Airsim](#installing-unreal-engine-and-airsim)
    - [Installing Unreal Engine and Airsim for Ubuntu 18.04](#installing-unreal-engine-and-airsim-for-ubuntu-1804)
- [Installing PX4](#installing-px4-for-ubuntu-1804)


## Installing Unreal Engine and AirSim

Below is a guide for getting AirSim and Unreal Engine set up on your machine.

**MacOS**: [https://microsoft.github.io/AirSim/build_macos/](https://microsoft.github.io/AirSim/build_macos/)

**Linux**: Follow the [guide below](#installing-unreal-engine-and-airsim-for-ubuntu) or follow the guide here: [https://microsoft.github.io/AirSim/build_linux/](https://microsoft.github.io/AirSim/build_linux/)

After following the guide, you should have Unreal Engine and AirSim set up on your machine.

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


## Installing PX4 for Ubuntu 18.04

1. Open a terminal and make yourself a member of the group “dialout” with the command `sudo usermod -a -G dialout $USER`
2. Logout and login again (or just restart your computer) to allow this group change to take effect.
3. Download the development toolchain installation script with:
`wget https://raw.githubusercontent.com/PX4/Devguide/v1.9.0/build_scripts/ubuntu_sim_nuttx.sh`
4. Run the script with the command  `source ubuntu_sim_nuttx.sh`
5. Restart your computer on completion.
6. Open a terminal and go to where you want to install the PX4 Firmware repository.
7. Run git `clone --recursive -j8 https://github.com/PX4/Firmware.git`
8. Go to the newly-cloned Firmware directory and checkout this “known good” branch: `git checkout v1.9.2`
    \
    **All of the previous steps you should only ever do once, but this next step you will do every time you want to start the PX4 SITL (which is every time you want to run flight code)**
9. `make px4_sitl_default none_iris` **(the first time you do this step, you will encounter several red-texted messages: in response to each one type ‘u’ and hit enter)**
After you have completed step 6 an interactive command prompt will begin. The information on this screen is important to successfully connecting the PX4 SITL with AirSim. **Make sure that your AirSim settings file matches with the highlighted portions below:**\
    PX4 SITL Console:
    ```
    INFO  [simulator] Waiting for simulator to connect on TCP port 4560
    INFO  [init] Mixer: etc/mixers/quad_w.main.mix on /dev/pwm_output0
    INFO  [mavlink] mode: Normal, data rate: 4000000 B/s on udp port 14570 remote port 14550
    INFO  [mavlink] mode: Onboard, data rate: 4000000 B/s on udp port 14580 remote port 14540
    ```
    \
    AirSim Settings file:
    ```
    {
        "SettingsVersion": 1.2,
        "SimMode": "Multirotor",
        "Vehicles": {
            "PX4": {
                "VehicleType": "PX4Multirotor",
                "UseSerial": false,
                "UseTcp": true,
                "TcpPort": 4560,
                "ControlPort": 14580,
                "params": {
                    "NAV_RCL_ACT": 0,
                    "NAV_DLL_ACT": 0,
                    "LPE_LAT": 47.641468,
                    "LPE_LON": -122.140165,
                    "COM_OBL_ACT": 1
                }
            }
        }
    }
    ```

Now you are ready to begin experimenting with your flight code in the simulator! Check out [https://github.com/mavlink/MAVSDK-Python](https://github.com/mavlink/MAVSDK-Python) (specifically the examples folder) for some inspiration). Once you clone the repo, make sure to `pip3 install mavsdk` before trying to run any of the examples.

