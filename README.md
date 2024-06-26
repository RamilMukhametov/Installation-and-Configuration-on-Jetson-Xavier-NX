# Installation and Configuration on Jetson Xavier NX

## Introduction

Useful links, preparations and common mistakes for the process of preparing Jetson Xavier NX.

## JetPack SDK 5.1.2

NVIDIA JetPack SDK is the most comprehensive solution for building end-to-end accelerated AI applications. JetPack provides a full development environment for hardware-accelerated AI-at-the-edge development on Nvidia Jetson modules.

Мore info on [this page](https://developer.nvidia.com/embedded/jetpack-sdk-512)

## Installing JetPack

[NVIDIA SDK Manager Method](https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html)

[SD Card Image Method](https://docs.nvidia.com/jetson/archives/r35.1/DeveloperGuide/text/SD/FlashingSupport.html#to-upgrade-jetpack-4-x-to-jetpack-5-x-on-jetson-xavier-nx-p3668-0000)

## Requirements

1. Check if your Jetson is really Xavier, not Nano (usually it should have active cooling and SSD instead of MicroSD)
1. Open [this link](http://www.yahboom.net/study/Jetson-Xavier-NX) and read the instructions briefly
1. Preferable use Debian-based linux

## Operating System Installation

### Step 1: Installing NVIDIA SDK Manager

1. Go to [NVidia website](https://developer.nvidia.com/sdk-manager) and click "download .deb Ubuntu".
1. You'll be asked for registration/login, do so.
1. On this step, it may prevent you from downloading. Just press "Join the NVIDIA Developer Program" on [this page](https://developer.nvidia.com/developer-program) and you'll be allowed to proceed.
1. Go to Downloads folder and install SDK. In my case, it was `sudo dpkg -i ./sdkmanager_2.1.0-11660_amd64.deb`
1. On this step, you may see errors like `dpkg: error processing package sdkmanager (--install): dependency problems - leaving unconfigured`. It can be solved by command `sudo apt --fix-broken install` and repeating `sudo dpkg -i ./sdkmanager_2.1.0-11660_amd64.deb` (just for confidence)
1. From now on, you should see SDKManager in your app list.
1. Open NVIDIA SDK Manager and log in

### Step 2: Preparing Jetson and installing system

1. Connect the jumper caps to the FC REC and GND pins, that is, the second and third pins of the carrier board below the core board.
1. Connect Jetson via MicroUSB to your PC with launched NVIDIA SDK.
1. Connect power supply (9V-20V) to the Jetson.
1. A window should appear asking you to select the model of the connected device. Select your device (mine is Developer Kit Version) and press Continue.
1. Select the required components (I selected all) and press Continue.
1. Wait for downloading and installing.
1. Type Username and Password, press Next.
1. Wait for Jetson to turn on, connect the display with HDMI.
1. Wait for the OS login screen on Jetson, log in.
1. On your PC, click "install" in SDK Manager.
1. On this step, you may see some troubles with Disk space and Internet connection. To fix Internet, just connect your Jetson to WiFi.
1. To fix Disk Space, firstly you should try to check if there any possibility to install SDK without transfering whole system to SSD. To do this, open terminal on your Jetson and type `df -h`. If the available space is indeed less than the required 15.53 GB and it's impossiblee to provide the required amount of space, you need to press Cancel, leave it at the window "SDK Manager is about to install SDK components on your Jetson Xavier NX module" and spend some time transfering the system to an SSD.
1. If everything is ok, just proceed with installing SDK. If not, disconnect Jetson from your PC and go to the Step 3; don't close NVIDIA SDK Manager, you will proceed from the same window.  

### Step 3: Mounting and using SSD

1. Disconnect Jetson from PC and remove the jumper. Turn it on.
1. Open terminal at your Jetson and execute commands `df -h` to ensure that there is no SSD and `sudo fdisk -l` to find disk `/dev/nvme0n1: 119.25 GiB`.
1. Open the disk partition tool `Disks`, select 128 GB Disk, find sub-menu and press Format disk -> Don't overwrite existing data (Quick); Compatible with modern systems and hard disks > 2TB (GPT) -> Format.
1. Press Plus-shaped button and add partition with 128Gb size and Ext4, name it "SSD128", press Create.
1. Press Triangle-shaped button to mount partition.
1. Return to terminal and use `df -h` command to check for `/dev/nvme0n1p1 117G`.
1. ```bash
    cd ~
    git clone https://github.com/jetsonhacks/rootOnNVMe.git
    cd rootOnNVMe/
    ./copy-rootfs-ssd.sh # password required
    ./setup-service.sh # password required
    sudo reboot now
    ```
1. Open terminal again and check storage of `/` path using `df -h`. It should be equal to 117G.
1. If you haven't installed SDK components yet, connect your Jetson to PC with MicroSD and connect the jumper caps. Jetson shold be turned on and logged in and the jumper should be connected to start the process.
