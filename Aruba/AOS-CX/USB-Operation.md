# Aruba CX USB operation

Insert the flash drive in the USB port.

Console into the switch and run the following. Obviously you need to change the file name if you aren't upgrading to 10.05.0021.

```
usb mount
start-shell
cd /mnt/usb
/mnt/usb$ ls 
 ArubaOS-CX_8325_10_05_0021.swi  'System Volume Information'
exit
copy usb:/ArubaOS-CX_8325_10_05_0021.swi primary
usb unmount
```
