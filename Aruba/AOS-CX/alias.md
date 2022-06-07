# Some useful alias commands

The alias names are from Cisco aliases I have used over the years. For example, sis was `show interface status` which doesn't exist on the CX platform. You may want to rename the aliases if you are in an Aruba CX only environment.

**Standalone Switch**
```
alias sis show interface br
alias siib show ip interface brief
alias spi show power brief
alias aaa show run | in aaa
alias snmp show run | in snmp
alias cppm show port-access clients
alias uid led loca fla
alias uidoff led loca off
alias pwr sh power br | i del
```

**Switches in a vsf pair**
```
alias sis show interface br
alias siib show ip interface brief
alias spi show power brief
alias aaa show run | in aaa
alias snmp show run | in snmp
alias cppm show port-access clients
alias uid led loca fla vsf memb $1
alias uidoff led loca off vsf memb $1
alias pwr sh power br | i del
```

For the UID aliases the $1 is a variable that represents the first argument after the alias. For example, to turn on the LED of member 2:

```
switch(config)# uid 2
```

Note, on the CX platform you must be in global configuraton mode to use the UID alias.
