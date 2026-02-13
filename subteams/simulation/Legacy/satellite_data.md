---
permalink: /simulation/importing_satellite_data
---

# Using Satellite Data in Unreal Engine

[Back to Simulation Docs](/docs/simulation/)

To ease map creation, we can use topographical and photographical data from satellites to approximate the competition environment based off its actual location.

## Retrieving Data

We will use [terrain.justgeektechs.com](https://terrain.justgeektechs.com/#/), an open-source web tool for downloading satellite data for Unreal Engine, to obtain our data.

1. Go to the website listed above and navigate to the `Settings` tab
2. Provide a `Mapbox Access Token` to the corresponding setting
    - if you do not have an access token, you will have to create a Mapbox account and create one ([click here for more information](https://docs.mapbox.com/help/glossary/access-token/))
    - (we should probably have a token for our subteam if we want to use this service in the forseeable future. talk to the subteam lead if you're reading and get them to do this)
3. Scroll down to the `Download Folder` setting and change it to your desired download location
4. Scroll down to the bottom of the `Settings` tab and click the `Save Settings` button
5. Return to the `Map` tab and navigate to your desired location
6. Click the map to select an area
    - this will highlight a square area of the map in turquoise
    - the further zoomed in you are, the smaller the area will be
    - you should highlight the smallest area that completely contains the area you wish to download
7. Fully select your desired area by double-clicking the area
    - You need to save the `Unreal X, Y, and Z-scale` information that appears
8. Set the following parameters on the left:
    - Export Type: `Unreal Heightmap`
    - Export Options: check `Zrange-sea level=0`, `Download Satellite`, and `Combine Unique Features`
    - Landscape Size: `8129x8128`
        - this can be something else, but make sure the size is high enough satisfactory resolution (above 4033x4033 will probably be fine)
9. Download the data by clicking `Download HeightMap`

## Importing Data into Unreal

[This YouTube video](https://www.youtube.com/watch?v=Z3e5Zaxmo1c) provides information on how to import and texture the data you downloaded with the above steps.