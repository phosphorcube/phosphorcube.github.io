---
layout:	post
title: "Linux troubles again..."
date:  2021-03-21 20:59:50 -0800
categories: linux
---
Linux troubles again, this time it's my audio that's not working. I didn't even realize it wasn't working for a while. Then it randomly hit me, no audio. Obviously, this is not enough to convince me to switch to Windows, as I'm not a fan of that operating system. However it is really annoying so I'm gonna have to try and fix it.

# Diagnosing the problem
Let's see what shows up in `/proc/asound/card0`
```
[root@magenta-matrix ~]# ls -lah /proc/asound/card0
total 0
dr-xr-xr-x 21 root root 0 Mar 22 21:02 .
dr-xr-xr-x 13 root root 0 Mar 22 21:02 ..
-r--r--r--  1 root root 0 Mar 22 21:02 codec#0
-r--r--r--  1 root root 0 Mar 22 21:02 codec#2
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.0
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.1
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.2
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.3
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.4
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.5
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.6
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.7
-rw-r--r--  1 root root 0 Mar 22 21:02 eld#2.8
-r--r--r--  1 root root 0 Mar 22 21:02 id
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm0c
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm0p
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm10p
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm3p
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm7p
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm8p
dr-xr-xr-x  4 root root 0 Mar 22 21:02 pcm9p
[root@magenta-matrix ~]#
```

Looks pretty typical, I noticed that I didn't have alsa-utils installed, time to install it. After installing it I ran `aplay -l`.
```
[root@magenta-matrix ~]# aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: PCH [HDA Intel PCH], device 0: ALC257 Analog [ALC257 Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 9: HDMI 3 [HDMI 3]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 10: HDMI 4 [HDMI 4]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
[root@magenta-matrix ~]#
```

Again, pretty typical. Let's run `alsa-mixer`.
```
[root@magenta-matrix ~]# alsamixer
cannot open mixer: Host is down
[root@magenta-matrix ~]#
```
Oh?

Let's check the kernel logs!
```
[    8.365403] snd_hda_intel 0000:00:1f.3: DSP detected with PCI class/subclass/prog-if info 0x040380
[    8.365438] snd_hda_intel 0000:00:1f.3: enabling device (0000 -> 0002)
[    8.365616] snd_hda_intel 0000:00:1f.3: bound 0000:00:02.0 (ops i915_audio_component_bind_ops [i915])
[    8.403612] input: Wacom HID 51CD Pen as /devices/pci0000:00/0000:00:15.1/i2c_designware.1/i2c-7/i2c-WCOM51CD:00/0018:056A:51CD.0003/input/input16
[    8.403721] input: Wacom HID 51CD Finger as /devices/pci0000:00/0000:00:15.1/i2c_designware.1/i2c-7/i2c-WCOM51CD:00/0018:056A:51CD.0003/input/input17
[    8.403907] wacom 0018:056A:51CD.0003: hidraw1: I2C HID v1.00 Mouse [WCOM51CD:00 056A:51CD] on i2c-WCOM51CD:00
[    8.417150] snd_hda_codec_realtek hdaudioC0D0: autoconfig for ALC257: line_outs=1 (0x14/0x0/0x0/0x0/0x0) type:speaker
[    8.417155] snd_hda_codec_realtek hdaudioC0D0:    speaker_outs=0 (0x0/0x0/0x0/0x0/0x0)
[    8.417157] snd_hda_codec_realtek hdaudioC0D0:    hp_outs=1 (0x21/0x0/0x0/0x0/0x0)
[    8.417159] snd_hda_codec_realtek hdaudioC0D0:    mono: mono_out=0x0
[    8.417160] snd_hda_codec_realtek hdaudioC0D0:    inputs:
[    8.417161] snd_hda_codec_realtek hdaudioC0D0:      Mic=0x19
[    8.417162] snd_hda_codec_realtek hdaudioC0D0:      Internal Mic=0x12
[    8.419652] ieee80211 phy0: Selected rate control algorithm 'iwl-mvm-rs'
[    8.419997] thermal thermal_zone7: failed to read out thermal zone (-61)
[    8.421603] iwlwifi 0000:00:14.3 wlp0s20f3: renamed from wlan0
[    8.480274] input: HDA Intel PCH Mic as /devices/pci0000:00/0000:00:1f.3/sound/card0/input19
[    8.480304] input: HDA Intel PCH Headphone as /devices/pci0000:00/0000:00:1f.3/sound/card0/input20
[    8.480330] input: HDA Intel PCH HDMI/DP,pcm=3 as /devices/pci0000:00/0000:00:1f.3/sound/card0/input21
[    8.480354] input: HDA Intel PCH HDMI/DP,pcm=7 as /devices/pci0000:00/0000:00:1f.3/sound/card0/input22
[    8.480375] input: HDA Intel PCH HDMI/DP,pcm=8 as /devices/pci0000:00/0000:00:1f.3/sound/card0/input23
[    8.480396] input: HDA Intel PCH HDMI/DP,pcm=9 as /devices/pci0000:00/0000:00:1f.3/sound/card0/input24
[    8.480418] input: HDA Intel PCH HDMI/DP,pcm=10 as /devices/pci0000:00/0000:00:1f.3/sound/card0/input25
```
Looks normal? I don't see why it's not working.

Maybe a kernel update? Oh nope... :/
I could try building the latest kernel from source but I don't want to.

I didn't even bother trying `pactl info`
```
alice :: magenta-matrix [~] % pactl info
Server String: /run/user/1000/pulse/native
Library Protocol Version: 34
Server Protocol Version: 34
Is Local: yes
Client Index: 34
Tile Size: 65472
User Name: alice
Host Name: magenta-matrix
Server Name: PulseAudio (on PipeWire 0.3.24)
Server Version: 14.0.0
Default Sample Specification: float32le 2ch 48000Hz
Default Channel Map: front-left,front-right
Default Sink: @DEFAULT_SINK@
Default Source: @DEFAULT_SOURCE@
Cookie: 76cb:e022
alice :: magenta-matrix [~] %
```
Oh nothing looks out of the ordinary, so it looks normal again. Yet pavucontrol shows nothing!
![screenshot](/assets/linux-troubles-again/Screenshot_20210322_211309.png)

Hmm, maybe I could try reinstalling pipewire with `sudo pacman -S pipewire-alsa pipewire-pulse pipewire-jack pipewire` and rebooting? Sadly nope, doesn't work. :/
Last thing I'll try is falling back to the `linux-lts` kernel package. Unfortunately tho, that did not fix it, leading me to believe it's not a kernel problem after all.

# Pipewire
Pipewire is a new thing, replacement for pulseaudio. It's great when it works, but it might be problematic since it's really new. Maybe I should try switching back to pulseaudio!
My dependency tree is an absolute nightmare!
```
alice :: magenta-matrix [~] % sudo pacman -R pipewire pipewire-pulse pipewire-jack pipewire-alsa
checking dependencies...
error: failed to prepare transaction (could not satisfy dependencies)
:: removing pipewire-pulse breaks dependency 'pulseaudio-bluetooth' required by gnome-bluetooth
:: removing pipewire-pulse breaks dependency 'pulseaudio' required by gnome-settings-daemon
:: removing pipewire-alsa breaks dependency 'pulseaudio-alsa' required by gnome-settings-daemon
:: removing pipewire breaks dependency 'libpipewire-0.3.so=0-64' required by gst-plugin-pipewire
:: removing pipewire breaks dependency 'pipewire' required by krfb
:: removing pipewire breaks dependency 'pipewire' required by kwin
:: removing pipewire breaks dependency 'pipewire' required by mutter
:: removing pipewire-pulse breaks dependency 'pulseaudio' required by pavucontrol-qt
:: removing pipewire breaks dependency 'pipewire' required by pipewire-media-session
:: removing pipewire breaks dependency 'libpipewire-0.3.so=0-64' required by pipewire-media-session
:: removing pipewire-pulse breaks dependency 'pulseaudio' required by plasma-pa
:: removing pipewire breaks dependency 'pipewire' required by xdg-desktop-portal
alice :: magenta-matrix [~] %
```
I have plenty of btrfs snapshots so might as well live on the edge and do `sudo pacman -Rdd pipewire pipewire-pulse pipewire-jack pipewire-alsa`!

Installing pulseaudio:
```
$ sudo pacman -S pulseaudio pulseaudio-bluetooth pulseaudio-alsa pulseaudio-jack pulseaudio-zeroconf
```

Let's see if audio is fixed!
I'm back after rebooting typing this in vim with no graphical interface, because removing pipewire broke plasma! Using `nmtui` to connect to the internet!
`pacman -Syu $(pacman -Qqn) --overwrite="*"` to force reinstall all packages.
Reboot, and audio works! Pipewire was the problem!

# Conclusion
In conclusion pipewire was the problem, an update to pipewire broke audio! Maybe I should file a bug report, maybe there's already a bug report, I'll have to see, but for now I'm back on pulseaudio!
