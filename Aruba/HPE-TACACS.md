<a href="https://mwhubbard.blogspot.com"><img alt="GitHub" src="https://img.shields.io/github/license/rikosintie/CookBook"></a>
<a href="https://twitter.com/rikosintie"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/rikosintie?style=social"></a>

# aaa settings for Clearpass/ISE

**The clearpass server is at 192.168.254.231**
```
tacacs-server host 192.168.254.231 key QWER!@#$werER
aaa accounting exec start-stop tacacs
aaa accounting system stop-only tacacs
aaa accounting commands stop-only tacacs 
aaa authentication telnet login tacacs local
aaa authentication telnet enable tacacs local
aaa authentication ssh login tacacs local
aaa authentication ssh enable tacacs local
aaa authentication console login tacacs local
aaa authentication console enable tacacs local
aaa authentication login privilege-mode
```
