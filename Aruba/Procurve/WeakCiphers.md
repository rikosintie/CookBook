Remove weak ciphers

This is one of those things that will change over time.  

**You have to keep up with cybersecurity bulletins from Aruba.**

From global configuration mode, enter the following:

```
no ip ssh cipher aes128-cbc
no ip ssh cipher 3des-cbc
no ip ssh cipher aes192-cbc
no ip ssh cipher aes256-cbc
no ip ssh mac hmac-md5
no ip ssh mac hmac-md5-96
```
This is based on 16.10.0018. I was not able to remove the DHE from it.  

Aruba released 10.16.0020 that does remove it by default.

DHE Key Exchange has been around for decades. The tool D(HE)ater will DoS any device using DHE for SSH or TLS.

Just pip install dheater and you can DoS any type of device.

D(HE)ater - D(HE)ater is an attacking tool based on CPU heating in that it forces the ephemeral variant of Diffie-Hellman key exchange (DHE) in given cryptography protocols (e.g. TLS, SSH). It is performed without calculating a cryptographically correct ephemeral key on the client-side, but with a significant amount of calculation on the server-side. Based on this, a denial-of-service (DoS) attack can be initiated, called D(HE)at attack (CVE-2002-20001).

Go to
https://asp.arubanetworks.com/notifications/subscriptions

To subscribe to Aruba Security notifications
