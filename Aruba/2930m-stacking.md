<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Switch Stacking #

**Concepts**

The 2930M supports stacks of up to 10 switches. You can only stack similar switches. For example, a 2930M with another 2930M.
One switch in the stack is designated as “Commander” and one switch is elected to be the “Standby”. The other switches are 
designated “Member”. The Commander is responsible for the overall management of the stack. The Standby provides redundancy 
for the stack and takes over stack management operations if the Commander fails, or if a Commander failover is forced by an administrator.

The Members are not part of the overall stack management, they must manage their local subsystems and ports to operate correctly 
as part of thestack. The Commander and Standby are also responsible for their own local subsystems and ports. Switch Stack Management (stacking)
enables you to use a single IP address and standard network cabling to manage a group of up to 10 total switches in the same IP subnet (broadcast domain). 

**Using stacking for these switches enables you to:**
* Simplify management of small workgroups or wiring closets while scaling your network to handle increased bandwidth demand.
* Add switches to your network without having to first perform IP addressing tasks.
* Reduce the number of IP addresses needed in your network.
* Reduce downtime with high availability in the event of a failure.  The otherswitches are designated “Member”. The Commander is responsible for the overall management of the stack. TheStandby provides redundancy for the stack and takes over stack management operations if the Commander fails,or if a Commander failover is forced by an administrator.

**NOTE:**
> In the default configuration, stacking is enabled on these switches. However, if a 2930M switch is powered on and it does not have a Stacking Module installed, 
>stacking is disabled. If a Stacking Module is subsequently installed in the switch, stacking must be enabled from the switch CLI (in the configuration context) 
> by entering the following command:
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


Notice the the ID isn't set. Run the following to set the stack ID to 1

`switch(config)#stacking set-stack
`
<p>&nbsp;</p>

**Cabling** 

The stack module has two ports. Connect Port 1 from the first switch to port 2 of the second switch. 
Continue until you reach the last switch. Then connect port 1 of the last switch to port 2 of the first switch. This is called a "Ring Topology". 
Just remember 1 to 2, 1 to 2, 1 to 2 until you have all switches connected! 
<p>&nbsp;</p>
At this point, power on switch two and let it completely boot. This switch will become the standby.


![](/Aruba/images/2930M-Stack-Topo.png)
<p>&nbsp;</p>

Only power up one switch at a time. Check the progress of the new switch using 
`show stacking
`

**show commands**
```
show stacking
```

Continue powering up the one switch at a time until all switches are online and in the stack.



## Reference ##
[See Chapter 9](https://support.hpe.com/hpesc/public/docDisplay?docLocale=en_US&docId=emr_na-a00050240en_us)
