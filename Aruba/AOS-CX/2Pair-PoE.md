# ARUBA-SA-20220418-PLWB01 :: Aruba Class-6 PoE Switches and Unify IP Phones PoE Incompatibility Issues (Rev-1)

**For AOS-CX Products:**

Starting with 10.09.xxxx, Aruba Class-6 switches provide a per-interface configurable option (power-pairs) to limit the Inrush to 2-pairs only, similar to Class-4 switch ports.

The following procedure describes the actions required to mitigate this issue on Aruba Class-6 PoE switches.

Use "alt-a" only configuration.

            (1) Use firmware version of 10.09 on the switch, where alt-a only power configuration is available.
            (2) Configure the "alt-a" parameter on the interface connected to the Unify IP phone.
            
```
configure terminal
interface 1/1/1     #interface connected to IP phone
power-over-ethernet power-pairs alt-a
```


(3) Verify "power-pairs" on interface is configured to "alt-a" (2-Pair only power delivery)
```
show power-over-ethernet 1/1/1
Check for Power Pairs Configured of the interface:
Power Pairs Configured   : alt-a
```

*********** The Aruba Advisory **************</br>
</br>
**DESCRIPTION**
When connected to Aruba Class-6 PoE Switches, some Unify IP phones draw excessive power that may result in damage to the phone.
Some Unify IP phones exhibit a PoE incompatibility with Aruba Class-6 PoE switches that might result in a hard failure of the phone. The failure may not occur upon initial connection to the Aruba switch. Rather, the failure symptom may become visible after a switch reboot, switch power cycle, phone reboot, or disconnecting and reconnecting the phone to the switch.

**Root Cause:**

As soon as the Unify IP phone is connected to the switch, the PoE detection cycle starts. Upon identifying a valid PD connection, the switch delivers power over all 4-pairs of the Ethernet cable. Aruba Class-6 PoE ports provide an Inrush current limit at POWER_UP to 425mA.
During the initial POWER_UP Inrush phase, some Unify IP phones may exceed IEEE 802.3 power draw specifications when connected to Aruba Class-6 PoE switches. This might result in physical damage to the IP phone, impacting further PD detection response to the PSE.

 
**SCOPE**

Hardware Products affected:
Aruba 6300, 6400, 4100i Switch Series Class-6 PoE Products
Aruba 2930M Switch Series Class-6 PoE Products </br>

|  OS   | Series | Part Number   | Product Description                |
|-------:|:--------:|:-------------:|:----------------------------------------|
|AOS-CX | 6300     | JL659A       | Aruba 6300M 48SR5 CL6 PoE 4SFP56 Swch   |
|AOS-CX | 6300     | JL660A       | Aruba 6300M 24SR5 CL6 PoE 4SFP56 Swch   |
|AOS-CX | 6400     | R0X40B       | Aruba 6400 48p 1GbE CL6 PoE 4SFP56 Mod  |
|AOS-CX | 6400     | R0X41A       | Aruba 6400 48p SR5 CL6 PoE 4SFP56 Mod   |
|AOS-CX | 4100i    | JL817A       | Aruba 4100i 12G CL4/6 POE 2SFP+ DIN Sw  |
|AOS-CX | 4100i    | JL818A       | Aruba 4100i 24G CL4/6 POE 4SFP+ Sw      |
|AOS    | 2930M    | R0M67A       | Aruba 2930M 40G 8SR PoE Class 6 1s Swch |
|AOS    | 2930M    | R0M68A       | Aruba 2930M 24SR PoE Class 6 1s Swch    |
</br>

**Firmware Versions affected: (list all versions):**
</br>
All firmware versions supporting Class-6 PoE.

# RESOLUTION and WORKAROUNDS

      1.    What is a permanent fix for the issue? 

Once damage has occurred (first time connection to a Class-6 PoE switch), the phone can no longer reliably power up via PoE. An alternate power source, such as an AC wall adapter, or replacement of the damaged phone is required.


      2.    What steps may be used as a workaround?

Aruba switches that support a max of Class-4 PoE over 2-pairs are not exposed to the interoperability issue described in this document. Connecting your Unify IP phones to Aruba Class-4 switches will help prevent this issue.

 

**For AOS-CX Products:**

Starting with 10.09.xxxx, Aruba Class-6 switches provide a per-interface configurable option (power-pairs) to limit the Inrush to 2-pairs only, similar to Class-4 switch ports.

The following procedure describes the actions required to mitigate this issue on Aruba Class-6 PoE switches.

Use "alt-a" only configuration.

**For AOS-Switch Products:**
</br>
Starting with firmware version 16.10.0013, Aruba Class-6 switches provide a per-interface configurable option (force-2pair-mode) to limit the Inrush to 2-pairs only, similar to Class-4 switch ports.

</br>
The following procedure describes the actions required to mitigate this issue on Aruba Class-6 PoE switches.

            (1) Use firmware version of 16.10.0013 on the switch, where the force-2pair-mode power configuration is available.

            (2) Configure force-2pair-mode on the interface connected to the Unify IP phone.

```
configure terminal
no interface <port-list> power-over-ethernet
power-over-ethernet force-2pair-mode ports <port-list>
interface <port-list> power-over-ethernet
```
</br>
Note: The enabled force-2pair-mode works only after PoE is reset on the port. Enabling force-2-pair mode when the port is powered will not affect the default 4-pair power mode. 

            (3) Verify force-2pair-mode is configured on the interface connected to the Unify IP phone. 

(switch)# show power-over-ethernet brief 
 Status and Configuration Information
   Available: 740 W  Used: 5 W  Remaining: 635 W
 
PoE    Pwr  Pwr      Pre-std Alloc Alloc  PSE Pwr PD Pwr  PoE Port     PLC PLC 
Port   Enab Priority Detect  Cfg   Actual Rsrvd   Draw    Status       Cls Type
------ ---- -------- ------- ----- ------ ------- ------- ------------ --- ----
 1    Yes  low      off     usage usage  0.0 W   0.0 W   Disabled     0    -  
 2    Yes  low      off     usage usage  0.0 W   0.0 W   Disabled     0    - 
 .
 . 
 10   Yes  low      on      usage usage  4.5 W   4.4 W   Delivering$  5    3
 $ - Indicates force-2Pair-mode applied port

