<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Aruba 5412r disable ipv6 on vlan

If any of the following IPv6-enabling commands are configured on a VLAN, IPv6 remains enabled on that VLAN. 

To disable IPv6 on a VLAN, the following commands must be removed from the VLAN's configuration:

* ipv6 enable
* ipv6 address dhcp full [rapid-commit]
* ipv6 address autoconfig
* ipv6 address fe80::<interface-id> link-local
* ipv6 address <prefix>:<interface-id>

If any of the above remain enabled, IPv6 remains enabled on the VLAN and, at a minimum, a link-local unicast address is present.

Example

```
show management
Internet (IPv6) Service

  Interface Name  : DEFAULT_VLAN
  IPv6 Status     : Enabled

  Address    |                                             Address
  Origin     | IPv6 Address/Prefix Length                  Status
  ---------- + ------------------------------------------- -----------
  autoconfig | fe80::8a3a:30ff:fe76:8a00/64                preferred

conf t
(config)# vlan 1
no ipv6 address fe80::8a3a:30ff:fe76:8a00/64
no ipv6 add
no ipv6 address fe80::8a3a:30ff:fe76:8a00 link-local
no ipv6 address dhcp full
```
