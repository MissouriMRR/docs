---
permalink: /flight/installation_guide/
---

# Flight Docs

[Back to Flight Docs](/docs/flight/)

# Virtual Machine Installation Process

**Step 1: Install a Linux Virtual Machine**

In order to run Multirotor code and program effectively with DroneKit and ArduPilot, a
Linux virtual machine with Ubuntu 22.04 is recommended. Use the following link to download
the Ubuntu OS:

### Ubuntu Link:

-   [https://releases.ubuntu.com/jammy/](https://releases.ubuntu.com/jammy/)

And choose among the following links for your preferred virtual machine:

### Virtual Machine Links:

-   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
-   [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

A Linux virtual machine is not required; it is entirely possible to run Multirotor code
using macOS. However, **_please_** do not use Windows. The following instructions are
based on Ubuntu 22.04.

### Using Windows Subsystem for Linux (WSL):

Alternatively, on Windows, you can instead install a more lightweight Linux VM using
Windows Subsystem for Linux (WSL). Simple install
[Ubuntu 22.04 from the Microsoft Store](https://www.microsoft.com/store/productId/9PN20MSR04DW).
Then, in the Windows Terminal app, click the + button in the title bar and choose Ubuntu
to open a new terminal in your Ubuntu VM.

\
**Step 2: Installing Basic Software**

Since you have created a new virtual machine, some basic software must be installed before
we can proceed. First and foremost, Git. Use the following command:

```
sudo apt install git
```

Next is Python, the language we will be programming in for the majority of the semester.
We will be using Python 3.10 which can be installed through the terminal, or,
alternatively, through the following link:

-   [https://www.python.org/downloads/](https://www.python.org/downloads/)

The first command to copy is:

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

\
**Step 4: Cloning Important Repositories**

Without entering any repos from the previous step, copy the following command into a
terminal:

Again, still in the home repository, copy the following into the terminal:

```
git clone https://github.com/MissouriMRR/SUAS-2025.git --recursive
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

For the next piece of software, open a web browser and use the following link:

-   [https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html)

This page redirects you to the instructions for downloading QGroundControl, a
supplementary software that allows you to get an overhead map of the drone during flight.
Follow the instructions for Linux download, or if you’re the brave soul using macOS, use
the instructions for that operating system. Be sure to follow the instructions for
downloading the installer file as well as any extra packages required (like libfuse2).

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
