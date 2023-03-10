---
permalink: /flight/jmavsim/
---

# Flight Docs

[Back to Flight Docs](/docs/flight/)


### **What is jMAVSim?**
jMAVSim is a simple multirotor/Quad simulator that allows you to fly copter type vehicles running PX4 around a simulated world. It is easy to set up and can be used to test that your vehicle can take off, fly, land, and responds appropriately to various fail conditions (e.g. GPS failure).

Command to build jMAVsim:


```
make px4_sitl_default jmavsim
```

If built properly, the terminal should display 
```
[init] shell id: 140735313310464
[init] task name: px4

______  __   __    ___
| ___ \ \ \ / /   /   |
| |_/ /  \ V /   / /| |
|  __/   /   \  / /_| |
| |     / /^\ \ \___  |
\_|     \/   \/     |_/

Ready to fly.


pxh>
```

### **Commander commands**

**These commands directly control the actions of the simulated drone**

```
commander <command> [arguments...]

Commands:

   start
     [-h]        Enable HIL mode

   calibrate     Run sensor calibration
     mag|accel|gyro|level|esc|airspeed Calibration type
     quick       Quick calibration (accel only, not recommended)

   check         Run preflight checks

   arm
     [-f]        Force arming (do not run preflight checks)

   disarm

   takeoff

   land

   transition    VTOL transition

   mode          Change flight mode
     manual|acro|offboard|stabilized|rattitude|altctl|posctl|auto:mission|auto:l
                 oiter|auto:rtl|auto:takeoff|auto:land|auto:precland Flight mode

   lockdown
     [off]       Turn lockdown off

   stop

   status        print status info
   ```


### **Useful drone hotkeys**

**Views:**

F - First-person-view camera.
S - Stationary ground camera.
G - Gimbal camera.
Z - Toggle auto-zoom for Stationary camera.
+/- - Zoom in/out
0/ENTER - Reset zoom to default.


**Actions:**

Q - Disable sim on MAV.
I - Enable sim on MAV.
H - Toggle HUD overlay.
C - Clear all messages on HUD.
R - Toggle data report sidebar.
T - Toggle data report updates.
D - Toggle sensor parameter control sidebar.
F1 - Show this key commands reference.
P - Pause.
ESC - Exit jMAVSim.
SPACE - Reset vehicle & view to start position.


**Manipulate Vehicle:**

ARROW KEYS - Rotate around pitch/roll.
END/PG-DN - Rotate CCW/CW around yaw.
SHIFT + ARROWS - Move N/S/E/W.
SHIFT + INS/DEL - Move Up/Down.
NUMPAD 8/2/4/6 - Start/increase rotation rate around pitch/roll axis.
NUMPAD 1/3 - Start/increase rotation rate around yaw axis.
NUMPAD 5 - Stop all rotation.
CTRL + NUMPAD 5 - Reset vehicle attitude, velocity, & accelleration.


**Manipulate Environment:**

ALT +

ARROW KEYS - Increase wind deviation in N/S/E/W direction.
INS/DEL - Increase wind deviation in Up/Down direction.
NUMPAD 8/2/4/6 - Increase wind speed in N/S/E/W direction.
NUMPAD 7/1 - Increase wind speed in Up/Down direction.
NUMPAD 5 - Stop all wind and deviations.
CTRL+ Manipulate - Rotate/move/increase at a higher/faster rate.
