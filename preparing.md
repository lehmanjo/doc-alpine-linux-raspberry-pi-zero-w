# Preparing the SD card (linux)

Download the right tarball from https://alpinelinux.org/downloads/.  It should be **armhf** for Raspberry PI.

```
# cd /tmp
# wget http://dl-cdn.alpinelinux.org/alpine/v3.12/releases/armhf/alpine-rpi-3.12.0-armhf.tar.gz
Connecting to dl-cdn.alpinelinux.org (151.101.2.133:80)
saving to 'alpine-rpi-3.12.0-armhf.tar.gz'
alpine-rpi-3.12.0-ar 100% |*******************************************************************************| 81.0M  0:00:00 ETA
'alpine-rpi-3.12.0-armhf.tar.gz' saved
```

Mount the first (512MB) partition and extract tarball.

```
# mkdir /mnt/1
# mount /dev/mmcblk0p1 /mnt/1
# df -h /mnt/1
Filesystem                Size      Used Available Use% Mounted on
/dev/mmcblk0p1          511.0M      4.0K    511.0M   0% /mnt/1
# tar -xvzf /tmp/alpine-rpi-3.12.0-armhf.tar.gz -C /mnt/1/
./
./cmdline.txt
...
...
# du -sh /mnt/1
88.8M   /mnt/1
```

Add ext4 filesystem to cmdline.txt

```
# cd /mnt/1
# vi cmdline.txt
```

> modules=loop,squashfs,sd-mod,usb-storage **,ext4** quiet console=tty1


