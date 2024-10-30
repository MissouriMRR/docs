---
permalink: /simulation/cesiumforunreal/
---

# Using **Cesium For Unreal** to find real-world locations

[Back to Simulation Docs](/docs/simulation/)

# Table of Contents
- 
- [Importing your Model](##Importing-your-Model)

## Installing RealityScan and Blender

**Prerequisites:**

1. Have Unreal Engine 4.27 [downloaded](https://store.epicgames.com/en-US/download)

## Downloading and Installing Cesium for Unreal
1. Go [here](https://www.fab.com/listings/76c295fe-0dc6-4fd6-8319-e9833be427cd)
2. Select 'Add to My Library' and agree to the terms
3. Load up the Epic Games Launcher and go to Unreal Engine -> Library and scroll all the way to the bottom.
4. Click Install to Engine and select 4.27
5. Launch Unreal 4.27
6. Go to Games -> Blank and use these settings(recommended):
   - Blueprint
   - Maximum Quality
   - Raytracting Disabled
   - No Starter Content
7. Name your project and click 'Create Project'
8. Go to Edit -> Plugins -> Geospatial and check Cesium for Unreal
   - You may need to restart Unreal

## Creating a Cesium Level
1. Once you've restarted Unreal, go to Window -> Cesium
2. Make sure your level is devoid of any objects
3. Go to World Settings and search 'Enable World Bounds', then uncheck it.
4. Click the plus next to 'Cesium SunSky'
   - If your screen turns white and stays white, go to Edit -> Project Settings
   - Then go to Engine, click the search bar and search 'Auto Exposure'
   - Make sure that both 'Auto Exposure' and 'Extend default luminesence range in Auto Exposure settings' are checked
   - You may have to restart Unreal
5. Go to the **Cesium** panel that you opened earlier. Click on the **Connect to Cesium ion** button
6. A pop-up browser window will open. If you are not logged in, log in to your Cesium ion account.
7. Once logged in, you'll see a prompt asking you to allow Cesium for Unreal to access your assets, select allow and return to Unreal Engine
8. Once you've done this, you'll need to create a default Access Token for your project.
   - Click on the Token icon in the Cesium panel
   - A new window will appear and give your token a name if you want, then click **Create New Project Default Token**
9. In the **Cesium** panel, click **Cesium World Terrain + Bing Maps Aerial imagery**
10. This should create terrain for your level, to change where you're at:
    - Select the **CesiumGeoreference** object and go to details
    - Where it says **Origin Latitute**/**Origin Longitude**/**Origin Height**, you are able to change the coordinates of where your level is centered.
  




