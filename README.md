# archinstall-mbp

A profile for [python-archinstall](https://github.com/archlinux/archinstall), that installs drivers and packages for T2 Macs.

Currently you must choose systemd-boot as your bootloader, grub will not work. See [#3](https://github.com/Redecorating/archinstall-mbp/issues/3)

## Usage
1. If you are not on a MacBookPro15,4 or MacBookPro16,3 (13-inch, Two Thunderbolt 3 ports), 
   in MacOS, run `ioreg -l | grep RequestedFiles`. Make sure you can refer to the
   output of this command while your Mac is booted into the Arch Install ISO.
3. Use a T2 Mac specific ISO from [here](https://dl.t2linux.org/archlinux/iso/index.html).
4. Boot the install ISO, and connect to internet.
5. Run this:
```shell
wget https://bit.ly/3amlr9v -O apple-t2.py
sh apple-t2.py
python -m archinstall
```
5. Enable Bluetooth with `systemctl enable bluetooth` if you want it.
6. If you are on a MacBookPro16,1/2/4, do this as the
   automation of it is broken.
   
   ```
   pacman -U /usr/local/src/t2linux/mbp-16.1-linux-wifi-*.pkg.tar.zst
   ```

At the profiles section, you **need** to select the "apple-t2" profile. The
apple-t2 profile will ask you if you want a second profile.

## What does it install?

Installs Archinstall to the live session if it's missing (as the latest
MBP ISO was made before archinstall was included)

Patched 'linux-mbp' kernel

DKMS 'apple-bce' driver (keyboard, trackpad, audio) 

DKMS 'apple-ibridge' driver (touchbar)

ALSA card profile for the T2 audio device (including alternate ones for
16 inch MacBooks, which have 6 speakers)

t2linux repo for updates to kernel

Installs WiFi firmware, walks you through selection

On MacBookPro16,1/2/4 and MacBookAir9,1 models, installs a kernel with an alternate
WiFi patch that was made for M1 Macs that works on those models.

NVRAM remount as read only because T2 likes to panic

Unload Touchbar driver before suspend to fix resume

Makes linux-mbp kernel the default kernel to boot with, or the alternate wifi kernel
if it was installed.

Installs `iwd` and sets it as NetworkManager's WiFi backend, if needed
installs iwd 1.13.

TODO:
- install mbpfan
- configure apple-ib-tb and apple-bce options
- hybrid graphics?
- only show top level profiles as chainload options
- make into a plugin when they are implemented into archinstall.
- get github ci to test it
- get github ci to make the packages
