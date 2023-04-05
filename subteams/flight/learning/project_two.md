---
permalink: /flight/project_two/
---


# Flight Docs

[Back to Flight Home](/docs/flight/)

### Second Project: Go to Waypoint

Through this project you will learn about
more commonly used MAVSDK functions.

\
**Objectives:**
 - Connect to a drone object
 - Set drone takeoff altitude
 - Utilize map tools to import GPS coordinates
 - Use MAVSDK travel commands

\
**Process**

1.) Create a new file and name it go_to_waypoint.py (Don't forget the .py)

2.) At the top of the file, type ```import asyncio``` and below it ```from mavsdk import System```

3.) Make an asynchronous function called "run()"

4.) To connect to the simulator drone, paste in this code

```
    drone = System()                                    ## Creates a new system object named drone that can be used later when  
                                                        ## calling any type of system methods
    await drone.connect(system_address="udp://:14540")  ## The connect function is used to connect the physical drone and the simulated drone. 
                                                        ## The system address parameter can be changed to connect to either the real drone, or the simulator drone.

    status_text_task = asyncio.ensure_future(print_status_text(drone))

    print("Waiting for drone to connect...")
    async for state in drone.core.connection_state():   ## Loops over the connection state function to check if drone is properly connected to the given system address
        if state.is_connected:                          ## Prints the following statement if properly connected
            print(f"-- Connected to drone!")
            break
```

5.) Arm the drone with the command drone.action.arm() *(Be sure to do it asynchronously!)*

6.) Set the takeoff altitude for the drone with the following line ```await drone.param.set_param_float("MIS_TAKEOFF_ALT", 25)``` (The number you use for the altitude is arbitrary for now.)

7.) Call the takeoff function
 
8.) Use a tool like Google Earth / Google Maps to find a GPS coordinate in the Missouri S&T golf course

9.) Use the MAVSDK commands to travel to the desired waypoint ```go_to_location```

10.) Return to home using the built-in ```return_to_launch()``` function



\
**QGroundControl and JMAVSIM**
 - To open QGroundControl, right click the app image and select run.
 - To open JMAVSIM, type ```./opensitl.sh``` in your terminal and press enter to run.
 - Wait for both to open and connect before running your code.