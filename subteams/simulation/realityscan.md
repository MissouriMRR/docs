---
permalink: /simulation/realityscan/
---

# Using RealityScan to Create 3-D Models

[Back to Simulation Docs](/docs/simulation/)

# Table of Contents
- [Installing RealityScan and Blender](#Installing-RealityScan-and-Blender)    
- [Creating Your Model](##Creating-your-Model)
- [Getting Access to your Model](##Getting-Access-to-your-Model)
- [Converting your Model](##Converting-your-Model)
- [Importing your Model](##Importing-your-Model)

## Installing RealityScan and Blender

**Prerequisites:**

1. Make an account with Epic Games (https://www.unrealengine.com/id/login).
2. Download RealityScan on your mobile device.
3. Download Blender on your computer (https://www.blender.org/download/).

## Creating your Model
1. Once you are in the RealityScan app, you will need to sign into Epic Games.
2. Press the blue + button.
3. Press auto capture and walk around your object.
   - RealityScan recommends to avoid clear, shiny, and plain object such as glass, metal, and featureless plastics.
   - Take at least 20 pictures.
   - It'll process the pictures as you take them.
   - Press the blue → when you're done.
5. Crop your imagine down to size, you'll want to have the lowest amount of polygons as possible.
6. Name your model and press "Process Now"
7. It'll usually take awhile for the model to process (More pictures → more time).

## Getting Access to your Model
1. You can either share on sketchfab(requires a sketchfab account) or export it as a 3D model.
   - Sketchfab (You only have ~30 uploads per month)
     - Press "Share on Sketchfab" and give it a title.
     - You can choose to add more attributes to it but they're not necessary.
     - Press "Share on Sketchfab".
   - Export 3D Model
     - Find a way to access your .obj file (Ex: Google Drive).
3. Download your zip from either Google Drive or, for Sketchfab,
   - Go to your Sketchfab account picture in the top right of the website and go to "Models".
   - Select your model.
   - Scroll down to and select "Download 3D Model", it should be right beneath your profile picture and name.
   - Download it as a .obj file.

## Converting your Model
1. In your downloads folder there'll be a (MODELNAME).zip, unzip this and go to (MODELNAME)/source and unzip shareModel.zip.
   - Don't close this folder.
2. Converting from .obj to .fbx
   - Launch Blender and under "New File", select General.
   - Delete the pregenerated cube.
   - Go to File > Import > Wavefront (.obj).
   - Once it's loaded, go to File > Export > FBX (.fbx).

## Importing your Model
1. In Unreal, click the green "Add/Import v" button.
2. From there go to Import Asset > Import Game/...
3. Select your .fbx file.
4. Click "Import All" if asked.
5. If you get an error in Message Log just close out of it.
6. You should be able to click and drag your asset into the Unreal environment.
