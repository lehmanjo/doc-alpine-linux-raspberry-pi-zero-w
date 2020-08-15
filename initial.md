# Initial Setup

Insert SD card into new Raspberry Pi Zero W and boot.

### Login

```
localhost login: root
Welcome to Alpine!
...
...
```

### Run initial setup

```
# setup-alpine
...
Available keyboard layouts:
...
Select keyboard layout [none]: en
...
Select keyboard layout [en]: us
...
Select variant []: us-intl
...
Enter system hostname [localhost]: localhost
...
Available interfaces are: wlan0
...
Which one do you want to initialize? [wlan0]: wlan0
...
Type the wireless network name to connect to: MYWIFIID
...
Type the "MYWIFIID" network Pre-Shared Key: MYWIFIPASSWORD
...
Ip address for wlan0? [dhcp]: dhcp
...
Do you want to do any manual network configuration? [no]: no
...
Changing password for root
New password: ENTERNEWPASSWORD
Retype password: ENTERNEWPASSWORD
...
Which timezone are you in? [UTC]: UTC
...
HTTP/FTP proxy URL? [none]: none
...
Which NTP client to run? [chrony]: chrony
...
Enter mirror number (1-nn) or URL to add [1]: 1
...
Which SSH server? [openssh]: openssh
...
Enter where to store configs [mmcblk0p1]: mmcblk0p1
...
Enter apk cache directory [/media/mmcblk0p1/cache]: /media/mmcblk0p1/cache
...
```

### Last tweaks

Check IP address on wireless interface (wlan0)

```
# ifconfig wlan0 | grep inet
          inet addr:192.168.0.140  Bcast:0.0.0.0  Mask:255.255.255.0
...
```

Update Alpine packages

``` 
# apk update && apk upgrade
fetch http://your_mirror/mirror/alpine/v3.12/main/armhf/APKINDEX.tar.gz
3.12.0 [/media/mmcblk0p1/apks]
v3.12.0-241-g30378ef162 [http://your_mirror/mirror/alpine/v3.12/main]
OK: 4793 distinct packages available
OK: 19 MiB in 43 packages
```

Add e2fsprogs for filesystem utilities

```
# apk add e2fsprogs
...
```

Temporarily enable root login via SSH.

```
# grep PermitRootLogin /etc/ssh/sshd_config
#PermitRootLogin prohibit-password
# the setting of "PermitRootLogin without-password".
...
# echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
...
# grep PermitRootLogin /etc/ssh/sshd_config
#PermitRootLogin prohibit-password
# the setting of "PermitRootLogin without-password".
PermitRootLogin yes
```

Enable WiFi during boot

```
# rc-update add wpa_supplicant boot
...
```

Persist changes before reboot

```
# apk update && apk upgrade
...
# lbu_commit -v
...
...
# sync
...
# reboot
```

Wait for Raspberry Pi Zero W to reboot.  You can then either login directly or over the network using SSH.


[back](https://github.com/lehmanjo/doc-alpine-linux-raspberry-pi-zero-w)
