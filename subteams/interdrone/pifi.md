---
permalink: /interdrone/wifi_access_point/
---
---
# WiFi Access Point Setup (Pi 1)

Pi 1 acts as the command Pi and hosts a WiFi access point that the Flutter app connects to. The onboard `wlan0` is used for the BATMAN-adv mesh, while a USB WiFi adapter (`wlan1`) is configured as an access point.

## Architecture

Flutter app connects to Pi 1 via AP (wlan1 DroneCommandNet 192.168.50.1)

Pi 1 communicates to rest of the drones over BATMAN-adv (wlan0 169.254.97.1)



## Prerequisites

A USB WiFi adapter that supports AP mode plugged into Pi 1
Verify AP mode is supported:
    
```bash
iw phy phy1 info | grep -A 10 "Supported interface modes"
```

You must see `AP` in the list.


## Setup

### 1. Check NetworkManager is running
```bash
systemctl status NetworkManager
```

### 2. Create the Access Point
Current password is multirotor123 (Needs to be over a certain length)

```bash
nmcli connection add type wifi ifname wlan1 con-name "DroneAP" autoconnect yes ssid "DroneCommandNet" \
  mode ap \
  ipv4.method shared \
  ipv4.addresses 192.168.50.1/24 \
  wifi-sec.key-mgmt wpa-psk \
  wifi-sec.psk "multirotor123" \
  802-11-wireless.band bg \
  802-11-wireless.channel 6
```

### 3. Bring it up
```bash
nmcli connection up DroneAP
```

### 4. Verify
```bash
iw dev wlan1 info
ip a show wlan1
```

`wlan1` should show `type AP` and IP `192.168.50.1`.



## Connecting Devices

Devices connecting to `DroneCommandNet` will receive an IP in the `192.168.50.10-254` range via DHCP (managed automatically by NetworkManager).

To see connected devices:
```bash
cat /var/lib/misc/dnsmasq.leases
ip neigh show | grep 192.168.50
```



## Persistence Across Reboots

The `DroneAP` connection is saved by NetworkManager with `autoconnect yes` so it comes back up automatically after reboot.

The BATMAN startup script (`batman-mesh-setup.sh`) contains a Pi 1 exception that prevents it from taking over `wlan1` for the mesh:

```bash
if [ "$PI_NUMBER" = "1" ]; then
    log_message "Pi 1 detected - wlan1 reserved for AP, using wlan0 for mesh"
    UAIN="wlan0"
fi
```

This ensures `wlan1` always stays as the AP on Pi 1 while `wlan0` handles the mesh.



## Subnet Configuration

The AP uses a separate subnet (`192.168.50.0/24`) from the BATMAN mesh (`169.254.97.0/16`). This ensures Linux routes traffic correctly:

| Subnet | Interface | Purpose |
|---|---|---|
| `192.168.50.0/24` | `wlan1` | App WiFi connection |
| `169.254.97.0/16` | `bat0` | BATMAN-adv mesh |

Verify routing is clean:
```bash
ip route show
```

Should show two separate routes — one via `wlan1` and one via `bat0`.



## Troubleshooting

### DroneAP not showing up after reboot
Check if a stale `dnsmasq` instance is blocking it:
```bash
sudo pkill dnsmasq
nmcli connection up DroneAP
```

### Can't connect to the AP
Check that `hostapd` (via NetworkManager) is running:
```bash
journalctl -u NetworkManager --since "2 minutes ago" | grep DroneAP
```

### Phone gets wrong subnet IP
If the phone gets a `169.254.x.x` address instead of `192.168.50.x`, the AP subnet may have changed. Verify:
```bash
nmcli connection show DroneAP | grep ipv4.addresses
```

Should show `192.168.50.1/24`. If not, fix it:
```bash
nmcli connection modify DroneAP ipv4.addresses 192.168.50.1/24
nmcli connection down DroneAP && nmcli connection up DroneAP
```

### Temporarily connecting Pi 1 to internet
To get internet access on Pi 1 without losing the AP config:
```bash
sudo nmcli connection down DroneAP
sudo nmcli connection add type wifi con-name "temp-wifi" ifname wlan1 ssid "NETWORK_NAME" wifi-sec.key-mgmt wpa-psk wifi-sec.psk "PASSWORD" ipv4.method auto
sudo nmcli connection up temp-wifi
```

When done, restore the AP:
```bash
sudo nmcli connection down temp-wifi
sudo nmcli connection up DroneAP
```