# Simulation - Executable Setup Guide

## Prerequisites

Before running the simulation, make sure you have the following installed:

1. **WSL2** with **Ubuntu 24.04** — install by running in an admin command prompt:
   ```
   wsl --install -d Ubuntu-24.04
   ```
2. **Podman and podman-compose** installed inside your WSL Ubuntu. Run the following inside WSL:
   ```
   sudo apt-get install -y podman
   pip3 install podman-compose
   ```
3. **SUAS-2025 repo** cloned somewhere on your machine (e.g., `D:\repos\SUAS-2025`)

## Step 1: Clone the SUAS-2025 Repo (if you don't have it)

If you already have the SUAS-2025 repo cloned, skip this step.

```
git clone https://github.com/MissouriMRR/SUAS-2025.git
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
- After running `setx`, **close and reopen** any terminals for the variable to take effect
- If the variable still isn't recognized after reopening, **restart your PC**

## Step 3: Install and Run the Simulation

1. Run the **Multirotor_Simulation_Installer** provided to you and follow the installation prompts.
2. Once installed, launch the simulation from where it was installed.

The simulation will automatically start WSL and the required containers. **Wait 30–60 seconds on the main menu** before entering a level to give the containers time to fully initialize.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Simulation shows "SUAS_REPO_PATH is not set" | Set the environment variable (Step 2) and relaunch the executable |
| Container never becomes ready | Make sure Podman is installed inside WSL. Check with: `wsl -d Ubuntu-24.04 -- podman ps` |
| WSL distro not found | Verify your distro name matches with: `wsl -l -v` (should show `Ubuntu-24.04`) |
| Terminal doesn't open when entering a level | Check that `run_container.sh` exists in your SUAS repo and is executable |
| "Permission denied" errors in WSL | Run `chmod +x run_container.sh` inside your SUAS repo directory in WSL |
| "Error at startup: bind" popup | Run `wsl --shutdown` in a Command Prompt to clear stale WSL processes, then relaunch the executable. If that doesn't work, restart your PC. |
