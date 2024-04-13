![LFS in VirtualBox](https://user-images.githubusercontent.com/1611077/33808510-16825dd2-dde8-11e7-9a1c-0ca0bc3ff2b5.png)

## Description

This repository contains docker configuration to build bootable iso image with [Linux From Scratch 8.1](http://www.linuxfromscratch.org/lfs/downloads/8.1/LFS-BOOK-8.1.pdf).

## Why

General idea is to learn Linux by building and running LFS system in isolation from the host system.

## Structure

Scripts are organized in the way of following book structure whenever it makes sense. Some deviations are done to make a bootable iso image.

## Build

Use the following command:

```sh
docker rm lfs
docker build --tag lfs .
sudo docker run -it --privileged --name lfs lfs # this performs the privileged build in the phase 2 container
docker commit lfs lfs_phase2    # convert phase 2 container into image
docker build --tag lfs_phase3 -f Dockerfile2 . # this dockerfile builds the lfs_phase3 image FROM the lfs_phase2
docker run -it --privileged --name lfs_phase3 lfs_phase3 # this performs the privileged build in the phase 3 container
docker cp lfs_phase3:/tmp/lfs.iso . # copy all the necessary files out of phase 3 container
```

If you get an error losetup: /tmp/ramdisk: failed to set up loop device, then just retry.

Please note, that extended privileges are required by docker container in order to execute some commands (e.g. mount).

## Usage

Final result is bootable iso image with LFS system which, for example, can be used to load the system inside virtual machine (tested with VirtualBox).

## License

This work is based on instructions from [Linux from Scratch](http://www.linuxfromscratch.org/lfs) project and provided with MIT license.

## Issues in 8.2 compare to 8.1

Missing commands after comparing the contents of the ramdisks

* openssl
* c_rehash
* wgetrc, wget
* engine-1.1
* libcrypto.pc
* libssl.pc
* make-ca.sh
