---
permalink: /simulation/install/easy_2/
---

# Simulation Binary Setup

These steps will explain how to configure the simulation executable.

## Installing the Simulation

[Sample Environment Download (TODO)](localhost:3000) 

This executable is the actual simulation and will not be run inside a Virtual Machine. If all goes well you should be able to run the executable without issue. This specific world is the default blocks, refer to (TODO) for more environments. 

After installing and running at least once, navigate to C:/Users/*Your User*/Documents/Airsim/Settings.json on your computer. This is where information about the drone inside the simulator is held. 

Open the JSON and paste the following code into it:
```json
{
  "SettingsVersion": 1.2,
  "SimMode": "Multirotor",
  
  "Vehicles": {
      "PX4": {
          "VehicleType": "PX4Multirotor",
          "UseSerial": false,
          "UseTcp": true,
          "TcpPort": 4560,
          "ControlPortLocal": 14540,
          "ControlPortRemote": 14580,
          "ControlIp":"remote",
          "LocalHostIp":"192.168.56.1",
          "params": {
              "NAV_RCL_ACT": 0,
              "NAV_DLL_ACT": 0,
              "LPE_LAT": 47.641468,
              "LPE_LON": -122.140165,
              "COM_OBL_ACT": 1
              
          },
          "Cameras" : {
              "down": {
                  "CaptureSettings" : [
                      {
                          "ImageType" : 0,
                          "Width" : 1920,
                          "Height" : 1080,
                          "FOV_Degrees": 90
                      }
                  ],
                  "X": 0.00, "Y": 0.00, "Z": 0.00,
                  "Pitch": 270.0, "Roll": 0.0, "Yaw": 0.0
              }
          }
      }
  }

}
```

(Keep this open as you'll need it for the next step)

# *AND BE SURE TO SAVE IT*

### Now to Configure Your Network
### [Next Step](docs/simulation/install/easy_3/)
