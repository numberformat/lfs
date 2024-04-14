![LFS in Virtual Machines](https://github.com/numberformat/lfs/releases/download/v8.2.1/Screenshot.2024-04-13.at.8.30.00.PM.png)

## Description

This repository contains docker configuration to build bootable ISO image with [Linux From Scratch 8.2](http://www.linuxfromscratch.org/lfs/downloads/8.2/LFS-BOOK-8.2.pdf).

## Why

General idea is to learn Linux by building and running LFS system in
isolation from the host system.

## Structure

Scripts are organized in the way of following book structure whenever
it makes sense. Some deviations are done to make a bootable iso image.

## Build

Use the following commands:

```sh
sudo ./phase1.sh
sudo ./phase2.sh # If you get an error losetup: /tmp/ramdisk: failed to set up loop device, then just retry.
```

Please note, that extended privileges are required by docker container
in order to execute some commands (e.g. mount).

## Usage

Final result is bootable iso image with LFS system which, for example, can be used to load the system inside virtual machine (tested
with VirtualBox and Proxmox VE).

Below is a screenshot of the files contained within the ISO image. The image is bootable using Proxmox VE, VirtualBox or any other Virtual Environment.

![LFS in Virtual Machines](https://github.com/numberformat/lfs/releases/download/v8.2.1/Screenshot.2024-04-13.at.8.41.03.PM.png)

It uses RAMDisk thus does not require you to install it into any partition. The please note that your changes will be lost when the VM is restarted. To get around it you can just mount a SMB or NFS share and save your data there.

From here you can proceed to install the operating system onto your hard drive using raw command line tools. This is similar to the way you would setup Arch Linux but without the pre-written setup scripts. But honestly there is not much you can do with a bare bones LFS system more than getting networking setup and pinging a few sites.

To get networking setup review the "General Network Configuration" of the LFS book. Its all documented there so I am not going to repeat all of that. It will guide you thru creating some config files and get you to the point where you can start to ping different servers on the internet. Be sure to select a network device which is compatible with the stock kernel. For example I had to choose Intel E1000 network card in my Proxmox VE hardware settings screen. Take note that any config setting you make will be lost on the next reboot.

This is what I did to get the networking up.

```sh
ip link
# get your network card name from above for example enp0s18 or eth0
cd /etc/sysconfig/
# copy or edit the stock config in case your card IS named eth0
cp ifconfig.eth0 ifconfig.enp0s18
# edit the above file and assign the static IP
# DHCP is not supported in LFS that package gets introduced in Beyond LFS.
# bring up the network
ifup enp0s18
# ping google.com or any other site
```

## Troubleshooting

If you have problems with master branch, please try to use stable version from the latest release with toolchain from archive.

## License

This work is based on instructions from [Linux from Scratch](http://www.linuxfromscratch.org/lfs)
project and provided with MIT license.
