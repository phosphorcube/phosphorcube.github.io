---
layout:	post
title: "I decided to install Gentoo"
date:  2020-12-20 12:15:00 -0800
categories: linux
---

I rarely if ever update this blog, maybe once every few months. Anyways I might one day find the motivation to get back into blogging, who knows these days. Anyways I decided to write this post because I'm using Gentoo now, a source based Linux distro.

# **Why Gentoo?**
I was using Arch for a while, then started looking into Gentoo. I liked the way it's package management works, being source based. So one day I took the leap, I installed Gentoo on my main computer, and I like it.

Why I use Gentoo Linux:
* USE flags! they are amazing and provide granular control over every package installed on my system
* Optimization! now this is sorta overrated, **do not use ``-funroll-loops`` or ``-ffast-math`` because those will make your system VERY unstable**, instead settle on an march closest to your CPU and use CPU_FLAGS_X86
* Control over the kernel! of course you can compile the kernel on any distro but on Gentoo it's the main way, however there is precompiled kernels if that is not your thing since configuring the kernel yourself can be intimidating if you aren't familiar with it, there's just something so satisfying about compiling your own kernel, and seeing eight penguins on the screen when starting up

# **Why not Gentoo?**
Now I am not gonna be one of those people to say "install Gentoo" since Gentoo might not be the distro for everyone.

Why you might not want to use Gentoo:
* Compiling takes *forever* for a lot of packages, especially on a slow system
* You might spend a lot of time configuring your computer the way you want
* If you just want a stable system with little maintainance, Gentoo isn't the distro for you, I'll get into stability later
* **Do not use Gentoo if you are a beginner to Linux**, I'm being dead serious about this, jumping into a distro like Gentoo as a beginner is a mistake and a half

# **Stability**
Gentoo can be as stable or as unstable as you want, unfortunately a lot of people don't know the best practices for Gentoo and many completely screw up their system's stability. A Gentoo system that's configured properly tends to be very stable, I rarely have to reboot and when I do reboot it's usually by choice.

# **Updating and maintaining a Gentoo install**
This is tedious, I wrote a script to automate it for the most part, you'll still have to intervene in some cases and update your kernel manually.

# **Conclusion**
Gentoo is a good distro for some people, I like Gentoo. You might too.
