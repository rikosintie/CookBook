# Adding Authentication to OSPF #

Without authenticaton enabled an attacker with access to the LAN could become part of the OSPF process and ultimately a Machine in the Middle. 

If this occurs, routing could be altered or a Denial of Service performed.

To prevent this you should always use authentication when deploying routing protocols.


## Prcedure ##

THe fisrt step is to create a keychain name:password pair. </br>

```

key-chain "OSPF-Auth"
key-chain "OSPF-Auth" key 1 key-string "O$FPkey4HQ"

```
Note that you can have more than one key-chain. In this example,the key-chain name is OSPF-Auth. </br>

**WHy would you need more than one key-chain?**
</br>
You may have vendors that need access to a specific OSPF process or you may have organizational requirements.

