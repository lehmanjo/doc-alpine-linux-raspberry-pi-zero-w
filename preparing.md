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
