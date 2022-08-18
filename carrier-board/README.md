# Information about the carrier board

The DeskPi Super6C supports 6 Raspberry Pi or compatible CM4
form factor compute board with each having access to an OTG
micro USB plug, an NVMe M.2 SSD slot under the carrier board,
and gigabit Ethernet.

## Maxim device

31 Jul 2022 - On booting into Manjaro Arm I noticed a maxim device:-

```txt
SPI driver max3421-hcd has no spi_device_id for maxim,max3421
```

## Real Time Clock

18 Aug 2022 - I've noticed I need to remove the RTC device from my DTS file as the Super6C carrier board does not include one, so I get an error about the RTC device not being found in dmesg today.

## USB connectors and support

Only the OTG micro USB connector works OOTB using the soquartz-cm4
device definition. Unsurprising, but a little annoying. It doesn't
appear to be powered either so you need a powered OTG hub. I need to do
work to enable these carrier board USB headers.

04 Aug 2022 - So I've mostly snapped the OTG connector off the board,
as I knew I would eventually. It's waaaay too flimsy to rely on for
USB peripherals. Now redoubling my efforts to get the
[SOQuartz compute board](../soquartz/README.md)
and it's USB-host working in addition to the USB OTG connection
that comes configured OOTB. I've devised a DTS file for this, a copy
of which is in this repo for convenience until it's working and
contributed back to the Linux Kernel repo.

18 Aug 2022 - I've rebuilt the Manjaro Arm image with USB support built in. 
Not quite fully working (wont boot!) but I now have a way to 
[build and test my DTS file](../soquartz/image-build-manjaro.md) at least 
to enable future testing.

## Further information

- [DeskPi Super6C purchasing page](https://deskpi.com/collections/deskpi-super6c/products/deskpi-super6c-raspberry-pi-cm4-cluster-mini-itx-board-6-rpi-cm4-supported)
- [DeskPi Super6C wiki page](https://github.com/DeskPi-Team/super6c)
- [Jeff Geerling's DeskPi Super6C issue page](https://github.com/geerlingguy/raspberry-pi-pcie-devices/issues/336)