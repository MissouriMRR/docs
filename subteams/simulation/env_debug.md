---
permalink: /simulation/environment-debug/windows
---

# Debugging Environment (Windows)

[Back to Simulation Docs](/docs/simulation/)

There are many points of failure when attempting to run your Python code unrelated to your code. Below are some solutions to common problems. 

## Problem List

- [PX4 won't connect](#px4-wont-connect)
- [MavSDK won't connect to PX4](#mavsdk-server-wont-connect-to-px4)
- [No module named 'encodings' when starting PX4 (Windows)](#no-module-named-encodings-when-starting-px4-windows)

## PX4 Won't Connect

Most often, this means you forgot to start the Unreal Engine simluation. However, if this is not the case, it's possible that PX4 is looking at the wrong TCP port.

To fix this:
1. find the correct TCP port 
    - if you haven't already, run `\scripts\run-servers.ps1`
    - in PX4's console, locate the line `Waiting for simulator to accept connection on TCP port XXXX`
        - the port listed is what you need
2. open your `\scripts\airsim-settings.json` file in a text editor
3. replace the value of `Vehicles.PX4.TcpPort` with the port you discovered
4. save your changes, then run `\scripts\update_settings.bat`
    - this will update your AirSim settings for you (AirSim has, like, five places it looks for settings files, but this will always put it in the correct location)
    - you may have to reopen your Unreal project for changes to take hold

Now, you should be all set!

## MavSDK Server Won't Connect to PX4

Hopefully, the default port provided in `\scripts\server-config.json`, will be correct; however, if MavSDK won't connect to PX4, then the port is likely incorrect.

To fix this:
1. run `\scripts\run-servers.ps1` if you haven't already
2. wait until PX4 is fully loaded
    - you will have to start your Unreal Engine simulation with the `Play` button to ensure PX4 progresses far enough to accept a mavlink connection
    - if PX4 doesn't connect to anything after starting the simulation, check the [above section](#px4-wont-connect) for a possible fix
3. if MavSDK's console output remains `Waiting to discover system on udp://:insert_port_here...` even after PX4 has connected to the simulation, you likely have the incorrect port. If it connects, then nothing is wrong!
4. locate the *final occurrance* of a message similar to `mode: Onboard, data rate: 4000 B/s on udp port 14280 remote port 14030` in **PX4**'s console output
    - the listed *remote port* is the one you need
5. replace the value of `drone_port` in your `\scripts\server-config.json` file with the remote port you found
    - if you do not have `\scripts\server-config.json`, then you must first run `\scripts\setup.bat`

Now, you should be all good!

## No module named 'encodings' when starting PX4 (Windows)

This error might be caused by window's enviroment variables interfering with PX4's virtual python enviroment. 

To fix this:
1. Search 'enviroment variables' in the window's search.
2. click **Environment Variables...**
2. Delete PYTHONPATH and PYTHONHOME

Now, it should work! 
