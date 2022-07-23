# Pine64 SOQuartz CM4 compute board

RaspberryPi Compute Module 4 (CM4) boards are very hard to come by currently
due to the global component shortage in 2022. I looked for CM4 compatible
alternatives and found this Pine64 board. I have a PinePhone and PinePhone Pro
already so fealt comfortable ordering this board to try it out with the
DeskPi Super6C.

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

23 Jul 2022 - The DTS/DTB file rebuild didn't help. Likely due to feedback from 
[Peter Gies aka pgwipeout](https://github.com/geerlingguy/raspberry-pi-pcie-devices/issues/336#issuecomment-1191700612)
that mentions I should create a new DTS file for the combination of this SOQuartz-CM4 board and
the DeskPi Super6C carrier board. I will do that eventually - the DeskPi Super6C has a USB hub
I'll need to support too. In the meantime I've ordered an OTG USB hub for the other
available micro USB connector. I'll need this eventually anyway if interacting with any other
board, as the Super6C has a micro USB OTG connector for each CM4 board.

TODO complete initial OS install

## Installing a boot OS to the NVMe SSD

I have a Samsung Evo980 SSD spare, so I decided to add this for the SOQuartz node. I'm hoping
to use this drive as the boot device, then remove the eMMC drive entirely. Not sure if this
will work so watch this space...

TODO Complete testing of SSD boot...

## Creating a dedicated DTS file for the SOQuartz-CM4 and DeskPi-Super6C combination

TODO create the new DTS and customise for the DeskPi super6C hardware, test, contribute back
to the Linux kernel and let the Manjaro ARM team know.
