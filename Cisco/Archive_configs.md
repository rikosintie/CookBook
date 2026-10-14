# Save configuration to an archive directory

This saves a copy of the running configuration every time you do a `write mem`. 
It's useful if you don't have a server that you can send backups to. I have an 
example of how to do that in my [daily switch backup appendix](https://rikosintie.github.io/Discovery/appendix/appendix-daily-switch-backup/).

The order matters:

1. Create the directory first.
2. Configure `path`.
3. Configure `write-memory`.

If `write-memory` is entered before the directory exists, the trigger never activates. 
Remove it with `no write-memory` and enter it again.

Always use the absolute path `flash:/archive`. A relative path like `flash:archive` is 
resolved from your current directory, so if you have done a `cd flash:/archive`, the 
switch tries to write to `flash:/archive/archive/` and fails.

----------------------------------------------------------------

The complete configuration:

```text
! from enable mode, not config mode
mkdir flash:/archive
conf t
archive
 log config
  logging enable
  logging size 1000
 path flash:/archive/$h-
 write-memory
end
```

The switch keeps the 10 most recent archives and deletes the oldest when it saves a new one. The list is kept in memory, so after a reload the switch starts a new list and the files from before the reload are never deleted. They don't use much space, but see [Remove archives](#remove-archives) to clear them.

----------------------------------------------------------------

## Replacing a running config from archive

If a new configuration turns out to cause a problem, you can restore from the archive. First, look at the files in the archive:

```text
show archive
The maximum archive configurations allowed is 10.
There are currently 10 archive configurations saved.
The next archive file will be named flash:/archive/cdlp-voice--<timestamp>-11
 Archive #  Name
   1        flash:/archive/cdlp-voice--Oct--3-11-18-52.176-PDT-1
   2        flash:/archive/cdlp-voice--Oct--3-11-39-59.915-PDT-2
   3        flash:/archive/cdlp-voice--Oct--3-12-48-47.450-PDT-3
   4        flash:/archive/cdlp-voice--Oct--3-12-49-11.948-PDT-4
   5        flash:/archive/cdlp-voice--Oct--3-16-41-41.450-PDT-5
   6        flash:/archive/cdlp-voice--Oct--3-17-30-40.331-PDT-6
   7        flash:/archive/cdlp-voice--Oct--3-17-38-11.258-PDT-7
   8        flash:/archive/cdlp-voice--Oct--3-17-46-39.584-PDT-8
   9        flash:/archive/cdlp-voice--Oct--3-21-54-20.489-PDT-9
   10       flash:/archive/cdlp-voice--Oct--3-21-59-16.539-PDT-10 <- Most Recent
```

----------------------------------------------------------------

Then run the following to restore archive 9:

```bash
configure replace flash:/archive/cdlp-voice--Oct--3-21-54-20.489-PDT-9
This will apply all necessary additions and deletions
to replace the current running configuration with the
contents of the specified configuration file, which is
assumed to be a complete configuration, not a partial
configuration. Enter Y if you are sure you want to proceed. ? [no]: y
```

This rolls back to the selected configuration without a reboot. The replacement is loaded into the running configuration only, so do a `wr mem` to save it.

----------------------------------------------------------------

## Create these diff aliases

The commands can be hard to remember. The aliases are created in config mode.

```text
alias exec adiff show archive config differences system:running-config
alias exec sdiff show archive config differences
```

----------------------------------------------------------------

## Show config differences

Show the difference between the running configuration and the startup configuration. In this case I changed the description on Gi1/0/19:

```text
sdiff
interface GigabitEthernet1/0/19
 -description < Data/Voice >
```

----------------------------------------------------------------

Compare the running configuration to an archive file:

```text
adiff flash:/archive/CDLP-Office--Oct--2-20-37-33.862-PDT-2
!Contextual Config Diffs:
crypto pki certificate chain SLA-TrustPoint
 +certificate ca 01 nvram:CiscoLicensi#1CA.cer
crypto pki certificate chain TP-self-signed-1749111093
 +certificate self-signed 01 nvram:IOS-Self-Sig#3.cer
interface Vlan10
 +ip pim sparse-dense-mode
interface Vlan20
 +ip pim sparse-dense-mode
-ip igmp snooping querier
```

This shows the differences between the running configuration and `flash:/archive/CDLP-Office--Oct--2-20-37-33.862-PDT-2`.

**Reading the markers:** `adiff` compares the running config (first) to the archive (second), so the markers show what would change to turn the running config back into the archive:

- `+` - the line is in the archive but not in the running config (it was **removed** since the archive was saved)
- `-` - the line is in the running config but not in the archive (it was **added** since the archive was saved)

In this example, `ip pim sparse-dense-mode` was removed from the SVIs and `ip igmp snooping querier` was added after the archive was saved.

----------------------------------------------------------------

## Remove archives

An archive file doesn't use much storage, but if you want to clear the switch for eBay or remove the archives:

```text
delete /force /recursive flash:/archive
mkdir flash:/archive
conf t
 archive
  no path flash:/archive/$h-
  path flash:/archive/$h-
  no write-memory
  write-memory
 end
wr mem
show archive
```

Removing and re-entering `path` clears the in-memory list. Without it, the switch keeps trying to delete the old files on every save and logs a `No such file or directory` error until they roll off.
