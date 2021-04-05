# Password recover on Provision switches #

## Method 1 ##
Using a paper clip, press the reset button on the front of the switch for 7-10 second. The switch will reboot and the password will be cleared.

If that doesn't work, try method 2

## Method 2 ##

Connect a console cable
* Power cycle the switch
* Press and hold the 0 (zero) key until you see "=>"

The switch uses standard Linux commands.

**View the directories**
```
=>ls/
cfa0/
```

**Change to the cfa0 directory**
```
=>cd cfa0
```

**List files in cfa0**
```
=>ls
System Volume Information/
PRIMARY.SWI
secondary.swi
WC_16_10_0009.swi
resetCause.log
tmp/
boot.ini
ssh/
tr69_log
core/
rbtcnt
cfg/
systime
crash/
iflags
last_login
mgrinfo.txt
```
**Remove the mrginfo.txt file**
```
=>rm mgrinfo.txt
```

Verify that the file was removed
```
=>ls
System Volume Information/
PRIMARY.SWI
secondary.swi
WC_16_10_0009.swi
resetCause.log
tmp/
boot.ini
ssh/
tr69_log
core/
rbtcnt
cfg/
systime
crash/
iflags
last_login
=>
```

**Power cycle the switch.
When the switch reboots you will be prompted to enter the new credentials.**

```
Configure the manager credentials :
Username: admin
Password:
Re-enter the new password:
Your previous successful login (as manager) was on 2020-10-02 10:21:58
 from the console
```

Refereence
[Aruba Commnunity](https://community.arubanetworks.com/browse/articles/blogviewer?blogkey=e641a8e4-7cd1-498c-888d-835e4e01ec51)
