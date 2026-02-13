---
permalink: /simulation/install/
---

# Simulation Docs

[Back to Simulation Docs](/docs/simulation/)

# Installation Process

## Step 1: Basic Environment Setup

Follow the [flight install instructions](/docs/flight/installation_guide/).

## Step 2: Install Unreal Engine & AirSim

### Installing Unreal Engine

We will be using Unreal Engine for simulating virtual drones. If you have the Epic Games Launcher installed already (e.g., if you own *Fortnite*), you can download Unreal Engine from the "Unreal Engine" tab. If you don't have the Epic Games Launcher, then you will have to [download](https://store.epicgames.com/en-US/download) it to install Unreal Engine.

The Unreal Engine version you will download is **4.27.2**. AirSim, which allows us to simulate multirotors in Unreal, does *not* work with Unreal Engine 5 or greater.

Finally, run Unreal Engine at least once before continuing to the next steps.

### Installing AirSim

#### Visual Studio

To install AirSim, you must first install Microsoft's [Visual Studio Community 2022](https://visualstudio.microsoft.com). This will also be the IDE you will use for programming C++ code for Unreal.

When installing, you must select the following:
- `C++ Development Pack`
- `Windows SDK 10`

#### AirSim

AirSim is an Unreal Engine plugin for simulating multirotors and cars that was previously maintained by Microsoft; it is the backbone of simulation.

To install AirSim, follow these steps:

1. clone the AirSim GitHub repository: `git clone https://github.com/Microsoft/AirSim`
    - the location of your local copy does not matter
2. Launch `x64 Native Tools Command Prompt for VS 2022` (using Windows search)
    - navigate to where you cloned AirSim with the `cd` command
    - run the command `.\build.cmd`
    - wait for AirSim to build
