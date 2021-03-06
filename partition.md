# Preparing the SD card

These steps need to be carried out on an existing, accessible and installed linux system with an SD card reader, preferably an Alpine Linux system.

### Find the micro SD card

Shows how to find the SD card device name and confirm that SD card has been detected by Linux after inserting SD card.

```
# dmesg | grep mmc
[    3.031337] mmc0: SDHCI controller on PCI [0000:02:00.0] using ADMA
[ 1654.693236] mmc0: new ultra high speed SDR104 SDHC card at address aaaa
[ 1654.701652] mmcblk0: mmc0:aaaa SC32G 29.7 GiB
[ 1654.720429]  mmcblk0: p1 p2 p3
```

### Partition SD card 

Example: 32GB or larger Micro SD card

1. Boot Partition (512MB) [FAT16]
2. Swap Partition (2GB) [Swap]
3. Root Partition (Remainder) [Ext4]


```
# fdisk /dev/mmcblk0
Command (m for help): m
Command Action
a       toggle a bootable flag
l       list known partition types
n       add a new partition
o       create a new empty DOS partition table
p       print the partition table
w       write table to disk and exit

Command (m for help): o
...
...
Command (m for help): p
...
...
Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
Command (m for help): n
Partition type
   p   primary partition (1-4)
   e   extended
p
Partition number (1-4): 1
First sector (16-62333951, default 16):
Using default value 16
Last sector or +size{,K,M,G,T} (16-62333951, default 62333951): +512M

Command (m for help): p
Disk /dev/mmcblk0: 30 GB, 31914983424 bytes, 62333952 sectors
973968 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1    0,1,1       1023,3,16           16    1048591    1048576  512M 83 Linux

Command (m for help): n
Partition type
   p   primary partition (1-4)
   e   extended
p
Partition number (1-4): 2
First sector (1048592-62333951, default 1048592):
Using default value 1048592
Last sector or +size{,K,M,G,T} (1048592-62333951, default 62333951): +2G

Command (m for help): p
Disk /dev/mmcblk0: 30 GB, 31914983424 bytes, 62333952 sectors
973968 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1    0,1,1       1023,3,16           16    1048591    1048576  512M 83 Linux
/dev/mmcblk0p2    1023,3,16   1023,3,16      1048592    5242895    4194304 2048M 83 Linux

Command (m for help): n
Partition type
   p   primary partition (1-4)
   e   extended
p
Partition number (1-4): 3
First sector (5242896-62333951, default 5242896):
Using default value 5242896
Last sector or +size{,K,M,G,T} (5242896-62333951, default 62333951):
Using default value 62333951

Command (m for help): p
Disk /dev/mmcblk0: 30 GB, 31914983424 bytes, 62333952 sectors
973968 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1    0,1,1       1023,3,16           16    1048591    1048576  512M 83 Linux
/dev/mmcblk0p2    1023,3,16   1023,3,16      1048592    5242895    4194304 2048M 83 Linux
/dev/mmcblk0p3    1023,3,16   1023,3,16      5242896   62333951   57091056 27.2G 83 Linux

Command (m for help): l

 6 FAT16                  41 PPC PReP Boot          a8 Darwin UFS
 e Win95 FAT16 (LBA)      82 Linux swap             be Solaris boot
 f Win95 Ext'd (LBA)      83 Linux                  eb BeOS fs

Command (m for help): t
Partition number (1-4): 1
Hex code (type L to list codes): 6
Changed system type of partition 1 to 6 (FAT16)

Command (m for help): t
Partition number (1-4): 2
Hex code (type L to list codes): 82
Changed system type of partition 2 to 82 (Linux swap)

Command (m for help): a
Partition number (1-4): 1

Command (m for help): p
Disk /dev/mmcblk0: 30 GB, 31914983424 bytes, 62333952 sectors
973968 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1 *  0,1,1       1023,3,16           16    1048591    1048576  512M  6 FAT16
/dev/mmcblk0p2    1023,3,16   1023,3,16      1048592    5242895    4194304 2048M 82 Linux swap
/dev/mmcblk0p3    1023,3,16   1023,3,16      5242896   62333951   57091056 27.2G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table

# sync

```

### Format partitions

```
# mkdosfs -F 32 /dev/mmcblk0p1
mkfs.fat 4.1 (2017-01-24)
```

```
# mkswap /dev/mmcblk0p2
Setting up swapspace version 1, size = 2147479552 bytes
...
```

```
# mkfs.ext4 /dev/mmcblk0p3
mke2fs 1.45.6 (20-Mar-2020)
...
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```


[back](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w)
