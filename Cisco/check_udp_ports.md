# Check UDP ports

IP protocol 17 is UDP.

```text
cdlp-office#show udp detail
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17       --listen--          10.100.130.200    123   0   0 2001001   2
 Queues: output 0
         input  0 (drops 0, max 64, highwater 2)
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17(v6)   --listen--          --any--           123   0   0 2020001   2
 Queues: output 0
         input  0 (drops 0, max 64, highwater 0)
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17     0.0.0.0             0 10.100.130.200   2228   0   0   211   0
 Queues: output 0
         input  0 (drops 0, max 64, highwater 0)
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17       --listen--          224.0.1.40        496   0   0 2000061   0
 Queues: output 0
         input  0 (drops 0, max 512, highwater 0)
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17(v6) ::                  0 --any--            53   0   0 2000011   0
 Queues: output 0
         input  0 (drops 0, max 64, highwater 0)
Proto        Remote      Port      Local       Port  In Out  Stat TTY OutputIF
 17     0.0.0.0             0 10.100.130.200     53   0   0 2000011   0
 Queues: output 0
         input  0 (drops 0, max 64, highwater 0)
```

----------------------------------------------------------------

224.0.1.40 is the Cisco-RP-Discovery group, where PIM routers listen for RP (rendezvous point) </br> mapping announcements, and UDP 496 is the port Auto-RP uses. Its counterpart, 224.0.1.39, is </br> Cisco-RP-Announce. It's running because `ip pim sparse-dense-mode` is enabled on the SVIs.

On a Layer 2 switch PIM does nothing useful. Remove it from the SVIs and replace the </br> IGMP querier function it was providing:

```text
interface Vlan10
 no ip pim sparse-dense-mode
interface Vlan20
 no ip pim sparse-dense-mode
!
ip igmp snooping querier
```

UDP 53 in the output is the DNS server (`no ip dns server`). UDP 123 is NTP.
