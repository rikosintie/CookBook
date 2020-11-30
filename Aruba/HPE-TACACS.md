## The clearpass server is at 192.168.254.231 ##

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
