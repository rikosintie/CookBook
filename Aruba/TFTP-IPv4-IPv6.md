# Using IPv6 to make life easy</br>
An IPv6 Link local address is assigned automatically when the switch boots. You can use this to upgrade firmware or configure the switch. Unlike IPv4, no DHCP server is needed.</br>

### Setup</br>
Connect an Ethernet cable to the switch using one of the front panel ports.</br>
Connect the other end of the Ethernet cable to your laptop. 

### Use LLDP to find the switch IPv6 Address</br>
In my case, I use Linux or macOS and I have the lldp daemon running. 

If you are on Windows can install [Open LLDP Client for Windows](https://github.com/chall32/LDWin)

If you can't run an lldp daemon then you can console into the switch to find the address. 

Here is the output of the **lldp** daemon on my MacBook:
```
┌─[mhubbard@HP8600-4] - [/private/tftpboot] - [2683]
└─[$] lldpcli                                                                             [10:46:46]
[lldpcli] # sh ne
-------------------------------------------------------------------------------
LLDP neighbors:
-------------------------------------------------------------------------------
Interface:    en12, via: LLDP, RID: 2, Time: 0 day, 00:16:06
  Chassis:
    ChassisID:    mac 10:4f:58:f0:e3:40
    SysName:      Aruba-2930F-48G-740W-PoEP-4SFPP
    SysDescr:     Aruba JL558A 2930F-48G-740W-PoE+-4SFP+ Switch, revision WC.16.07.0003, ROM WC.16.01.0005 (/ws/swbuildm/rel_xanadu_zeus_qaoff/code/build/lvm(swbuildm_rel_xanadu_zeus_qaoff_rel_xanadu_zeus))
    MgmtIP:       fe80::124f:58ff:fef0:e340
    Capability:   Bridge, on
    Capability:   Router, off
  Port:
    PortID:       local 1
    PortDescr:    1
    TTL:          120
  Unknown TLVs:
    TLV:          OUI: 00,16,B9, SubType: 2, Len: 2 00,01
```

You can see that the switch has link local address fe80::124f:58ff:fef0:e340.</br>


### Use THC-IPv6 to find the IPv6 address</br>
There is a great IPv6 pentesting project called [thc-ipv6](https://github.com/vanhauser-thc/thc-ipv6)</br>

It has a tool called `detect-new-ip6` that listens for ICMP6 DAD (duplicate address detection) packets and displays them. 

I had it running in a terminal on Linux laptop and it displayed the address when the switch started. This output is from a second switch that was connected to my Linux laptop. Unfortunately, THC only works on Linux.

```
┌─[mhubbard@1S1K-G5-5587] - [~/Insync/michael.hubbard999@gmail.com/Google Drive/04_Tools/thc-ipv6] - [633]
└─[$] sudo detect-new-ip6 enp60s0                                                                                                                    [10:41:23]
Started ICMP6 DAD detection (Press Control-C to end) ...
Detected new ip6 address: fe80::66e8:81ff:fe17:3280
```

THC-IPv6 also includes a tool called `alive6` that quickly scans for link local addresses. Very useful if you connect several switches and want to find the ipv6 address.

The Kali guys have some examples of how to use thc-ipv6 at [THC-IPV6 Package Description](https://tools.kali.org/information-gathering/thc-ipv6)

The tools require root access, hence the sudo.


### SSH to the switch</br>
On Linux/macOS, ssh is built in. On the Windows you can use Putty or your favorite GUI tool.

```
┌─[mhubbard@HP8600-4] - [~/GoogleDrive/04_Tools/elpscrk] - [2685]
└─[$] ssh fe80::124f:58ff:fef0:e340%en12                                                  [11:00:33]
The authenticity of host 'fe80::124f:58ff:fef0:e340%en12 (fe80::124f:58ff:fef0:e340%en12)' can't be established.
RSA key fingerprint is SHA256:3png59nIWVKJUf7YefNQ8eI7bNzTdb/BJFY8FNZpKR4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'fe80::124f:58ff:fef0:e340%en12' (RSA) to the list of known hosts.
```

### To copy firmware to the primary flash</br>

My MacBook has IPv6 link local address fe80::88d:6ca7:81cb:18f2.</br>

`
copy tftp flash fe80::88d:6ca7:81cb:18f2%vlan1 WC_16_10_0016.swi primary 
`</br>
Note that you have to add %vlan1 to the end of the address. This tells the switch to use the link local address on vlan1 for tftp.

When that completes, copy the firmware to the secondary flash

`Aruba-2930F-48G-740W-PoEP-4SFPP# copy flash flash sec`

Now boot to the new firmware

`boot system flash primary`


















