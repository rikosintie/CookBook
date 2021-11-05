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
```
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

##Remember that you have to configure authentication on both routers for the neighborship to come up##

##Show Commands##</br>

