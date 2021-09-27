<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Using Aliases

Aliases are a great time saver if you run the same commands on a regular basis.

To create an alias:
* Enter global configuraiton mode
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

To view aliases

```
sh run | i alia
alias "wr" "write memory"
alias "ipb" "sh interfaces br | i Up"
```
Here are some aliases that I include in most Aruba legacy switches

```
alias sis "show interface status"
alias sii "show ip"
alias sid "show name"
alias spb "show power brief"
alias aaa "show run | in aaa"
alias snmp "show run | in snmp"
alias vlan "show vlans custom id ipaddr ipmask state"
```

**Example Output**</br>

**show ip**</br>

```
sii

 Internet (IP) Service

  IP Routing : Disabled

  Default Gateway :                
  Default TTL     : 64   
  Arp Age         : 20  
  Domain Suffix   : example.com               
  DNS server      : 1.1.1.1                            

                       |                                            Proxy ARP 
  VLAN                 | IP Config  IP Address      Subnet Mask     Std  Local
  -------------------- + ---------- --------------- --------------- ----------
  DEFAULT_VLAN         | Disabled 
  User                 | Manual     10.50.32.200    255.255.255.0    No    No
  OpenQuery            | Manual     10.50.33.200    255.255.255.0    No    No
  
  ```

  **show spb**

  
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
**vlan**

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
 
 
  
  
