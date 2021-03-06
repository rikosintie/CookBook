# Configure DHCP Snooping 
[HPE DHCP Snooping](https://techhub.hpe.com/eginfolib/networking/docs/switches/WB/15-18/5998-8152_wb_2920_asg/content/ch11s02.html)

The authorized server is the authorized DHCP server for the organization. If there are multiple servers insert multiple lines with one IP on each line.

In this example:

* Authorized DHCP Server - 10.20.20.26
* Vlans to enable DHCP Snooping on - 2, 4-10, 15

```
HP-2920-24G-PoEP(config)# dhcp-snooping
HP-2920-24G-PoEP(config)# dhcp-snooping authorized-server 10.20.20.26
HP-2920-24G-PoEP(config)# no dhcp-snooping option 82
HP-2920-24G-PoEP(config)# dhcp-snooping vlan 2 4-10 15
```

The port that the DHCP server is connected to must be "trusted".
Ports to downstream switches must also be trusted. 


In this example, ports 4-7 are connected to the DHCP server and three downstream switches.

```
dhcp-snooping trust 4,5,6,7
```

## DHCP binding database 
DHCP snooping maintains a database of up to 8192 DHCP bindings on untrusted ports. Each binding consists of:

* Client MAC address
* Port number
* VLAN identifier
* Leased IP address
* Lease time

The switch can be configured to store the bindings at a specific URL so they will not be lost if the switch is rebooted. 
If the switch is rebooted, it will read its binding database from the specified location. To configure this location use this command.

**Syntax:**

[no]dhcp-snooping database [file <tftp://<ip-address>/<ascii-string>>][delay <15-86400>][timeout <0-86400>]

* file - Must be in Uniform Resource Locator (URL) format – “tftp://ip-address/ascii-string”. The maximum filename length is 63 characters.
* delay - Number of seconds to wait before writing to the database. Default = 300 seconds.
* timeout -Number of seconds to wait for the database file transfer to finish before returning an error. A value of zero (0) means retry indefinitely. Default = 300 seconds.

A message is logged in the system event log if the DHCP binding database fails to update. 
To display the contents of the DHCP snooping binding database, enter this command.

**Show Commands**
```
show dhcp-snooping
show dhcp-snooping binding
show dhcp-snooping stats
```

## Enabling debug logging
To enable debug logging for DHCP snooping, use this command.

**Syntax:**

[no]debug security dhcp-snooping [agent|event|packet]

* agent - Displays DHCP snooping agent messages.
* event - Displays DHCP snooping event messages.
* packet - Displays DHCP snooping packet messages.

**A tool for conducting a DHCP Exhaution attack**
DHCPig is a free, open source tool for testing DHCP security. It's worth your time to grab it, set up a lab, and run wireshark
while you test your DHCP security. DHCP fingerprinting is a key component in modern security components like Clearpass. The more you know about
DHCP the better. 

* [DHCPig](https://github.com/kamorin/DHCPig)
* [Defcon Presentation slides on DHCP Finger Printing](https://www.defcon.org/images/defcon-19/dc-19-presentations/Bilodeau/DEFCON-19-Bilodeau-FingerBank.pdf)
* [Defcon Video Presentation on DHCP Finger Printing](https://av.tib.eu/media/40610)
