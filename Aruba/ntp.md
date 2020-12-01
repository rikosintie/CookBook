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

**Add authentication if the NTP server supports it:**

This example enables NTP authentication, add authentication secret keys into the database, and specifies a subset of keys which are trusted.

```
ntp authenticate
ntp authentication-key <key-id> md5 <key-secret>
ntp trusted-key <key-id>
```

## Show Commands

* show time
* show ntp status
