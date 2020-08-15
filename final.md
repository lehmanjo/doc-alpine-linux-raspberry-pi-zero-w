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

### Fixup WiFi.

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




