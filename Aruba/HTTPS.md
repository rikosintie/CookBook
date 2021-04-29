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

If `crypto host-cert` is not found use the following. Change the values to match your deployment.

``` 
crypto pki enroll-self-signed certificate-name MPH-COMP-5412 key-type rsa key-size 2048  
subject common-name Staff Org RIM-USD Org-unit IT State CA Country US Locality Lake_Arrowhead 
valid-start 04/28/2021 valid-end 04/28/2025
```
You probably won't be able to paste that as one long command. My experience is that you will have to type the entire line.

To show the identity-profile:
```
show crypto pki identity-profile
Switch Identity:
  ID Profile Name    : Default
    Common Name (CN) : Staff
    Org Unit (OU)    : IT
    Org Name (O)     : RIM-USD
    Locality (L)     : Lake_Arrowhead
    State (ST)       : CA
    Country (C)      : US
```

To show the certificate
```
show crypto pki local-certificate
   Name                 Usage         Expiration     Parent / Profile
   -------------------- ------------- -------------- --------------------
   MPH-COMP-5412        Web           2025/04/28     default
```

**Disable http and enable https:**
```
no web-management
web-management ssl
```

To remove all all certificates, related private keys, ta profiles and switch identity profiles, enter
`crypto pki zeroize`

**Remove weak ciphers**

The first thing to do is disable TLS 1/0/1.1

From global configuration mode

```
tls application web-ssl lowest-version ?
 default               Configure the default version of TLS1.1 as the lowest version of TLS for the specified application
 tls1.0                Configure TLS1.0 as the lowest version of TLS for the specified application
 tls1.1                Configure TLS1.1 as the lowest version of TLS for the specified application
 tls1.2                Configure TLS1.2 as the lowest version of TLS for the specified application
```
Select tls1.2

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
