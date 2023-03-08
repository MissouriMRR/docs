---
permalink: /flight/
---

# Flight Docs

[Back to Software Docs](/docs/)

Here you will find documentation and tutorials relating to the flight subteam, which is responsible for tasks related to autonomous flight.

\
*Virtual Machine Installation Process*

**Step 1: Install a Linux Virtual Machine**

In order to run Multirotor code and program effectively with MAVSDK & PX4, a Linux virtual machine with Ubuntu 20.04 is recommended. Use the following link to download the Ubuntu OS: 

### Ubuntu Link:
-  https://ubuntu.com/download/desktop  

And choose among the following links for your preferred virtual machine: 

### Virtual Machine Links:
- https://www.vmware.com/products/workstation-player.html  
- https://www.virtualbox.org/wiki/Downloads  

A Linux virtual machine is not required; it is entirely possible to run Multirotor code using Mac OS. However, ***please*** do not use Windows. The following instructions are based on Ubuntu 20.04. 


\
**Step 2: Installing Basic Software** 

Since you have created a new virtual machine, some basic software must be installed before we can proceed. First and foremost, Git. Use the following command: 

```
sudo apt install git 
```

Next is Python, the language we will be programming in for the majority of the semester. We will be using Python 3.10 which can be installed through the terminal, or, alternatively, through the following link:

- https://www.python.org/downloads/  

The first command to copy is: 

```
sudo apt install software-properties-common -y 
```
 

After that command finishes, use the next command: 

```
sudo add-apt-repository ppa:deadsnakes/ppa 
```
 

When prompted, hit enter and allow the downloads to finish. These commands allow for this version of Python to be easily updated from the command line using  

```
sudo apt update 
```
 

Finally, use the command 

```
sudo apt install python3.10  
```

To install Python 3.10 on your virtual machine. 

You will need to change the Python interpreter within your IDE of choice, however, independently. 

 

You can verify the version of Python installed to your computer using the following command: 
(The V should be capitalized)

```
python3.10 -V
```

If everything was installed correctly, you should see Python 3.10.4 or above. 


\
**Step 3: Installing PX4 Firmware (Duration ~45 minutes)**

Copy the following Git command into a terminal: 

```
git clone https://github.com/PX4/PX4-Autopilot.git --recursive 
```
 

After the repo has fully downloaded, copy the next command into a terminal to setup PX4: 

```
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh 
```
 

While this command is running, you may be prompted to download some auxiliary libraries that are associated with PX4. Follow the git prompts given to you, and when those repositories are downloaded, run the above command again. 

After PX4 is completely installed, you will need to add the MAVSDK functionality to Python’s package manager. Run the following command: 

```
pip3 install mavsdk 
```


\
**Step 4: Cloning Important Repositories (Duration: ~5 Minutes)**

Without entering any repos from the previous step, copy the following command into a terminal: 

```
git clone http://github.com/mavlink/MAVSDK-Python.git --recursive 
```
 

Again, still in the home repository, copy the following into the terminal: 

```
git clone https://github.com/MissouriMRR/SUAS-2023.git --recursive 
```


\
**Installing Useful Programming Tools (Duration ~10 Minutes)**

With the next command, install the Poetry software into the Virtual Machine: 

```
sudo pip3 install poetry 
```

Poetry is used to create a separate shell, or virtual terminal, through which you can run code as a safety precaution. 

Next, install Pre-commit with the following command: 

```
sudo pip3 install pre-commit 
```

Before you push any code to a branch in a Multirotor repo, run Pre-commit by typing pre-commit into the terminal before pushing code, and after adding files to a commit. This will auto-format your code and make it look somewhat nice. 

For the next piece of software, open a web browser and use the following link: 

- https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html  

This page redirects you to the instructions for downloading QGroundControl, a supplementary software that allows you to get an overhead map of the drone during flight. Follow the instructions for Linux download, or if you’re the brave soul using Mac OS, use the instructions for that operating system. 

When attempting to run QGroundControl from the terminal, and you get an error of 

“AppImages require FUSE to run” 

this can be resolved using the command 

```
sudo apt install libfuse2 
```


\
**Creating a Convenience File (Optional Step)**

In a blank text page, copy the following lines of text: 

    cd PX4-Autopilot 

    export PX4_HOME_LAT=37.9490953  

    export PX4_HOME_LON=-91.7848293  

    #export PX4_SIM_SPEED_FACTOR=1.5  

    make px4_sitl_default jmavsim 

If you did not download the PX4 repositories into your home repo, then alter the first line to include the path to the PX4 repo. Save the file as 	opensitl.sh 

Now, each time you want to start the drone simulator, use the bash command  

```
./opensitl.sh 
```

And JMAVSIM will boot up using the coordinates for Multirotor’s testing field, instead of the default location in Germany.  

If you want the drone to move faster during simulations, simply remove the # from the fourth line in this file to alter a global speed constant in PX4. 

 

** If you attempt to run this new bash script and you receive the following error: 

bash: ./opensitl.sh: Permission denied 

You can easily resolve this by changing the file permissions using the following command: 

```
chmod u+x opensitl.sh   
```