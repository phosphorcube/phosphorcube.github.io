---
layout: post
title: "Using Wayland daily on KDE Plasma, My Experience"
date:  2021-07-14 09:30:50 -0800
categories: linux
---

# **Introduction**
Ever since KDE Plasma 5.21 it's been fully usable under Wayland. To the point where I never need to boot back into Xorg anymore!
KDE Plasma is my favorite desktop environment, atm. Plasma for a long time was the one thing holding me back from using Wayland. It wasn't that stable, and the experience on Xorg was overall far less buggy.
Xorg is slowly going away, Wayland is the future. While I don't think this will happen very fast, I with more and more desktop environments adopting Wayland support, there will be less and less reason to use Xorg.
Since the experience using Wayland vs Xorg is almost indistinguishable for the end user, more distros are adopting a Wayland session as default, ones like Fedora, and Ubuntu. For GNOME this has been fine, however
using Plasma on Wayland wasn't very good experience. There were tons of bugs and issues. However, since 5.21 that's changed, and you can use Plasma on Wayland with little issues.

# **My Experience**
Initially, I had some bugs with applications like OBS. OBS only supports Wayland through pipewire for screen and window capture.
With pipewire, Wayland has been very usable. I recommend using Pipewire if you are on Wayland.
Another problem is the mouse goes out the window when you capture it using an application like QEMU, which is annoying, you need to use a USB tablet as the mouse device on QEMU. 

Wayland does a lot of things such as handle DPI scaling, graphics tablets, touchpads and touchscreens, much better than Xorg. During Plasma 5.21 I switched between Xorg and Wayland but mostly stuck with Xorg, since Plasma 5.22 I started using Wayland pretty much exclusively. I'm at a point where if an application doesn't work on Wayland, I simply don't use that application anymore, although I haven't really run into that.

# Wayland advantages
* Better support for things like graphics tablets, touch screens, and touchpads
* Better DPI scaling
* More modern display server, fixes problems that Xorg had a difficult time fixing
* Wayland is a lot less complex making it simpler to maintain
* Possibly more that I don't know about off the top of my head

# Disadvantages (from my experience)
* Can't forward Wayland windows over SSH (annoying but not a deal breaker for me)
* Bugs in certain applications
* Some applications don't work at all
* Less stable in some cases

# **Conclusion**
Wayland lately has been very stable with little issues. Almost all of my applications have been working well, and there's been few issues. I embrace modern technologies because it's the future, and I like embracing the future. If you like to embrace the future too, you should use Wayland. You can help out by submitting bug reports, or even fixing bugs in software. As time goes on more and more of Xorg will be replaced with Wayland.
