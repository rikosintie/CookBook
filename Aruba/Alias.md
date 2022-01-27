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
alias "pwr" "sh power br | i Delivering"
alias aaa "show run | in aaa"
alias "cppm" "show port-access clients"
alias "user" "sh ip ssh strict"
alias snmp "show run | in snmp"
alias vlan "show vlans custom id ipaddr ipmask state"
alias "uid" "chassislocate member $1 blink"
alias "uidoff" "chassislocate member $1 off"
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

If the switch isn't stacked modifiy the alias:
alias "uid" "chassislocate blink"
alias "uidoff" "chassislocate off"
```
I have been adding `stacking enable` to my template so that aliases and interface names are consistent between stacks and standalone switches. </br>

For example, with stacking disabled, a Procurve 2930 will refer to the first copper port as "1". 

With stacking enabled, it will refer to it as 1/1. 

I can use logic in my Jinja2 template to deal with the differences but I have found customers coming from Cisco like 1/1 better that 1 anyway.

 
 ## Usage Notes ##
 You can use the normal "|" commands with the alias. For exmaple, I wanted to see just VoIP devices that were profiled with cppm: </br>
 
 ```
2930# cppm | i Vo
  1/9   24d9213a95d2  24:d9:21:3a:87:d2 10.233.12.42     *COR_WIRED_VoI... MAC   20                                                     
  1/19  24d9213a9588  24:d9:21:3a:87:88 10.233.12.61     *COR_WIRED_VoI... MAC   20     
```
</br>
You can can also use standard Linux regex in the pipe command. In this example, I wanted to display just ports 3/46, 3/47, 3/48. I used square brackets [] and then entered 6-8 in the brackets. You can see that it returned only the ports I was interested in.
</br>
</br>

```
2930# sh int status | i 3/4[6-8]
  3/46                Up      Auto          1000FDx  100/1000T  20     10      
  3/47                Down    Auto          1000FDx  100/1000T  20     10      
  3/48                Down    Auto          1000FDx  100/1000T  20     10  
```
  
  
