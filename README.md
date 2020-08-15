# doc-alpine-linux-raspberry-pi-zero-w


## Preparing the SD card (linux)

Find the micro SD card

```
# dmesg | grep mmc
[    3.031337] mmc0: SDHCI controller on PCI [0000:02:00.0] using ADMA
[ 1654.693236] mmc0: new ultra high speed SDR104 SDHC card at address aaaa
[ 1654.701652] mmcblk0: mmc0:aaaa SC32G 29.7 GiB
[ 1654.720429]  mmcblk0: p1 p2 p3
```
