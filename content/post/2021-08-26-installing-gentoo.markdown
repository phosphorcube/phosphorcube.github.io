---
layout: post
title: "Installing Gentoo"
date:  2021-08-26 23:45:50 -0800
categories: linux
---

# **Introduction**
Gentoo is a source based Linux distribution which is very flexible and can be customized to fit the user's needs. ~~A bit ago I started using Gentoo Linux daily.~~ EDIT: Back to using Arch Linux. Here's the install procedure, because a lot of people seem to think
it's difficult.

# **Creating a live USB**
Obviously before we can begin, we need to boot the computer into some kind of environment. I opt for [systemrescuecd](https://www.system-rescue.org/).

You can use dd to create the USB.
```
$ dd if=systemrescuecd.iso of=/dev/sdb bs=4096 status=progress
```

If you are on Windows you can use [win32diskimager](https://win32diskimager.org/).

# **Preparations**
Boot into systemrescuecd the same way you'd boot into any other USB. Once booted you can either start the GUI or continue in the command line, your choice. I will be going through the steps in the command line.

# Partitioning the disk
List the disks out using lsblk, you should see an output like this:
```
NAME                                          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda                                             8:0    0 931.5G  0 disk  
└─sda1                                          8:1    0 931.5G  0 part  
sr0                                            11:0    1     2K  0 rom   
```

This command lists out block devices. Keep note of the hard disk or SSD you want to install to because you'll need to know it for preparing the disk.

Run the cfdisk command with the name of the disk you want to install to.
```
# cfdisk /dev/sda
```

Follow the instructions for creating a new partition, should be self explanatory, but if you're not sure how to use cfdisk, [this article](https://www.makeuseof.com/how-to-create-resize-and-delete-linux-partitions-with-cfdisk/) explains better than me. 
If you are using `MBR` or `DOS` you will need to mark the first partition as `Bootable`.

This is an example layout:
```
Boot partition: /dev/sda1   fat32   500MB (must be fat32 if you're using UEFI, because that's the EFI system partition)
Root partition: /dev/sda2   ext4    241GB (you're gonna need a lot for compiling)
Swap partition: /dev/sda3   swap    8GB
```

Formatting and creating the filesystems.
```
# mkfs.vfat -n boot /dev/sda1 (creates a fat32 filesystem with the label boot)
# mkfs.ext4 -L linux /dev/sda2 (creates an ext4 filesystem with the label linux)
# mkswap -L swap /dev/sda3 (creates a swap partition with the label swap)
```

Mounting the disks
```
# mkdir -p /mnt/gentoo
# mount /dev/sda2 /mnt/gentoo (mount root partition in /mnt/gentoo)
# mkdir /mnt/gentoo/boot (create boot directory)
# swapon /dev/sda3 (swapon the swap partition)
```

# Installing the root filesystem
You will need a root filesystem tarball to use for installing Gentoo, you can get it from https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/current-stage3-amd64-openrc/.
```
# cd /mnt/gentoo (enter the directory where the root partition is mounted)
# wget https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/20210801T170533Z/stage3-amd64-openrc-20210801T170533Z.tar.xz (download a stage tarball as close to this date as possible)
```

Unpack the stage tarball
```
# tar xvpf stage3-amd64-openrc-20210801T170533Z.tar.xz --xattrs-include="*" --numeric-owner
```

Configure some things like the `make.conf`
```
# vim /mnt/gentoo/etc/portage/make.conf
```

A default `make.conf` will look something like this
```
# These settings were set by the catalyst build script that automatically
# built this stage
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example
COMMON_FLAGS="-O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"

# NOTE: This stage was built with the bindist Use flag enabled
PORTDIR="/var/db/repos/gentoo"
DISTDIR="/var/cache/distfiles"
PKGDIR="/var/cache/binpkgs"

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C
```

Customize it the way you want or need, here's an example of a customized one
```
# CFLAGS and other stuff specific to my CPU
COMMON_FLAGS="-march=skylake -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
MAKEOPTS="-j7"
VIDEO_CARDS="intel i965 iris"
INPUT_DEVICES="wacom libinput"
CPU_FLAGS_X86="aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4_1 sse4_2 ssse3"

# USE flags
USE="-gnome -ppp wayland X xinerama bluetooth pulseaudio networkmanager \
     qt5 kde dvd cdr avahi Xvaapi sound vim-syntax x264 \
     zsh-completion udev ios gphoto2"

# Build a binary package that I can distribute to other machines
PORTAGE_NICENESS=14
L10N="en en-US"

# Set targets for certain packages
ADA_TARGET="gnat_2021"
QEMU_SOFTMMU_TARGETS="arm aarch64 alpha i386 x86_64 sparc sparc64 ppc ppc64 riscv32 riscv64 mipsel mips64el mips mips64"
QEMU_USER_TARGETS="x86_64 i386 aarch64 arm"
GRUB_PLATFORMS="efi-64 pc"

# Accept all licenses, except those with a EULA
ACCEPT_LICENSE="* -@EULA"

# Set default emerge options
EMERGE_DEFAULT_OPTS="${EMERGE_DEFAULT_OPTS} --jobs 4 --usepkg-exclude 'sys-kernel/gentoo-sources virtual/*'"

# Portage directories
PORTDIR="/var/db/repos/gentoo"
DISTDIR="/var/cache/distfiles"
PKGDIR="/var/cache/binpkgs"
CCACHE_DIR="/var/cache/ccache"

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C

# Mirrors
GENTOO_MIRRORS="https://mirrors.advancedhosters.com/gentoo/ http://gentoo.mirrors.easynews.com/linux/gentoo/ http://mirrors.rit.edu/gentoo/ http://www.gtlib.gatech.edu/pub/gentoo"
```

A quick run down of what the basic options do

# CFLAGS
`COMMON_FLAGS` is basically where your compiler flags will go. You can read a bit about safe `CFLAGS` [here](https://wiki.gentoo.org/wiki/Safe_CFLAGS). CFLAGS tell the compiler what optimizations and options to use, when
compiling the code. So it basically tells the compiler how it wants to produce the code. Some CFLAGS which you should pay attention to are `-O`, this is optimization level, it goes all the way up to `-O3`. `-O2` is the
recommended and known stable optimization level. `-O0` is basically no optimization, so it's usually used for debugging, and `-O1` is little optimization. You might want to pay attention to `-march` and `-mtune` which tells
the compiler how to produce the best and most optimized code for your CPU microarchitecture. `-march` is forwards compatible, but not backwards compatible, so binaries compiled using `-march=core2` will run on Core 2 and up.
`-mtune` tells the compiler to optimize code for a specific CPU microarchitecture, without breaking compatibility with older CPUs. An example would be `-march=haswell -mtune=skylake`, what this would do is produce binaries
compatible with Haswell and up, but optimized for Skylake, so it won't perform as well as `-march=skylake` but your binaries will remain compatible with the older microarchitecture. I recommend avoiding most `CFLAGS`, that you
might see on forums and things, such as `-funroll-all-loops` and `-ffast-math`. These `CFLAGS` are typically very aggressive and can produce unstable and/or broken code. `-pipe` will not make any difference on the generated code, but will make compilation faster, turn this off if you are on a limited amount of RAM.

# CPU_FLAGS_X86
`CPU_FLAGS_X86` is what tells the compiler which CPU instructions to enable, you can see which your CPU supports by running the `lscpu` command, and checking out flags. This is optional and you probably don't have to worry
about it right now, but it's something you can either setup now or later. It tells the compiler to produce code that takes advantage of the instructions in your CPU, so if you want binaries compatible with an older CPU, only
use instructions compatible with that older CPU, otherwise you'll get `illegal instruction` when trying to run the binaries on an older CPU that's missing that instruction.

# MAKEOPTS
`MAKEOPTS` tells portage how many threads to give the compiler. It's recommended to only enable as many `MAKEOPTS` as you have RAM to support it. If you are running into compilation failures due to lack of RAM, or lock ups,
lower your `MAKEOPTS`. You should have at least 2GB RAM for each `MAKEOPT` you have. For `MAKEOPTS=j4` you should have at least 8GB RAM.

# USE flags
`USE` tells portage what global use flags to enable. If you want to enable `qt5` and `kde` globally, such as if you are a KDE user. You can enable that by setting `USE` in `make.conf`. For individual packages you should
instead customize `/etc/portage/package.use`. You can set USE flags for individual packages for example `app-editors/emacs dbus gtk gpm gui inotify libxml2 sound source ssl xft xwidgets` which sets these use flags for `emacs`.
`/etc/portage/package.use` can either be a directory or a file, by default it's a directory now. A quick way to enable use flags for a package would be `# echo "app-editors/emacs dbus gtk gpm gui inotify libxml2 sound source ssl xft xwidgets" > /etc/portage/package.use/emacs`.

# ACCEPT_LICENSE
`ACCEPT_LICENSE` tells portage which licenses to accept by default, by default I believe it accepts free software licenses, but I have it set to the old behavior `ACCEPT_LICENSE="* -@EULA"` which is accept any license, except
EULAs.

# Configuring the Gentoo repos
Create the `repos.conf` directory
```
# mkdir --parents /mnt/gentoo/etc/portage/repos.conf
```

Copy the Gentoo repository configuration file to `repos.conf`
```
# cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```

# **Chrooting**
Enter the following commands to get into the chroot environment.
```
# mount -t proc /proc /mnt/gentoo/proc
# mount -R /sys /mnt/gentoo/sys
# mount -R /dev /mnt/gentoo/dev
# chroot /mnt/gentoo /bin/bash
# source /etc/profile
# export PS1="(chroot) ${PS1}" (so we know we are in the chroot)
# echo "nameserver 1.1.1.1" > /etc/resolv.conf (we need a resolv.conf for networking as it provides DNS info, I just use the 1.1.1.1 DNS for the chroot)
```

# Syncing the portage tree
At this point you should sync the portage tree before going any further
```
# emerge-webrsync (grabs a recent snapshot of portage, might be a couple days old)
# emerge --sync (syncs portage with the latest repositories on the Gentoo servers as of the moment you run it)
```

# Selecting a profile
List profiles to see which one you want
```
# eselect profile list
Available profile symlink targets:
  [1]   default/linux/amd64/17.1 (stable) *
  [2]   default/linux/amd64/17.1/selinux (stable)
  [3]   default/linux/amd64/17.1/hardened (stable)
  [4]   default/linux/amd64/17.1/hardened/selinux (stable)
  [5]   default/linux/amd64/17.1/desktop (stable)
  [6]   default/linux/amd64/17.1/desktop/gnome (stable)
  [7]   default/linux/amd64/17.1/desktop/gnome/systemd (stable)
  [8]   default/linux/amd64/17.1/desktop/plasma (stable)
  [9]   default/linux/amd64/17.1/desktop/plasma/systemd (stable)
  [10]  default/linux/amd64/17.1/desktop/systemd (stable)
  [11]  default/linux/amd64/17.1/developer (stable)
  [12]  default/linux/amd64/17.1/no-multilib (stable)
  [13]  default/linux/amd64/17.1/no-multilib/hardened (stable)
  [14]  default/linux/amd64/17.1/no-multilib/hardened/selinux (stable)
  [15]  default/linux/amd64/17.1/no-multilib/systemd (dev)
  [16]  default/linux/amd64/17.1/systemd (stable)
  [17]  default/linux/amd64/17.0 (dev)
  [18]  default/linux/amd64/17.0/selinux (dev)
  [19]  default/linux/amd64/17.0/hardened (dev)
  [20]  default/linux/amd64/17.0/hardened/selinux (dev)
  [21]  default/linux/amd64/17.0/desktop (dev)
  [22]  default/linux/amd64/17.0/desktop/gnome (dev)
  [23]  default/linux/amd64/17.0/desktop/gnome/systemd (dev)
  [24]  default/linux/amd64/17.0/desktop/plasma (dev)
  [25]  default/linux/amd64/17.0/desktop/plasma/systemd (dev)
  [26]  default/linux/amd64/17.0/developer (dev)
  [27]  default/linux/amd64/17.0/no-multilib (dev)
  [28]  default/linux/amd64/17.0/no-multilib/hardened (dev)
  [29]  default/linux/amd64/17.0/no-multilib/hardened/selinux (dev)
  [30]  default/linux/amd64/17.0/systemd (dev)
  [31]  default/linux/amd64/17.0/x32 (dev)
  [32]  default/linux/amd64/17.0/musl (exp)
  [33]  default/linux/amd64/17.0/musl/hardened (exp)
  [34]  default/linux/amd64/17.0/musl/hardened/selinux (exp)
  [35]  default/linux/amd64/17.0/uclibc (exp)
  [36]  default/linux/amd64/17.0/uclibc/hardened (exp)
```

You can set a profile with
```
# eselect profile set 5 (setting the desktop profile, which pulls in desktop packages)
```

# Setting timezone
Now it's time to set the timezone, list available timezones in `/usr/share/zoneinfo`
```
# ls /usr/share/zoneinfo 
```

Set your timezone by putting the name in `/etc/timezone`.
```
# echo "US/Pacific" > /etc/timezone
```

Reconfigure timezone data now
```
emerge --config sys-libs/timezone-data
```

# Updating world
The tarball is usually a couple days out of date, so you will need to update world.
```
# emerge -vuaDN world
```

# Installing your favorite editor
At this point I install vim because that's the editor I like to use.
```
# emerge vim
```

# Setting the locale
Locales are the language, but also rules for your system, such as strings and date and time. These are case sensitive and must be typed correctly. You can find a list in `/usr/share/i18n/SUPPORTED`
```
# vim /etc/locale.gen
```

Example `/etc/locale.gen`
```
en_US ISO-8859-1
en_US.UTF-8 UTF-8
C.UTF8 UTF-8
```

Then generate the locales
```
# locale-gen
```

Then set the default locale
```
# eselect locale list
Available targets for the LANG variable:
  [1]   C *
  [2]   C.utf8
  [3]   en_US
  [4]   en_US.iso88591
  [5]   en_US.utf8
  [6]   POSIX
  [ ]   (free form)
# eselect locale set 5
```

Next update your environment
```
# env-update && source /etc/profile && export PS1="(chroot) ${PS1}"
```

# Kernel
You can either configure the kernel, or use a preconfigured one. I won't go over configuring the kernel because that's a more advanced thing, for now let's use a preconfigured one.
```
# emerge sys-kernel/linux-firmware (install firmwares)
# emerge sys-kernel/installkernel-gentoo
# emerge sys-kernel/gentoo-kernel-bin (installs a prebuilt kernel, omit the -bin if you want portage to compile it)
```

# Bootloader
Install a bootloader, grub2 for example. You will need to make sure the correct `GRUB_PLATFORMS` are selected.
```
# echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf (build GRUB with x86_64 UEFI support only)
# echo 'GRUB_PLATFORMS="efi-32"' >> /etc/portage/make.conf (build GRUB with i386 UEFI support only)
# echo 'GRUB_PLATFORMS="pc"' >> /etc/portage/make.conf (build GRUB with legacy support only)
# echo 'GRUB_PLATFORMS="pc efi-64"' >> /etc/portage/make.conf (build GRUB with x86_64 UEFI and legacy support)
# echo 'GRUB_PLATFORMS="pc efi-64 efi-32"' >> /etc/portage/make.conf (build GRUB with i386 UEFI, x86_64 UEFI, and legacy support)
```

Install GRUB in legacy mode
```
# grub-install /dev/sda
```

Install GRUB in UEFI mode
```
# mount -o remount,rw /sys/firmware/efi/efivars
# grub-install --target=x86_64-efi --efi-directory=/boot
```

Generate GRUB config
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

# fstab
The `fstab` is the file that tells the computer how to mount each block device, I'm gonna configure it `by-label` like so.
```
# vim /etc/fstab
LABEL=boot  /boot   vfat    defaults,noauto 0 2
LABEL=linux  /   ext4    defaults    0 1
LABEL=swap  none    swap    defaults    0 0
```

# Setting hostname
Set the hostname with
```
# echo 'hostname="phosphor"' > /etc/conf.d/hostname
```

# Configuring networking
The easiest way to do this is using networkmanager
```
# emerge networkmanager
# rc-update add NetworkManager default
```

Another way to configure the ethernet interface for example
```
# emerge dhcpcd
# vim /etc/conf.d/net
config_eth0="dhcp"
# ln -s net.lo net.eth0 
# rc-update add net.eth0 default
```

# Setting root password
Set your root password like so
```
# passwd root
```

# Finishing up
Now that Gentoo is installed you can exit the chroot environment
```
# exit
```

Reboot
```
# reboot
```

If things turn out well you are in a minimal Gentoo system, I won't cover installing the desktop environment yet and post configuration just yet, but I'll add that to the post later on.
