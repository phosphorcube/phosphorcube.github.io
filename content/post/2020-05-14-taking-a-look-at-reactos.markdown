---
layout:	post
title: "Taking a look at ReactOS, a real open source Windows XP replacement?"
date:  2020-05-14 11:16:30 -0800
categories: misc
---
# **Introduction**
I got bored and decided to check out ReactOS, an open source alternative to Windows. It is the effort of a team who did some clean room reverse engineering of Windows and [wrote their code from scratch](https://reactos.org/wiki/ReactOS_FAQ). 

# **Impressions**
ReactOS is quite good for what it is, it is quite unstable and has poor hardware support but that is to be expected. I got it to boot on a couple computers laying around the house but I couldn't get the drivers to install. Here is a list of supported hardware on the [ReactOS wiki](https://reactos.org/wiki/Supported_Hardware), your hardware may or may not work. This operating system is extremely lightweight and runs on only 64MB RAM. There has even been some work to getting ReactOS running on the original Xbox which is quite exciting even if it's not very usable.
![ReactOS on Pentium 4 desktop](/assets/reactos_hardware/IMG_20200428_163915.jpg)
![ReactOS on Inspiron 6000](/assets/reactos_hardware/IMG_20200429_113253.jpg)

# **Virtualbox**
In virtualbox it works the best. I've played around with some stuff in virtualbox and we'll just use that because it works well enough to do some testing.
![ReactOS in virtualbox](/assets/reactos_virtualbox/Screenshot_20200514_101114.png)

# **Testing**
I got Firefox to install from the ReactOS application manager and was able to do some web browsing. Unfortunately when I tried to go to YouTube it would freeze the entire VM for an unapparent reason.
![ReactOS YouTube](/assets/reactos_virtualbox/Screenshot_20200514_101245.png)

ReactOS doesn't come with many built in applications, the ones it does come with are pretty standard for Windows or really any operating system.
![ReactOS Applications](/assets/reactos_virtualbox/Screenshot_20200514_104441.png)

ReactOS does come with an application manager where you can install some applications like Firefox, Abiword, Bochs.
![ReactOS running 3rd party applications](/assets/reactos_virtualbox/Screenshot_20200514_105734.png)

# **Conclusion**
ReactOS is a great operating system, it is quite buggy but that is to be expected. In the future I am optimistic that it will improve and maybe become a usable lightweight replacement for running Windows programs on old computers with Pentium III, 4, Core 2 Duo, and other old CPUs. As well as a free and open source replacement for Windows on modern PCs. I am excited to see the future of ReactOS and hope to see it ported to ARM, PPC, and other architectures. ReactOS would be a great OS to run on your old Pentium II laptop collecting dust in the closet or Pentium 4 desktop in storage, or a lightweight Windows alternative to run in virtualization for running Windows applications on macOS, Linux, or BSD. I don't think it will be replacing Windows on your PC anytime soon if ever, but it's a great operating system to play around with. I don't recommend trying it out on your main PC or using it for any serious tasks as it has weird filesystem bugs and stuff which could cause you to lose all your data. I would wait for ReactOS to come out of alpha to use it for any serious tasks.