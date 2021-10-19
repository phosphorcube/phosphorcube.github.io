---
layout:	post
title: "Running Mac OS X 10.2 in QEMU"
date:  2020-03-21 13:14:30 -0800
categories: apple
---
# **Introduction**
If you remember my old blog (it no longer exists btw), I had a post where I ran OS X 10.2 in QEMU. That got popular so I decided to rewrite it in my jekyll blog.

# **Tutorial**
First off this is what you need:
 1. [OS X 10.2 isos](https://winworldpc.com/product/mac-os-x/102)
 2. QEMU
 3. A computer with a reasonable CPU and enough RAM
   * (recommended to have at least a Core 2 Duo and 4GB RAM)
 4. Command line knowledge
 5. Patience
   
Now we can get started!
1. Create a blank hard drive image to install OS X to:
  ```
  $ qemu-img create -f qcow2 102.img 10G
  ```
  * This will make a 10GB qcow2 file named 102.img, feel free to adjust the command.
2. Put your OS X 10.2 iso files in the same folder as everything else, this is self explanatory.
3. Start QEMU with the following command (adjust to your needs)
  ```
  $ qemu-system-ppc -L pc-bios -M mac99 -m 2048 -cdrom disk1.iso -hda 102.img -boot d
  ```
4. Open Disk Utility and format the drive:
  ![Disk Utility](/assets/osx-ppc-qemu/Screenshot_20200320_212618.png)
5. Restart QEMU (I know it sounds weird but just do it to avoid this)
  ![Installation Error](/assets/osx-ppc-qemu/Screenshot_20200320_212817.png)
6. Continue install like normal, I decided to install everything
  ![Install](/assets/osx-ppc-qemu/Screenshot_20200320_212725.png)
7. When the install finishes close QEMU again
   ![Successful install](/assets/osx-ppc-qemu/Screenshot_20200320_214408.png)

8. Switch to disk 2 and switch boot drive to C using the following command
  ```
  $ qemu-system-ppc -L pc-bios -M mac99 -m 2048 -cdrom disk2.iso -hda 102.img -boot c
  ```
9. Boot back up and wait for the install to finish
  ![Second stage](/assets/osx-ppc-qemu/Screenshot_20200320_221232.png)
10. Setup OS X like normal and press Command+Q at the registration step
  ![Registration](/assets/osx-ppc-qemu/Screenshot_20200320_221920.png)

When you get to the desktop you should have successfully installed OS X 10.2!
  ![Desktop](/assets/osx-ppc-qemu/Screenshot_20200320_222111.png)

Now you can change the resolution and have fun!
  ![Resolution change](/assets/osx-ppc-qemu/Screenshot_20200320_222457.png)

# **Why do this?**
Well...if you want to have some fun with older versions of OS X, or you want to see how things used to be back then. Maybe you have older unsupported software. Or any other reasons that I don't know about

# **Issues?**
It seems to work pretty well aside from a few issues.
* No hardware acceleration
* No audio
* Slower performance

I'm not sure if audio could be fixed but the other two certainly can't.
