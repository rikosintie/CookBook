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

* Vlan 254 - Managment vlan for the AP
* Vlans 88,92,96,200,204,208 - Tagged vlans for SSIDs

**Create the customer-ap-profile**

Replace "Customer" in the next example with the name of your customer

```
device-profile name Customer-ap-profile
 tagged-vlan 88,92,96,200,204,208
 untagged-vlan 254
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

# Example #
**Aruba AP225 connected to port 1 of a 2930f-24**

**View Profile Configuration**
```
show device-profile conf

 Device Profile Configuration

 Configuration for device-profile : customer-ap-profile
  untagged-vlan      : 254
  tagged-vlan        : 88,92,96,200,204,208
  ingress-bandwidth  : 100%
  egress-bandwidth   : 100%
  cos                : None
  speed-duplex       : auto
  poe-max-power      : Class/LLDP
  poe-priority       : critical
  allow-jumbo-frames : Disabled
  allow-tunneled-node: Enabled
```
**View Profile Status**
```
show device-profile status

 Device Profile Status

 Port          Device-type          Applied device profile
 ------------- -------------------- ----------------------
 1             aruba-ap             customer-ap-profile
```
**Show running configuration of interface 1**

```
show run int 1

Running configuration:

interface 1
   untagged vlan 1
   spanning-tree admin-edge-port
   spanning-tree bpdu-protection
   exit
```
Show vlan 254 interfaces

```
show vlan 254

 Status and Counters - VLAN Information - VLAN 254

  VLAN ID : 254
  Name : management
  Status : Port-based
  Voice : No
  Jumbo : No
  Private VLAN : none
  Associated Primary VID : none
  Associated Secondary VIDs : none

  Port Information Mode     Unknown VLAN Status
  ---------------- -------- ------------ ----------
  1                DEV-PROF Learn        Up
  25               Tagged   Learn        Down
  26               Tagged   Learn        Down
  27               Tagged   Learn        Down
  28               Tagged   Learn        Down
```

Show vlan 88 interfaces
```
show vlan 88

 Status and Counters - VLAN Information - VLAN 88

  VLAN ID : 88
  Name : Wireless Guest vlan88
  Status : Port-based
  Voice : No
  Jumbo : No
  Private VLAN : none
  Associated Primary VID : none
  Associated Secondary VIDs : none

  Port Information Mode     Unknown VLAN Status
  ---------------- -------- ------------ ----------
  1                DEV-PROF Learn        Up
  25               Tagged   Learn        Down
  26               Tagged   Learn        Down
  27               Tagged   Learn        Down
  28               Tagged   Learn        Down


  Overridden Port VLAN configuration

  Port  Mode
  ----- ------------
  1     DEV-PROF

```
