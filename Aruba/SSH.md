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
```
