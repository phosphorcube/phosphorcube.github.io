---
layout:	post
title: "How I triple booted my MacBook Pro"
date:  2019-12-01 15:09:34 -0800
categories: misc
---
# **Introduction**
***Important: This has only been tested on the MacBook7,1 and MacBook Pro7,1 and I take no responsibility if something goes wrong!***
So I've been thinking about doing this for quite a long time. You may think well it's just a MacBook so it can't possibly be that hard, I mean PCs have been doing this for years. Well yes and no.
Before I explain why let me just tell you a bit of back story to why I needed to do this, I've basically been in a situation where I need to use Windows while on the go (yes it's rare but sometimes happens).
My MacBook Pro running macOS High Sierra as it's only OS didn't really cut it so I had to run a virtual machine (ugh a Core 2 Duo running a VM). 
I've been thinking about upgrading my MacBook for a while but I'm gonna need to wait until I get more money. So because using VMs is slow and macOS can't do everything I need I came up with the idea of triple booting.
My setup basically is macOS High Sierra, Xubuntu, and Windows 10. Easy right? If it was easy then this post wouldn't exist. I've been running into multiple issues before finally getting it to work.
So I opted for this partition layout:
```
| Partition     | Size (250GB SSD)     | Identifier  |
| ------------- | -------------------- | ----------- |
| EFI      	| 200MB 	       | disk0s1     |
| macOS         | 80.0GB               | disk0s2     |
| Linux         | 84.9GB               | disk0s3     |
| Windows	| 84.9GB	       | disk0s4     |
```

# **So why can't I just install all three OS's and have them coexist together?**
Well to answer that we need to go back to the time when MacBooks were basically the only laptop with UEFI, most OS's didn't
support UEFI or GPT. It was actually quite rare to find one that did, with the exception of macOS. Windows 7 did have partial support for UEFI and GPT but would fail to boot in a pure UEFI environment
since the generic graphics drivers are unable to initialize.

# **What does this have to do with now?**
Windows 8.0+ as well as every Linux distro developed starting with 2014 supports UEFI. So why would we have problems triple booting them?
It's kinda complicated, Macs work in a different way from most traditional PCs so even at firmware level they're fundamentally different. Due to that we have our own unique problems that we need to work around.
Getting Linux installed was actually pretty easy in UEFI mode, but there is a few things to consider. Since Windows wants to be running in CSM mode we are dealing with the same restrictions such as a hard four partition limit.
Due to this we need to get rid of our recovery partition, **before doing this disable SIP since it needs to be disabled in order to install rEFInd.** Refer to [how to disable SIP](https://www.macworld.co.uk/how-to/mac/how-turn-off-mac-os-x-system-integrity-protection-rootless-3638975/ "how to disable SIP").
From what I discovered through a bit of messing around as well as researching is that UEFI assigns a different PCI ID than CSM, this is fine for macOS since macOS knows how to handle it while Windows and Linux don't. 
On Linux the nouveau drivers work fine but when you install the NVIDIA drivers Xorg will usually refuse to start. Probably related to the ACPI, will do more research later. My solution on Linux was creating a grub script that set the PCI IDs on boot. 
On Windows I'm thinking that either an EFI script or DSDT patch should fix it but because I haven't gotten around to that yet let's just use hybrid MBR for now.

# **Ubuntu installation**
With SIP disabled you can now delete the recovery partition using something like gparted to avoid hitting the 4 partition limit **do not touch the ESP**, now reboot into macOS and use disk utility to partition the drive into three. **Make sure to make the partition you plan on installing Windows on a Fat32 partition since that seems to trigger hybrid MBR!**
Now reboot into the Ubuntu (or Xubuntu or whatever you want) installer and procede with the install like normal. When it says **Installation type** press **Something else** and then select the partition you made and mount it at /.
Now make sure the top partition is set to EFI or ESP (it's a 200MB EFI partition). Now you can finish the installation like normal. When rebooting let it auto boot into Ubuntu and make sure everything works correctly.

# **Drivers in Ubuntu**
With Ubuntu installed and everything working you might think you're done. Until you try to connect to wifi and that doesn't work, this is because the proprietary broadcom drivers aren't installed.
Connect to ethernet, open a terminal and do the following:
```
$ sudo apt-get update
$ sudo apt-get full-upgrade
$ sudo apt-get install firmware-b43-installer
```
Create a file ``/etc/modprobe.d/b43-firmware.conf`` and put in the following contents:
```
options b43 pio=1 qos=0

blacklist wl
blacklist brcmsmac
blacklist brcmfmac
blacklist b43legacy
blacklist bcm43xx
blacklist brcm80211
```
Now reboot your MacBook and make sure wifi works. With wifi working we can now move on to graphics, this is gonna be more tricky but doable. Verify that you're running in UEFI mode by doing:
```
$ dmesg | grep -i efi
```
If the output shows something like this:
```
[    0.000000] efi: EFI v1.10 by Apple
```
...then you're running in UEFI mode.
Once you've verified that you're in UEFI mode continue by creating a file ``/etc/grub.d/01_enable_vga.conf`` with the following contents:
```
cat << EOF
setpci -s "00:17.0" 3e.b=8
setpci -s "04:00.0" 04.b=7
EOF
```
Now you need to run these two commands:
```
$ sudo chmod 755 /etc/grub.d/01_enable_vga.conf
$ sudo update-grub
$ sudo reboot
```
If everything went successfully you should be able to run these commands and get the same output:
```
$ sudo setpci -s "00:17.0" 3e.b
08
$ sudo setpci -s "04:00.0" 04.b
07
```
Now install the NVIDIA drivers with the following command:
```
$ sudo apt-get install nvidia-340 # or just use the Additional drivers GUI that Ubuntu provides
```
For working brightness control issue the following command:
```
$ sudo nvidia-xconfig
```
Now edit /etc/X11/xorg.conf and add the following section:
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "GeForce 320M"
    Option         "RegistryDwords" "EnableBrightnessControl=1"
EndSection
```
Now we can move onto Windows.
# **Windows installation**
Use this webpage to download a Windows 10 iso [UUP dump](https://uupdump.ml/).
Burn it to a DVD and eject the DVD. Reboot into macOS and install [rEFInd bootloader](https://sourceforge.net/projects/refind/).
Reboot the Mac with the DVD inserted and boot it from the rEFInd boot menu. During the installation select the 3rd partition you made for Windows and format it during the Windows setup then install to it.
When the installer reboots select Windows from the rEFInd menu. Continue the installation like normal. Once you reach the desktop download the following: [bootcamp drivers](https://support.apple.com/kb/DL1836) [NVIDIA drivers](https://us.download.nvidia.com/Windows/320.00/320.00-notebook-win8-win7-64bit-international-beta.exe)

# **Drivers in Windows**
Extract the bootcamp driver folder and copy both that and the NVIDIA drivers to a flash drive and do the following:
Run CMD as administrator (Use Shift + fn + F10 for right click)
```
C:\> cd path\to\folder (ex: C:\BootCamp5.1.5722\BootCamp\Drivers\Apple
C:\> Bootcamp.msi
```
This bypasses the system check that prevents newer bootcamp drivers from installing on the older MacBook. Once done reboot.
Now install the NVIDIA drivers like normal and reboot.

# **What I learned**
* MacBooks handle things differently from traditional PCs especially at firmware level.
* PCI IDs have to be exact in order for drivers to work properly.
* Best thing to do in this case is get a better laptop if you can afford it or stick to virtual machines.

# **What can be improved**
* Hopefully one day I can get Windows running in UEFI mode with NVIDIA drivers and everything!
* Linux could be streamlined with a script someday if I have time to write one.

# **Some Links (I didn't think of this all on my own)**
* <https://askubuntu.com/questions/264247/proprietary-nvidia-drivers-with-efi-on-mac-to-prevent-overheating/613573#613573>
* <https://askubuntu.com/questions/76081/brightness-not-working-after-installing-nvidia-driver>
* <https://wiki.archlinux.org/index.php/MacBookPro7,1>
* <https://www.reddit.com/r/mac/comments/3fjyn2/install_windows_10_with_boot_camp_drivers_on_old/>

