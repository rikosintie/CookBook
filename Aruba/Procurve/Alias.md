<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Using Aliases

Aliases are a great time saver if you run the same commands on a regular basis.

To create an alias:
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

To view alias configurations

```
sh run | i alia
alias "wr" "write memory"
alias "ipb" "sh interfaces br | i Up"
```

To show aliases along with the command

```
show alias

  Name                             Command                                      
 -------------------------------- ---------------------------------------------
 sa                               show alias                                   
 tl                               terminal length 40                           
 tw                               terminal width 150                           
 aaa                              show run | in aaa                            
 uid                              chassislocate member $1 blink                
 cppm                             show port-access clients                     
 s-ii                             show ip                                      
 user                             sh ip ssh strict                             
 vlan                             show vlan custom id name:15 ipaddr ipmask ...
 s-dns                            show ip dns                                  
 dhcpsb                           show dhcp-snooping binding                   
 dhcpss                           show dhcp-snooping stats                     
 s-lans                           show vlan custom id name:15 ipaddr ipmask:...
 s-name                           show name | i [a-z] | [A-Z]                  
 s-snmp                           show run | in snmp                           
 uidoff                           chassislocate member $1 off                  
 s-power                          sh power br | i Delivering                   
 s-vlans                          show ip                                      
 debug-on                         debug destination session                    
 debug-off                        no debug destination session                 
 s-ospf-ne                        sh ip ospf neighbor                          
 s-ospf-ext                       sh ip ospf external-link-state               
 s-ospf-int                       sh ip ospf interface                         
 s-power-br                       show power brief                             
 s-int-trans                      show interfaces transceiver                  
 s-int-status                     show interface status                        
 create-backup                    copy config config1 config backup            
 s-backup-conf                    show config backup                           
 s-bcast-limit                    show rate-limit bcast $1                     
 s-diff-backup                    cfg-restore flash backup diff                
 s-arp-throttle                   show ip arp-throttle                         
 s-icmp-settings                  show ip icmp                                 
 s-instrument-monitor             show instrumentation monitor                 
 s-instrument-routing             show instrumentation routing  
```

Here are some aliases that I include in most Aruba legacy switches

```
alias "s-dns" "show ip dns"
alias "s-lans" "show Vlan custom id name:15 ipaddr ipmask:17 ipconfig state jumbo"
alias "s-name" "show name | i [a-z] | [A-Z]"
alias "s-snmp" "show run | in snmp"
alias "s-power" "sh power br | i Delivering"
alias "s-vlans" "show ip"
alias "s-ospf-ne" "sh ip ospf neighbor"
alias "s-ospf-ext" "sh ip ospf external-link-state"
alias "s-ospf-int" "sh ip ospf interface"
alias "s-power-br" "show power brief"
alias "s-int-trans" "show interfaces transceiver"
alias "s-int-status" "show interface status"
alias "s-backup-conf" "show config backup"
alias "s-bcast-limit" "show rate-limit bcast $1"
alias "s-arp-throttle" "show ip arp-throttle"
alias "s-icmp-settings" "show ip icmp"
alias "s-ii" "show ip"
alias "s-instrument-monitor" "show instrumentation monitor"
alias "s-instrument-routing" "show instrumentation routing"
alias "aaa" "show run | in aaa"
alias "cppm" "show port-access clients"
alias "diff-backup" "cfg-restore flash backup diff"
alias "create-backup" "copy config config1 config backup"
alias "debug-on" "debug destination session"
alias "debug-off" "no debug destination session"
alias "dhcpsb" "show dhcp-snooping binding"
alias "dhcpss" "show dhcp-snooping stats"
alias "sa" "show alias"
alias "tl" "terminal length 40"
alias "tw" "terminal width 150"
alias "uid" "chassislocate member $1 blink"
alias "uidoff" "chassislocate member $1 off"
alias "user" "sh ip ssh strict"
alias "Vlan" "show Vlan custom id name:15 ipaddr ipmask ipconfig state voice jumbo"
```

**Example Output**</br>

**show ip**</br>

```
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

  **show power-over-ethernet brief**

  
  ```
  spb

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
**show vlans custom id ipaddr ipmask state**

```
vlan

Status and Counters - VLAN Information - Custom view

 VLANID IP Addr                                        IP Mask         State
 ------ ---------------------------------------------- --------------- -----
 1      fe80::8e85:c1ff:fe51:f84a%vlan1                /64             Down 
 10     10.50.32.200                                   255.255.255.0   Down 
 20     10.50.184.200                                  255.255.255.0   Down 
 25     10.50.11.200                                   255.255.255.0   Down 
 
 ```
 
 **Turn chassis LED on**
 
 To enable the LED on member 1:
 ```
uid 1         
uidoff 1

If the switch isn't stacked, modify the alias:
alias "uid" "chassislocate blink"
alias "uidoff" "chassislocate off"
```
I have been adding `stacking enable` to my template so that aliases and interface names are consistent between stacks and standalone switches. </br>

For example, with stacking disabled, a ProCurve 2930 will refer to the first copper port as "1". 

With stacking enabled, it will refer to it as 1/1. 

I can use logic in my Jinja2 template to handle the differences, but I have found that customers coming from Cisco prefer 1/1 over 1 anyway.

 
 ## Usage Notes ##
 You can use the normal "|" commands with the alias. For example, I wanted to see just VoIP devices that were profiled with cppm: </br>
 
 ```
2930# cppm | i Vo
  1/9   24d9213a95d2  24:d9:21:3a:87:d2 10.233.12.42     *COR_WIRED_VoI... MAC   20                 
  1/19  24d9213a9588  24:d9:21:3a:87:88 10.233.12.61     *COR_WIRED_VoI... MAC   20
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
  
  
