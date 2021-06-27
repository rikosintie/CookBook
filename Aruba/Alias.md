<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# Using Aliases

Aliases are a great time saver if you run the same commands on a regular basis.

To create an alias:
* Enter global configuraiton mode
* Enter a name, then the command surrounded by ".

```
test-5412-MDF(config)# alias ipb "sh interfaces br | i Up"

To run the alias

test-5412-MDF#ipb
  B3*          1000SX     | No        Yes     Up     1000FDx    NA   off  0
  B6           SFP+DA3    | No        Yes     Up     10GigFD    NA   off  0
  D21          100/1000T  | No        Yes     Up     1000FDx    MDI  off  0
  D24          100/1000T  | No        Yes     Up     1000FDx    MDIX off  0
```

To view aliases

```
sh run | i alia
alias "wr" "write memory"
alias "ipb" "sh interfaces br | i Up"
```
