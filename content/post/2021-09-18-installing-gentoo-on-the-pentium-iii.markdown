---
layout: post
title: "Installing Gentoo on the Pentium III"
date:  2021-09-18 19:16:50 -0800
categories: linux
---
**This blog post targets EXPERIENCED Gentoo users only, I'm not helping you, and you are on your own! The future is uncertain, since this setup involves using old versions of some packages, and patches for others, it's uncertain whether or not it'll work tomorrow, or whether or not you'll be able to update this install in the future, so use at your own risk!**

This post is not 100% complete, but if I don't upload it now I won't ever.

# **Introduction**
As you probably know, the Pentium III was released back in 1999. It is a CPU by Intel that's been used in desktops, laptops, and more recently, in the embedded world. Discontinued back in 2007, it lacks many features of modern Intel CPUs, including SSE2, which won't cause problems for the minimal Gentoo install, however it will cause problems when we build and install Xorg, due to rust being a requirement for many packages. Rust requires SSE2 to build, and the Pentium III lacks the SSE2 instruction. Gentoo has a binary package for rust, however it too requires SSE2 as the binary was built with SSE2.

Altogether the installation took a couple days, some of that was compile times, as well as getting everything to work. It took two days to get to the point where I can start an X server.

# **Problems and work arounds**
Without SSE2 we can technically build Xorg, however things like polkit, librsvg, and many others will require SSE2. I've been putting together [an overlay](https://github.com/alicela1n/gentoo-piii) for versions of packages, as well as patches for some packages, such as polkit to get around the SSE2 requirement. I'm not planning on actively maintaining it, so if you have any issues I may or may not fix it if you open a GitHub issue, or submit a poll request.Use it at your own risk, of course.

Since rust is out of the question for now, we can mask it to ensure that it doesn't get pulled in.
```
/etc/portage/package.mask
# Rust cannot be bootstrapped on this old architecture
# Masking for now
dev-lang/rust
dev-lang/rust-bin
virtual/rust
```

Since you cannot use the desktop profile, you will need to pull in `x11-base/xorg-server` manually. Add my overlay and `echo "sys-auth/polkit duktape" > /etc/portage/package.use/polkit` and then `emerge --oneshot polkit` from it, although soon duktape support will be merged into polkit upstream, so this won't be required anymore. Then `emerge x11-base/xorg-server` to pull in Xorg.

LXDE and other desktop environments require `gnome-base/librsvg`, which upstream requires rust to build. In my overlay I have an old version of this package that works without rust, that same version is in the Gentoo repositories as of this date, however it's uncertain whether or not it'll be kept, and that's why it's in my overlay. In your `package.mask` add the following:
```
# Requires rust
>=gnome-base/librsvg-2.48
>=x11-themes/adwaita-icon-theme-40.1.1
```

# **Software**
In terms of web browsers I decided to go with Pale Moon as it's a lightweight browser, based off of Firefox 3 or 4. It can be compiled without rust and without SSE2, which is perfect for the Pentium III.

# **Conclusion**
So once all that is done, you should have a Gentoo install on the Pentium III without having rust. Is it worth it? Well it's a fun experiment, however probably not if you actually want to use this. The future is uncertain, and it might not keep working forever. Maybe I'm just a masochist. Knowing these experiments have no practical use, I just like doing them anyway.
