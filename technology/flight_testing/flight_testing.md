# Flight Testing

### Introduction
This documentation will go over all you need to know for running code on all of our drones, going from packing up all the stuff you need from the bay to a connection to the drone outside.

## Step 1: Pack Everything You Need

For testing you will need the following items:
* Ground Station
    * This doesn't have to be the actual full ground station if you don't feel like bringing it out, this can be someone's laptop. All it needs is the code you want to run and QGroundControl.
* Drone
* Router
    * This actually depends on if the drone has an onboard computer or not. If yes, you will need a router. We use the router to connect on the same network as the drone to SSH into the computer and remotely run the code.
* Telemetry Receivers/Radios
    * Used for telemetry from the drone to the ground station. This is how we get the data from the drone to the ground station.
* Portable Battery OR Generator
    * Used to power everything on the ground. If testing for more than 1-2 hours a generator will be needed.

## Step 2: Setup the Ground Station
Once you bring everything outside and open everything up, it's time to setup everything you will need.

Log into the ground station. Need the password? Ask one of the leads.



First, if you are using a router, connect the ground station to the drone. Check `192.168.0.1` to see if the onboard computer is connected.


## Step 3: Run Code
TODO

## Tips
### SSH over USB
While testing Maverick there would be a lot of times where the TX2 computer would not autoconnect to the router's network. Instead of bringing out a monitor and using a GUI to fix it, SSHing over USB using minicom saves a lot of time.
```bash
# Install minicom
sudo apt install minicom

# Find USB path
ls /dev/ttyUSB*
ls /dev/ttyACM*
# The address will be something like /dev/ttyUSB0 or /dev/ttyACM0

# Run minicom
minicom -D /dev/ttyUSB0
# Now you will have an emulated terminal to the onboard computer
```


## Troubleshooting
### Connection Troubles
TODO
