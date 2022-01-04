---
layout: post
title: "The Paradox Of Backwards Compatibility"
date:  2022-01-03 15:55:34 -0800
categories: misc
---

# Backwards compatibility is a mess
Backwards compatibility is a mess. There's not any other way to put it. You can either have an operating system that's
backwards compatible to a point where it's inflexible in trying to make any changes because those changes could break
compatibility with older software. You can have an operating system that is backwards compatible to a certain point,
but after that point older software will no longer be supported, or you can have an operating system that isn't
backwards compatible at all, in which all software has to be updated to support the changes made to each release of
the operating system. One minor change in an ABI (application binary interface) can be enough to break a lot of software,
sometimes this software will have to be recompiled against the new ABI, or parts of the code will need to be changed or even
rewritten entirely. Take Linux for example, often times libraries change requiring software to be recompiled against the new
libraries. While the libc itself probably hasn't changed, one library changing could be enough to break compatibility with
that binary. Windows has a selling point of being backwards compatible, and Microsoft really tries to not break compatibility
with older software, with that said the system libraries often times have to account for this backwards compatibility, or you
need to install multiple versions of a library. Look at macOS, it's somewhere in the middle in terms of backwards compatibility,
software written for an old version of macOS might work on the next release, but it's often times hit or miss. Too much backwards
compatibility hurts progress, and means that the operating system can't make improvements that could potentially break older software,
too little backwards compatibility is also bad, it's an annoyance to both developers and end users to constantly have their software
broken with each and every change to the operating system, there needs to be a middle ground where an operating system can make
improvements, while still keeping some backwards compatibility.

# Some solutions
There's a few solutions to this problem, one is virtualizing an older version of the operating system to run older software that no
longer works. However this is annoying, cumbersome, and uses a lot of resources. Another solution includes shipping compatibility
layers, either included with the operating system or as optional. One solution that flatpak uses is shipping multiple versions of
libraries and having them pulled in as dependencies for the application, so when you install it you get the correct libraries.
However it's done, there's still ways to achieve backwards compatibility. While not always pretty, they do work. Backwards compatibility
is a paradox, there is no way to achieve 100% backwards compatibility.
