---
layout: default
permalink: /raspberry_pi_sim/
title: Run Simulation from Raspberry Pi
---

# Run Simulation from Raspberry Pi

A step-by-step guide to running the simulation from a Raspberry Pi and connecting it to your computer.

---

## Install Sim & WSL Ubuntu 24.04

Download Raspberry Pi Imager:  
[https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/)

Set the following:

- Device: Raspberry Pi Zero 2 W  
- OS: Other General Purpose → Ubuntu → 24.04  
- Storage: Select your SD card (Generic Storage Device)  
- Next
- Edit Settings
- UNDER GENERAL
- Enable Set hostname = sim
- Enable Set username and password
- Username = sim
- Password = sim
- Enable Configure wireless LAN
- SSID = MST-GUEST
- Password = Miner2020
- Enable Hidden SSID
- Wireless LAN country = US
- Enable Set locale settings
- Time zone = America/Chicago
- UNDER SERVICES
- Enable Enable SSH
- Select Use password authentication
- Save

Flash the image to the SD card.

---

## Set Up the Raspberry Pi and Connect via SSH

Insert the SD card into the Pi and plug in:

- Power (micro USB power port)  
- Keyboard (micro USB data port)  
- Display (micro HDMI)  

Once the Pi finishes booting, run:

```bash
ip a
```

Look for your IP under `wlan0`, for example:

```bash
inet 192.168.1.15/24
```

On your computer, open PowerShell and connect:

```powershell
ssh sim@192.168.1.15
```

When prompted:
- Accept fingerprint → type `yes`  
- Password → `sim`  

---

## Set Up the Repository and Environment

Clone the test repository onto the Pi:

```bash
git clone https://github.com/PG13park/simpitest.git
cd simpitest
```

Install `uv`:

```bash
curl -LsSf https://astral.sh | sh
```

Set up the environment:

```bash
uv sync
```

---

## Allow MAVLink Through Firewall (Windows)

On your computer, open **PowerShell as Administrator** and run:

```powershell
New-NetFirewallRule -DisplayName "Allow MAVLink 5762 TCP" -Direction Inbound -Protocol TCP -LocalPort 5762 -Action Allow```

Also run:

```powershell
New-NetFirewallRule -DisplayName "Allow MAVLink 5762 UDP" -Direction Inbound -Protocol UDP -LocalPort 5762 -Action Allow
```

---

## Start the Simulation

Open your simulation in Unreal Engine.

Then open a new terminal and start WSL:

```bash
wsl -d Ubuntu
```

Navigate to your project:

```bash
cd /mnt/c/.../.../SUAS-2025
```

Start the Unreal editor player and run:

```bash
./run_container.sh
```

---

## Connect from the Raspberry Pi

From the Pi (SSH session), run:

```bash
uv run AirSimControllableFlight.py
```

---

## Fix Connection Issues

If you are not SSH’d in or the connection fails, manually set the IP.

Edit the file:

```bash
sudo nano AirSimControllableFlight.py
```

Replace the connection address with your computer’s IP (find it using `ipconfig` on Windows).

Save and exit:
- Ctrl + O  
- Enter  
- Ctrl + X  

## Done

You should now have the simulation running and connected to your Raspberry Pi.
