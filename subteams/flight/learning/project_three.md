---
permalink: /flight/project_three/
---


# Flight Docs

[Back to Flight Home](/docs/flight/)


### Third Project: Mission Mode

In this project you will utilize the skills you previously 
learned to complete a more independent drone mission.

\
**Objective:**
 - Do everything using a MAVSDK mission

\
**Process**

1.) Create a new file and name it go_to_waypoint.py (Don't forget the .py)

2.) At the top of the file, type ```import asyncio``` and below it ```from mavsdk import System```

3.) Make an asynchronous function called "run()"

4.) To connect to the simulator drone, paste this code in the run() function you just created

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

5.) A mission plan is comprised of mission items. Mission item parameters are as follows: 
```
MissionItem (Latitude, Longitude, Relative Altitude, Speed, Flythrough Boolean)
```

Here's a filled out example, all of the empty parameters are for camera controls

```
mission_items = []
    mission_items.append(MissionItem(47.398039859999997, 8.5455725400000002, 25, 10, True, float('nan'), float('nan'), MissionItem.CameraAction.NONE, float('nan'), float('nan'),  float('nan'), float('nan'), float('nan')))
``` 

Utilizing the format above, create two more MissionItem objects using the below coordinates, and 
compile them into a ```MissionPlan object:  mission_plan = MissionPlan(mission_items)```

```
   (37.94852048112047, -91.78427643078165)
   (37.94852048108085, -91.78427643078165)
   (37.94852048104123, -91.78427643078165)
```

```
Commands:


await drone.mission.set_return_to_launch_after_mission(True)
await drone.mission.upload_mission(mission_plan)
await drone.action.arm()
await drone.mission.start_mission()
```
6.) Create a main function to run the code  

```
if __name__ == "__main__":
    asyncio.run(run())
```

\
**QGroundControl and JMAVSIM**
 - To open QGroundControl, right click the app image and select run.
 - To open JMAVSIM, type ```./opensitl.sh``` in your terminal and press enter to run.
 - Wait for both to open and connect before running your code.