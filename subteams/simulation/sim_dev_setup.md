# Simulation Repository - Team Setup Guide

## Prerequisites

Before pulling, make sure you have the following installed:

1. **Unreal Engine 4.27** (default install: `C:\Program Files\Epic Games\UE_4.27`)
2. **Visual Studio 2022** with the **C++ Desktop Development** workload
3. **WSL2** with **Ubuntu 24.04** — install by running in an admin command prompt:
   ```
   wsl --install -d Ubuntu-24.04
   ```
4. **Podman and podman-compose** installed inside your WSL Ubuntu. Run the following inside WSL:
   ```
   sudo apt-get install -y podman
   pip3 install podman-compose
   ```
5. **SUAS-2025 repo** cloned somewhere on your machine (e.g., `D:\repos\SUAS-2025`)

## Step 1: Clone or Pull the Simulation Repository

If you haven't cloned it yet:
```
git clone git@131.151.19.149:SimulationRepository.git
```

If you already have it:
```
git checkout master
git pull origin master
```

## Step 2: Set the SUAS_REPO_PATH Environment Variable

Open a **Command Prompt** and run the following, replacing the path with wherever you cloned the SUAS-2025 repo:

```
setx SUAS_REPO_PATH "/mnt/d/repos/SUAS-2025"
```

**Important notes:**
- The path **must** be in WSL format: use `/mnt/c/...` or `/mnt/d/...` instead of `C:\...` or `D:\...`
- Example conversions:
  - `D:\repos\SUAS-2025` becomes `/mnt/d/repos/SUAS-2025`
  - `C:\Users\john\SUAS-2025` becomes `/mnt/c/Users/john/SUAS-2025`
- After running `setx`, **close and reopen** any terminals, editors, or Unreal Engine for the variable to take effect
- If the variable still isn't recognized after reopening, **restart your PC**

## Step 3: Build the Project

Open a command prompt and run the following build command. Adjust the paths if your UE4 install or repo location differs:

```
"C:\Program Files\Epic Games\UE_4.27\Engine\Build\BatchFiles\Build.bat" SimulationEditor Win64 Development "<YOUR_REPO_PATH>\Simulation.uproject" -waitmutex
```

For example, if your repo is at `D:\repos\SimulationRepository`:
```
"C:\Program Files\Epic Games\UE_4.27\Engine\Build\BatchFiles\Build.bat" SimulationEditor Win64 Development "D:\repos\SimulationRepository\Simulation.uproject" -waitmutex
```

This will take approximately 2-3 minutes on the first build.

## Step 4: Open and Test in Unreal Engine

1. Open `Simulation.uproject` in Unreal Engine 4.27
2. If prompted to import new source files, click **Yes**
3. Click **Play** — the project will boot WSL and start the simulation containers automatically. **Wait 30–60 seconds on the main menu** before entering a level to give the containers time to fully initialize.
4. Select a level (L_SUAS or L_IARC)
5. Verify the following:
   - A terminal window opens when entering the level with the container prompt visible (e.g., `root@...:#`)
   - The back button on each level works and returns you to the main menu
   - When you press **Stop** in the editor, the terminal window closes and WSL shuts down automatically

## Troubleshooting

| Problem | Solution |
|---------|----------|
| UE4 logs show "SUAS_REPO_PATH is not set" | Set the environment variable (Step 2) and restart the editor |
| Container never becomes ready | Make sure Podman is installed inside WSL. Check with: `wsl -d Ubuntu-24.04 -- podman ps` |
| Build fails with compiler errors | Ensure Visual Studio 2022 C++ Desktop Development workload is installed |
| WSL distro not found | Verify your distro name matches with: `wsl -l -v` (should show `Ubuntu-24.04`) |
| Terminal doesn't open when entering a level | Check that `run_container.sh` exists in your SUAS repo and is executable |
| "Permission denied" errors in WSL | Run `chmod +x run_container.sh` inside your SUAS repo directory in WSL |
| "Error at startup: bind" popup | Run `wsl --shutdown` in a Command Prompt to clear stale WSL processes, then reopen the project. If that doesn't work, restart your PC. |
