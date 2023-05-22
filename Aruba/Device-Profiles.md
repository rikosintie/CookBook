# Create Device Profiles for Aruba APs

For this example, the customer has the following models:
* 225
* 305
* 515
* 575


**Create aliases to make it easier to troubleshoot**

* alias "devconf" "show device-profile config"
* alias "devstat" "show device-profile status"

**Create the device profiles**

In this case, I have 225 APs and we are adding some 575s. Unfortunately, the 225 don't have a version of firmware that 
is compatible with the 575s so I created a new vlan "200" for the 575s.


```
--------------- Device Profiles ------------------------------------

device-profile name RHS-AP-profile
   untagged-vlan 254
   tagged-vlan 88,200,253
exit 

device-profile name RHS-AP-575-profile
   untagged-vlan 250
   tagged-vlan 88,200,253
exit 
```

Then I added the identities for the APs

```
--------------- 220 series APs --------------------------------------

device-identity name "AP22X"
    lldp sys-desc "(MODEL: 22"
    exit

device-profile device-type "AP22X"
    associate "RHS-AP-profile"
    enable
    exit

--------------- 310 series APs -------------------------------------

device-identity name "AP31X"
    lldp sys-desc "(MODEL: 31"
    exit

device-profile device-type "AP31X"
    associate "RHS-AP-profile"
    enable
    exit

--------------- 510 series APs --------------------------------------

device-identity name "AP51X"
    lldp sys-desc "(MODEL: 51"
    exit

device-profile device-type "AP51X"
    associate "RHS-AP-profile"
    enable
    exit

--------------- 575 series APs ---------------------------------------

device-identity name "AP575"
    lldp sys-desc "(MODEL: 575"
    exit

device-profile device-type "AP575"
    associate "RHS-AP-575-profile"
    enable
    exit
```

Now you can use the alias to see what APs are on the switch:

```
devstat 

 Device Profile Status

 Port          Device-type          Applied device profile
 ------------- -------------------- ----------------------
 13            AP51X                RHS-AP-profile        
 14            AP51X                RHS-AP-profile        
 15            AP51X                RHS-AP-profile        
 16            AP51X                RHS-AP-profile        
 17            AP51X                RHS-AP-profile        
 18            AP51X                RHS-AP-profile        
```
