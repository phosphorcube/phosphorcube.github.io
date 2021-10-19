---
layout:	post
title: "Tegra 3 unbricking notes"
date:  2021-02-05 15:09:34 -0800
categories: misc
---

This is just a quick little post to keep notes for how I unbricked my ASUS MemoPad ME301T.

# Tegra 3 unbricking
Using [tegra 3 unbrick](https://github.com/tofurky/tegra30_debrick) I managed to unbrick the MemoPad.

# Enter APX mode
Different depending on device.

# Pwning the bootrom
```
# ./fusee-launcher/fusee-launcher.py ./payload/uart_payload_n7.bin -P 7330
```

# Is your BCT Good?
If you don't have a Nexus 7 or Ouya you'll need a BCT. You can dump it with:
```
# ./utils/nvflash_v1.13.87205 --getbct --bct BCT_READBACK_N7.BIN --configfile ./utils/flash.cfg
```

Check it with:
```
# bct_edit BCT_READBACK_N7.bin
```

If good, make sure you have an EBT image, if you don't you're probably screwed

If bad, make sure you have a BCT and EBT image, if not you're probably screwed

# If you have both, boot into nv3pserver mode:
```
# /utils/nvflash_v1.13.87205_miniloader_patched --setbct --bct $BCT --configfile ./utils/flash.cfg --bl $EBT--go
```

Reflash to eMMC with:
```
# ./utils/nvflash_v1.13.87205_miniloader_patched --resume --download EBT $EBT --configfile ./utils/flash.cfg
```

Hopefully it works!