<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>


## Configure SSH

```
crypto key generate ssh rsa bits 2048
ip ssh
no telnet-server
```

**Disable older ciphers and hmacs**
```
no ip ssh cipher aes128-cbc
no ip ssh cipher 3des-cbc
no ip ssh cipher aes192-cbc
no ip ssh cipher aes256-cbc
no ip ssh mac hmac-md5
no ip ssh mac hmac-md5-96
no ip ssh kex diffie-hellman-group-exchange-sha256
no ip ssh kex diffie-hellman-group14-sha1
```

### Note, this PSA is for Aruba CX switches but the attack works on SSH/TLS with any device that uses DHE key exchange.
```
Diffie-Hellman Key Agreement Protocol Vulnerability (CVE-2002-20001)
  ---------------------------------------------------------------------
    The Diffie-Hellman Key Agreement Protocol allows remote attackers 
    (from the client side) to send arbitrary numbers that are actually 
    not public keys and trigger expensive server-side DHE 
    modular-exponentiation calculations, aka a D(HE)ater attack.
    Successful exploitation of this vulnerability can lead to a
    denial of service on the AOS-CX switch. 
```
## References
* [Diffe-Hellman Heater Attack code](https://github.com/Balasys/dheater)
* [Diffie-Hellman Key Agreement Protocol Vulnerability CVE-2002-20001](https://www.arubanetworks.com/assets/alert/ARUBA-PSA-2022-004.txt)
