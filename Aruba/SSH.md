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
