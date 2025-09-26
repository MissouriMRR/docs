---
permalink: /flight/installation_guide/
---

# Flight Docs

[Back to Flight Docs](/docs/flight/)

## Installation Process

### Step 1: Install a Linux Virtual Machine (Optional)

In order to run Multirotor code and program effectively with DroneKit and ArduPilot, a Linux virtual machine (VM) with Ubuntu 24.04 is recommended. If you're on Windows 11, [using Windows Subsystem for Linux (WSL) is recommended](#alternative-using-windows-subsystem-for-linux-wsl), instead.

Use the following link to download the Ubuntu OS:

- [https://releases.ubuntu.com/noble/](https://releases.ubuntu.com/noble/)

And choose among the following links for your preferred hypervisor:

- [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

#### Alternative: Using Windows Subsystem for Linux (WSL):

On Windows, you can install a more lightweight Linux VM using Windows
Subsystem for Linux (WSL). To install Ubuntu 24.04 using WSL, open a PowerShell instance and run the following:

```ps1
wsl --install "Ubuntu-24.04"
```

To run your virtual machine, execute `wsl` in any PowerShell/Terminal. To see more commands, you can use `wsl --help`.

If you will be using our custom Unreal-based simulation (or need to network between native Windows programs and programs running in WSL), it is easiest to use mirrored networking mode. Unfortunately, this is only compatible with WSL2, meaning that Windows 11 is required.

Run the following command in PowerShell to set up mirrored networking:

```powershell
"[wsl2]`nnetworkingMode=mirrored" | Out-File -Encoding ascii -FilePath "$env:UserProfile\.wslconfig"
```

Alternatively, you may copy the following into a file named `.wslconfig` within your user directory:

```txt
[wsl2]
networkingMode=mirrored
```

For your changes to take effect, you need to restart WSL. To do this, run:

```ps1
wsl --shutdown
wsl # restart wsl
```

#### More Information on Virtual Machines

You can read more about virtual machines on
[our page about virtual machines](/docs/virtual_machines/).

### Step 2: Installing Git

> For the rest of this guide, make sure to execute commands within your virtual machine!

We manage our source files using [Git](https://git-scm.com) and [GitHub](https://github.com); you will need Git installed on your computer and a GitHub account to contribute to our projects.

#### Install Git

##### Linux/WSL (Ubuntu)

Run the following command:

```bash
sudo apt update && sudo apt install git
```

##### Windows Native

To install on Windows natively, you must use an installer:

- [https://git-scm.com/downloads/win](https://git-scm.com/downloads/win)

WSL has access to your files on your Windows machine, so you can use Git directly on Windows if you desire. Keep in mind that only the commands involving `git` will work outside of WSL in the context of this guide; everything else should be executed within WSL.

#### Create a GitHub Account

_If you don't have a GitHub account_, visit this link to create an account:

- [https://github.com/signup](https://github.com/signup)

It is recommended you use a personal email and not your school email when creating your account so you have complete control over your account even after you graduate.

##### SSH Keys

You will need to associate an SSH key with your GitHub account to push to our repository. GitHub has a good tutorial on how to set this up, so check it out:

- [https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### Step 3: Environment Setup

#### Clone The Repository

First, you must clone the flight team's Git repository. Run the following in your shell:

```bash
git clone git@github.com:MissouriMRR/SUAS-2025.git
```

> This will create a new folder, `SUAS-2025`, in your current working directory. Make sure to run that command in the folder into which you want to download the code!
> You will need to have [SSH Keys](#ssh-keys) set up for this command to work.

#### Install Host Dependencies

Next, change directories to the repository:

```bash
cd SUAS-2025
```

In the repository, we have a script to install the necessary host packages. If you will not need GPU access when coding, run the following:

```bash
./install.sh
```

If you need GPU access, run the following:

```bash
./install.sh nvidia  # if you have an nvidia GPU
# OR
./install.sh amd     # if you have an AMD GPU
```

> AMD GPUs are currently not supported (I have an nvidia GPU, so I don't know the AMD install stuff). If you have an AMD GPU, feel free to add to the install script and docs!

You will also need the drivers corresponding to your GPU:

- Nvidia CUDA Toolkit: <https://developer.nvidia.com/cuda-zone>
- AMD ROCm Install: <https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/quick-start.html>

**You will need to restart your terminal/shell for the installation to complete.**

#### Building Containers

To simplify (most) of environment setup, we have [containerized](https://en.wikipedia.org/wiki/Containerization_(computing)) our environment. Specifically, we use [Podman](https://podman.io), which is pretty much the same as its more popular counterpart, [Docker](https://www.docker.com). (Podman and Docker both follow the [Open Container Initiative](https://opencontainers.org) standards, so they are largely interchangable.)

We use a companion command called `podman-compose` (similar to `docker-compose`) that allows us to define how to run our containers in a `compose.yml` file.

To build our containers, run the following command from within the `SUAS-2025` folder:

```bash
podman-compose build
```

This may take a while, so do something else in the meantime.

### Step 4: Running Containers (Using the Environment)

We have two containers: `env` and `sim`. The `env` container contains everything you need to run your code; the `sim` container will run an [ArduPilot](https://ardupilot.org) drone simulation upon startup.

> By default, the `sim` container is meant to be used with the Simulation Subteam's Unreal simulation. If you need to override this, use the `compose.override.yml` file to override the `command` property for the `sim` service to the desired command you can run ([see here](#example-overriding-the-sim-containers-start-command)). If you don't know how compose files work, you can look to `compose.yml` for reference or [read this](https://docs.docker.com/compose/).

For ease of use, we have a `run_containers.sh` script. To run both `env` and `sim`, simply run:

```bash
./run_containers.sh
```

To run the `env` container on its own, run:

```bash
./run_containers.sh env
```

To attach to the `env` container (connect to it using an interactive shell), run:

```bash
./run_containers.sh attach env
```

> Your local SUAS repository code will be mounted in the `env` container, so any changes made to your local copy of the code is automatically reflected in the container, and vice versa. Essentially, the `env` container is a glorified virtual environment.

To detach, run the following:

```bash
exit
```

> This will also shut down the `env` container; you'll need to start it again.

To shutdown any running containers, do:

```bash
./run_containers.sh shutdown
```

For more commmands, run:

```bash
./run_containers.sh help
```

If you want to take matters into your own hands, you'll need to know how to run/use containers:

- `podman` Documenation: https://docs.podman.io/en/latest/Tutorials.html
- `podman-compose` Documentation: https://docs.podman.io/en/latest/markdown/podman-compose.1.html
- `docker` Documentation: https://docs.docker.com/build/
   - 99% of stuff that applies to Docker applies to Podman 
- `docker-compose` Documentation: https://docs.docker.com/compose/

#### Configuring the Containers

If you need to configure how a container is run, create a file called `compose.override.yml` in the `SUAS` repository's root directory. This file will allow you to override parameters set in `compose.yml` without modifying up `compose.yml`. If you ran `install.sh` with a GPU selected, `compose.override.yml` should already exist.

For more information on compose files, see the following:

- `podman-compose` Documentation: https://docs.podman.io/en/latest/markdown/podman-compose.1.html
- `docker-compose` Documentation: https://docs.docker.com/compose/

##### Example: Overriding The `sim` Container's Start Command

By default, the `sim` container is configured to run the following command on startup:

```bash
python /ardupilot/Tools/autotest/sim_vehicle.py -v ArduCopter -f airsim-copter --out=127.0.0.1:14550
```

This is the command needed to connect to AirSim (the foundation of the Simulation Subteam's Unreal-based simulation of the SUAS competition). If you need to run a different command, you can use `compose.override.yml`:

```yaml
version: "3"
services:
  sim:
    command: python /ardupilot/Tools/autotest/sim_vehicle.py -v copter -L GolfCourse --map
```

This will override the `command` field for the `sim` service; essentially, whatever is in the `command` field will run upon container startup. The example above works for a non-Unreal based drone simulation.

If you already have content in `compose.override.yml`, such as enabling GPU usage, just append what you need to the file:

```yaml
version: "3"
services:
  env:
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
  sim:
    command: python /ardupilot/Tools/autotest/sim_vehicle.py -v copter -L GolfCourse --map
```

The above combines the alternate `sim` command with an Nvidia GPU-enabled `env` container.

### Step 5: Installing Useful Programming Tools

#### Visual Studio Code

Visual Studio Code (VS Code) is a popular code editor with numerous free extensions you
can download to add features and customizations; however, if you wish to use a different IDE/text editor, feel free to do so.

If you are using WSL, VS Code might already be installed. You can check if VS Code is installed by seeing if the following command has any output:

```bash
which code
```

If VS Code is not installed, you can run the following command to install VS Code:

```bash
sudo snap install --classic code
```

Then, to open VS Code, simply run the following command in the directory you want to edit
code in:

```bash
code .
```

Before you push any code to a branch in a Multirotor repo, run Pre-commit by typing
`pre-commit` into the terminal (from within the `env` container if you don't have it installed locally) before pushing code, and after adding files to a commit. This
will auto-format your code and make it look somewhat nice.

### QGroundControl (non-WSL users)

For the next piece of software, open a web browser and use the following link:

- [https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html)

This page redirects you to the instructions for downloading QGroundControl, a
supplementary software that allows you to get an overhead map of the drone during flight.
Follow the instructions for Linux download, or if you’re the brave soul using macOS, use
the instructions for that operating system. Be sure to follow the instructions for
downloading the installer file as well as any extra packages required (like libfuse2).

#### Mission Planner (WSL users)

If you're using WSL, you should install Mission Planner instead of QGroundControl:
[https://ardupilot.org/planner/docs/mission-planner-installation.html](https://ardupilot.org/planner/docs/mission-planner-installation.html).
