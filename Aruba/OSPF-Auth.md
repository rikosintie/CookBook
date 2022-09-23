# Adding Authentication to OSPF #

Without authenticaton enabled an attacker with access to the LAN could become part of the OSPF process and ultimately a Machine in the Middle. 

If this occurs, routing could be altered or a Denial of Service performed.

To prevent this you should always use authentication when deploying routing protocols.

## Background ##

**Types of Authentication** </br>
There are three different types of authentication available for OSPF version 2:
* Null authentication: Null authentication means that there is no authentication, which is the default.
* Clear text authentication: In this method of authentication, passwords are exchanged in clear text on the network
* Cryptographic authentication: This cryptographic method uses the open standard MD5 (Message Digest type 5) encryption.


**Enabling OSPF Authentication** </br>
OSPF authentication can be enabled in two ways:
* Per interface: Authentication is enabled per interface using the "md5-auth-key-chain" command.
* Area authentication: Authentication for area can enable using "area authentication" command.


## Prcedure ##

The first step is to create a keychain name:password pair. </br>

```
key-chain "OSPF-Auth"
key-chain "OSPF-Auth" key 1 key-string "O$FPkey4HQ"
```
Note that you can have more than one key-chain. In this example, the key-chain name is OSPF-Auth. </br>

**Why would you need more than one key-chain**
</br>
You may have vendors that need access to a specific OSPF interface or you may have organizational requirements.

**Add authentication to vlan 850** </br>
```dart
vlan 850
   name Uplink
   untagged  1/A1-1/A4
   ip address 10.251.34.122 255.255.255.252
   exit

router ospf
   area backbone
   redistribute connected route-map SITE51
   enable
   exit

route-map SITE51 permit seq 10
   set tag 850
   exit

vlan 850
   ip ospf 10.251.34.122 area backbone
   ip ospf 10.251.34.122 network-type point-to-point
   ip ospf 10.251.34.122 md5-auth-key-chain "OSPF-Auth"
   exit
```

**Remember that you have to configure authentication on both routers for the neighborship to come up**  </br>

You can run `show logging -r` to check on the state. Here are the logs on the fisrt switch before I added authentication to the second switch. </br>

```
E 11/05/21 22:45:27 03132 OSPF: ST1-CMDR: RECV: Discarding packet on interface
            vl850 : Invalid authentication type (5 times in 60 seconds)
```

As soon as I added authentication to the second switch </br>
```
I 11/05/21 22:47:22 02703 OSPF: ST1-CMDR: ADJCHG: Neighbor with Router ID
            10.251.34.121 on vlan-850 moved to Full state - adjacency formed.
```

## Show Commands </br>
I created some aliases and added them to my template for OSPF. These make it fast and easy to review the OSPF state. </br>

```
alias "ospf" "sh ip ospf interface VLAN 850"
alias "ospfne" "sh ip ospf neighbor"
alias "ospfext" "sh ip ospf external-link-state"
```
**Using the aliases** </br>

Show the OSPF interface state</br>
```dart
test# ospf

 OSPF configuration and statistics for VLAN 850

 OSPF Interface Status for 10.251.34.122

  IP Address      : 10.251.34.122       Status  : enabled 
  Area ID         : backbone       

  State  : Point-to-point               Auth-type : md5   
  Cost   : 1                            Chain     : COR                        
  Type   : Point-to-point               Priority  : n/a

  Transit Delay     : 1                 Retrans Interval  : 5   
  Hello Interval    : 10                Rtr Dead Interval : 40        
  Designated Router : n/a               Events            : 0         
  Backup Desig. Rtr : n/a               Passive           : no 
  Neighbors         : 1         
```
**Show the OSPF neighbors** </br>
```dart
test # ospfne

 OSPF Neighbor Information

  Router ID       Pri IP Address      NbIfState State    QLen  Events Status
  --------------- --- --------------- --------- -------- ----- ------ ------
  10.251.34.121   n/a 10.251.34.121   n/a       FULL     0     7      None  
```
**Show OSPF external link state** </br>
```dart
test# # ospfext

 OSPF External LSAs

  Link State ID   Router ID       Age  Sequence #  Checksum  
  --------------- --------------- ---- ----------- ----------
  10.70.16.0      10.70.16.200    1655 0x80000001  0x00003297
  10.70.17.0      10.70.16.200    1656 0x80000001  0x000027a1
  10.70.18.0      10.70.16.200    1656 0x80000001  0x00001cab
 ```
 **There are several other OSPF show commands you can experiment with**</br>
 ```
 test# sh ip ospf 
 area                  Show the details of all the OSPF areas configured on the device.
 external-link-state   Show the Link State Advertisements from throughout the areas to which the device is attached.
 general               Show OSPF basic configuration and operational information.
 interface             Show OSPF interfaces' information.
 link-state            Show all Link State Advertisements from throughout the areas to which the device is attached.
 neighbor              Show the devices that have established a neighbor relationship with this device.
 redistribute          List protocols which are being redistributed into OSPF.
 restrict              List routes which will not be redistributed via OSPF.
 spf-log               List the OSPF SPF(Shortest Path First Algorithm) run count for all OSPF areas and last ten Reasons for running SPF.
 statistics            List OSPF packet statistics( OSPF sent,recieved and error packet count) of all OSPF enabled interfaces.
 traps                 Show OSPF traps enabled on the device.
 ```
 ## References ##
 
[How To Configure MD5 Authentication For OSPF](https://community.arubanetworks.com/blogs/esupport1/2020/04/30/how-to-configure-md5-authentication-for-ospf-in-multi-os-environment)

