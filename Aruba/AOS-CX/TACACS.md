# Configure AOS-CX switches to use TACACS authentication

Clearpass server is at 10.20.100.50

```
tacacs-server key plaintext SuperS3cr3t!
!
!
tacacs-server host 10.10.100.50 tracking enable
tacacs-server timeout 30

aaa group server tacacs CPPM-TACACS-GRP
    server 10.20.10.50

aaa authentication allow-fail-through
aaa authentication login default group CPPM-TACACS-GRP local
aaa authorization commands default group CPPM-TACACS-GRP local
y
aaa accounting all-mgmt default start-stop group CPPM-TACACS-GRP
```

*Show Commands*

```
sh aaa server-groups

sh run | i aaa
aaa authentication allow-fail-through
aaa group server tacacs CPPM-TACACS-GRP
aaa authentication login default group CPPM-TACACS-GRP local
aaa authorization commands default group CPPM-TACACS-GRP local
aaa accounting all-mgmt default start-stop group CPPM-TACACS-GRP
    vsx-sync aaa mclag-interfaces static-routes

```

*Debug Commands*

```
 debug aaa
  all    Enable all debug modules
  event  To trace AAA events
```
