---
layout:	post
title: "Installing Gentoo on Raspberry Pi 4"
date:  2021-04-06 11:12:50 -0800
categories: linux
---

# Introduction
If you remember Sakaki's Gentoo images for the Raspberry Pi 4, they were great. It was one of the best performing distros for the Raspberry Pi 4. Since Sakaki no longer updates them they are effectively dead.

Gentoo is one of my favorite Linux distros because of it's insane level of customization and control, over just about everything. In the make.conf you have a lot of control over your installation.

# Installation
Using qemu-aarch64-static you can setup a chroot and compile on your host.
Recommended because the Raspberry Pi 4 is slow. This is usually how people do embedded development with Gentoo.

Almost all of the Xfce packages are masked for arm64, you'll have to unmask them.

I don't feel like writing a Gentoo install guide, check these pages for reference!
* https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install
* https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install
* https://wiki.gentoo.org/wiki/Raspberry_Pi
* https://wiki.gentoo.org/wiki/Handbook:AMD64

I'll post my make.conf on GitHub later.

# Usability
Gentoo is one of the most optimized distros, unfortunately this comes at the cost of compile times. You can setup distcc, or a binhost if you wish. Compiling on the Pi itself takes a while. When you have everything compiled it's really usable.

# Leaving off
Should you install Gentoo on the Raspberry Pi 4? If you want tons of customization options, great optimization, granular package management, and have enough time and knowledge to troubleshoot and get it up and running, sure. For the average user I recommend sticking to Raspberry Pi OS, there's also Arch Linux ARM which is quite customizable but you won't have to do any compiling.

# Screenshot
![Screenshot](/assets/gentoo-pi4/Screenshot_2021-04-09_14-21-05.png)
