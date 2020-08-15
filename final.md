# Final steps

### Swap

Enable swap on boot and start swap

```
# rc-update add swap boot
 * service swap added to runlevel boot
...
# rc-service swap start
 * Activating swap devices ...  
...
# rc-update -u
 * Caching service dependencies ...   
...
# dmesg | grep -i mmcblk0p2
[  105.858317] Adding 2097148k swap on /dev/mmcblk0p2.  Priority:-2 extents:1 across:2097148k SSFS
...
# free
              total        used        free      shared  buff/cache   available
Mem:         443568       29048      396876         168       17644      416272
Swap:       2097148           0     2097148
```

### Fixup WiFi

Credit goes to https://wiki.alpinelinux.org/wiki/Connecting_to_a_wireless_access_point.

```
localhost:~# apk add wireless-tools wpa_supplicant
OK: 105 MiB in 56 packages
...
localhost:~# rc-update add wpa_supplicant boot
 * rc-update: wpa_supplicant already installed in runlevel `boot'; skipping
...
localhost:~# rc-update add wpa_cli boot
 * service wpa_cli added to runlevel boot
```

### Optional: Upgrade to bleeding "edge" 

This will upgrade Alpine Linux to the "latest" build (may not be stable).  This is not required.

```
# vi /etc/apk/repositories
...
# cat /etc/apk/repositories
#http://your_mirror/mirror/alpine/v3.12/main
#http://your_mirror/mirror/alpine/v3.12/community
http://your_mirror/mirror/alpine/edge/main
http://your_mirror/mirror/alpine/edge/community
http://your_mirror/mirror/alpine/edge/testing
...
# apk update && apk upgrade
...
OK: 14731 distinct packages available
Upgrading critical system libraries and apk-tools:
...
(15/23) Upgrading linux-firmware-brcm (20200519-r0 -> 20200519-r1)
(16/23) Upgrading linux-rpi (5.4.43-r0 -> 5.4.57-r0)
...
==> initramfs: creating /boot/initramfs-rpi
OK: 106 MiB in 56 packages
...
# ls -l /boot
lrwxrwxrwx    1 root     root            15 Aug 15 13:45 /boot -> media/mmcblk0p1
...
localhost:~# ls -l /boot/
total 10972
-rw-r--r--    1 root     root       2442274 Aug  8 15:13 System.map-rpi
lrwxrwxrwx    1 root     root             1 Aug 15 14:07 boot -> .
-rw-r--r--    1 root     root        171168 Aug  8 15:13 config-rpi
drwxr-xr-x    3 root     root          4096 Aug 15 14:06 dtbs-rpi
-rw-------    1 root     root       3482224 Aug 15 14:07 initramfs-rpi
-rwxr-xr-x    1 root     root       5125088 Aug  8 15:13 vmlinuz-rpi
...
# uname -a
Linux localhost 5.4.43-0-rpi #1-Alpine Thu May 28 09:54:10 UTC 2020 armv6l Linux
```

Reboot to load latest kernel.

```
# sync
...
# reboot
```

Check kernal after reboot

```
# uname -a
Linux localhost 5.4.57-0-rpi #1-Alpine Sat Aug 8 17:24:11 UTC 2020 armv6l Linux
```


### Fixup Config

You can either re-run "setup-alpine" or run the specific configuration scripts you want.

* __setup-timezone__ to change to different timezone.  Default timezone during installation was UTC
* __setup-hostname__ to change hostname from localhost
* __setup-dns__ to setup a DNS domain name for your Raspberry Pi


### Optional

#### Add Unprivileged User


Install useful packages

```
# apk add bash sudo
(1/3) Installing readline (8.0.4-r0)
(2/3) Installing bash (5.0.18-r0)
Executing bash-5.0.18-r0.post-install
(3/3) Installing sudo (1.9.1-r0)
Executing busybox-1.32.0-r1.trigger
OK: 109 MiB in 59 packages
```

Add groups and user (e.g. username "me")
```
# addgroup sudo
# addgroup -g 1000 me
# adduser -s /bin/bash -h /home/me -G me me
Changing password for me
New password:
Retype password:
passwd: password for me changed by root
...
# addgroup me sudo
...
# grep sudo /etc/group
sudo:x:1000:me
```

Edit /etc/sudoers to enable sudo group.

```
# vi /etc/sudoers
...
# grep ^%sudo /etc/sudoers
%sudo ALL=(ALL) ALL
```

Remove remote root login via SSH.

```
# grep ^PermitRoot /etc/ssh/sshd_config
PermitRootLogin yes
...
localhost:~# vi /etc/ssh/sshd_config
```

Reboot and login as new user (e.g. "me").

```
$ whoami
me

$ sudo whoami
...
[sudo] password for me:
root

$ sudo apk update && sudo apk upgrade
...
OK: 14706 distinct packages available
OK: 109 MiB in 59 packages
```

#### Add Compilers

##### CLANG C/C++

* https://clang.llvm.org/

```
$ sudo apk add clang cmake make llvm clang-dev
```

##### GNU C/C++

* https://gcc.gnu.org/

```
$ sudo apk add gcc g++ cmake make gdb
```

