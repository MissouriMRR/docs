---
permalink: /interdrone/
---


# Setup and other information
Welcome to the code setup portion of the Interdrone Communication documentation. THis guide walks through everything needed to have everything set up and running. Warning in advanced, this is a very long and detailed document so I wish you luck in going through all of it.


#  Drone Mesh Network Setup Guide
**Project:** IARC-10 | **Hardware:** Raspberry Pi Zero 2W | **OS:** Ubuntu 25.10

---

## 1. Initial Image Flashing
When using the **Raspberry Pi Imager**, apply the following custom settings:

### General Tab
* **Hostname:** `mrrdt-#` (replace # with drone ID)
* **Username:** `mrrdt-#`
* **Password:** `mrrdt`
* **Wireless LAN:** * **SSID:** `MST-GUEST` (or local wifi)
    * **Password:** `miner2020`
    * **Country:** `US`
    * **Hidden SSID:** **UNSELECTED**

### Services Tab
* **Enable SSH:** Use password authentication.

---

## 2. Network Discovery & Connection
If you are on Windows, ensure you have **nmap** and **PuTTY** (for `plink`) installed.

### Automatic Discovery (PowerShell)
Run the scanning script from your local machine to find the Pi's IP:
```powershell
# Default scan
powershell -ExecutionPolicy Bypass -File .\ssh_scanner.ps1 /23 -Username mrrdt-#

# If password isn't default
powershell -ExecutionPolicy Bypass -File .\ssh_scanner.ps1 /23 -Username mrrdt-# -Password [YOUR_PASS]


Scan for devices with SSH open:

```bash
nmap -p 22 --open <ip-addr>/23 | findstr "Nmap scan report"
```

SSH in:

```bash
ssh <drone-name>@<ip-addr>
```

---

## 2️⃣ Update + Install Dependencies

```bash
sudo apt update
sudo apt install net-tools network-manager batctl iw -y
```

---

## 3️⃣ Install Python (uv)

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
sudo mv ~/.local/bin/uv /usr/local/bin/
sudo mv ~/.local/bin/uvx /usr/local/bin/

uv python install 3.12
uv venv --python 3.12
source .venv/bin/activate
```

---

## 4️⃣ Clone Repo

```bash
cd ~
git clone https://github.com/MissouriMRR/IARC-10.git
cd IARC-10
git checkout IARC-LVP
```

---

## 5️⃣ (Optional) Change Network

```bash
cd /etc/netplan
sudo vi 50-cloud-init.yaml
sudo netplan apply
```

---

#  BATMAN Mesh (Manual Setup)

Find USB adapter name:

```bash
ip link show
```

Configure mesh:

```bash
sudo nmcli device set <UAIN> managed no
sudo ip link set <UAIN> down
sudo iw dev <UAIN> set type ibss
sudo ip link set <UAIN> up
sudo iw dev <UAIN> ibss join my-batman-mesh 5200 HT20 fixed-freq 02:ca:fe:ca:ca:40
sudo batctl if add <UAIN>
sudo ip link set bat0 up
sudo ip addr add 169.254.97.X/24 dev bat0
```

- Replace `X` with unique number (same subnet).

---

#  Startup Script Setup

Set hostname:

```bash
sudo hostnamectl set-hostname mrrdt-#
sudo reboot
```

Install mesh script:

```bash
sudo vi /usr/local/bin/batman-mesh-setup.sh
sudo chmod +x /usr/local/bin/batman-mesh-setup.sh
```

Add systemd service:

```bash
sudo vi /etc/systemd/system/batman-mesh.service
sudo systemctl daemon-reload
sudo systemctl enable batman-mesh.service
sudo systemctl start batman-mesh.service
```

Debug:

```bash
sudo systemctl status batman-mesh.service
sudo journalctl -u batman-mesh.service -f
```

---

#  Message Structure

### Option 1

```python
MessageData = {
    "messageId": 4,
    "timestamp": 0.0,
    "senderId": droneId,
    "payload": "Hello server!",
}
```

### Option 2

```python
MessageData = {
    "messageId": 4,
    "payload": {}
}
