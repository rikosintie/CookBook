# The CX line has a dedicated management Ethernet port. 

## Configuring the management port
By default, management interface is set to operate as a DHCP client.</br>

To use a static IP address:

```
8325(config-if-mgmt)# show running-config current-context
interface mgmt
    no shutdown
    ip static 192.168.1.2/24
    default-gateway 192.168.1.1
    nameserver 1.1.1.1 8.8.8.8
```
The mgmt interface is part of the mgmt vrf.</br>

```
8325# ping 192.168.1.2 vrf mgmt          
PING 192.168.1.2 (192.168.1.2) 100(128) bytes of data.
108 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=0.029 ms
108 bytes from 192.168.1.2: icmp_seq=2 ttl=64 time=0.033 ms
108 bytes from 192.168.1.2: icmp_seq=3 ttl=64 time=0.034 ms
108 bytes from 192.168.1.2: icmp_seq=4 ttl=64 time=0.054 ms
108 bytes from 192.168.1.2: icmp_seq=5 ttl=64 time=0.040 ms
```

### A note about lldp on the mgmt interface
LLDP is on by default. A show running-config current-context will not list lldp transmit/receive but you can see that it is working:</br>

```
8325# sh run int mgmt
interface mgmt
    no shutdown
    ip static 192.168.1.2/24
    default-gateway 192.168.1.1
    nameserver 1.1.1.1 8.8.8.8
8325# sh lldp ne

LLDP Neighbor Information 
=========================

Total Neighbor Entries          : 1
Total Neighbor Entries Deleted  : 0
Total Neighbor Entries Dropped  : 0
Total Neighbor Entries Aged-Out : 0

LOCAL-PORT  CHASSIS-ID         PORT-ID                      PORT-DESC                    TTL      SYS-NAME    
-----------------------------------------------------------------------------------------------------------
mgmt        ea:e6:4b:18:de:20  00:1c:c2:53:df:4c            en12                         120      hp8600-4.corp.vectorusa.com        
8325# 
```

If you disable LLDP using the `no lldp transmit`and `no lldp receive` command:

```
8325(config-if-mgmt)# no lldp transmit 
8325(config-if-mgmt)# no lldp receive 
8325(config-if-mgmt)# show running-config current-context
interface mgmt
    no shutdown
    no lldp transmit
    no lldp receive
    ip static 192.168.1.2/24
    default-gateway 192.168.1.1
    nameserver 1.1.1.1 8.8.8.8

8325(config-if-mgmt)# end
8325# sh lldp ne

LLDP Neighbor Information 
=========================

Total Neighbor Entries          : 0
Total Neighbor Entries Deleted  : 0
Total Neighbor Entries Dropped  : 0
Total Neighbor Entries Aged-Out : 0

LOCAL-PORT  CHASSIS-ID         PORT-ID                      PORT-DESC                    TTL      SYS-NAME    
-----------------------------------------------------------------------------------------------------------
```

## The Linux shell
The CX line allows you to drop into the Linux shell. Bash is the default shell. See `customizing the bash shell` </br>
in this repo for information on making shell more user friendly and safer!

To drop into the shell use the `start-shell` command. Here you can see that the mgmt interface is eth0 in the shell.
```
8325# start-shell
8325:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 64:E8:81:1C:6B:01
          inet addr:192.168.1.2  Bcast:0.0.0.0  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:117 errors:0 dropped:0 overruns:0 frame:0
          TX packets:101 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:42962  TX bytes:10259
          Interrupt:16

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Bcast:0.0.0.0  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:88 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:8384  TX bytes:8384
```
List the routing table for the shell</br>
```
8325:~$ ip route 
default via 192.168.1.1 dev eth0 proto static 
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.2
```

List the name servers. Note that we didn't add the IPv6 name servers.</br>
```
8325:~$ cat /etc/resolv.conf | grep name
nameserver 1.1.1.1
nameserver 8.8.8.8
nameserver 1.0.0.1
nameserver 8.8.4.4
nameserver 2606:4700:4700::1111
nameserver 2001:4860:4860::8888
nameserver 2606:4700:4700::1001
nameserver 2001:4860:4860::8844
```

The `list` command dumps all available options in the mgmt context </br>

8325(config-if-mgmt)# list 
* list
* exit
* end
* show running-config current-context
* ip dhcp
* ip static A.B.C.D/M
* no ip static A.B.C.D/M
* ip static A.B.C.D W.X.Y.Z
* no ip static A.B.C.D W.X.Y.Z
* ip static X:X::X:X/M
* no ip static X:X::X:X/M
* default-gateway A.B.C.D
* no default-gateway A.B.C.D
* default-gateway X:X::X:X
* no default-gateway X:X::X:X
* nameserver A.B.C.D
* nameserver A.B.C.D A.B.C.D
* nameserver X:X::X:X
* nameserver X:X::X:X X:X::X:X
* nameserver A.B.C.D X:X::X:X
* nameserver X:X::X:X A.B.C.D
* no nameserver A.B.C.D
* no nameserver A.B.C.D A.B.C.D
* no nameserver X:X::X:X                                       
* no nameserver X:X::X:X X:X::X:X                              
* no nameserver A.B.C.D X:X::X:X                               
* no nameserver X:X::X:X A.B.C.D                               
* shutdown                                                     
* no shutdown                                                  
* lldp transmit                                                
* lldp receive                                                 
* lldp trap enable                                             
* no lldp trap enable                                          
* no lldp transmit                                             
* no lldp receive                              
