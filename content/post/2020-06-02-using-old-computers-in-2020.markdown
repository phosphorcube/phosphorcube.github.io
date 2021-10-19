---
layout:	post
title: "Operating systems to use on old computers in 2020"
date:  2020-06-02 16:15:34 -0800
categories: misc
---
# **Introduction**
Here's a hypothetical situation, imagine you are using your computer and suddenly it stops powering on. No apparent reason, it just simply doesn't work anymore. However you do have a Pentium 4 computer sitting in your closet collecting dust. So being that it's your only functional computer, you plug it in and power it up. It runs an operating system from many years ago, Windows XP or an old Linux distro probably. Not very usable for 2020, so here are some more modern operating systems that are usable on old hardware.

# **Linux**
Here's some distros I can think of that run on old hardware:
1. Slackware
2. Debian
3. Gentoo
4. Void
5. Alpine
6. Linux from Scratch, buildroot, or other DIY

# Let's compare and contrast each of these:
1. Slackware is one of the lightest distros, but it's also older.
2. Debian is a bit newer, but it's more heavy and uses systemd.
3. Gentoo is very lightweight, very customizable, and can be optimized for the hardware it's running on. Although you will need to compile everything running on the system which is very time consuming, uses CPU, and you probably won't be able to use the machine while it's going on. You can setup an external build server using distcc and portage is highly configurable.
4. Void Linux uses runit as init system, it's a rolling release and newer than the other two on this list. Sadly it's lacking software in it's repos.
5. Alpine is the lightest on the list, but this comes at a cost. It uses musl instead of glibc meaning a lot of software will be incompatible, it also is lacking software in it's repos due to being designed for running embedded applications or inside a container.
6. DIY, this is an option but is time consuming, involves compiling, and is impractical for most people. There are things to help out such as the Linux from Scratch handbook guiding you through or buildroot automating the process. Since you don't have any form of package management you'll need to manage all software on your own.

# **BSD**
Now I'm not really a big BSD fan so I don't know too much about it, but here's me ranking some of the BSDs:
1. OpenBSD
2. NetBSD
3. FreeBSD

# Let's compare and contrast:
1. OpenBSD, lightweight and simple. For me a bit too simple, doesn't have an intuitive way to connect to wifi networks, sucks on laptops. Uses very little RAM and runs really well on old hardware, would recommend on old computers.
2. NetBSD, I actually haven't played with this much. With a custom kernel it can run on as little as 4MB RAM, also very lightweight and customizable just like OpenBSD. Not very intuitive when it comes to installing software, but you can install a package manager. Would recommend on old computers.
3. FreeBSD, heavier than NetBSD and OpenBSD but has a lot to offer. I wouldn't go as far as saying that I recommend on old computers but on lower end and mid range computers it's a decent choice.

# **Windows**
So Windows 10 runs pretty decent even on old computers if you throw in an SSD, decent graphics card, and enough RAM. I wouldn't recommend using Windows 10 if you have a CPU that predates the penryn Core 2 Duos or Intel GMA graphics. It seems to run well on my Core 2 Duo MacBook Pro but can get sluggish at times. Definitely put in an SSD and some more RAM if you want to use Windows 10. I actually have run Windows 10 on 192MB RAM if you want to check that out [here](https://twitter.com/0xN0ri/status/1245757272996765697?s=20).

# **Other**
Some obscure operating systems I've tried:
1. ReactOS
2. Haiku
3. FreeDOS

# Let's compare and contrast:
1. ReactOS can natively run programs compiled for Windows XP, but it has poor hardware support, is very buggy, and not recommended for daily use. ReactOS is designed to be an open source alternative to Windows, with the ability to run Windows applications and drivers. It looks and feels like Windows XP (with classic theme but it's compatible with Windows XP themes). I made a post about it more in depth [here](/taking-a-look-at-reactos).
2. Haiku, I haven't played around with this much so I can't recommend it, it can run some open source applications like firefox and is [posix compliant](https://discuss.haiku-os.org/t/is-haiku-a-unix-like-os/8801#:~:text=According%20to%20the%20POSIX%20article,POSIX%2Dcompliant%20and%20UNIX%20variants.). Not sure what hardware support is like since I haven't tested on real hardware. It's designed to be an open source replacement to BeOS with compatibility and the same look and feel.
3. FreeDOS, this is really just thrown in for the sake of why not. I don't recommend using this unless you're just looking to use legacy DOS applications, as it is DOS. FreeDOS is designed as an open source replacement for MS-DOS, designed to run on machines of the era but can run on modern machines with limited driver compatibility.

# **Conclusion**
Linux is a nice choice, there's many distros you can use. BSDs are also a nice choice, but more difficult to setup and don't have as good of software compatibility. Windows is a terrible choice for anything older than penryn Core 2 Duos as it gets really sluggish, and the more obscure operating systems are quite buggy and not recommended for daily use.
