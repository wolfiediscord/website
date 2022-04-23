---
title: "How to Turn Debian 11 into LMDE 5"
date: 2022-04-22
draft: false
toc: false
images:
tags:
  - OS Tutorial
---

![Linux Mint Background](/post-photos/linux-mint.png)
_Linux Mint Background_

**Note**: This is not endoursed by the Debian or Linux Mint Debian Edition team. Continue with this guide at your own risk.

Today, I decided to try and take a bare Debian 11 installation and turn it into an LMDE 5 install. How hard could it be?

# Why would you do this?
I already use LMDE 5 on my laptop. The only thing I don't like about it is how many packages are pre-installed (over 2000!). The "Expert Mode" installer does not give you a minimal option, unlike Ubuntu. I decided to try this out in a VM just to see how hard it is to do.

# Installing Debian 11
I went ahead and did a _very_ barebones installation. During the desktop environment selection, I only chose "Standard System Utilties". This means that there is no desktop installed at all. 

# After Installation
Once you have finished installing Debian 11, you will have no network connectivity. You need to use the `apt-cdrom` command to mount your installation media (this means you need the offline installer). Once you have done this, run these commands:
```
sudo apt update
sudo apt install network-manager
sudo reboot
```
Once you have rebooted, you can take out your installation media. If there is still no network connectivity, then run the command below.
```
systemctl enable --now NetworkManager 
```
- [apt-cdrom Man Page](https://manpages.debian.org/bullseye/apt/apt-cdrom.8.en.html)

# Fixing Apt Sources
After you have network connectivity, you need to first fix your apt sources. Edit the sources.list file using this command: 
```
sudo nano /etc/apt/sources.list
``` 
Once you have done this, make this file look EXACTLY like this:
```
deb https://deb.debian.org/debian/ bullseye main contrib non-free
deb https://deb.debian.org/debian/ bullseye-updates main contrib non-free
deb https://security.debian.org/ bullseye-security main contrib non-free
deb https://deb.debian.org/ bullseye-backports main contrib non-free
```
Once you have done this, Hit `Ctrl` + `x`, press `y`, then press `Enter`. Then, run these commands:
```
sudo apt update; sudo apt upgrade
sudo apt install gpg
```
This will install the `gpg` package, which is required for using `apt-key`.

# Adding the Linux Mint Keyring
Now that we have `apt-key` ready, type the commands below to add the Linux Mint Repository Key.
```
wget http://packages.linuxmint.com/pool/main/l/linuxmint-keyring/linuxmint-keyring_2016.05.26_all.deb
sudo apt install ./linuxmint-keyring_2016.05.26_all.deb
```
After you have done this, we are ready to add the Linux Mint repositories.

# Adding the Linux Mint Repositories & Fixing mintupdate
Now that we have the Linux Mint keyring, we need to change our apt sources. Run these commands below.
```
sudo rm /etc/apt/sources.list
sudo nano /etc/apt/sources.list.d/official-package-repositories.list
```
Once you are in `nano`, make the file look EXACTLY like this:
```
deb http://packages.linuxmint.com elsie main upstream import backport

deb https://deb.debian.org/debian/ bullseye main contrib non-free
deb https://deb.debian.org/debian/ bullseye-update main contrib non-free
deb https://security.debian.org/ bullseye-security main contrib non-free

deb https://deb.debian.org/debian/ bullseye-backports main contrib non-free
```
After you have done this, press `Ctrl` + `x`, then `y`, then `Enter`. Now, we need to configure our pinning. Run the command below.
```
sudo nano /etc/apt/preferences.d/official-package-repositories.pref
```
Make this file look exactly like this:
```
Package: *
Pin: origin live.linuxmint.com
Pin-Priority: 750

Package: *
Pin: release o=linuxmint,c=upstream
Pin-Priority: 700

Package: *
Pin: release o=LP-PPA-linuxmint-daily-build-team-daily-builds
Pin-Priority: 700
```
After you have done this, press `Ctrl` + `x`, then `y`, then `Enter`. We are going to do the same thing to a different file. Type the command below.
```
sudo nano /etc/apt/preferences.d/official-extra-repositories.pref
```
Make it look exactly like this:
```
Package: *
Pin: origin "build.linuxmint.com"
Pin-Priority: 700
```
After you have done this, press `Ctrl` + `x`, then `y`, then `Enter`. We are now ready to install the desktop environment.

# Installing Cinnamon
Now that we have done all of the configuration of apt, we can now install the Cinnamon desktop environment. You can also install MATE or XFCE, but this won't be covered in this guide. Run the commands below.
```
sudo apt update; sudo apt upgrade
sudo apt install cinnamon nemo mintdesktop mintinstall mintmenu mintsources mintsystem mintupdate mint-x-icons mint-y-icons mint-themes mint-mirrors gnome-terminal lightdm slick-greeter lightdm-settings dmz-cursor-theme 
```
This will install the Cinnamon desktop, Nemo (the file manager), the Software Sources App, the Mint Updater, the Software Center, the Linux Mint Themes, a list of mirrors (for Software Sources app), Gnome Terminal, LightDM, the Linux Mint greeter for LightDM, the LightDM configuration tool, and the default Linux Mint cursors. 

You might think we are ready to reboot into the desktop, but we aren't there yet! We still need to configure LightDM.

# Configuring LightDM
First, we need to change the greeter from the default GTK greeter to the Linux Mint greeter. Type the command below.
```
sudo nano /etc/lightdm/lightdm.conf
```
Go all the way down until you see the `greeter-session` underneath `[Seat:*]`. Change this value to:
```
greeter-session=slick-greeter
```
After you have done this, press `Ctrl` + `x`, then `y`, then `Enter`. Now, we need to enable the LightDM greeter. Type the command below.
```
systemctl enable lightdm
```
Once you have done this, type `sudo reboot`. We can now start configuring the Cinnamon desktop.

# Fixing the Theme
The first thing you may notice is that the theme is not present. Open the `System Settings` application, go into `Themes`, then change the options to the ones below.

**Window Borders**: Mint-Y

**Icons**: Mint-Y-Dark (can be any of the Mint-Y icons)

**Controls**: Mint-Y-Dark (can be any of the Mint-Y themes)

**Mouse Pointer**: DMZ-White (can be any of the DMZ cursors)

**Desktop**: Mint-Y-Dark (can be any of the Mint-Y themes)

Now that we have the theme figured out, lets fix LightDM again.

# Showing Users In LightDM
You may have noticed that you need to type out your username in LightDM first. To fix this, go into `System Settings`, then `Login Window`, then `Users`. Once you are here, uncheck the `Hide the user list *` option. Restart your system, and you shouldn't have to type your username out anymore.

# What Now?
From here, you are free to configure your system how you want. Some of the Linux Mint specific apps are listed below.
```
thingy, webapp-manager, pix, hypnotix, bulky, drawing, xed, warpinator, xreader, xviewer
```

# Conclusion
Changing from Debian 11 to LMDE 5 is not super hard to do. It requires some advanced knowledge about Linux. I would recommend new users to just download LMDE 5 from the Linux Mint website. Only advanced users should attempt this. Overall, this leads to a system with less packages, and a lighter system overall. I recorded around 550MB of RAM being used on a cold boot. Thanks for reading this guide, and I hope you found it helpful.
