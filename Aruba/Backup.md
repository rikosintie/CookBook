## Create backup jobs

* Backup the current startup config (config file in flash)
* Do a write mem to save the curring running-config to flash
* Copy running-config to a tftp server if the security policy allows it

```
job "backup" at 02:59 "copy config config1 config backup"
job "wr-mem" at 03:00 config-save "wr mem"
job "backup2" at 03:02 "copy running-config tftp 10.20.2.70 switch.wri"
```

# Working with config files
Display the configuration files in flash

```
sh config files

Configuration files:

 id | act pri sec | name
 ---+-------------+------------------------------------------------
  1 |  *   *   *  | config1
  2 |             | backup
  3 |             | 
  4 |             | 
  5 |             | 
```
**Review a configuration file**

`
show config <filename>
`

**Delete a config file**

```
erase config backup
```

Verify the deletion
```
sh config files

Configuration files:

 id | act pri sec | name
 ---+-------------+------------------------------------------------
  1 |  *   *   *  | config1
  2 |             | 
  3 |             |  
  4 |             | 
  5 |             | 
```
If you delete the configuration file that is active, the switch will reload.


**Boot from the backup configuration**

`
boot system flash primary config backup
`

**Copy the backup configuration from the TFTP server**

Note:If you are using the Out of Band interface you must include oobm.

The format is: 

`
copy tftp config <dest-file> <ip-addr> <remote-file> <pc | unix> [oobm]
`

This example copies switch.wri from the tftp server 10.20.2.79 to the startup-config


`
copy tftp startup-config 10.20.2.79 switch.wri oobm
`
