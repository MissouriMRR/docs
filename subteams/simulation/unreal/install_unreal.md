---
permalink: /simulation/install/
---

# Installing Unreal Engine

[Back to Simulation Docs](/docs/simulation/)

It is recommended that you run Unreal Engine on Linux. If you do not have a native Linux machine, see [Virtual Machines](/docs/virtual_machines/) for help creating an Ubuntu Virtual Machine.

Below is a guide for getting AirSim and Unreal Engine set up on your Linux machine.

## Accessing Unreal Engine Repository

To get started, you will need an Epic Games Account and a GitHub account. These are necessary for you to get access to the Unreal Engine source code repository on Github.

1.


## Compiling Unreal Engine

1. Navigate to [https://github.com/EpicGames/UnrealEngine](https://github.com/EpicGames/UnrealEngine). You will need to be logged in with the GitHub account that you linked to your Epic Games account.
2. Click on the green Code button and select Download ZIP. This will download the repo in a zip file.\
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/simulation/unreal/images/code_button.jpg)
3. Once it is downloaded, move it to your desired location (recommendation is to move it to somewhere in the Documents folder). Right click and select "Open with Archive Manager". Extract the zip file. (Archive Manager may freeze up for a bit before it shows a progress bar).\
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/simulation/unreal/images/archive_manager.jpg)
4. Once it is extracted, open the extracted folder `UnrealEngine-release` in a terminal.
5. In your terminal, run the command `./Setup.sh`. This will set up dependences needed for Unreal Engine. It will take some time. At some point, you will get a pop up about file types. Press yes. You will also need to enter your password in the terminal when prompted.
6. Once the previous command is finished, run the command `./GenerateProjectFiles.sh`
7. Once the previous command is finished, run the command `make`. This will compile Unreal Engine, which may take an hour or more.

You should now have Unreal Engine compiled and set up on your machine.

## Setting Up AirSim

We will now go through the process of setting up AirSim. This should go much faster.

1. In your Documents folder, open a terminal window.
2. Run the command `git clone https://github.com/Microsoft/AirSim.git`
3. Move to the new AirSim directory with the command `cd AirSim`
4. Run the command `./setup.sh`
5. Run the command `./build.sh`

You should now have AirSim set up on your machine.

