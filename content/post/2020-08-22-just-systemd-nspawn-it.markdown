---
layout:	post
title: "Need to use a different Linux distro? Just systemd-nspawn it!"
date:  2020-08-22 08:00:34 -0800
categories: linux
---

I've been using Linux on my main computer for over a year and a half now, distro hopping because I always somehow find something wrong with the distro I'm using. Whether it be that I need to use a different version of the GCC compiler than the one in the distro's repos or something else. I've been trying Ubuntu, Debian, Arch, Gentoo, and some other ones like Fedora, Manjaro, openSUSE, Void Linux but none of them seemed to solve the problems I was having where I always need access to a package not in that distro's repositories, Arch is the best right for me right now because it has the AUR (Arch User Repository) which has most stuff but not everything. There's a few solutions I could think of but I hated all of them.

# **Bedrock Linux**
Bedrock Linux is a script that turns your Linux install into a hybrid thing where you have access to tons of different Linux distros, in theory it sounds great but in practice it's just horrible. It's a mess and there's always a possibility of breaking your bedrock install. I don't recommend using this at all.

# **Virtual Machines**
I was thinking of setting up some virtual machines of different distros using libvirt, but it's an extreme waste of resources and consumes tons of drive space, I also don't have access to all the computer's resources inside the VM which is a problem when I want to speed up compiling or do more intensive tasks.

# **Dualbooting**
No, just no.

# **Chroot**
Chroot is a decent solution but it's limited and doesn't have the same advantages as using a container system like systemd-nspawn.

# **Docker or other container systems**
Not built into the OS and I prefer things that are built in because it's more minimal. Docker and other solutions are also typically more complicated to setup and I like simplicity.

# **This is where systemd-nspawn comes in**
Systemd-nspawn is simple to configure and use, built into most Linux distros that use systemd (almost all of them at this point), and lightweight. It has many advantages over chroot such as:
* More control over your container
* Native solution, no need to use bash scripts or anything to automate it since it just runs as a systemd service
* Easy to setup, again...no bash scripts or anything else needed
* Better security, systemd-nspawn restricts access to various kernel interfaces to read only, such as /sys, /proc/sys or /sys/fs/selinux. Network interfaces and system clock cannot be changed from inside the container, device nodes cannot be created, and kernel modules cannot be loaded or unloaded. (source: [arch wiki](https://wiki.archlinux.org/index.php/Systemd-nspawn))
* Fully virtualizes filesystem hiearchy and process tree, IPC subsystems and host and domain name. (source: [arch wiki](https://wiki.archlinux.org/index.php/Systemd-nspawn))

# **Setting up systemd-nspawn**
Some good places to look are the [Arch Wiki](https://wiki.archlinux.org/index.php/Systemd-nspawn), [Debian Wiki](https://wiki.debian.org/nspawn), and [freedesktop.org](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html).

# **Use cases**
* Development
* Testing and packaging for different distros
* [Containerizing graphical applications](https://ramsdenj.com/2016/09/23/containerizing-graphical-applications-on-linux-with-systemd-nspawn.html)
* Sandboxing

# **Conclusion**
I think systemd-nspawn is a really nice solution, it seems to work well and is a simple, lightweight solution that is built into the OS.
