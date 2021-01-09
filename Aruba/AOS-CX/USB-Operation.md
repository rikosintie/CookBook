# Aruba CX USB operation

Insert the flash drive in the USB port.

The CX OS is a Linux based OS so we will use some standard Linux commands. 

To list the files on the drive do the follwoing:

```
usb mount
start-shell
cd /mnt/usb
/mnt/usb$ ls 
 ArubaOS-CX_8325_10_05_0021.swi  'System Volume Information'
exit
```

To copy a file from the flash drive to the primary flash:

```
copy usb:/ArubaOS-CX_8325_10_05_0021.swi primary
usb unmount
boot system primary
```
