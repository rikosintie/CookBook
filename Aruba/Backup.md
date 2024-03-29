<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

## Create backup jobs
The "job" command allows you to create custom scheduled tasks. Below are three examples.

* Backup the current startup config (config file in flash)
* Do a write mem to save the curring running-config to flash
* Copy running-config to a tftp server if the security policy allows it

```
job "backup" at 02:59 "cfg-backup running-config config backup"
job "wr-mem" at 03:00 config-save "wr mem"
job "backup2" at 03:02 "copy running-config tftp 10.20.2.70 switch.wri"
```
<p>&nbsp;</p>

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
<p>&nbsp;</p>

**Manually create a backup**
```
test# copy config config1 config backup-test


test# sh config files

Configuration files:

 id | act pri sec | name
 ---+-------------+------------------------------------------------
  1 |  *   *   *  | config1
  2 |             | backup-test
  3 |             |
  4 |             |
  5 |             |
```

**Review a configuration file**

`
show config backup
`
<p>&nbsp;</p>

**Compare running-config to a file in flash**

`
test(config)#cfg-restore flash backup diff 
`

<p>&nbsp;</p>

**Delete a config file**

```
test#erase config backup
```

If you delete the configuration file that is active, the switch will reload.

<p>&nbsp;</p>

**Boot from the backup configuration**

`
boot system flash primary config backup
`
<p>&nbsp;</p>

**Copy the backup configuration from a TFTP server**

Note: If you are using the Out of Band interface you must include oobm.

The format is: 

`
copy tftp config <dest-file> <ip-addr> <remote-file> <pc | unix> [oobm]
`

This example copies switch.wri from the tftp server 10.20.2.79 to the startup-config

`
copy tftp startup-config 10.20.2.79 switch.wri oobm
`

<p>&nbsp;</p>

**Configuration Restore**

You can do a live restore from a file in flash or on a tftp/sftp server to the running-config. 

While the restore is in progress all processes are blocked from making changes:

* other CLI sessions
* the Web UI
* SNMP
* REST


`test(config)#cfg-restore flash backup
`

<p>&nbsp;</p>
<p>&nbsp;</p>

**Automatic  rollback**

(From the document in the reference section below)

The cfg-restore command can be used in conjunction with the alias command and job scheduler function to provide a means for automatically reverting to a stable configuration. Use this when an modifying the running configuration remotely in case you lose connectivity to the switch and are unable to revert their changes.

```
alias "cfg-rollback" "cfg-restore flash backup"
job "auto-rollback" delay 00:00:15 "cfg_rollback" count 1
```

After the administrator is finished making configuration changes, and the resulting configuration is stable, the job can be removed.

`
no job "auto-rollback"
`



# Reference

[CONFIGURATION RESTORE WITHOUT REBOOT](https://higherlogicdownload.s3.amazonaws.com/HPE/MigratedAssets/Config_Restore_without_Reboot.pdf)
