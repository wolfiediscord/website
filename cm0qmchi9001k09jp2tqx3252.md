---
title: "Debian Troubles and Why I Switched Back"
datePublished: Wed Dec 16 2020 00:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cm0qmchi9001k09jp2tqx3252
slug: debian-troubles-and-why-i-switched-back
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725622194800/6e8a45be-d41c-4f85-8e28-32164408dafa.jpeg

---

*Debian Logo*

I have been using Debian again for the past two weeks. I think that the experience is great, even though I have had a few issues (mostly KDE related).

## The Fedora Story

I switched back to Debian for one major issue. My old NVIDIA graphics card was acting up on Fedora and causing it to slow down immensely and freeze the system, forcing me to reboot. I also switched back because I know how Debian functions compared to not knowing much in Fedora. Fedora is great on newer hardware, but my older hardware was not very well suited for it.

## Trying Arch Again

Before I switched back to Debian, I tried Arch Linux again. It was okay. I got my setup the way I like it with dwm, a tiling window manager, and some more suckless utilites, but there was one major issue that prevented me from staying on Arch. That issue was suspending; no matter what I did, the system would not suspend. It would keep the light on and the fans wouldn't shut down either. It wouldn't wake up from this state at all. I looked around and found out it was an issue with my CPU, but I could not find a fix for it online.

## Going Back to Debian with Some Mishaps

I decided to give up on Arch Linux and go back to Debian. I went through the install process and got my system set up pretty nicely. However, when I went to update, I saw i386 instead of amd64. "Oh no!" I realized at the time. I managed to install a 32 bit version of Debian instead of a 64 bit version. The entire reason why this happened was because I thought that a 32 bit version would work well on my Macbook 4,1. I went and downloaded the 64 bit version and reinstalled with the KDE Desktop.

## The KDE Experience

After reinstalling Debian and choosing the KDE desktop, I decided to debloat the system quite a bit. I removed some of the unnecessary packages (at least for me) such as the screen reader and other stuff. Overall, Debian with KDE is really good, and I would consider it one of the more modern desktops for Debian. It was great for me for about seven days or so, until disaster struck. Every time I would lock the system, there would be no prompt for my password. I would have to go into a tty and unlock the session using loginctl. I decided to switch to a more reliable desktop environment and go to XFCE.

## The XFCE Experience

Immediately after installing XFCE, I noticed one thing. It was very ugly. I decided to try and rice it a bit using the arc theme and papirus icon theme. It looks alright now, but I really liked my KDE setup better. Overall, XFCE has been super reliable for me so far, and it is faster than KDE. There are a few things I miss that I had in KDE, such as KWin Scripts (to emulate a dwm-like experience), easy theming, and fancy graphics. I think that in the future I will go back to using dwm instead of any desktop environment, but for now I am fine using XFCE.

## Conclusion

Debian has been great so far for me. Other than a few issues that have to deal with the KDE desktop environment, it has been a good experience. Debian is fast, reliable, and I love how packages are not up to date even on my desktop. I don't use my desktop for things that would require the latest and greatest, and I think that stability is more important than new features. That is why I am sticking to Debian for the time being.