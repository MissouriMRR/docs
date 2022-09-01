---
permalink: /virtual_machines/
---

# Virtual Machines

[Back to Docs](/docs/)

Virtual Machines allow you to run a virtual operating system on your host os. For example, if you have a Windows computer, you can create a Linux virtual machine, which would allow you to run a Linux desktop in a window. (very oversimplified)

It is recommended that you create, run, and test your code on Linux (usually Ubuntu). This is for a number of reasons, most importantly that Windows is often inconsistent and may require workarounds for certain tools/technologies to work correctly.

Below is a guide on creating an Ubuntu Linux virtual machine using VirtualBox on a Windows host machine. Ubuntu is a popular distribution (distro) of Linux and is very user-friendly. VirtualBox is a popular hypervisor (essentially a program that can create/run virtual machines).

## Downloads

To begin, you will need to download a couple of things:

- VirtualBox: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Ubuntu ISO image: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

## Installing VirtualBox

Once the installer for VirtualBox is downloaded, launch it and follow the prompts to install VirtualBox.

## Creating the Virtual Machine

1. Open VirtualBox
2. Click the "New" button at the top
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/add_button.png)
3. Give the machine a name and select the folder and OS.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/vm_name.png)
4. Allocate system memory (RAM).
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/ram.png)
5. Select "Create Virtual Hard Disk Now".
6. For Hard Disk Type, select VDI.
7. Select either "Dynamically allocated" or "Fixed size". A fixed size virtual disk will use all space that you allocate to it on disk. A dynamically allocated disk will fluctuate in size and will use *up to* the amount that you allocate. Dynamically allocated will use less space, but typically comes at a performance hit.
8. Select the amount of storage you want your virtual hard disk to have. You should allocate at least 12 GB of storage.
9. You should now have a new virtual machine in your list.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/new_vm.png)

## Allocating VM Resources

1. On your new VM, click on settings. You will need to change the resource allocation before starting your machine.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/settings_button.png)
2. Under General > Advanced, enable bidirectional shared clipboard. This will allow you to copy/paste between your VM and your host machine.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/shared_clipboard.png)
3. Under System > Processor, set the number of CPU cores you wish to allocate to the machine. VirtualBox will not actually hold exclusivity over these cores, so TLDR you want to assign at least 2-4 CPU cores if you can. (The more you assign, the faster the machine can be.)
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/cpu.png)
4. Under Display > Screen, crank the video memory to max. (Assuming you have at least 128 MB of VRAM that is, which you probably do.)
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/vid_mem.png)
5. Click OK to save your changes.

## Starting Your VM

Now that all of the settings are taken care of, you can start your machine for the first time. We will now have to take that Ubuntu ISO that you downloaded and use it to install an OS on the VM.

1. On your VM, click start.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/settings_button.png)
2. VirtualBox will prompt you to select a start-up disk. Think of it like inserting a CD to install your OS, where the ISO file we downloaded is the CD. Choose the folder icon and navigate to where you downloaded the Ubuntu ISO file to. Once you have it selected, press start.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/start_up_disk.png)
3. Select Try or Install Ubuntu with the arrow keys and press enter. You will now boot into the Ubuntu installer.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/try_install.png)
4. Select Install Ubuntu. As far as Ubuntu is aware, it is running on a bare-metal computer. It doesn't know that it's in a VM.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/install_ubuntu.png)
5. Select your keyboard layout.
6. Select your software options.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/software.png)
7. Select "Erase disk and install Ubuntu". That may sound scary, but don't worry. The disk that it is referring to is the virtual hard disk that you created earlier. The machine is incapable of touching anything outside of the virtual hard disk, so your files are safe. Click "Install Now". On the prompt about disk changes, select "Continue".
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/erase_disk.png)
8. Choose your time zone.
9. Fill out your information. Make sure to choose a password that you will remember. We will be needing it a lot.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/who_you.png)
10. It will now begin installing the operating system. Sit back and wait for it to finish. Once the installation is complete, select "restart now".
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/wait_for_install.png)
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/restart_now.png)
11. The installation medium should have been automatically ejected, so press enter once the machine prompts you. You should now be booting into your newly create Ubuntu VM.
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/installation_medium.png)
![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/technology/virtual_machines/images/login_screen.png)