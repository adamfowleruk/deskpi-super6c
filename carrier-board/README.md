# Information about the carrier board

## Maxim device

31 Jul 2022 - On booting into Manjaro Arm I noticed a maxim device:-

```txt
SPI driver max3421-hcd has no spi_device_id for maxim,max3421
```

04 Aug 2022 - So I've mostly snapped the OTG connector off the board,
as I knew I would eventually. It's waaaay too flimsy to rely on for
USB peripherals. Now redoubling my efforts to get the
[SOQuartz compute board](../soquartz/README.md)
and it's USB-host working in addition to the USB OTG connection
that comes configured OOTB. I've devised a DTS file for this, a copy
of which is in this repo for convenience until it's working and
contributed back to the Linux Kernel repo.