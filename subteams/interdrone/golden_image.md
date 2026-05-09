---
permalink: /interdrone/golden_image/
---

# Golden Image Instructions

These steps were done using WSL. The steps may be different on Linux (primarily mounting the sd card)


## Steps to Export Image:
Windows:

- Attach SD card
- Download Win32DiskImager https://sourceforge.net/projects/win32diskimager/files/latest/download

    Note: You can use WSL instead of this software but it’s a pain
- Specify a path for your image in the “Image File” text box and select the SD card device on the right
- Then click the read button and it will start copying.

If you are trying to image a pi, scroll down. Don’t press read. Do not press read.
DO NOT PRESS READ UNLESS YOU ACTUALLY ARE SUPPOSED TO

- Open WSL and navigate to where the image is stored
    EX: cd /mnt/c/Dev/Code-Stuff/MultiRotor/PI-Images
- Run the following commands to shrink the image size (removes free space)
```
bash# Install PiShrink (Linux or WSL only)
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
sudo mv pishrink.sh /usr/local/bin/pishrink
# Shrink (also auto-compresses with -z flag)
sudo pishrink drone-golden-v1.0.img
# Note: May need to sudo apt install parted if you don’t have it
```
Do this!

---

Linux: 

- Attach SD card to computer
- Run this command to find it’s name: `lsblk`
- Run your dd command normally: `sudo dd if=<PATH_TO_NAME_FROM_lsblk> bs=4M status=progress conv=sync,noerror \ | gzip > ~/drone-golden-v1.0.img.gz`
- Image will be created eventually
- Run the following commands to shrink the image size (removes free space)
```
bash# Install PiShrink (Linux only)
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
sudo mv pishrink.sh /usr/local/bin/pishrink

# Shrink (also auto-compresses with -z flag)
sudo pishrink -z drone-golden-v1.0.img
# Note: May need to sudo apt install parted if you don’t have it
```
Do this!

---


### Steps to Load Image on New PI:
Windows:
- Unzip the image if it’s a .gz: `gunzip name.img`
- Navigate to where your img is stored in WSL (/mnt/… for windows file system)
- Download the update-user-and-pass.sh from the repo and put it in the directory where your image be
- Update the following variables in update-user-and-pass.sh according to your values:
    - IMAGE
    - NEW_USER
    - NEW_PASS (should be mrrdt)
    - MOUNT_POINT (if needed)
- Run update-user-and-pass.sh: `sudo bash update-user-and-pass.sh`

Image updated!
- Put in the SD card your writing the image to.
- Open up Win32DiskImager and set image file as the image you just updated with update-user-and-pass.sh.
- Select the SD card you’re writing to and click “Write”
- Boot it
- Next time you SSH do the following:	
`ssh-keygen -R <ip-of-pi>`
EX: ssh-keygen -R 192.168.0.159
(Only applicable if you did this all on one network and the ip is the same)
- When you boot into the pi, do the following to update hostname (needed to fix issue with batman startup)
```
sudo nano /etc/cloud/cloud.cfg
Change preserve_hostname to true
sudo hostnamectl set-hostname mrrdt-#
sudo nano /etc/hosts
Change to mrrdt-#
sudo reboot
```
IF YOU DON’T REBOOT BEFORE DOING WIFI SETTINGS IT WILL BREAK STUFF BAD!!!!

- Change network if needed
```
sudo nmcli dev wifi list 
To see available networks 
sudo nmcli --ask dev wifi connect "SSID_Value" password "password" ifname wlan1 name wificonnection
```
- Make sure to update the wlan# that isn’t running batman.


## Troubleshooting:
- \d windows thing
Fix with: `sed -i 's/\r//' update-user-and-pass.sh`
