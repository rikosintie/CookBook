<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>


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
<br/>

**Vlan configuration**

The command "dhcp-server" must be added to the vlan that the server will provide addresses for

```
vlan 254
   name "Device Management"
   tagged A1-A8,B1-B8,C1-C12,C14-C24,D1-D23
   untagged C13
   ip address 10.112.254.254 255.255.255.0
   dhcp-server
   exit
```
<br/>

## Show commands ##

show dhcp-server [binding|conflicts|database|statistics|pool <POOL-NAME>]

**Parameters and options**

binding

* Display the DHCPv4 server address bindings on the device.

conflicts

* Display address conflicts found by a DHCPv4 server when addresses are offered by a client.

database

* Display DHCPv4 server database agent information.

statistics

* Display DHCPv4 server statistics.

pool
 
* <POOL-NAME> Display the DHCPv4 server IP pool information

<br/>
<br/>

## Clear commands ##

### clear dhcp-server conflicts ###

**Syntax**  
clear dhcp-server conflicts <IP-ADDR>

**Description**
* Reset DHCPv4 server conflicts database. If IP address is specified, reset only that conflict.

**Parameters**  
dhcp-server

* Clears the DHCPv4 server information.

ip-addr

* Specify the IP address whose conflict is to be cleared.

### Reset all DHCP server and BOOTP counters ###

**Syntax**  
clear dhcp-server statistics

**Description**

* Reset all DHCP server and BOOTP counters




