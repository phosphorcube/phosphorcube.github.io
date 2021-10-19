---
layout:	post
title: "Using Kubuntu on my old MacBook Pro"
date:  2021-04-11 11:23:50 -0800
categories: linux
---
I've used a 2010 MacBook Pro as my main laptop for a couple years, it's old, slow, and the last version of macOS to support it is 10.13.6. I replaced my MacBook a little over a year ago, so it hasn't been doing much lately.
![Picture](/assets/kubuntu-on-2010-mbp/IMG_3435.JPG)

# Introduction
So macOS 10.13, it's ancient, at this point a lot of applications are dropping support, and it's happening really fast too. I still use my 2010 MacBook Pro as my secondary laptop because it's still quite useful as a little 13" laptop. It has tons of ports, and the size is great. Sadly it's old, it has a Core 2 Duo P8800 and an NVIDIA GeForce 320M, I maxed out the RAM at 8GB and installed a 250GB SSD.

I decided to wipe macOS completely, and install Kubuntu on it to use as a secondary laptop. So far it's been going well, I've tried other distros like Arch Linux, Gentoo, Debian, Fedora, and they don't work nearly as well as the Ubuntu family.

# Kubuntu Installation
Installation was pretty easy, I just burned Kubuntu to a DVD and booted it in legacy mode, why? So I didn't have to do any hacks to make the NVIDIA GPU work properly. Installing from a DVD was quite a nostalgic experience. Something I don't do too often..
Once booted I was able to share the ethernet connection from my Lenovo IdeaPad and run the Kubuntu installer. During installation it pulled the `broadcom-wl` drivers. These seem to be fine at the moment, if there's any problems I could use `b43` or a USB adapter. After installation I installed the NVIDIA 340 drivers and was up and running.

# Performance
Using the proprietary NVIDIA 340 drivers which Ubuntu still maintains, performance seems to be decent. I would use `nouveau` if it had better compatibility, and didn't have graphical glitches with 3D acceleration.

# What can I use this laptop for?
Well there's lots of things, here's a list:
1. Web browsing
2. Light programming
3. Talking on chats
4. Connecting to computers remotely (what I usually use it for)
5. Light photo editing with Krita and GIMP
6. Media consumption
7. Most other things you can do on a laptop, just at a slower performance (it's 11 years old)

# Glxgears
![Glxgears](/assets/kubuntu-on-2010-mbp/Screenshot_20210411_113815.png)
