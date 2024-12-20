---
permalink: /flight/installation_guide/
---

# Flight Docs

[Back to Flight Docs](/docs/flight/)

# Virtual Machine Installation Process

**Step 1: Install a Linux Virtual Machine**

In order to run Multirotor code and program effectively with DroneKit and ArduPilot, a
Linux virtual machine (VM) with Ubuntu 22.04 is recommended. Use the following link to
download the Ubuntu OS:

### Ubuntu Link:

-   [https://releases.ubuntu.com/jammy/](https://releases.ubuntu.com/jammy/)

And choose among the following links for your preferred virtual machine:

### Virtual Machine Links:

-   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
-   [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

A Linux VM is not required; it is entirely possible to run Multirotor code using macOS.
However, **_please_** do not use Windows. The following instructions are based on Ubuntu
22.04.

### Alternative: Using Windows Subsystem for Linux (WSL):

Alternatively, on Windows, you can install a more lightweight Linux VM using Windows
Subsystem for Linux (WSL). Simply install
[Ubuntu 22.04 from the Microsoft Store](https://www.microsoft.com/store/productId/9PN20MSR04DW).
Then, in the Windows Terminal app, click the + button in the title bar and choose Ubuntu
to open a new terminal in your Ubuntu VM.

### More Information on Virtual Machines

You can read more about virtual machines on
[our page about virtual machines](/docs/virtual_machines/).

\
**Step 2: Installing Basic Software**

Since you have created a new virtual machine, some basic software must be installed before
we can proceed. First and foremost, Git. Use the following command:

```
sudo apt install git
```

Next is Python, the language we will be programming in for the majority of the semester.
We will be using Python 3.10 which can be installed through the terminal. The first
command to copy is:

```
sudo apt install software-properties-common -y
```

After that command finishes, use the next command:

```
sudo add-apt-repository ppa:deadsnakes/ppa
```

When prompted, hit enter and allow the downloads to finish. These commands allow for this
version of Python to be easily updated from the command line using

```
sudo apt update
```

Finally, use the command

```
sudo apt install python3.10
```

to install Python 3.10 on your virtual machine.

You will need to change the Python interpreter within your IDE of choice, however,
independently.

You can verify the version of Python installed to your computer using the following
command: (The V should be capitalized)

```
python3.10 -V
```

If everything was installed correctly, you should see Python 3.10.4 or above.

\
**Step 2.5: Installing Visual Studio Code (Optional)**

Visual Studio Code (VS Code) is a popular code editor with numerous free extensions you
can download to add features and customizations. If you are using WSL, VS Code might
already be installed. You can check if VS Code is installed by seeing if the following
command has any output:

```
which code
```

If VS Code is not installed, you can run the following command to install VS Code:

```
sudo snap install --classic code
```

Then, to open VS Code, simply run the following command in the directory you want to edit
code in:

```
code .
```

\
**Step 3: Installing ArduPilot Firmware**

Copy the following Git command into a terminal:

```
git clone --recurse-submodules https://github.com/ArduPilot/ardupilot.git
```

After the repo has fully downloaded, copy the following commands into a terminal to setup
ArduPilot:

```
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y
./waf configure --board sitl
./waf copter
cd ..
```

After ArduPilot is completely installed, you will need to add the DroneKit functionality
to Python’s package manager. The version of DroneKit we use is newer than the version
available on PyPI. Run the following commands to install DroneKit:

```
wget https://github.com/MissouriMRR/docs/raw/refs/heads/main/subteams/flight/installation_guide/dronekit-2.9.2-py310-none-any.whl
pip3 install dronekit-2.9.2-py310-none-any.whl
```

_Note: pip installs Python packages, whereas apt installs system packages._

\
**Step 4: Cloning Important Repositories**

First, you need to create an SSH key to be allowed to push to our repositories. Run the
following command to create an SSH key:

```
ssh-keygen -f ~/.ssh/my_github_key
```

**_Note: never share the contents of your SSH key._**

To add the SSH key to your GitHub account, go to https://github.com/settings/keys, click
the button labeled 'New SSH key', and then for the Key field paste the output of the
following command:

```
cat ~/.ssh/my_github_key.pub
```

Then, whenever you open a new terminal, enter the following commands to activate your SSH
key:

```
eval $(ssh-agent -s)
ssh-add ~/.ssh/my_github_key
```

This will allow you to push to our repositories on GitHub.

To clone our main SUAS repository, run the following command:

```
git clone git@github.com:MissouriMRR/SUAS-2025.git --recursive
```

\
**Step 5: Installing Useful Programming Tools**

With the next command, install the Poetry software into the Virtual Machine:

```
sudo pip3 install poetry
```

Poetry is used to create a virtual environment that we use to safely and conveniently
manage the libraries that our code depends on.

Next, install pre-commit with the following command:

```
sudo pip3 install pre-commit
```

Before you push any code to a branch in a Multirotor repo, run Pre-commit by typing
pre-commit into the terminal before pushing code, and after adding files to a commit. This
will auto-format your code and make it look somewhat nice.

### QGroundControl (non-WSL users)

For the next piece of software, open a web browser and use the following link:

-   [https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html)

This page redirects you to the instructions for downloading QGroundControl, a
supplementary software that allows you to get an overhead map of the drone during flight.
Follow the instructions for Linux download, or if you’re the brave soul using macOS, use
the instructions for that operating system. Be sure to follow the instructions for
downloading the installer file as well as any extra packages required (like libfuse2).

### Mission Planner (WSL users)

If you're using WSL, you should install Mission Planner instead of QGroundControl:
[https://ardupilot.org/planner/docs/mission-planner-installation.html](https://ardupilot.org/planner/docs/mission-planner-installation.html).

\
**Step 6: Creating a Convenience File (Optional)**

In a blank text page, copy the following lines of text:

```
if [ ! -d "./ardupilot" ]; then
    echo $'Either ardupilot has not been downloaded yet or you are running this script in the wrong place.\nTo install ardupilot, follow the instructions at\n\thttps://ardupilot.org/dev/docs/building-setup-linux.html#setting-up-the-build-environment-linux-ubuntu\nThen build ardupilot by running:\n\t./waf configure --board sitl\n\t./waf copter\nIf you already installed it, then you most likely didn\'t put this script in the parent folder of the ardupilot folder.'
    exit 1
fi

cd ardupilot

if ! grep -q "Multirotor Locations" Tools/autotest/locations.txt; then
    echo "GolfCourse not found, adding Multirotor Locations to Tools/autotest/locations.txt..."
    echo $'# Multirotor Locations\nGolfCourse=37.9490953,-91.7848293,0,0' >> Tools/autotest/locations.txt
fi

python3.10 Tools/autotest/sim_vehicle.py -v copter -L GolfCourse --map
```

Save the file as opensitl.sh

Now, each time you want to start the drone simulator, use the bash command

```
./opensitl.sh
```

If you attempt to run this new bash script and you receive the following error:

bash: ./opensitl.sh: Permission denied

You can easily resolve this by changing the file permissions using the following command:

```
chmod u+x opensitl.sh
```

\
**Step 7: Installing Dependencies in Poetry**

Navigate to the SUAS repo in your terminal. Then, activate the Poetry virtual environment:

```
poetry shell
```

You will need to do this every time you work in the SUAS repo to be able to use the
dependencies our code requires.

Next, run the following command to install the dependencies (you need to do this only
once):

```
poetry install
```

You can exit Poetry by running the following command while Poetry is active:

```
exit
```
