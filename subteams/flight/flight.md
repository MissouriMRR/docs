---
permalink: /flight/
---

# Flight Docs

[Back to Software Docs](/docs/)

Here you will find documentation and tutorials relating to the flight subteam, which is responsible for tasks related to autonomous flight.

\
***What technologies do we use?***

The flight subteam utilizes the following:
- PX4
- jMAVSim
- QGroundControl
- MAVSDK

\
PX4 is the flight control software used for sending commands to the drone.

jMAVSim is a simulator that allows you to fly drones around in a simulated world while running PX4.

QGroundControl provides flight controls, mission capabilities, and an interactive overhead view of the drone's environment.

MAVSDK is the python package we use to code.

```
    Examples of MAVSDK Commands:

        goto - Tells the drone where to fly when given coordinates.

        return - Tells the drone to return to its original launch location.

        telemetry - Grabs the drones current position and returns it.
        
```

\
***How do I get started?***

Find the installation guide at the bottom of this page and follow the instructions to get your coding environment set up.
 
### Links ###

- [Installation Guide](/docs/flight/installation_guide/)
- [jMAVSim Information Page](/docs/flight/jmavsim/)