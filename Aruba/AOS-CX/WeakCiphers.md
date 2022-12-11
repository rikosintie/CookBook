# Remove weak ciphers

This is one of those things that will change over time. You have to keep up with cybersecurity bulletins from Aruba.

From global configuration mode, enter the following:

```
ssh key-exchange-algorithms curve25519-sha256 curve25519-sha256@libssh.org ecdh-sha2-nistp384
ssh macs hmac-sha2-256 hmac-sha2-512 hmac-sha2-512-etm@openssh.com
ssh public-key-algorithms rsa-sha2-512 ssh-ed25519 ecdsa-sha2-nistp256 ecdsa-sha2-nistp384
```

Here is an example of a script kiddie level attack that will kill any device running DHE.  

Just `pip install dheater` and you can DoS any type of device.  

[D(HE)ater](https://github.com/Balasys/dheater) - D(HE)ater is an attacking tool based on CPU heating in that it forces the ephemeral variant of [Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie–Hellman_key_exchange) (DHE) in given cryptography protocols (e.g. TLS, SSH). It is performed without calculating a cryptographically correct ephemeral key on the client-side, but with a significant amount of calculation on the server-side. Based on this, a [denial-of-service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) attack can be initiated, called [D(HE)at attack](https://dheatattack.com/) ([CVE-2002-20001](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2002-20001)).  

Go to  
[https://asp.arubanetworks.com/notifications/subscriptions](https://asp.arubanetworks.com/notifications/subscriptions)  

To subscribe to Aruba Security notifications
