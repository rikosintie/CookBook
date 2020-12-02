# Out of Band Management

Most Aruba switches have a 100Mb "Out of Band Management" interface. 

This interface is particularly useful in a lab environment. You can connect the OOBM interface to your corporate network and manage the 
switch while testing the customer's configuration.

```
conf t
HP-2920-24G-PoEP(config)# oobm
HP-2920-24G-PoEP(oobm)#ip address 10.253.7.11 255.255.252.0
HP-2920-24G-PoEP(oobm)#ip default-gateway 10.253.4.1
HP-2920-24G-PoEP(oobm)#enable
HP-2920-24G-PoEP(oobm)#end
HP-2920-24G-PoEP#wr mem
```
**Review the configuration**
```
HP-2920-24G-PoEP# sh run oobm

Running configuration:

oobm
   ip address 10.253.7.11 255.255.252.0
   ip default-gateway 10.253.4.1
   exit
```
**Ping from the OOBM interface**
```
HP-2920-24G-PoEP# ping 10.253.4.1 source oobm
10.253.4.1 is alive, time = 5 ms
```
