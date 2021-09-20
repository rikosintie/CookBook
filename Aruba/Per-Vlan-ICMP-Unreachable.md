**The ArubaOS switches can't do per vlan "no ip icmp unreachable" like Cisco**

To enable globally you would do the following:

```
test(config)# no ip icmp unreachable
```

Personally, I would have that in my default template anyway, along with:
```
(config)# no ip icmp redirects
```

But if you have to do a per vlan `no icmp unreachable` you can do it using an ACL.

```
ip access-list extended "155"

     10 deny icmp 0.0.0.0 255.255.255.255 0.0.0.0 255.255.255.255 3
     20 permit ip 0.0.0.0 255.255.255.255 0.0.0.0 255.255.255.255
   exit
``` 
Then apply it to the vlan using:

```
ip access-group in
```

