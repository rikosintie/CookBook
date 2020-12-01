# Configure https and remove weak ciphers

**Create the self signed certificate**
```
crypto host-cert generate self-signed bits 2048
```
You will be prompted for:
* Validity start date
* Validity end date
* Common name (Use the switch IP address)
* Organizational Unit [Dept Name]
* Organization [Company Name]
* City
* State Name
* Country Code

**Disable http and enable https:**
```
no web-management
web-management ssl
```

**Remove weak ciphers**

The first thing to do is disable TLS 1/0/1.1
```
tls application web-ssl lowest-version tls1.2 
```

From global configuration mode enter the following to review all available ciphers
```
tls application web-ssl lowest-version tls1.2 disable-cipher ?
```

This example removes the triple DES and RSA ciphers. Google recommends removing all 3Des and RSA based ciphers.

**To remove these ciphers paste the following into global configuration mode:**

*Note: You will be prompted (Y/N) whether you want to kill current TLS sessions.*

```
tls application web-ssl lowest-version tls1.2 disable-cipher des3-cbc-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes256-gcm-sha384
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes128-gcm-sha256
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes256-sha384
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes256-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes128-sha256
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-aes128-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdh-rsa-des-cbc3-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdhe-rsa-aes128-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdhe-ecdsa-aes256-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdhe-rsa-aes256-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdhe-ecdsa-des-cbc3-sha
y
tls application web-ssl lowest-version tls1.2 disable-cipher ecdhe-rsa-des-cbc3-sha
y
```

To verify what ciphers the switch offers use this nmap script:
```
nmap --script ssl-cert,ssl-enum-ciphers -p 443 <switch ip>
```
