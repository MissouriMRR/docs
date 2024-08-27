---
permalink: /simulation/install/pre-configured/easy_3/
---

# Network Configuration
In this step, we configure the networks of both the virtual image and your computer so they can communicate.These steps are the easiest to mess up so read carefully

Note: These steps are intended for Windows. 

## Airsim Network

First we will edit the Airsim simulation JSON with VirtualBox's IP address. 

1. Go to command prompt and enter `ipconfig`.
2. Look for Ethernet Adapter Ethernet 2 or 3. This will have a different name depending on the computer, so specifically look for the ethernet who isn't disconnected. 
3. Copy the IPv4 Address, and paste it to replace the LocalHostIp address in the previous JSON. 

(Hold on to this IP address)

## Firewall Rules

Next we will configure your firewall. 

1. In the Windows search bar, search "firewall" and click the first option that appears. 
2. Next, in the top left menu, press the option saying *Inbound Rules*
3. In the left menu, click on *New Rule* and select Port then next. 
4. Select TCP, and enter 4560 in the textbox then next. 
5. Press next 2 more times allowing the connection and selecting all networks for when the rule applies. 
6. Then, choose a name of your chose and hit finish.
7. Repeat steps 4-6, but this time select UDP, and enter 14540.

## Sample Drone IP

Here we will configure the Sample Drone Virtual Image to direct flight controller traffic to the right IP address.
1. Login to Sample Drone Image and open the terminal.
2. Enter the command `nano ~/.bashrc`, and press the down arrow to scroll to the bottom. 
3. At the bottom you will see a line that says `export PX4_SIM_HOST_ADDR=127.0.0.1`. Replace the 127.0.0.1 with the ip address you discovered in the first section. 
4. Save and exit by pressing ctrl-x, pressing y, and pressing enter.
5. Finally enter `source ~/.bashrc` and you're done!

### [Now to fly the drone](/docs/simulation/flying/)
