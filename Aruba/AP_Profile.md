# Aruba Switch AP Profiles #

Device profile configuration allows you to dynamically detect an Aruba AP, which is directly connected to the switch.
The profile applies predefined configurations to ports on which the Aruba AP is detected.

The only drawback to using profiles is that when an AP is connecte to the switch, a "show run int X" shows the hard coded config, not the dynamic configuration.

To view the dynamic configuration you need to use the:

```
show device-profile config
```

## Create the profile for Aruba APs ##
In this example, the following vlans are used:

* Vlan 100 - Managment vlan for the AP
* Vlans 8, 9, 10 - Tagged vlans for SSIDs

**Create the customer-ap-profile**

Replace "Customer" in the next example with the name of your customer

```
device-profile name Customer-ap-profile
 tagged-vlan 8-10
 untagged-vlan 100
exit
```

**Associcate the the aruba-ap device profile to the customer AP Profile**
```
device-profile type aruba-ap 
associate Customer-ap-profile
enable
exit
```

## Show the profile settings ##
* sh device-profile config
* sh device-profile status

