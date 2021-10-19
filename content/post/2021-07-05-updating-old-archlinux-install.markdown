---
layout:	post
title: "Case Study: Updating old install of Arch Linux"
date:  2021-07-05 15:12:50 -0800
categories: linux
---
![screenshot prior upgrade](/assets/upgrading-old-arch-install/Screenshot_20210702_165829.png)
# Introduction
So there's this myth that if Arch Linux gets too old then you can't update the install anymore. I don't exactly know what classifies as "too old" since I've managed to update installs up to 2 years out of date. However, I decided to go a step further and update an install that's 5 years out of date.

# Problems
The best way to avoid running into serious problems is to read the [Arch Linux news letter](https://archlinux.org/news/) as it typically tells you how to fix any problems you may run into. So it's not as straight forward as running `pacman -Syu`, I tried that and ran into keyring problems. What I tried doing was installing the new `archlinux-keyring` before doing anything else. However, [pacman couldn't recognize the new zstd format of packages](https://archlinux.org/news/now-using-zstandard-instead-of-xz-for-package-compression/) since libarchive added support in 2018. I had to use a [statically linked pacman binary](https://pkgbuild.com/~eschwartz/repo/x86_64-extracted/).
![screenshot of pacman error](/assets/upgrading-old-arch-install/Screenshot_20210702_195325.png)

# How I got Arch Linux to upgrade
So after downloading a statically linked pacman binary, I was able to run:
```
# ./pacman-static -Sy archlinux-keyring
```
Which installed the new keyring, and after that I could run
```
# ./pacman-static -Su --overwrite="*"
```
`--overwrite` is needed to overwrite any conflicting files by force.
After that it went smoothly. I had to force restart the VM because it was unable to shut down and restart properly.
![screenshot post upgrade](/assets/upgrading-old-arch-install/Screenshot_20210702_202652.png)

# Conclusion
So while it's true that you will run into problems upgrading an old install of Arch Linux, it's a myth that these problems are unfixable and you have to reinstall if your install gets too old. As I've demonstrated you can fix these problems if you reinstall the keyring (which you usually have to do if your install gets too out of date anyway) and use a static pacman binary to work around the error about unsupported package format. So don't stress too much about updating your Arch Linux install, you should be fine as long as you read the news letter and are aware of possible problems you may run into, usually they're very fixable.
