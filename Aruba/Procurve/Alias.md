<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Using Aliases

Aliases are a great time saver if you run the same commands on a regular basis.

## To create an alias:
* Enter global configuration mode
* Enter a name, then the command surrounded by ".

```
test-5412-MDF(config)# alias ipb "sh interfaces br | i Up"

To run the alias

test-5412-MDF#ipb
  B3*          1000SX     | No        Yes     Up     1000FDx    NA   off  0
  B6           SFP+DA3    | No        Yes     Up     10GigFD    NA   off  0
  D21          100/1000T  | No        Yes     Up     1000FDx    MDI  off  0
  D24          100/1000T  | No        Yes     Up     1000FDx    MDIX off  0
```

## To view alias configurations

```
sh run | i alias
alias "wr" "write memory"
alias "ipb" "sh interfaces br | i Up"
```

----------------------------------------------------------------

### To show aliases along with the command

```
show alias

 Name                             Command                                      
 -------------------------------- ---------------------------------------------
 sa                               show alias                                   
 sab                              sh alias | i ^ b-                            
 sas                              sh alias | i ^ s-                            
 sat                              sh alias | i ^ t-                            
 b-tl                             terminal length 40                           
 b-tw                             terminal width 150                           
 s-ii                             show ip                                      
 b-uid                            chassislocate member $1 blink                
 s-aaa                            show run | in aaa                            
 s-dns                            show ip dns                                  
 s-job                            show job                                     
 s-trk                            show trunk                                   
 b-user                           sh ip ssh strict                             
 s-lans                           show vlan custom id name:15 ipaddr ipmask:...
 s-name                           show name | i [a-z] | [A-Z]                  
 s-snmp                           show run | in snmp                           
 t-cppm                           show port-access clients                     
 s-power                          sh power br | i Delivering                   
 s-vlans                          show ip                                      
 b-uidoff                         chassislocate member $1 off                  
 s-ospf-ne                        sh ip ospf neighbor                          
 b-debug-on                       debug destination session                    
 s-ospf-ext                       sh ip ospf external-link-state               
 s-ospf-int                       sh ip ospf interface                         
 s-power-br                       show power brief                             
 s-ssh-user                       sh ip ssh strict                             
 b-debug-off                      no debug destination session                 
 s-int-trans                      show interfaces transceiver                  
 s-lacp-peer                      show lacp peer                               
 t-link-flap                      show fault-finder link-flap                  
 s-dhcp-stats                     show dhcp-snooping stats                     
 s-int-status                     show interface status                        
 s-lacp-local                     show lacp local                              
 s-backup-conf                    show config backup                           
 s-bcast-limit                    show rate-limit bcast $1                     
 s-diff-backup                    cfg-restore flash backup diff                
 t-arp-protect                    show logg -r | i arp-protect                 
 s-arp-throttle                   show ip arp-throttle                         
 s-dhcp-binding                   show dhcp-snooping binding                   
 b-create-backup                  copy config config1 config backup            
 s-icmp-settings                  show ip icmp                                 
 s-broadcast-storm                show fault-finder broadcast-storm            
 t-broadcast-storm                show fault-finder broadcast-storm            
 s-arp-protect-stats              show arp-protect statistics $1               
 s-instrument-monitor             show instrumentation monitor                 
 s-instrument-routing             show instrumentation routing  
```

----------------------------------------------------------------

## Here are some aliases that I include in most Aruba legacy switches

```
alias "sa" "show alias"
alias "sab" "sh alias | i ^ b-"
alias "sas" "sh alias | i ^ s-"
alias "sat" "sh alias | i ^ t-"
alias "b-tl" "terminal length 40"
alias "b-tw" "terminal width 150"
alias "s-ii" "show ip"
alias "b-uid" "chassislocate member $1 blink"
alias "s-aaa" "show run | in aaa"
alias "s-dns" "show ip dns"
alias "s-job" "show job"
alias "s-trk" "show trunk"
alias "b-user" "sh ip ssh strict"
alias "s-lans" "show vlan custom id name:15 ipaddr ipmask:17 ipconfig state jumbo"
alias "s-name" "show name | i [a-z] | [A-Z]"
alias "s-snmp" "show run | in snmp"
alias "t-cppm" "show port-access clients"
alias "s-power" "sh power br | i Delivering"
alias "s-vlans" "show ip"
alias "b-uidoff" "chassislocate member $1 off"
alias "s-ospf-ne" "sh ip ospf neighbor"
alias "b-debug-on" "debug destination session"
alias "s-ospf-ext" "sh ip ospf external-link-state"
alias "s-ospf-int" "sh ip ospf interface"
alias "s-power-br" "show power brief"
alias "s-ssh-user" "sh ip ssh strict"
alias "b-debug-off" "no debug destination session"
alias "s-int-trans" "show interfaces transceiver"
alias "s-lacp-peer" "show lacp peer"
alias "t-link-flap" "show fault-finder link-flap"
alias "s-dhcp-stats" "show dhcp-snooping stats"
alias "s-int-status" "show interface status"
alias "s-lacp-local" "show lacp local"
alias "s-backup-conf" "show config backup"
alias "s-bcast-limit" "show rate-limit bcast $1"
alias "s-diff-backup" "cfg-restore flash backup diff"
alias "t-arp-protect" "show logg -r | i arp-protect"
alias "s-arp-throttle" "show ip arp-throttle"
alias "s-dhcp-binding" "show dhcp-snooping binding"
alias "b-create-backup" "copy config config1 config backup"
alias "s-icmp-settings" "show ip icmp"
alias "s-broadcast-storm" "show fault-finder broadcast-storm"
alias "t-broadcast-storm" "show fault-finder broadcast-storm"
alias "s-arp-protect-stats" "show arp-protect statistics $1"
alias "s-instrument-monitor" "show instrumentation monitor"
alias "s-instrument-routing" "show instrumentation routing"```
```

----------------------------------------------------------------

### Grouping the aliases

I start the alias name with one of these letters:

- b - These are basic commands like sending debug messages to the screen
- s - These are `show commands`
- t - These are troubleshooting commands like `show logg -r | i arp-protect` to display arp-protect messages.

The advantage to this is that the AOS-S switches (Formerly ProCurve) have tab completion. So you can type
`s-`, press tab to see all of the show aliases:

```
s-
 s-power
 s-bcast-limit
 s-power-br
 s-int-status
 s-int-trans
 s-snmp
 s-ospf-ext
 s-lans
 s-ospf-int
 s-ospf-ne
 s-instrument-routing
 s-instrument-monitor
 s-icmp-settings
 s-dns
 s-vlans
 s-arp-throttle
 s-diff-backup
 s-name
 s-ii
 s-dhcp-binding
 s-dhcp-stats
 s-ssh-user
 s-arp-protect-stats
 s-job
 s-lacp-peer
 s-lacp-local
 s-trk
 s-aaa
 s-broadcast-storm
```

or type `t-`, press tab to see all of the troubleshooting aliases:

```
t-
 t-arp-protect
 t-cppm
 t-link-flap
 t-broadcast-storm
```

Finally, type `b-`, press tab to see all of the basic aliases:

```
b-
 b-tl
 b-tw
 b-uid
 b-uidoff
 b-create-backup
 b-user
 b-debug-on
 b-debug-off
```

### Aliases that need a parameter

The `s-bcast-limit` alias uses the symbol `$1` to represent an argument. This is standard Linux BASH Shell style. The command looks
like this:

`alias "s-bcast-limit" "show rate-limit bcast $1"`

To use the alias:

```
s-bcast-limit 5

 Broadcast-Traffic Rate Limit Maximum %

  Port  | Inbound Limit Mode     
  ----- + ------------- ---------
  5     | 1             %        
```

The `%` under Mode means the rate is configured in percent. You can also use Kpps:

```
s-bcast-limit 1

 Broadcast-Traffic Rate Limit Maximum %

  Port  | Inbound Limit Mode     
  ----- + ------------- ---------
  1     | 100           kbps     

```

### Usage Notes ###

You can use the normal "|" commands with the alias. For example, I wanted to see just VoIP devices that were profiled with cppm: </br>
 
 ```
2930# cppm | i Vo
  1/9   24d9213a95d2  24:d9:21:3a:87:d2 10.233.12.42     *WIRED_VoI... MAC   20                 
  1/19  24d9213a9588  24:d9:21:3a:87:88 10.233.12.61     *WIRED_VoI... MAC   20
```
</br>
You can also use standard Linux regex in the pipe command. In this example, I wanted to display just ports 3/46, 3/47, 3/48. I used square brackets [] and then entered 6-8 in the brackets. You can see that it returned only the ports I was interested in.
</br>
</br>

```
2930# sh int status | i 3/4[6-8]
  3/46                Up      Auto          1000FDx  100/1000T  20     10      
  3/47                Down    Auto          1000FDx  100/1000T  20     10      
  3/48                Down    Auto          1000FDx  100/1000T  20     10  
```
 
----------------------------------------------------------------

### Interface descriptions

Cisco has the `show interface description` command so that you can look at interface descriptions. I usually enclose my
interfaces desrciption in <> brackets. For example `< Office Ubiquiti >` so that I can filter on only ports that have descriptions. For example:

```
show int des | i <
Vl10                           up             up       < Management >
Vl11                           up             up       < Servers >
Vl20                           up             up       < Users >
Vl30                           up             up       < Voice >
Vl40                           up             up       < Surveillance >
Vl100                          up             up       < Management >
Gi1/0/1                        up             up       < Office Ubiquiti >
Gi1/0/2                        down           down     < HP z420 >
Gi1/0/3                        up             up       < Fortinet >
Gi1/0/4                        up             up       < MyQ hub >
Gi1/0/5                        up             up       < Hikvision NVR >
Gi1/0/6                        down           down     < HP z420 >
Gi1/0/7                        down           down     < NetGear 5 port >
Gi1/0/25                       down           down     < Drop behind Vizio >
Gi1/0/26                       down           down     < Epson Printer >
Gi1/0/27                       down           down     < Drop by Troy >
Gi1/0/28                       down           down     < Drop by Troy >
Gi1/0/45                       up             up       < Aruba AP in office >
Gi1/0/47                       up             up       < NUC ESXi 8 >
Gi1/0/48                       down           down     <Cisco 2960s>
```

Using that command, I don't see every port, just ports with a `<`. Not so important on a single switch, but on a modular core or a large stack it's much better.

**HPE Procurve Name command**

The ProCurve switches use `name` instead of description. The `<` character isn't allowed. I use this alias and it seems to work as long as the port
name isn't all numeric:

```
alias "s-name" "show name | i [a-z] | [A-Z]"

s-name
 Port Names
  Port  Type       Name                                                        
  1     100/1000T  test                                                        
  2     100/1000T  Ubiquiti-Nano                                               
  3     100/1000T  ArubaAP                                                     
  4     100/1000T  .188-d8d43c-651b3e-camper                                   
  5     100/1000T  Gar-North-.182                                              
  9     100/1000T  North-interior-.194                                         
  15    100/1000T  test                                                        
  18    100/1000T  05-Camper                                                   
  21    100/1000T  IP-Camera05-.185                                            
  22    100/1000T  02-Front-Door                                               
  23    100/1000T  Garage-Center-.193                                          
  24    100/1000T  North-interior-.194          
```

----------------------------------------------------------------

## Example Alias Output

**show ip**</br>

```
alias "s-ii" "show ip"

s-ii

 Internet (IP) Service

  IP Routing : Enabled 


  Default TTL     : 64   
  Arp Age         : 20  
  Domain Suffix   : pu.pri                        
  DNS server      : 192.168.10.222                          

                       |                                            Proxy ARP 
  VLAN                 | IP Config  IP Address      Subnet Mask     Std  Local
  -------------------- + ---------- --------------- --------------- ----------
  DEFAULT_VLAN         | DHCP/Bootp
  User                 | Manual     192.168.10.52   255.255.255.0    No    No
  OCC_DHCP_20          | Manual     10.164.24.200   255.255.255.0    No    No
  voice                | Disabled 
  IOT0                 | Disabled 
  IOT1                 | Disabled 
  IOT2                 | Disabled 
  IOT3                 | Disabled 
  test                 | Manual     10.10.100.1     255.255.255.0    No    No
  OSPF-Peering         | Manual     10.254.34.18    255.255.255.252  No    No
 


Loopback Interface

  Loopback     | IP Config    IP Address      Subnet Mask    
  ------------ + ------------ --------------- ---------------
           lo0 | Manual                1.1.1.1 255.255.255.255
  
  ```

----------------------------------------------------------------


  **show power-over-ethernet brief**

  
  ```
alias "s-power-br" "show power brief"

s-power-br

 Status and Configuration Information

  Member 1 Power
  Available: 740 W  Used: 0 W  Remaining: 740 W

 PoE    Pwr  Pwr      Pre-std Alloc Alloc  PSE Pwr PD Pwr  PoE Port     PLC PLC 
 Port   Enab Priority Detect  Cfg   Actual Rsrvd   Draw    Status       Cls Type
 ------ ---- -------- ------- ----- ------ ------- ------- ------------ --- ----
 1/1    Yes  low      off     usage usage  0.0 W   0.0 W   Searching    0    -  
 1/2    Yes  low      off     usage usage  0.0 W   0.0 W   Searching    0    -  
 1/3    Yes  low      off     usage usage  0.0 W   0.0 W   Searching    0    -  
 
 ```

----------------------------------------------------------------


**show vlan custom id name:15 ipaddr ipmask:17 ipconfig state jumbo**

```
alias "s-lans" "show vlan custom id name:15 ipaddr ipmask:17 ipconfig state jumbo"

s-lans 

Status and Counters - VLAN Information - Custom view

 VLANID VLAN name       IP Addr         IP Mask           IPConfig   State Jumbo
 ------ --------------- --------------- ----------------- ---------- ----- -----
 1      DEFAULT_VLAN                                      DHCP/Bootp Down  No   
 10     User            192.168.10.52   255.255.255.0     Manual     Up    No   
 20     OCC_DHCP_20     10.164.24.200   255.255.255.0     Manual     Down  No   
 60     IOT0                                              Disabled   Up    No   
 61     IOT1                                              Disabled   Up    No   
 62     IOT2                                              Disabled   Up    No   
 63     IOT3                                              Disabled   Up    No   
 100    test            10.10.100.1     255.255.255.0     Manual     Down  No   
 850    OSPF-Peering    10.254.34.18    255.255.255.252   Manual     Down  No   
 30     voice                                             Disabled   Up    No   

 
 ```

----------------------------------------------------------------

 
**Turn chassis LED on**

Turning on the LED is very useful if you are remote and have a person on site helping you. It reduces confusing when all they have to do is 
look for the flashing LED to know which switch you are referring to.
 
To enable the LED on member 1:
 ```
alias "b-uid" "chassislocate member $1 blink"

b-uid 1
```

To disable the LED on member 1:

```
alias "b-uidoff" "chassislocate member $1 off"

uidoff 1
```

**NOTE**</br>
If the switch isn't stacked, modify the alias:

```
alias "uid" "chassislocate blink"
alias "uidoff" "chassislocate off"
```

I have been adding `stacking enable` to my template so that aliases and interface names are consistent between stacks and standalone switches. </br>

For example, with stacking disabled, a ProCurve 2930 will refer to the first copper port as "1". 

With stacking enabled, it will refer to it as 1/1. 

I can use logic in my Jinja2 template to handle the differences, but I have found that customers coming from Cisco prefer 1/1 over 1 anyway.
  
  
