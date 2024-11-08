---
permalink: /wsl/
---

# Windows Subsystem for Linux \(WSL\)

[Back to Docs](/docs/)

Virtual Machines are invaluable for executing code in isolated environments, or sandboxing, but they can be very resource intensive and may waste large amounts of storage space. Some projects do not require such an isolated perfect simulation of an environment.

Windows Subsystem for Linux is a Windows feature that allows for running a Linux environment on a Windows computer with much greater efficiency than Virtual Machines.

## Table of Contents

- **!! TODO THIS ONCE ALL SECTIONS MADE !!**

## Installing WSL

Microsoft has made it so that WSL can be enabled using one command.

The below command must be run in a Powershell terminal with admin priviledges.

To open an admin terminal: press the Windows key, search for powershell or terminal, and select the run as administrator option for it.

Powershell command:

```powershell
wsl --install
```

This should result in wsl being installed along with an Ubuntu linux distribution. Other distributions, if desired, can be searched for with the following Powershell command:

```powershell
wsl --list --online
```

If the desired distribution is in this list, then it's name can be entered into the following command to install it:

```powershell
wsl --install -d <DistroName>
```

## Using WSL

Now, installed Linux distributions should be searchable as installed programs on the host Windows system. Alternatively, Windows Terminal can be used to connect.

The terminal that the new Ubuntu install will use (the terminal given when lauching the WSL distribution) is Bash. If needed, more information on how use Bash can be found through internet searches or asking a sub-lead or other returning team member for help.

## Conveniently Using WSL With VSCode

VSCode, a popular IDE that is recommended for programming on Multirotor, has an extension that allows it to run programs through the Linux distribution and edit code files stored in the Linux instance.

## References

- [Microsoft WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/)
- [WSL VSCode Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
