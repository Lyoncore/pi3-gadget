# Raspberry Pi 3 Gadget Snap

This repository contains the source for an Ubuntu Core gadget snap for the Raspberry Pi 3.

Building it with snapcraft will automatically pull, configure, patch and build
the git.denx.de/u-boot.git upstream source for rpi_3_32b_defconfig at release v2017.05,
produce a u-boot.bin binary and put it inside the gadget.

It will then download the latest stable binary boot firmware
from https://github.com/raspberrypi/firmware/tree/stable/boot and add it to the gadget.

Last it will pull the latest linux-image-raspi2 from the xenial-updates archive, extract the
devicetree and overlay files from it and add them to the gadget as well.


## Gadget Snaps

Gadget snaps are a special type of snaps that contain device specific support
code and data. You can read more about them in the snapd wiki
https://github.com/snapcore/snapd/wiki/Gadget-snap

## Reporting Issues

Please report all issues on the Launchpad project page
https://bugs.launchpad.net/snap-pi3/+filebug

We use Launchpad to track issues as this allows us to coordinate multiple
projects better than what is available with Github issues.

## Building

To build the gadget snap locally on an armhf system please use `snapcraft`.

To cross build this gadget snap on a PC please install the gcc-arm-linux-gnueabihf package
before running `snapcraft`

## Building ubuntu core image with recovery

To build the Ubuntu core image with recovery partititon, please preparee pi3 model assertion.
Install/refresh the ubuntu-image in the edge(1.1+snap3).
```
Install:
$ snap install ubutnu-image --edge

Refresh:
$ snap refresh ubuntu-image --edge
```

Building the image with ubuntu-image by following command:
```
$ ubuntu-image --extra-snaps pi3_16.04-0.5_armhf.snap -w workdir <path to model assertion>/pi3.model --hooks-directory workdir/unpack/gadget/ubuntu-image-hooks/
```

After the image dd and boot success on Pi3 platform.
The command to trigger recovery on pi3 Ubuntu Core 16:
```
$ sudo fw_setenv snap_mode recovery
$ sudo reboot
```

Note: Currently, the recovery process requires to get user confirmation to prevent damage user data.
The user confirmation still output to serial console only. Please attach serial console and enter [y] enter in when you see the prompt:
```
Factory Restore will delete all user data, are you sure? [y/N]
```
## Launchpad Mirror and Automatic Builds.

All commits from the master branch of https://github.com/snapcore/pi3 are
automatically mirrored by Launchpad to the https://launchpad.net/snap-pi3
project.

The master branch is automatically built from the launchpad mirror and
published into the snap store to the edge channel.

You can find build history and other controls here:
https://code.launchpad.net/~canonical-foundations/+snap/pi3
