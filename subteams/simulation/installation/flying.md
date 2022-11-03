---
permalink: /simulation/flying/
---

# Flying the Drone with Code:

(This is based on what has been discovered.)

To fly the drone with code in Unreal:
1. Start the Unreal simulation
2. Start PX4 via the instructions above.
3. Connect a MavSDK server to the PX4 SITL
    1. E.g., run `.\mavsdk_server_bin.exe udp://:{UDP_PORT}`
        Replace {UDP_PORT} with the onboard remote port PX4 highlighted
    2. If you get a bind failed error when starting PX4 regarding the onboard port, use the new remote port
4. Run your code, connecting the System as follows:
    `drone: System = System(mavsdk_server_address="localhost")`
    `await drone.connect()`

