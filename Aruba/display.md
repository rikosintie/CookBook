<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Display commands

The Aruba provision switches are slightly different than Cisco when it comes to setting the cli display characteristics.

**Disable paging**

When I'm displaying the running configuration and other outputs to backup/figure out the switches operation I don't want to be hitting the space bar repeatedly.

Use the command `no page` to disable paging.

When you are done, just enter `paging` to re-enable paging.

**Set the terminal length**

I like to have my terminal application extended top to bottom on the screen. 
This leaves the output from the switch filling about half the screen. 
To fix that use `terminal length 55` to set the output to 55 lines. You may
have to adjust between 50-60 depending but it's much better than the default.

**Set the terminal width**

I find it a lot easier to read the out of commands if they aren't wrapping on the screen. This is especially true for the log. 
To solve the wrapping issue use `terminal width 150`. Depending on your monitor you may have to adjust this.

**Repeat the last command**

This is an unbelievably useful command. For example, say you have just connected a stacking cable and powered up a new member in a stack. 
You can use repeat command to know when the member is up.

```
show stacking
repeat
```
Repeat is useful anytime you are waiting for something to happen. Another example is waiting for an OSPF neighbor relationship to come online.

