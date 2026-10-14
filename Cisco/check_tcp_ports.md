# Check TCP ports

You can use the command `show tcp brief all` to check all listening ports. 

This output was captured while the HTTP server and DNS server were still enabled:

```text
show tcp brief all
TCB       Local Address               Foreign Address             (state)
FF970B6060  10.100.130.200.22          10.100.130.202.40144        ESTAB
FF99E874F0  ::.80                      *.*                         LISTEN
FF99E295C0  0.0.0.0.80                 *.*                         LISTEN
FF99D9BB50  ::.443                     *.*                         LISTEN
FF99E26CE8  0.0.0.0.443                *.*                         LISTEN
FF9950CCF8  0.0.0.0.53                 *.*                         LISTEN
FF993B4578  ::.53                      *.*                         LISTEN
```

----------------------------------------------------------------

- `::.80` means the switch is listening on port 80 for IPv6
- `0.0.0.0.80` means the switch is listening on port 80 for IPv4

The same applies to 443 and 53. To close them:

```text
no ip http server
no ip http secure-server
no ip dns server
```

----------------------------------------------------------------

You can filter for any port. Include the leading dot so `80` doesn't also match <br> addresses or port numbers that contain 80:

```bash linenums='1' hl_lines='1 4'
show tcp brief all | include \.80
FF99E874F0  ::.80                      *.*                         LISTEN
FF99E295C0  0.0.0.0.80                 *.*                         LISTEN
show tcp brief all | include \.443
FF99D9BB50  ::.443                     *.*                         LISTEN
FF99E26CE8  0.0.0.0.443                *.*                         LISTEN
```
