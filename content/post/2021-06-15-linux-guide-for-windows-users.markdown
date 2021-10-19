---
layout:	post
title: "How to switch from Windows to Linux"
date:  2021-06-15 15:12:50 -0800
categories: linux
---

# Warning
**Before getting into this it is assumed all your important data is backed up somewhere, whether that's a cloud service, or external device, this WILL wipe all your data, and I'm not responsible for anything that goes wrong!**

# Introduction
~~Microsoft announced their new Windows 11~~, (correction: the new Windows 11 was **leaked**, not officially announced) I hate it. This inspired me to write a blog post to help Windows users switch over from Windows 10 to Linux, I'm gonna be focusing on Kubuntu.

# Why Kubuntu?
* The user interface, KDE Plasma, is very good for people who are familiar with the Windows user interface
* Ubuntu supports secure boot out of the box so you don't have to disable anything to get Kubuntu to install
* Being LTS, it's extremely stable and reliable, there's a predictable 2 year release schedule
* You have access to newer programs using snaps and flatpaks
* Ubuntu being one of the most popular Linux distros, is supported extremely well both by software and hardware

# Getting started!
You will first need to go to the [Kubuntu website](https://kubuntu.org/) and [download](https://kubuntu.org/getkubuntu/) the [latest LTS](https://cdimage.ubuntu.com/kubuntu/releases/20.04.2/release/kubuntu-20.04.2.0-desktop-amd64.iso), yes I said LTS, I don't recommend non LTS releases of Ubuntu due to them having stability issues, these releases typically aren't tested for stability and might contain bugs, security issues, or other problems.

# Creating the installation USB
Since Ubuntu has a [great guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows) to do this, I won't go over it, I recommend following their guide [here](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows), just substitute the Ubuntu iso for the Kubuntu iso.

# Booting up and installation
Since this step is specific to your hardware, you'll have to look up how to enter the boot menu so you can boot from USB, some common key configurations are ESC, F2,F 8, F11, F12, Delete, and Enter. You'll have to look up how to do this.

Once booted you will see a screen like this.
![Kubuntu installation screen](/assets/linux-for-windows-users/Screenshot_20210615_153619.png)

Select the `Install Kubuntu` button. Selecting your keyboard layout and connecting to the network should be fairly straight forward.
![Kubuntu installation options](/assets/linux-for-windows-users/Screenshot_20210615_155723.png)

Once you are here, you should select the `Normal installation` as it has tons of useful applications, such as libreoffice, you should also select `Download updates while installing Kubuntu`, and `Install third-party software for graphics and Wi-Fi hardware and additional media formats` it will save tons of time.

Next you will need to setup your partitions, using `Guided - use entire disk and set up encrypted LVM` is recommended, but not required, as that will keep your data secure. However, it is recommended to set a strong password if you decide on this! However, I won't be using encrypted LVM because this is a virtual machine, so I will select `Guided - use entire disk`.
![Partition setup](/assets/linux-for-windows-users/Screenshot_20210615_160332.png)

After clicking on `Install Now` it will start installing the system, it will ask you to select a time zone, which is straight forward. Next it will ask you to setup a user name and password, and set a computer name.
![Account setup](/assets/linux-for-windows-users/Screenshot_20210615_160612.png)

The installation will take a few minutes to complete. Once done you will need to reboot and you should be at the Kubuntu desktop.
![Desktop](/assets/linux-for-windows-users/Screenshot_20210615_173103.png)

# Configuration
You can make Kubuntu more like Windows 7 and up by setting the bottom panel to `Icon-Only Task Manager` by right clicking the middle of the bottom panel and clicking on `Show Alternatives`.
![Changing the panel](/assets/linux-for-windows-users/Screenshot_20210615_173450.png)

You can also make the panel bigger by right clicking, clicking on `Edit Panel...` and adjusting the `Height`.
Getting used to the Kubuntu desktop should be a lot like getting used to a new version of Windows. There's tons of configuration options to play with, however I advise excercising caution if you are unsure what you are doing.

# Installing applications
You can install applications by going into `Discover`, and `Applications`, and you should see tons of applications that you can install. Many proprietary applications are distributed in `.deb`, which can be installed either from command line `sudo apt install ./application.deb`, or by right clicking the `.deb` file and clicking `Open with QApt Package Installer`.
![Discover](/assets/linux-for-windows-users/Screenshot_20210615_175223.png)

If you want an easy way to discover and install new applications, you can check out [flatpak](https://www.flatpak.org/).

# Installing wine
First go to [winehq.org](https://www.winehq.org), and click download, scroll down to Ubuntu, and click it. Here's a full [link](https://wiki.winehq.org/Ubuntu).

Now open up `Konsole`, and follow the wiki, I'll give a run down of the commands here:
```
$ sudo dpkg --add-architecture i386
$ wget -nc https://dl.winehq.org/wine-builds/winehq.key
$ sudo apt-key add winehq.key
$ sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu focal main'
$ sudo apt update
$ sudo apt install --install-recommends winehq-stable
```

Once done with that, you should be able to double click and run executables like they are native, you should also be able to install Windows applications, and run them like they are native applications. **Be aware not all will work!**

# Wrapping up
Hopefully by now you are setup with Kubuntu on your system. I recommend checking out [command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners) by Canonical. Learning how to use the command line will greatly help you in your Linux journey, although it's not required anymore.
