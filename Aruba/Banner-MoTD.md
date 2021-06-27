<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

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
