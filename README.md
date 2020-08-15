# doc-alpine-linux-raspberry-pi-zero-w

How to install Alpine Linux on Raspberry Pi Zero W in "classic" or "sys-mode" from a default "diskless" installation.

Credit goes to various authors of content on the Alpine Linux Wiki.

* https://wiki.alpinelinux.org/wiki/Classic_install_or_sys_mode_on_Raspberry_Pi
* https://wiki.alpinelinux.org/wiki/Raspberry_Pi#Persistent_Installation_on_Raspberry_Pi_3
* https://wiki.alpinelinux.org/wiki/Raspberry_Pi_Zero_W_-_Installation
* https://wiki.alpinelinux.org/wiki/Connecting_to_a_wireless_access_point


### Table of Contents

#### Step 1 - Partitioning the SD card

Describes steps for partitioning the SD card for a full installation with boot, swap and root partitions.  Assumes you have access to an existing linux or Alpine Linux system.  

Read steps [here](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w/blob/master/partition.md).


#### Step 2  - Preparing the SD card

Explains how to download and extract Alpine Linux onto the SD card's boot partition.  These steps assume you have access to an existing linux or Alpine Linux system.

Read steps [here](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w/blob/master/preparing.md).


#### Step 3 - Initial Setup

Shows the initial setup of Alpine Linux after booting the new Raspberry Pi Zero W off the SD card.  These steps are now performed on the target system.  It includes the default setup, including WiFi and temporarily allowing remote root access through OpenSSH.

Read steps [here](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w/blob/master/initial.md).


#### Step 4 - Conversion

Describes the steps that convert the installation from diskless to a classic disk-based, sys-mode installation.  I.e. Alpine Linux is configured to run off a root filesystem on the SD card rather than from memory.

Read steps [here](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w/blob/master/setup-sys.md).


#### Step 5 - Final Steps

Completes the installation and shows optional Linux configuration and software installation, including CLANG, GCC, G++ and RUST compiler installation.

Read final steps [here](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w/blob/master/final.md).



