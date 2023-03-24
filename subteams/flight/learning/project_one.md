---
permalink: /flight/project_one/
---


# Flight Docs

[Back to Flight Home](/docs/)


### First Project: Takeoff and Land

Through this first project you will become 
familiar with some basic MAVSDK functions.

\
**Objectives:**

 - Create object and Connect to drone
 - Set takeoff altitude
 - Call takeoff function
 - Call landing function
 - Kill drone

\
**Process**

1.) Create a new file and name it takeoff_and_land.py (Don't forget the .py)

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

8.) Use the ```asyncio.sleep()``` to have the drone wait in the air for the specified amount of seconds passed into the sleep() method

9.) Call the land function

10.) Call the drone.action.kill() function to kill the drone

11.) Create a main function, and within the main function, type asyncio.run(run()) 

\
**QGroundControl and JMAVSIM**
 - To open QGroundControl, right click the app image and select run.
 - To open JMAVSIM, type ```./opensitl.sh``` in your terminal and press enter to run.
 - Wait for both to open and connect before running your code.