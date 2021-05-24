# Configure DHCP Snooping 
[HPE DHCP Snooping](https://techhub.hpe.com/eginfolib/networking/docs/switches/WB/15-18/5998-8152_wb_2920_asg/content/ch11s02.html)

The authorized server is the authorized DHCP server for the organization. If there are multiple servers insert multiple lines with one IP on each line.

In this example:

* Authorized DHCP Server - 10.112.47.5
* Authorized DHCP Server - 10.112.105.254
* Authorized DHCP Server - 10.112.250.254
* Vlans to enable DHCP Snooping on - 1 105 253-254

```
HP-2920-24G-PoEP(config)# dhcp-snooping
HP-2920-24G-PoEP(config)# dhcp-snooping authorized-server 10.112.47.5
HP-2920-24G-PoEP(config)# dhcp-snooping authorized-server 10.112.105.254
HP-2920-24G-PoEP(config)# dhcp-snooping authorized-server 10.112.250.254
HP-2920-24G-PoEP(config)# no dhcp-snooping option 82
HP-2920-24G-PoEP(config)# dhcp-snooping vlan 1 105 253-254
```

The port that the DHCP server is connected to must be "trusted".
Ports to downstream switches must also be trusted. 


In this example, ports 8-10 are connected to the DHCP server and the upstream switche.

```
dhcp-snooping trust 8-10
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
## Example Show Commands 

 **show dhcp-snooping** 
```
 DHCP Snooping Information

  DHCP Snooping              : No 

  sh dhcp-snooping 

 DHCP Snooping Information

  DHCP Snooping              : Yes
  Enabled VLANs              : 1 105 254                                
  Verify MAC address         : Yes
  Option 82 untrusted policy : drop   
  Option 82 insertion        : No 
  Store lease database       : Not configured

  Authorized Servers
  ------------------
  10.112.47.5       
  10.112.105.254    
  10.112.250.254    
 

                  Max     Current Bindings
   Port  Trust  Bindings  Static   Dynamic
  -----  -----  --------  ----------------
    8.    Yes.     -        -        -
    9     Yes      -        -        -   
    10    Yes      -        -        -   

  Ports 1-7 are untrusted
``` 
 
 **show dhcp-snooping bin**
```
  MacAddress    IP              VLAN Interface Time Left
  ------------- --------------- ---- --------- ---------
  185e0f-a0c2de 10.112.44.83    1    1         84226    
  346895-1a3fb2 10.112.44.164   1    2         85616    
  0c84dc-5bd789 10.112.105.231  105  1         9813     
  b45d50-ceeb7d 10.112.250.36   254  2         13110 
 ```
 
## Enabling debug logging
To enable debug logging for DHCP snooping, use this command.

If you set the terminal width to 100 or 110, the logging will be easier to read.
`

**Syntax:**

[no]debug security dhcp-snooping [agent|event|packet]

* agent - Displays DHCP snooping agent messages.
* event - Displays DHCP snooping event messages.
* packet - Displays DHCP snooping packet messages.

To display the debug messages on the console
```
test#debug destination ?
 logging               Send debug messages to syslog server.
 session               Print debug messages to terminal.
 buffer                Print debug messages to a buffer in memory.

test# debug destination session
```
If you set the terminal width to 100 or 110 the logging messages will be easier to read:
`terminal width 110`

I also like to set the terminal length to 55 or 60 so that I can see more at one time.
`terminal length 60`

**A tool for conducting a DHCP Exhaution attack**
DHCPig is a free, open source tool for testing DHCP security. It's worth your time to grab it, set up a lab, and run wireshark
while you test your DHCP security. DHCP fingerprinting is a key component in modern security components like Clearpass. The more you know about
DHCP the better. 

* [DHCPig](https://github.com/kamorin/DHCPig)
* [Defcon Presentation slides on DHCP Finger Printing](https://www.defcon.org/images/defcon-19/dc-19-presentations/Bilodeau/DEFCON-19-Bilodeau-FingerBank.pdf)
* [Defcon Video Presentation on DHCP Finger Printing](https://av.tib.eu/media/40610)
