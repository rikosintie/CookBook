<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Configuring ARP-Protect #

ARP-Protect uses the IP to Mac address mapping created by DHCP Snooping to prevent L2 Man in The Middle attacks. It takes a bit more planning 
than DHCP Snooping because any device with a static IP address requires additional configuration before it can communicate on the network.

**Global Configuration**

```
HP-2920-24G-PoEP(config)# arp-protect
HP-2920-24G-PoEP(config)# arp-protect vlan 3-6 
```

Each interface to another switch requires arp-protect trust to be added.
```
interface A21
   dhcp-snooping trust
   name "Uplink to IDF-B NW"
   arp-protect trust
   exit
```

**Devices with static IPs require additional configuration**

You can add "arp-protect trust" to the interface or manually configure a static IP to Mac binding. It's better to create the static binding since 
adding the "arp-protect trust" bypasses the protection. This is especially important if an unmanaged switch is connected to the port and 
multiple devices are connected.

**Adding an IP-to-MAC binding to the DHCP database**

When DHCP Snooping is enabled the switch maintains a DHCP binding database, which is used for DHCP and ARP packet validation. 
Both the DHCP snooping and DHCP Option 82 insertion features maintain the lease database by learning the IP-to-MAC bindings 
on untrusted ports. Each binding consists of the client MAC address, port number, VLAN identifier, leased IP address, and lease time.

**Adding a static binding**

To add the static IP-to-MAC binding for a port to the database, enter the "ip source-binding" command at the global configuration level. 
Use the no form of the command to remove the IP-to-MAC binding from the database.

Syntax:
```
[no]ip source-binding <mac-address> vlan <vlan-id> <ip-address> interface <port-number>
```

**Sample configuration**
```
HP-2920-24G-PoEP(config)# ip source-binding 9457a5-133e94 interface vlan 3 10.20.3.47 interface 1/B7
```
or

```
interface 1/B7
   name "HP Printer 10.20.3.47 9457a5-133e94"
   arp-protect trust
   exit
```   

If you miss a device when updating the configuration it will be knocked off the network. You can quickly find these devices using the 
debug command and looking at the log. 


**Show command**
```
show arp-protect statistics 4-6
```
**Debugging**
```
debug arp-protect
show logging
```
Here is a sample of a log entry for a denied device.
```
0003:00:33:01.61 DARP mIpPktRecv:Deny ARP Req 3c9bd6-6d1604,10.20.100.224 port
   C20, vlan 23
```

