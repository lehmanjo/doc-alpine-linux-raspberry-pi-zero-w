# Preparing the SD card 
These steps need to be carried out on an existing, accessible and running Linux system, preferably an Alpine Linux system.

### Download Alpine Linux

Download the right tarball from https://alpinelinux.org/downloads/.  It should be **armhf** for Raspberry PI.

```
# cd /tmp
# wget http://dl-cdn.alpinelinux.org/alpine/v3.12/releases/armhf/alpine-rpi-3.12.0-armhf.tar.gz
Connecting to dl-cdn.alpinelinux.org (151.101.2.133:80)
saving to 'alpine-rpi-3.12.0-armhf.tar.gz'
alpine-rpi-3.12.0-ar 100% |*******************************************************************************| 81.0M  0:00:00 ETA
'alpine-rpi-3.12.0-armhf.tar.gz' saved
```

### Mount Boot Partition

Mount the boot (first) partition and extract tarball.

```
# mkdir /mnt/1
# mount /dev/mmcblk0p1 /mnt/1
# df -h /mnt/1
Filesystem                Size      Used Available Use% Mounted on
/dev/mmcblk0p1          511.0M      4.0K    511.0M   0% /mnt/1
# tar -xvzf /tmp/alpine-rpi-3.12.0-armhf.tar.gz -C /mnt/1/
...
...
# du -sh /mnt/1
88.8M   /mnt/1
```

### Update cmdline.txt

Add ext4 filesystem support to modules in cmdline.txt for later

```
# cd /mnt/1
# vi cmdline.txt
```

> modules=loop,squashfs,sd-mod,usb-storage,ext4 quiet console=tty1


### Optional: Create a usercfg.txt

* https://wiki.alpinelinux.org/wiki/Classic_install_or_sys_mode_on_Raspberry_Pi
* https://wiki.alpinelinux.org/wiki/Raspberry_Pi_Zero_W_-_Installation

e.g. minimal GPU memory, audio, no bluetooth

```
# cat /mnt/1/usercfg.txt
gpu_mem=16
dtparam=audio=on
dtoverlay=pi3-disable-bt
dtoverlay=w1-gpio
enable_uart=1
```

### Unmount SD Card

```
# sync
# cd /
# umount /mnt/1
```


You can now remove the SD card and insert it into the Raspberry PI Zero W.


[back](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w)

