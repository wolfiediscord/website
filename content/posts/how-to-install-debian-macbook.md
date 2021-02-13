---
title: "How to Install Debian 10 On a Macbook (Early 2008)"
date: 2021-02-12T23:07:06-06:00
draft: false
toc: false
images:
tags:
  - OS Tutorial
---

![Debian Logo](/post-photos/debian_logo.jpg)
_Debian Logo_

After reinstalling Debian 10 on my Macbook 4,1, I realized that I made a few mistakes in my guide. This post is a tutorial on how to properly install Debian 10 on a Macbook 4,1.

## Installation
When you go through the installation process, choose graphical install. You need to have a secondary mouse, as the trackpad won't work during the install, and it won't work very well after the install. Networking won't work either, so you need a full installation image unless you have an ethernet connection. Once you have completed the installation, continue with this tutorial.

## Post Installation
There are several things we need to address after post installation: networking, the trackpad, the iSight camera, and an issue with the bootloader. Let's begin fixing those things.

### Networking
If you do not have a working internet connection that isn't Wi-Fi, you need to get one in order to follow the next steps. Once you have a working internet connection via ethernet or other means, you need to make sure you have the right wireless card. I have a model that is compatible with Debian. You can follow the wiki page [here](https://wiki.debian.org/bcm43xx) to check if you have a Broadcom 43xx wireless card. Once you have verified that you have the right card, install the `b43-firmware-installer` package. After you have installed it, reboot the machine. Hopefully, you will have networking working, and you will be able to proceed with this tutorial. If not, there are various amounts of resources available on the internet.

### Trackpad
The first thing you want to do to fix the trackpad is to install the following packages. The command is below.
```
sudo apt install xserver-xorg-input-libinput xserver-xorg-input-evdev xserver-xorg-input-mouse
```
If your system has the `xserver-xorg-input-synaptics` package installed, remove it. It will interfere with the libinput driver we are going to proceed with. Once you have made sure that the package isn't on your system, type the following commands below.
```
sudo su
mkdir -p /etc/X11/xorg.conf.d
echo 'Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        Option "Tapping" "on"
EndSection' > /etc/X11/xorg.conf.d/40-libinput.conf
systemctl restart lightdm
```
The last command will send you back to the login screen, so make sure you have everything saved. If you have a different login manager than lightdm, replace lightdm with your login manager's name. Some examples would be `gdm3`, `sddm`, etc.

Once you have done that, you should have a functioning touchpad that you can use tap to click with. The Macbook 4,1 doesn't have a right click button, so you have to press with two fingers on the touchpad in order to get it to right click.

### iSight Camera
In order to get the iSight camera to work, you are going to need to find the firmware on the internet. If you have a dualboot of Debian and OS X, you can get the location of the firmware by typing the following commands:
```
sudo su
mkdir -p /MacOSX
mount -t hfsplus /dev/sdaXX /MacOSX
ls /MacOSX/System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns/AppleUSBVideoSupport.kext/Contents/MacOS/AppleUSBVideoSupport
exit
```
Make sure to replace the XX's with the valid drive. You don't want to mount your Linux drive to the folder! You can find out what drive your OS X installation is on by typing `sudo fdisk -l`. 

Once you have the location of the iSight Camera firmware, you can type in `sudo apt install isight-firmware-tools` into your terminal. It will ask you where the firmware is located. You need to type in the location of the firmware, which we did above. If you downloaded it, put the path to the file you downloaded. If everything goes right, it should work just fine.

### The Bootloader
There is an issue with the Debian 10 bootloader that happens upon post installation. This fix is a workaround, so don't expect it to work forever. The error you get is an error saying `flipdone timeout` or something along those lines. In order to fix it, you need to change one of the values of the `/etc/grub/default` file. From your terminal, you need to run these commands:
```
sudo nano /etc/default/grub
```
Once you are in the file, change the value of the `GRUB_CMDLINE_LINUX_DEFAULT` to the following:
```
GRUB_CMD_LINUX_DEFAULT="quiet splash video=SVIDEO-1:d"
```
Once you are done, press `Ctrl` + `X` to exit out of the nano text editor. If it asks you to save, say yes. Once you are back into your terminal, type in the following commands:
```
sudo upgrade-grub
sudo reboot
```
Once you have done that, the bootloader should be fine, and your Debian 10 installation should boot much faster.

## Conclusion
After you have done all of these things, you should have a fully functioning Debian 10 installation on your Macbook 4,1. If you have any issues, please let me know on the [Unsupported Macs Discord Server](https://discord.gg/XbbWAsE) in the #linux channel. Thank you for reading my article. The sources of this information are listed below.

### Sources
 - [Debian Wiki: bcm43xx](https://wiki.debian.org/bcm43xx)
 - [Debian Wiki: SynapticTouchpad](https://wiki.debian.org/SynapticsTouchpad)
 - [Debian Wiki: Macbook#iSight](https://wiki.debian.org/MacBook#iSight)
 - [ElementaryOS StackExchange: How to overcome “flip_done” timed out errors](https://elementaryos.stackexchange.com/questions/18783/how-to-overcome-flip-done-timed-out-errors)
