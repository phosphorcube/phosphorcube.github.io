---
layout:	post
title: "Dumping SHSH and restoring 32bit iOS device"
date:  2020-06-26 23:48:34 -0800
categories: apple
---

**!This was one of my quick written guides so if you have any improvements feel free to pull request them in!**
# **Introduction**
Sometimes you might be in a situation where your 32bit iPhone is bootlooping, passcode locked, or you are unable to jailbreak it. You think all hope is lost and you must restore, updating to the latest supported iOS version. That's not true. There's a way to restore the iPhone without updating to the latest version of iOS, then you get to keep the currently installed iOS version and not have to worry about it ever again as long as you keep your SHSH.

# **Tutorial**
What you need:
1. [Odysseus](/assets/32bit-ios-shsh/Odysseus.zip)
2. [synackuk's ipwndfu fork](https://github.com/synackuk/ipwndfu), I will assume you put this in your Odysseus folder
3. iOS device
4. A Mac because I can't compile the tools on Linux
5. Firmware bundle for device, probably included but maybe not

- Check what iOS version the device is running, you can achieve this by opening checkra1n or using an ssh ramdisk to cat out systemversion.plist, but you might already know this.
![checkra1n](/assets/32bit-ios-shsh/checkra1n.png)

- Create a custom ipsw using the following command, replace file name with your own ipsw for example
```
$ ./ipsw iPhone5,2_8.4.1_12H321_Restore.ipsw custom.ipsw --memory --bbupdate # only use --bbupdate if on iPhone or baseband iPad
```

- Extract iBSS from ipsw
```
$ ./xpwntool `unzip -j custom.ipsw 'Firmware/dfu/iBSS*' | awk '/inflating/{print $2}'` pwnediBSS
```

- Extract iBEC from ipsw
```
$ mv `unzip -j custom.ipsw 'Firmware/dfu/iBEC*' | awk '/inflating/{print $2}'` pwnediBEC
```

- Put iPhone into DFU mode

- Boot into pwned DFU mode
```
$ cd ipwndfu
$ ./ipwndfu -p
$ ./ipwndfu -l ../pwnediBSS
$ cd ..
```

- Load iBEC
```
./irecovery -f pwnediBEC
```

- Dump the blobs
```
$ ./irecovery -s
iRecovery> /send payload
iRecovery> go blobs
iRecovery> /exit
$ ./irecovery -g myblob.dump
$ ./irecovery -s
iRecovery> reboot
```

- Unpack and validate blobs
```
$ ./ticket myblob.dump myblob.plist matching.ipsw -z
$ ./validate myblob.plist matching.ipsw -z # If valid can rename to shsh
$ mv blob.plist ECID-Device-iOS.shsh
```
Make sure it's valid!

# **Restoring**
This is easily done with [futurerestore](https://github.com/tihmstar/futurerestore)

- Put iPhone into DFU mode

- Boot into pwned DFU mode
```
$ cd ipwndfu
$ ./ipwndfu -p
$ ./ipwndfu -l ../pwnediBSS
$ cd ..
```

- Use the following command if restoring iPhone or cellular iPad
```
./futurerestore --use-pwndfu --latest-baseband -t blob.shsh stockipsw.ipsw
```

- If on device with no baseband, iPod or wifi iPad
```
./futurestore --use-pwndfu --no-baseband -t blob.shsh stockipsw.ipsw
```

# using idevicerestore from OdysseusOTA 2 (included in my zip)
- Create a custom ipsw using the following command, replace file name with your own ipsw for example
```
$ ./ipsw iPhone5,2_8.4.1_12H321_Restore.ipsw custom.ipsw --memory --bbupdate # only use --bbupdate if on iPhone or baseband iPad
```

- Put shsh into shsh folder and name it properly
```
example: XXXXXXXXXXXX-iPhone5,1-6.0.1.shsh
```

- Put iPhone into DFU mode

- Boot into pwned DFU mode
```
$ cd ipwndfu
$ ./ipwndfu -p
$ ./ipwndfu -l ../pwnediBSS
$ cd ..
```

- Restore with the following command
```
./idevicerestore -w custom.ipsw
```