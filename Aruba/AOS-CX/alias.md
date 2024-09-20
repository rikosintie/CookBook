# Some useful alias commands

The alias names are from Cisco aliases I have used over the years. For example, sis was `show interface status` which doesn't exist on the CX platform. You may want to rename the aliases if you are in an Aruba CX only environment.

**Standalone Switch**
```
alias  cppm show port-access clients
alias snoop show dhcpv4-snooping binding;show dhcpv4-snooping statistics
alias csnoop clear dhcpv4-snooping statisticsalias  cppm show port-access clients 
alias sis show interface br | i $1
alias siib show ip interface brief
alias spb show power brief
alias pwr sh power br | i del
alias auth show run | in aaa
alias snmp1 show run | include snmp
alias cppm show port-access clients
alias uid led locator flashing
alias uidoff  led locator off
```

The `sis` alias has a variable in it `$1` which means it takes whatever is after `sis` and appends it. For exmaple:

```bash linenums='1' hl_lines='1 8'
sis up
1/1/1          1       access 1GbT           yes     up                              1000    --
1/1/17         1       trunk  1GbT           yes     up                              1000    Default-port
1/1/51         1       trunk  1G-SX          yes     up                              1000    Uplink to core 1/3/13
vlan1          --      --     --             yes     up                              --      Old-vlan
vlan254        --      --     --             yes     up                              --      Device-Management

sis down
1/1/2          1       access 1GbT           yes     down    Waiting for link        --      Default-port
1/1/3          1       access 1GbT           yes     down    Waiting for link        --      Default-port
1/1/4          1       access 1GbT           yes     down    Waiting for link        --      Default-port
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
