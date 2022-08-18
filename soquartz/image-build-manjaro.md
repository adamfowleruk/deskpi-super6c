# Building a soquartz Manjaro Linux image for the DeskPi Super6C

You have to do this in several steps:-

1. Build the linux-quartz64 kernel using a new rk3566-soquartz-super6c.dts file I've created
2. Build the Manjaro Arm Image, specifying this kernel, using an arm profile I've created
3. Flash to image to an eMMC/SD card, and edit the boot settings
4. Boot and test the device

I shall assume for this whole piece you are building from an Intel Manjaro
installation using the manjaro arm tools. (I.e. cross compiling) Install this by running:-

```sh
sudo pacman -S manjaro-arm-tools
```

WARNING: THE BELOW DOES NOT CURRENTLY BOOT. I HAVE NOT HAD TIME TO INVESTIGATE WHY
BUT WANTED TO SHARE THE FILES AS I'M GOING AWAY FOR A WEEK. LEAVE ANY COMMENTS
ON [THIS GITHUB ISSUE](https://github.com/adamfowleruk/deskpi-super6c/issues/1).

## Creating a custom Kernel

First download the latest linux-quartz64 kernel from pgwipeout's soquartz-cm4 work:-

```sh
git clone https://gitlab.com/pgwipeout/linux-next
```

Note: I've just been told by Strit from the Manjaro Arm team that you can now use the mainline
'linux' repo from the Manjaro team rather than need pgwipeout's repo. I've not tested this yet though.
This is because all the devices and patches pgwipeout created have been upstreamed back into the Linux
Kernel.

Now grab my [dts file](./manjaro-arm/rk3566-soquartz-super6c.dts) and place it in the
`arch/arm64/boot/dts/rockchip` folder.

Now download pgwipeout's Package for the CM4:-

```sh
git clone https://gitlab.manjaro.org/manjaro-arm/packages/core/linux-quartz64.git
```

Now grab my [PKGBUILD](./manjaro-arm/PKGBUILD) file and replace the one in `./linux-quartz64` with my one.

TODO INCLUDE TAR.GZ ING THE linux-next FOLDER, CALCULATING A SHA256SUM FOR IT, AND REPLACING THE SOURCE LINUX IN THE PKGBUILD.

Now you can build this package:-

```sh
sudo buildarmpkg -p linux-quartz64
```

TODO OVERWRITE THE CORRECT LINUX NEXT FILES ABOVE

You now have a custom linux kernel package in `/var/cache/manjaro-arm-tools/pkg`

## Create a custom Manjaro Arm image

I like xfce but you can use any edition below. I recommend running 'minimal' first to ensure the build works successfully before building a more complex edition like xfce.

First download the latest arm profiles

```sh
sudo getarmprofiles -f
```

This places all the arm device profiles in your `/usr/share/manjaro-arm-tools/profiles/arm-profiles/devices` folder.

Grab my [soquartz-super6c profile](./manjaro-arm/soquartz-super6c) and copy it to this folder. (You will have to do this as sudo)

Note that I've included i2c-tools and usbtools for extra debugging for convenience so I can investigate this board's hardware easier. I've also commented out 'linux' as we've built our own custom linux package.

There an issue with the current build system. To avoid hitting an artificial dist space limit, edit the following file:-

```sh
sudo vi /usr/share/manjaro-arm-tools/lib/functions.sh
```

Find the `create_img()` section and change `EXTRA_SIZE` from `800` to `1200`. Save the file.

Now build your custom ISO image using this command:-

```sh
sudo su -
buildarmimg -d soquartz-super6c -e xfce -i /var/cache/manjaro-arm-tools/pkg/aarch64/linux-quartz64-5.19.0-0.4-aarch64.pkg.tar.zst
```

This should take around 7-15 minutes depending on bandwidth. The final file will be in this folder:-

```sh
ls -la /var/cache/manjaro-arm-tools/img/Manjaro-ARM-xfce-soquartz-super6c-22.08.img.xz
```

Note: Your exact file name will depend upon when you ran the build (It's the current month and year).

## Flashing to a boot image

You can use dd or Etcher to flash the resultant image to an eMMC module or SD card. You'll need at least an 8GB card (It's over 4GB as an image).

You're not done yet though!!! Because I'm using pgwipeout's repos above rather than my own
the `extlinux.conf` file in the `/boot` partition on the drive you've just flashed is still using
his `rk3566-soquartz-cm4.dtb` file rather than the new one I've created.

To modify this, plug in your SD card or eMMC module (via an adapter) to a Mac, Windows or Linux
machine and edit the `BOOT_MJRO` drive's `/extlinux/extlinux.conf` file, and change
`rk3566-soquartz-cm4.dtb` to `rk3566-soquartz-super6c.dtb` and save the file.

Now safely disconnect the drive.

## Using the image

You should now be able to boot from a SOQuartz CM4 module in the first #1 bay of the DeskPi Super6C carrier board.

WARNING: AS MENTIONED AT THE TOP OF THIS FILE, THIS CURRENTLY DOES NOT WORK. SEE THAT COMMENT ABOVE.

## Further information

Here are a few useful links:-

- [How to Contribute to Manjaro Arm (and use the buildarm* tools)](https://forum.manjaro.org/t/wiki-how-to-contribute-to-manjaro-arm/35461)
- [Detailed Manjaro Arm Tools information](https://gitlab.manjaro.org/manjaro-arm/applications/manjaro-arm-tools/)