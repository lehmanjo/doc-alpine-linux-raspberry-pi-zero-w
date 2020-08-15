# Convert to sys-mode install

Convert from "diskless" to sys-mode install as described on Alpine Wiki

* https://wiki.alpinelinux.org/wiki/Classic_install_or_sys_mode_on_Raspberry_Pi

### Mount ext4 - new root filesystem

```
# dmesg | grep mmcblk
[    3.046589] mmcblk0: mmc0:aaaa SC32G 29.7 GiB
[    3.065272]  mmcblk0: p1 p2 p3
[    7.092256] EXT4-fs (mmcblk0p3): mounted filesystem with ordered data mode. Opts: (null)
...
...
# mount /dev/mmcblk0p3 /mnt
...
# df -h /mnt
Filesystem                Size      Used Available Use% Mounted on
/dev/mmcblk0p3           26.7G     44.0M     25.3G   0% /mnt
...
```

### Initialize root filesystem

You can ignore the errors referring to syslinux or extlinux.  This step takes a few minutes.

```
# setup-disk -m sys /mnt
...
Installing system on /dev/mmcblk0p3:
...
...
==> initramfs: creating /boot/initramfs-rpi
You might need fix the MBR to be able to boot
```

### Manual Adjustments

Now a number of manual steps are required as outlined on various Alpine Wiki pages.  Credit to their respective authors.

* https://wiki.alpinelinux.org/wiki/Raspberry_Pi#Persistent_Installation_on_Raspberry_Pi_3
* https://wiki.alpinelinux.org/wiki/Classic_install_or_sys_mode_on_Raspberry_Pi
* https://wiki.alpinelinux.org/wiki/Raspberry_Pi_Zero_W_-_Installation
 
 
- one
- two

 
```

```
