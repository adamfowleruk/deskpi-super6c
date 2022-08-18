# Pine64 SOQuartz CM4 compute board

RaspberryPi Compute Module 4 (CM4) boards are very hard to come by currently
due to the global component shortage in 2022. I looked for CM4 compatible
alternatives and found this Pine64 board. I have a PinePhone and PinePhone Pro
already so fealt comfortable ordering this board to try it out with the
DeskPi Super6C.

## Quick Links

- [Building a custom Manjaro Arm image for the soquartz on the super6c](./image-build-manjaro.md)

## Initial set up

Since July 2022 Manjaro Arm has shipped it's arm64 LiveCD image with built in support
for the RockChip used on the SOQuartz module. Flashing this to the detachable eMMC chip
resulted in a successful boot first time. (SD Card boot is also supported via the Super6C board).

The big issue with initial set up with the combination of the SOQuartz and the Super6C is that
neither USB interface works out of the box. The DWC2 interface exposed via a set of pins for
the front panel USB ports of a mini-itx case needs, well, a mini-itx case. If using Raspbian,
you also need to edit the boot file.

Alternatively, using the Super6C's 2 USB ports via the XHCI driver should work out of the box.
Due to a one line
[kernel configuration issue](https://lore.kernel.org/all/CAMdYzYqRcrawKc-GsTgwnPuJBJoKTn9Arfpj_Vjnt+3DeT7k9w@mail.gmail.com/T/) 
for the RockChip in Linux though, this interface is
in device mode (like a keyboard would be) rather than host mode (like a computer should be)!

23 Jul 2022 AM - The DTS/DTB file rebuild didn't help. Likely due to feedback from 
[Peter Gies aka pgwipeout](https://github.com/geerlingguy/raspberry-pi-pcie-devices/issues/336#issuecomment-1191700612)
that mentions I should create a new DTS file for the combination of this SOQuartz-CM4 board and
the DeskPi Super6C carrier board. I will do that eventually - the DeskPi Super6C has a USB hub
I'll need to support too. In the meantime I've ordered an OTG USB hub for the other
available micro USB connector. I'll need this eventually anyway if interacting with any other
board, as the Super6C has a micro USB OTG connector for each CM4 board.

23 Jul 2022 PM - The OTG USB cable worked BUT it MUST be powered using the external power micro usb cable.
Even a basic keyboard and mouse draws power. I've now installed Manjaro Arm and can run basic commands.
There are 
[a few oddities I've reported on the Manjaro Arm Forum](https://forum.manjaro.org/t/manjaro-arm-on-the-pine64-soquartz-cm4-with-deskpi-super6c-carrier-board/117445)
like software updates not running out of the box, and the first additional package install
hosing the whole system!... So a few things to work out...
But Manjaro Arm boots, installs, sees my PCIe bus and the Samsung NVMe SSD, and has modules
loaded for most devices except the Super6C's USB hub - but I'd expected that with the above
issues. Time for a reflash of the eMMC for attempt 3, and prepping my Manjaro Intel laptop
with an extra M.2 drive so I can grab all linux and Manjaro Arm source code and recompile
to my hearts content.

## Installing a boot OS to the NVMe SSD

I have a Samsung Evo980 SSD spare, so I decided to add this for the SOQuartz node. I'm hoping
to use this drive as the boot device, then remove the eMMC drive entirely. Not sure if this
will work so watch this space...

23 Jul 2022 PM - I'd forgotten I'd used this drive for a Ceph content drive in another box, so
I'll need to reformat and reinitialise the drive during a fresh install. The NVMe drive is
detected successfully though, and lsblk shows its partitions.

31 Jul 2022 - Researched how to boot from NVMe. Flashed the latest Manjaro Arm (25 July) to
the eMMC drive but for some reason it's not even booting now. I've plugged in a new empty
NVMe drive, also a new basic USB keyboard, and a new OS version. My fault for changing more
than one thing at a time I guess... 

Changing keyboard had no effect (no surprise!), removing
the NVMe drive had no effect, Reseated the soquartz board and eMMC had no effect. Flashed
the old 18th July Manjaro Arm to the eMMC and ... It worked again. Clearly the 25 Jul 2022
build is broken. I've reported that 
[on the Manjaro Arm forum](https://forum.manjaro.org/t/manjaro-arm-on-the-pine64-soquartz-cm4-with-deskpi-super6c-carrier-board/117445/5?u=adamfowleruk)
too.

I've confirmed that the U-Boot bootloader used by Manjaro Arm does not yet support the
PCIe driver for the RockChip and instead uses a generic PCIe driver. This means U-Boot
currently only supports booting from the SD Card, eMMC, or NetBoot (Ethernet). So I've
found an old SD Card kicking around and have flashed the boot drive to that. Similarly,
I've flashed the Manjaro image to the NVMe SSD. Interestingly, this uses the same
ROOTUUID and so will boot from the eMMC card but use the NVMe's root (i.e. / ) mount point.

When booting from the SD card though it doesn't even recognise the NVMe drive as a device.
There's a dmesg error there. Perhaps it didn't shut down properly or something...

But I've got a couple hundred MB of wasted space on the NVMe, but I can boot using the eMMC
card for bootloader only, and the NVMe drive for the system drive. So that's the basics working.

I've ordered a USB NVMe adapter so I can flash the other NVMe drives outside of the Super6C rig.
Hopefully I'll be able to use this new information to automate the provisioning of NVMe
drives for these rigs.

TODO Repair NVMe partition. Boot from SD card to NVMe drive for root partition instead of eMMC.

## Creating a dedicated DTS file for the SOQuartz-CM4 and DeskPi-Super6C combination

31 Jul 2022 - I've started to look at this and have duplicated the soquartz-cm4 dts as the basis.
I've also found in dmesg a bunch of interesting messages about devices seen but not loaded,
and attempted to load but failed. Not much and the system seems to be generally the same.
The DeskPi's Super6C's USB Hub is correctly and dynamically discovered, so the two DTS are
the same right now. As soon as I find a piece of hardware configured differently though I'll
modify the DTS file. I suspect the calendar chip will be one area of difference, as will an
as yet unidentified Maxim chip, which I suspect is a Serial I/O controller on the DeskPi
Super6c.

05 Aug 2022 - I've created a DTS file and made some changes. In addition to enabling the
USB2 OTG connector as pgwipeout has done in his DTS, I've also enabled the USB2 Host
support on the same USB2 PHY, and enabled the usb_host1_xhci support. I'm recompiling
the linux Kernel now for Manjaro and will post the DTS file, PKGBUILD, and any patches
on this repo once done, along with a custom build guide (which is severely lacking,
by the way, for people trying to add new carrier boards to Arm).

18 Aug 2022 - The DTS file has been created and I've managed to build and flash a 
[custom Manjaro Arm image](./image-build-manjaro.md). It doesn't boot yet, but it's
solid progress. My files are shared in the above link.