# SSH Hardening

Updated version of [Using SSH keys with a Cisco IOS switch](https://rikosintie.github.io/Ubuntu4NetworkEngineers/SSH/#using-ssh-keys-with-a-cisco-ios-switch), based IOS-XE 16.12.

## Get the key hash for your public keys

IOS-XE identifies public keys by their MD5 fingerprint, in uppercase without colons. On Mac/Linux/WSL, this prints the hash for every public key in `~/.ssh`:

```bash
for key in ~/.ssh/*.pub; do
  echo "File: $key"
  ssh-keygen -l -E md5 -f "$key" | awk '{print $2}' | tr -d ':' | tr 'a-z' 'A-Z'
  echo ""
done
```

Only RSA and ECDSA keys work on the 3850. IOS-XE 16.12 doesn't support Ed25519, so ignore the hash for any `id_ed25519.pub`.

### Configuration

```text
! Host keys
crypto key generate rsa modulus 4096 label SSH-KEYS
crypto key generate ec keysize 384 label EC-SSH-KEYS

! SSH server
ip ssh version 2
ip ssh authentication-retries 5
ip ssh source-interface Vlan10
ip ssh rsa keypair-name SSH-KEYS
ip ssh logging events
ip ssh dh min size 4096

! Public key authentication
ip ssh pubkey-chain
  username mhubbard
   key-hash ssh-rsa 5D24EA1D261C1836E437F4E67E2CEBEB
   key-hash ssh-rsa CCA79DBB37A7EA060C781DA2767509C0

! Remove weak algorithms
ip ssh server algorithm mac hmac-sha2-256 hmac-sha2-512
ip ssh server algorithm encryption aes256-ctr aes192-ctr aes128-ctr
ip ssh server algorithm kex ecdh-sha2-nistp256 ecdh-sha2-nistp384 diffie-hellman-group14-sha1
ip ssh server algorithm hostkey rsa-sha2-512 rsa-sha2-256

! Secure copy server
ip scp server enable
```

Notes:

- **Host key algorithm:** older templates used `hostkey ssh-rsa`, which signs with SHA-1. The 3850 on 16.12 supports `rsa-sha2-512` and `rsa-sha2-256`, so use those. The host key itself doesn't change, so existing `known_hosts` entries stay valid. Check what your version supports with `ip ssh server algorithm hostkey ?`.
- **KEX:** ECDH is listed first. `diffie-hellman-group14-sha1` stays last as a fallback for older tools. With these settings, modern OpenSSH clients connect without any overrides in `~/.ssh/config`.
- **EC key:** `EC-SSH-KEYS` is generated, but 16.12 only offers RSA host key algorithms, so it isn't used. It's there for code that supports ECDSA host keys.
- **`ip ssh dh min size 4096`** applies when the switch is the SSH client (for example `ssh` or `scp` from the switch to a server). It doesn't affect inbound connections.
- **SCP:** requires AAA. See the AAA section below.

### Restrict SSH to management addresses

Replace the IPs with your addresses.

```text
ip access-list standard MGMT
 permit 10.100.130.0 0.0.0.255
 permit 10.100.100.0 0.0.0.255
 permit 192.168.10.0 0.0.0.255
 deny any log
!
line vty 0 15
 access-class MGMT in vrf-also
 transport input ssh
```

- **`vrf-also` is required** if you use the dedicated management port (Gi0/0 in `Mgmt-vrf`). Without it, the access-class rejects every connection that arrives through a VRF interface, and the management port refuses SSH the one time you need it.
- `192.168.10.0/24` is the management port subnet (switch 192.168.10.251, laptop 192.168.10.252).
- `deny any log` writes rejected attempts to the log.
- Apply from the console, or keep a second session open while testing, so a mistake can't lock you out. The console isn't affected by `access-class`.
