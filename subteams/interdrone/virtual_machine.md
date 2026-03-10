---
permalink: /interdrone/
---

---
# Virtual Machine Setup
Plug in your wifi adapter
Setup a ubuntu desktop vm on your windows computer with virtual box 
[Virtual_Box](https://ubuntu.com/download/server)

Set username to mrrdt-iarc-desk-initals

EX: mrrdt-iarc-desk-mw

Set password to mrrdt

System Settings: 

Memory: 4096mb or 8192mb

Processor: 4-6 cpu cores

Display Settings: 

Video Memory: 128mb

Graphics Controller:VMSVGA

3D Acceleration: Enabled

USB Settings:

Make sure USB 3.0 is enabled

Right click and add filter from device for the wifi adapter

Power up the vm and login

Click Devices -> USB -> Wifi Adapter (startech) in the top left corner

Open the terminal

Run  ``` lsusb  ```

and verify that wifi adapter is there

Run  ``` iplink ``` and verify that wlx08beac45dcb0 (<UAIN>) is there

Now we can start actually setting ts up

Use these commands to install uv
 ```

sudo apt install curl

curl -LsSf https://astral.sh/uv/install.sh | sh

sudo mv ~/.local/bin/uv /usr/local/bin/uv

sudo mv ~/.local/bin/uvx /usr/local/bin/uvx

source $HOME/.local/bin/env

uv python install 3.12
 ```

Create a folder for iarc and enter it
```

mkdir IARC-DEV

cd IARC-DEV
```
Git clone the IARC repo in

Go to interdrone-communication or wherever the vm-batman-setup.sh is stored

Run the vm-batman-setup.sh 
```
sudo bash vm-batman-setup.sh
```
Batman should be running. Verify with 

Troubleshooting:

Copy and paste not working:

Devices - > copy and paste -> bidirectional 

Reboot

Use below to check batman status

iw dev wlx08beac45dcb7 link

VM not booting correctly

Make sure settings are correct as seen in setup

Literally just keep powercycling it (if setup is correct, this will hopefully always work)

VM not joining batman network correctly

Make sure wifi adapter is connected and setup correctly
Make sure you’re running the correct vm startup script

