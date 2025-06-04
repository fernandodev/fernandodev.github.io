---
layout: post
title: "Flashing linxdot helium miner"
date: 2025-06-04 21:00:13 +0200
categories: crypto
tags: crypto helium linxdot
keywords: crypto helium linxdot flash
---

![](https://raw.githubusercontent.com/fernandodev/linxdot-rockchip-flash/refs/heads/main/linxdot.png)

Impressive how certain projects just vanishes from day to nigh in the crypto world. Linxdot was a miner that simply stopped working. Not that it was giving any meaningfull reward, but hey! Why just dying from nothing?

I thought it was a Wifi issue and then after a very long time I entered the discord server of Linxdot. What a mess! Completely abandoned and I wonder why they still sell these devices, they don't even work anymore.

Anyway, I had this paper weight laying around and I needed to ressurect it. I then saw I guy talking about the [Crankk.io](https://crankk.io) project and it seemed it can not only mine Helium but also do some other mining activities.

But.... I needed to flash this piece of plastic and here we are after weeks researching (cause all tutorial were for Windows).

<!-- more -->

## Getting Started

```sh
brew install automake autoconf libusb lsusb pkg-config
git clone https://github.com/rockchip-linux/rkdeveloptool
cd rkdeveloptool
autoreconf -i
./configure
make
```

It is possible that during `./configure` phase you face some issue related to pointers or buffer overflow. Adjust the configuration accordingly. (usually ignore the warnings, because default treat warnings as errors)

* My current hardware is a M1 Pro.

## Boot loader

The file x.bin is the rockchip bootloader of this device, you will need it to be able to boot correctly your device.

## Flashing

This step is the trickiest one, follow accordingly!

1. Connect USB-C to the miner and on your laptop
2. Connect ethernet cable (yes, it cannot be just wifi)
3. Now hold the BT pair little button
4. While holding the BT pair button, connect the power plug, keep holding the button
5. Hold for more 8s or so

The LED lights are now 1 green and 1 red. It indicates that you are now on flash mode.

## Flashing

Check if the flash worked by checking the devices connected to the usb (sometimes you need to restart your laptop after installing `libusb` and `lsusb`)

```sh
lsusb
```

You should see something like:

```
Bus 002 Device 001: ID 2207:350a Fuzhou Rockchip Electronics Co., Ltd. Composite Device
Bus 000 Device 000: ID 2207:350a Fuzhou Rockchip Electronics Co., Ltd. USB 3.1 Bus
```

Then you successfully connected on flash mode. Raise your hands to sky, because this is hard sometimes.

Now check if you connected on `loader` mode:

```sh
sudo rkdeveloptool ld
```

This should print:

```
DevNo=1	Vid=0x2207,Pid=0x350a,LocationID=201	Loader
```

If it is not in loader mode, (maybe Maskrom), it means you have to do again. Disconnect the power and retry the flashing procedure.

1. Erase the flash memory
2. Install the bootloader

```sh
$ sudo rkdeveloptool ef
Erasing flash complete.
$ sudo rkdeveloptool db rk356x_spl_loader_ddr1056_v1.10.111.bin
Downloading bootloader succeeded.
```

Disconnect the power from the miner and enter on flash mode again

```sh
sudo rkdeveloptool ld
```

This should print:

```
DevNo=1	Vid=0x2207,Pid=0x350a,LocationID=201	Maskrom
```

Now you can upload the image you want to use on your device. Let's assume you want to use [Crankk OS](https://crankk.io/guides/crankk-official-guide-for-onboarding-a-linxdot-rk3566-gateway#section1)

```sh
$ sudo rkdeveloptool cs 1
Change Storage OK.
$ sudo rkdeveloptool wl 0 crankkos-linxdotrk3566-1.0.0.img
Write LBA from file (100%)
$ sudo rkdeveloptool td
Test Device OK.
$ sudo rkdeveloptool rd
Reset Device OK.
```

Now just restart your device, disconnect the usb, and crankk it!!!

## References

* [Rockchip Devtool](https://wiki.radxa.com/Rockpi4/install/rockchip-flash-tools)
* [Rockchip Flashing](https://wiki.radxa.com/Rockpi4/dev/usb-install)
* [Flash Linxdot](https://github.com/fernandodev/linxdot-rockchip-flash)
