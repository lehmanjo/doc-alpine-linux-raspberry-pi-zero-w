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
# dmesg | grep -i mmcblk0p2
[  105.858317] Adding 2097148k swap on /dev/mmcblk0p2.  Priority:-2 extents:1 across:2097148k SSFS
```





