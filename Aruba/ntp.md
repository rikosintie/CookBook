# Set up ntp on the switch

 It can take up to 15 minutes for NTP to get a proper sync done when you first enable it.

```
timesync ntp
no sntp
ntp unicast
ntp server 10.20.2.7 iburst
ntp server 129.15.6.29
ntp enable
time daylight-time-rule continental-us-and-canada
time timezone -480
```
## Show Commands

* show time
* show ntp status
