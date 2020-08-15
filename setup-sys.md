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
 
 Reminder
 * boot filesystem is mmcblk0p1 (always mounted on /media/mmcblk0p1)
 * swap filesystem is mmcblk0p2 (not mounted during installation)
 * root filesystem is mmcblk0p3 (temporarily mounted on /mnt)
 
Summary of steps
1. move the updated initramfs/boot files to the boot partition.
2. adjust links to boot partition
3. tweak fstab on root filesystem


Remount boot filesystem for editing
```
# mount -o remount /media/mmcblk0p1/ -rw
...
```

Remove default boot/ directory from boot partition.
```
# rm -fr /media/mmcblk0p1/boot
...
```

Remove boot symlink from new root filesystem
```
# rm /mnt/boot/boot
...
```

Move the newly generated initramfs/boot files from root filesystem to boot filesystem
```
# mv /mnt/boot /media/mmcblk0p1/boot
...
```

Create mountpoint for boot filesystem in root filesystem
```
# mkdir /mnt/media/mmcblk0p1
...
```

Blindly create symlink from boot in /boot filesystem to /boot in root filesystem.  Because the filesystems are not yet mounted the way the will be mounted after the installation completes.  This symlink must be relative and won't appear as valid until after installation completes.
```
# cd /mnt
# ln -s media/mmcblk0p1 boot
# ls -l boot
lrwxrwxrwx    1 root     root            15 Aug 15 13:45 boot -> media/mmcblk0p1
```

Update /etc/fstab in root filesystem
```
# vi /mnt/etc/fstab
...
# cat /mnt/etc/fstab
/dev/mmcblk0p1 /media/mmcblk0p1 vfat defaults 0 0
/dev/mmcblk0p2 swap  swap defaults 0 0
/dev/mmcblk0p3 /  ext4 defaults 1 1
/dev/cdrom /media/cdrom iso9660 noauto,ro 0 0
/dev/usbdisk /media/usb vfat noauto 0 0
```

### Adjust root filesystem in cmdline.txt

Update cmdline.txt in boot filesystem to load root filesystem from separate partition.

```
# vi /media/mmcblk0p1/cmdline.txt
...
# cat /media/mmcblk0p1/cmdline.txt
modules=loop,squashfs,sd-mod,usb-storage,ext4 root=/dev/mmcblk0p3 quiet console=tty1
```

That's it!

Reboot.



[back](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w)
