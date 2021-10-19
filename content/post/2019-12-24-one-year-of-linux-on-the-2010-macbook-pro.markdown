---
layout:	post
title: "One year of Linux on the 2010 MacBook Pro"
date:  2019-12-24 15:09:34 -0800
categories: linux
---
# **Introduction**
![MBP running Arch](/assets/linux-on-mbp/IMG_20191223_184702.jpg)
So I got the 2010 MacBook Pro to use as a MacBook for school and on the go. I started using macOS in 2016 when I built my first hackintosh because I liked the user interface, the dock at the bottom, the bar at the top, and it looked nice together. However as I started getting into embedded development and some other stuff I quickly realized that macOS was insufficient for what I needed to accomplish. I started looking into Linux, first starting with Ubuntu and moving my way over to Arch. For this blog post the distro is irrelevant since what I'm gonna cover should be universal across distros. However most if not all of my Linux experience is with Arch and Ubuntu* so if you want to ask me questions I'll try my best with other distros although Arch or Ubuntu* will be the easiest for me to cover.

# **Why Linux?**
Although macOS is a great OS for things like content creation, productivity, and even some programming I've been running into issues with it as my interests has been shifting over from iOS research to embedded development. One problem was that a lot of toolchains that support Linux are incompatible with macOS. This is because of the nature of macOS not handling Linux compiles very well since it's not Linux. It's much easier to compile Linux things directly on Linux itself than it is to cross compile for Linux from macOS.

Due to the change in platforms I was working with I decided that it would be best to get into Linux by using it as my main OS. Get immersed in the entire Linux experience. I've been a happy Linux user ever since right? Not exactly, I'll explain. 

# **Issues**
**This part will talk about Arch Linux.**
Installation of Arch Linux had some problems that I had to overcome. First thing I've noticed was that the wifi card was incompatible with Linux. Temporarily solved this with a USB wifi adapter but I ended up installing the b43-firmware package from the AUR. Had to run the wifi driver in pio mode which solved some of my issues. Drivers aren't the most stable so I still keep macOS dualbooted so I can switch back and forth (I still need macOS for some things).

I thought that everything went smoothly, had Xfce desktop environment as well as quite a few of my applications working well on Arch Linux. However there was problems. I installed Discord and started logging into my acc...no I didn't even get that far. When I installed Discord I encountered a blank screen when opening the app. In fact every electron app I use did this. I also noticed that my GPU performance was significantly worse than macOS and some games didn't work. Did some researching and realized it's because of the FOSS NVIDIA driver, also known as nouveau. This driver relies on reverse engineered closed source NVIDIA drivers and is a community effort which means it's not the most stable thing. I quickly realized that in order to achieve the most stability and have the best performance out of the card I needed the proprietary drivers. Well right now you might be thinking "just install the proprietary drivers" and I did, but I ran into more issues. I expected it to be seemless because they're drivers that come directly from NVIDIA and as I'm told the manufacturer makes the best drivers. I was wrong...so wrong that I actually almost had to end up reinstalling Arch over it. Then I found out about a workaround to get the drivers to work. The screen would stay black and xorg wouldn't start, I explained this [here](https://devfusediboot.github.io/misc/tech/stuff/2019/12/01/how-i-triplebooted-my-macbook-pro.html) as well as a solution on fixing it.

~~Additionally, audio was broken due to a kernel update. Linux 5.1 has an issue with audio that still hasn't been fixed to this day on this chipset, for now I use the linux-lts package which is 4.19.~~ Fixed in 5.4.6, tested 12/24/2019.

# **So now what?**
Arch seems to be the distro that works quite well on the MacBook. Has it's quirks but overall not terrible. I would recommed keeping a dualboot of macOS as Linux does have it's issues. I hope these issues get fixed but it seems to be usable for daily use. I'm hoping to get a better laptop that's better Linux supported.
