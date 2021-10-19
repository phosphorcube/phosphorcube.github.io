---
layout:	post
title: "AARCH64 Arch Linux ARM on Raspberry Pi 4"
date:  2020-04-10 12:16:30 -0800
categories: linux
---
# **Introduction**
So you may know about the Raspberry Pi, a single board computer designed for embedded applications and DIY projects. You may also know that some people use them as desktop computers. However, one of the biggest problems when it comes to the Pi 4 is poor 64bit support. There is little to none in mainline and the downstream kernel is a bit old. We won't be fixing that problem but we will be running Arch Linux on the Pi 4 in 64bit mode, be aware that we are using an older kernel and it's not the latest mainline support. Also beware that I am not responsible for any mistakes you make.

# **Tutorial**
First off this is what you need:
1. Raspberry Pi 4
2. SD card of at least a couple GB, mine is 16GB but 8GB and above should work
3. [Arch Linux ARM generic arm64 image tar](http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz)
4. [Manjaro ARM 64bit image for Raspberry Pi 4](https://osdn.net/projects/manjaro-arm/storage/rpi4/xfce/)
5. Linux

Now we can get started!
- Partition your SD card with the following layout (can be done using gparted or your preferred tool):

  ```
  [root@MeltBox code]# fdisk -l /dev/mmcblk0
  Disk /dev/mmcblk0: 14.86 GiB, 15931539456 bytes, 31116288 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: dos
  Disk identifier: 0x6c586e13

  Device         Boot  Start      End  Sectors  Size Id Type
  /dev/mmcblk0p1        2048   206847   204800  100M  b W95 FAT32
  /dev/mmcblk0p2      206848 31115263 30908416 14.8G 83 Linux
  [root@MeltBox code]#
  ```

- Create filesystems on the SD card 

  ```
  # mkfs.ext4 -l ROOT /dev/mmcblk0p2
  # mkfs.vfat -F 32 -n BOOT /dev/mmcblk0p1
  ```

- Mount the filesystems somewhere
   
  ```
  # mkdir -p /mnt/sdcard/root
  # mkdir -p /mnt/sdcard/boot
  # mount /dev/mmcblk0p2 /mnt/sdcard/root
  # mount /dev/mmcblk0p1 /mnt/sdcard/boot
  ```

- Extract the Arch Linux ARM root filesystem to the SD card
  
  ```
  # bsdtar -xpf ArchLinuxARM-aarch64-latest.tar.gz -C /mnt/sdcard/root
  ```

- Mount the Manjaro image as a loopback device
  
  ```
  # losetup -fP Manjaro-ARM-xfce-rpi4-20.02.img
  # mkdir -p /mnt/loop/root
  # mkdir -p /mnt/loop/boot
  # mount /dev/loop0p2 /mnt/loop/root
  # mount /dev/loop0p1 /mnt/loop/boot
  ```

- Copy the Manjaro boot files to SD card
  
  ```
  # cp -R /mnt/loop/boot/* /mnt/sdcard/boot/
  ```

- Copy some firmware files and modules over to SD card
  
  ```
  # cp -R /mnt/loop/root/usr/lib/modules/* /mnt/sdcard/root/usr/lib/modules/
  # cp -R /mnt/loop/root/usr/lib/firmware/* /mnt/sdcard/root/usr/lib/firmware/
  ```

- Configure fstab and add the following entries
  
  ```
  LABEL=ROOT  /    ext4   rw,relatime   0 1
  LABEL=BOOT  /boot   vfat    rw,relatime 0 2
  ```

- Unmount everything
  
  ```
  # umount /dev/loop0p1
  # umount /dev/loop0p2
  # losetup -v -d /dev/loop0
  # rm -r /mnt/loop
  # umount /dev/mmcblk0p1
  # umount /dev/mmcblk0p2
  # rm -r /mnt/sdcard
  ```

- Boot it and hope it works
- Login as ``alarm, password: alarm``
- Connect to ethernet because right now Arch doesn't have the installed wifi packages
- Use su to login as root, ``password: root``
- Initialize pacman keys and update packages
  
  ```
  # pacman-key --init
  # pacman-key --populate archlinuxarm
  # pacman -Syu
  ```

- Install wpa_supplicant and dialog
  
  ```
  # pacman -S wpa_supplicant dialog
  ```

- Install the rest of the packages you need and enjoy

# **Notes**
* This is NOT mainline, it's the downstream RPF based kernel included in Manjaro ARM
* This may NOT fully work, I haven't tested everything
* I am NOT responsible for any mistakes you make
* This was done for fun and out of curiosity

# **Extras**
Building your own kernel to enable kvm and stuff (will take a long time):
- Download Raspberry Pi source tree

  ```
  $ git clone --depth 1 https://github.com/raspberrypi/linux.git
  ```

- Install ``base-devel`` and ``bc``
   
  ```
  $ pacman -S base-devel bc
  ```

- cd into kernel source    
  
  ```
  $ cd linux
  ```

- Export Arch and Subarch
 
  ```
  $ ARCH=arm64
  $ SUBARCH=arm
  ```

- Export kernel

  ```
  $ KERNEL=kernel8
  ```

- Make the defconfig
   
  ```
  $ make bcm2711_defconfig
  ```

- Enable the options you want

  ```
  $ make menuconfig
  ```

- Build the kernel
   
  ```
  $ make -j4
  ```

- Install the kernel 
  
  ```
  # make modules_install
  # cp arch/arm64/boot/dts/*.dtb /boot/
  # cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
  # cp arch/arm64/boot/dts/overlays/README /boot/overlays/
  # cp arch/arm64/boot/Image /boot/kernel
  # sed s/kernel8.img/kernel/g /boot/config.txt
  ```
  
Hopefully things like KVM work now!