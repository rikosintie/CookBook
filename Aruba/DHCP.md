# Configuring DHCP on a Provision switch #

In this example I need to configure some Aruba access points. I was in the burn in room and used the switch I was connecting the APs to as a DHCP server.

The lease time is set to 4 hours. 

DHCP-Server configuration

```
dhcp-server pool "Wireless-mgmt"
   authoritative
   default-router "10.112.254.254"
   dns-server "1.1.1.1,9.9.9.9"
   domain-name "rowusd.org"
   lease 00:04:00
   network 10.112.254.0 255.255.255.0
   range 10.112.254.150 10.112.254.250
   exit
   
dhcp-server conflict-logging
dhcp-server enable
```
**Vlan configuration**

The command "dhcp-server" must be added to the vlan tha the server will provide addresses for

```
vlan 254
   name "Device Management"
   tagged A1-A8,B1-B8,C1-C12,C14-C24,D1-D23
   untagged C13
   ip address 10.112.254.254 255.255.255.0
   dhcp-server
   exit
```


