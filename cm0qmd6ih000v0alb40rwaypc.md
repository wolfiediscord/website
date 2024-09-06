---
title: "Running Debian 10 On My Macbook"
datePublished: Mon Oct 12 2020 00:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cm0qmd6ih000v0alb40rwaypc
slug: running-debian-10-on-my-macbook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725623034919/1052de10-cd38-4776-b34c-b559238a78c1.jpeg

---

*My Macbook Running Debian 10*

Yes, I know that I have written quite a bit of OS reviews, but this one is really good. Two days ago, I decided it would be nice to give Debian a try on my 4,1, as it has an older kernel and the lid closing might work. At the time of this post, the latest Debian release is Debian 10 Buster.

## Installing Debian

I downloaded the operating system on their website using the HTTP option (I found it to be the easiest). I used dd in order to flash it onto the USB flash drive I had on hand. I booted to it, and chose the Graphical Installer. When it booted into the installer, the trackpad did not work. I navigated all the way to selecting the Desktop Environment before I had to get a actual mouse. Luckily, there was one where I was staying (I was not at my house), and when I plugged it in, it worked and I selected Continue.

After the install, I booted into XFCE, which is the Desktop Environment I chose. There were some pretty major issues straight off the bat. The trackpad had no way of right clicking,the panel was not great, the WiFi card did not work, and the fans weren't turning on when they should. I managed to fix them and I am going to list the steps below.

## Fixing Networking

The most important thing I needed to get working was networking. Luckily, it was pretty easy. I had the Broadcom BCM4321. You might have a different card, so be sure to check the instructions on the Debian wiki page for your card. You need to have a Ethernet connection, so be sure to connect to the internet, otherwise you won't be able to install the necessary firmware. I installed the `firmware-b43-installer` package from the Debian repositories. Since I installed from a USB, the repositories were screwed up. In order to fix this, I nanoed into the `/etc/apt/sources.list` file and changed my repositories to match the Debian Wiki's sources.list example file. This fixed the repositories after I updated it using `sudo apt update`. Once I installed the firmware package, it seemed to go alright from there.

* [Debian Wiki: bcm43xx](https://wiki.debian.org/bcm43xx)
    
* [Debian Wiki: SourcesList](https://wiki.debian.org/SourcesList)
    

## Fixing the Trackpad

In order to fix the trackpad, I needed to add a configuration file and install a package. I used the instructions from a blog called the Rambling Moose, where they explained how to fix the issue. I installed the `xserver-xorg-input-synaptics` package after updating my xorg config files. After a reboot, it seemed to work just fine.

* [Rambling Moose: How to get Tap To Click back on your Debian 9 XFCE Linux install](http://www.ramblingmoose.com/2017/07/how-to-get-tap-to-click-back-on-your.html)
    

## Fixing the Panel

After I did that, I wanted to get the panel out of the way. I originally tried adding it to the autostart, but that ended up doing nothing. I finally figured out what the issue was. Make sure that when you log out, the checkbox saying "Save Session after Logout" or something similar is not checked. After doing this, the panel seemed to autostart just fine.

## Fixing the Fans

Originally I tried a utility made by datcuandrei called MacLinuxUtils. It is a nice Java program that I would highly recommend. I had a small issue initially with the program, as the xfce-terminal would kill the Java process after exiting, but the author fixed the issue and updated the release. In the end, I decided not to use the program for the time being. The author of the program told me that they would be adding features such as a system tray icon and a more customizable manual mode for the fans. Once these features are added, I will gladly switch to the application again. In the meantime, I decided to try macfanctld. The program runs as a systemd daemon and has a config file located in /etc. It made the laptop much cooler and is my second recommendation if you don't want to use MacLinuxUtils.

* [MacLinuxUtils](https://github.com/datcuandrei/MacLinuxUtils)
    
* [Ubuntu Manpage: macfanctld](https://manpages.ubuntu.com/manpages/bionic/man1/macfanctld.1.html)
    

## Overall Thoughts

Debian 10 has been great. I have been able to use suspend like normal, and I have been able to use my favorite web browser Brave with it. It feels more up to date than my old OS X Mountain Lion install. I am dualbooting both however, so if I need OS X I have it available. After fixing all of the issues mentioned above, it runs great. It even runs quite a bit cooler than OS X. It stays around 90-130°F. The RAM usage is also much better. It idles around 300-500MB. Debian 10 is a great option for those who have old macs and don't want to use an OS like Chrome OS on it. It is much better than Ubuntu in my opinion, which has its fair share of issues on this old machine. If you need a user friendly installer, I would recommend checking out the Debian Live ISOs, as it uses the Calemares installer.