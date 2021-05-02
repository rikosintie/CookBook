# Create a "Message of the Day" banner

The Aruba Provision is a little different than Cisco. You need to go into "non-Interactive mode" first.
```
session interactive-mode disable

banner motd ^
******************************************
Unauthorized access prohibited
******************************************
Customer Name

You have reached a <Customer name> device.

**************Warning ********************
This system is for the use of authorized clients only. 
^

session interactive-mode enable

 ```
