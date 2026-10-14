# Smart Install

Smart Install has been used in some very public Telco breaches By `Static Tundra`. Cisco's 
Talos group published a good blog on the [attack](https://blog.talosintelligence.com/static-tundra/).

It covers how the group exploits CVE-2018-0171 in the Smart Install feature of Cisco IOS, 
a flaw patched in 2018, going after unpatched and end-of-life devices to steal configs and 
keep persistent access. Talos links Static Tundra to the FSB’s Center 16 unit and says it 
has been running long-term intelligence operations for over a decade. Its targets since 
2015 include telecommunications, higher education and manufacturing.

---

## Smart Install on XE

Cisco has removed Smart Install from later versions of IOS-XE. 
On older switches (2960, 3750 on 15.x) disable it with `no vstack`.

To verify Smart Install is not running:

```text
show tcp brief all | include 4786
#
```

No output means Smart Install isn't listening.

---

## Smart Install Exploit Tool

There is also a python tool on GitHub called [Smart Install Exploit Tool](https://github.com/Sab0tag3d/SIETpy3). 
I used it back in 2018 to show customers how vulnerable they were. One customer had a 2950 
directly connected to the Internet. In a great Irony, ATT emailed them and told them they were vulnerable! 
That's how I got involved. Then a decade later ATT gets breached with the same CVE!
