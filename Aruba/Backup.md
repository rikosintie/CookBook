## Create backup jobs

* Backup the current startup config (config file in flash)
* Do a write mem to save the curring running-config to flash
* Copy running-config to a tftp server if the security policy allows it

```
job "backup" at 02:59 "copy config config1 config backup"
job "wr-mem" at 03:00 config-save "wr mem"
job "backup2" at 03:02 "copy running-config tftp 10.20.2.70 switch.wri"
```
