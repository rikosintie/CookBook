<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Switch Stacking #

**Concepts**

The 2930M supports stacks of up to 10 switches. You can only stack similar switches. For example, a 2930M with another 2930M.
One switch in the stack is designated as “Commander” and one switch is elected to be the “Standby”. The other switches are 
designated “Member”. The Commander is responsible for the overall management of the stack. The Standby provides redundancy 
for the stack and takes over stack management operations if the Commander fails, or if a Commander failover is forced by an administrator.

The members are not part of the overall stack management, they must manage their local subsystems and ports to operate correctly 
as part of the stack. The Commander and Standby are also responsible for their own local subsystems and ports. Switch Stack Management (stacking)
enables you to use a single IP address and standard network cabling to manage a group of up to 10 total switches in the same IP subnet (broadcast domain). 

**Using stacking for these switches enables you to:**
* Simplify management of small workgroups or wiring closets while scaling your network to handle increased bandwidth demand.
* Add switches to your network without having to first perform IP addressing tasks.
* Reduce the number of IP addresses needed in your network.
* Reduce downtime with high availability in the event of a failure.  The other switches are designated “Member”. The Commander is responsible for the overall management of the stack. The Standby provides redundancy for the stack and takes over stack management operations if the Commander fails, or if a Commander failover is forced by an administrator.

**NOTE:**
> In the default configuration, stacking is enabled on these switches. However, if a 2930M switch is powered on and it does not have a Stacking Module installed, 
>stacking is disabled. If a Stacking Module is subsequently installed in the switch, stacking must be enabled from the switch CLI (in the configuration context) 
> by entering the following command:</br>
`
switch(config)# stacking enable
`
<p>&nbsp;</p>

**Setup the Commander switch**

Power up the first switch and let it finish booting. Run the following command


```
show stacking
Stack ID         : NO ID - will merge upon connectivity  
```


Notice the ID isn't set. Run the following to set the stack ID to 1

`switch(config)#stacking set-stack
`

</br>
Set the commander's priority to 255. The range is 1-255, higher has more priority. The default is 128. </br>

`stacking member 1 priority 255
`
</br>
</br>
If you want switch 2 to be the standby, set its priority to 200 </br>
`stacking member 2 priority 200
`
</br>

Here is a show run of a 3 switch stack </br>
```
stacking
   member 1 type "R0M67A" mac-address b8:d4:e7:b6:a6:c0
   member 1 priority 255
   member 1 flexible-module A type JL083A
   member 2 type "JL322A" mac-address 8c:85:c1:53:bb:80
   member 2 priority 200
   member 2 flexible-module A type JL083A
   member 3 type "JL322A" mac-address 8c:85:c1:53:fe:40
   member 3 flexible-module A type JL083A
   exit
```
</br>

**Cabling** 

The stack module has two ports. Connect Port 1 from the first switch to port 2 of the second switch. 
Continue until you reach the last switch. Then connect port 1 of the last switch to port 2 of the first switch. This is called a "Ring Topology". 
Just remember 1 to 2, 1 to 2, 1 to 2 until you have all switches connected! 
<p>&nbsp;</p>
At this point, power on switch two and let it completely boot. This switch will become the standby.


![](/Aruba/images/2930M-Stack-Topo.png)
<p>&nbsp;</p>

## Preconfiguring stacking </br>
You can configure stacking in the configuration and load it onto a switch ahead of time. </br>

The configuration includes the MAC address of each member so the order you power up the switches doesn't matter.

To see the options on a switch you can do the following:</br>
```
stacking member 2 type ?
 JL319A               
 JL321A               
 JL320A               
 JL322A               
 JL324A               
 JL323A               
 R0M67A               
 R0M68A               
 ```
 The final command to add an R0M67A as member 2 to the configuration would be:</br>
 
 `
 stacking member 2 type R0M67A mac 64e881-381440
 `
 </br>
 
 If you have physical access to the switch you can use:

show stacking to get the type and mac: </br>

```
show stacking

Stack ID         : NO ID - will merge upon connectivity                                  
MAC Address      : 64e881-38144a    
Stack Topology   : No Stack Formed                         
Stack Status     : No Stack Formed                         
Split Policy     : One-Fragment-Up 
Uptime           : 0d 0h 25m   
Software Version : WC.16.10.0018

 Mbr                                                                            
 ID  Mac Address       Model                                 Pri Status         
 --- ----------------- ------------------------------------- --- ---------------
 *1  64e881-381440     Aruba R0M67A 2930M-40G-8SR-PoE-Cla... 128 Commander   
 ```
 
 If the switch is a 2930M you can add the module to the congifuration using:</br>
 
 ```
 stacking member 3 flexible-module A type       
 JL078A               
 JL081A               
 JL083A               

stacking member 3 flexible-module A type JL083A
```
</br>
This will let you load the complete configuration before the switches are stacked.

</br>
## If you want to stack the switches before configuring  

Connect the stacking cables, then power up one switch at a time, starting with member 1. Check the progress of the new switch using </br>
`
show stacking
`

You can use the "repeat" command to monitor the switches starting up</br>
```
show stacking
repeat
```
</br>Every 2 seconds the stacking command will be repeated. You can modify the repeat command with:
```
repeat 
 CMDLIST               The command number to repeat.
 count                 The number of times to repeat the command.
 delay                 The delay in seconds between command repeats.
 <cr>
```

**Setting the Stack Members before Powering On**</br>
If you have the switch model and  MAC addresses ahead of time you can prebuild the stacking order. 

The format is </br>
`
HP Switch(config)#: stacking member <n> type <JxxxxA> [mac <MAC-address>]
`
</br>
</br>
For Example,
</br>
`
member 2 type "JL322A" mac-address 8c:85:c1:53:aa:80
`
</br>

**Removing a member**</br>
If you want to remove a member from the stack use:
```
(config)# stacking member 2 remove
The specified stack member will be removed from the stack and 
its configuration will be erased. The resulting configuration 
will be saved. Continue [y/n]? y
```

There are more configuration commands availble: </br>
```
stacking ?
 disable               Disables stacking on the switch and prevents it from joining or creating a stack.
 enable                Enables stacking on the switch with the specified priority and preferred member ID for this switch.
 factory-reset         Erase the startup config and all knowledge of other stack members.
 member                Specify the stack member to be configured.
 set-stack             Makes the inactive stack active.
 split-policy          Sets the split policy of the stack.
```

**show commands**</br>
```
show stacking ?
 detail                Shows detailed information related to the current state of the stack.
 member                Shows detailed stacking related information about the specified stack members.
 stack-ports           Shows the current state of the stacking ports of all the stack members.
 <cr>
```
**show stacking detail**</br> 
```
2930-stack# sh stack det

Stack ID         : 0200b8d4-e7bbb700                                                     
MAC Address      : b8:d4:e7:bb:b7:0a
Stack Topology   : Ring                                    
Stack Status     : Active                                  
Split Policy     : One-Fragment-Up 
Uptime           : 2d 5h 42m   
Software Version : WC.16.10.0018

Name             : 2930-stack
Contact          : 
Location         : 


Member ID        : 1 
Mac Address      : b8:d4:e7:bb:b7:00
Type             : R0M67A
Model            : Aruba R0M67A 2930M-40G-8SR-PoE-Class6 Switch                
Priority         : 255
Status           : Commander      
ROM Version      : WC.17.02.0006                                     
Serial Number    : SG06KJS0AZ                                                   
Uptime           : 2d 5h 42m   
CPU Utilization  : 0%  
Memory - Total   : 337,453,568 bytes 
Free             : 169,782,544 bytes 
Stack Ports - 
#1 : Active, Peer member 2              
#2 : Active, Peer member 4



Member ID        : 2 
Mac Address      : 8c:85:c1:53:bb:c0
Type             : JL322A
Model            : Aruba JL322A 2930M-48G-PoE+ Switch                          
Priority         : 200
Status           : Standby        
```
</br>
**show stacking member**</br>
```
show stacking member 
 STACK-MEMBER-LIST     Enter a list of stack members or one stack-member for the 'members'command/parameter.
(config)# show stacking member 2

Member ID        : 2 
Mac Address      : 8c:85:c1:63:b6:00
Type             : JL322A
Model            : Aruba JL322A 2930M-48G-PoE+ Switch                          
Priority         : 128
Status           : Standby        
ROM Version      : WC.17.02.0006                                     
Serial Number    : SG12JQN0ZZ                                                   
Uptime           : 0d 0h 15m   
CPU Utilization  : 0%  
Memory - Total   : 338,244,096 bytes 
Free             : 192,039,168 bytes 
Stack Ports - 
#1 : Active, Peer member 3              
#2 : Active, Peer member 1
```
**show stack-port status**</br>
```
show stacking stack-ports 
  Member Stacking Port State    Peer Member Peer Port
  ------ ------------- -------- ----------- ---------
  1      1             Up       2           2        
  1      2             Down     0           0        
  2      1             Up       3           2        
  2      2             Up       1           1        
  3      1             Down     0           0        
  3      2             Up       2           1       
  ```
</br>
Continue powering up the one switch at a time until all switches are online and in the stack.</br>

## Password reset ##
The 2930M switches have a reset and clear button the the front panel. If you need to clear the username/password you can use a paperclip and press the "clear" button for 7-10 seconds. One issue with a stack is that you have to be on the Commander. Since you can't log in, you might not know which switch is the commander. In that case, you will have to connect a console cable and try one at a time.


## Reference ##
[See Chapter 9](https://support.hpe.com/hpesc/public/docDisplay?docLocale=en_US&docId=emr_na-a00050240en_us)</br>
[HPE Stacking Guide](https://techhub.hpe.com/eginfolib/networking/docs/switches/WB/15-18/5998-8156_wb_2926_atmg/content/ch06s02.html)
