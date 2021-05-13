# Using SSH keys for Authentication 

The Procurve switches allow you to use SSH keys instead of a username/password to login. <br/>

Why would you want to do this? It's more secure than using a username/password because you have to have the matching private key. 
You only need one private/public key pair. You can use the public key as many
times are you want. After all, that's why it's called the public key. 

Almost every SSH client, Putty, SecureCRT, Termius, etc. allow you to save the private key with the connection. There are also hardware security modules (HSM) like Yubikey 
that let you store the private key. I haven't used the Yubikey for switch logins but I listened to a podcast [How to use Yubikey with SSH](https://www.youtube.com/watch?v=QGZz_xb0fCU)
explaining how to use them to log into Linux servers. It seems straight forward if you have a Yubikey.

**Creating the SSH keys**
<br/>
On Linux/Mac, run the following. Note, you will be asked to enter a passphrase. When connecting, you will be asked to enter this passphrase. 
In that case you need:
* something you have - the private key
* something you know - the passphrase

Basically 2 factor authentication on a switch!

```
ssh-keygen                                                                                                                           [13:50:14]
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/mhubbard/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/mhubbard/.ssh/id_rsa.
Your public key has been saved in /Users/mhubbard/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:6e1qwmoilpbXGQ7hb+GUoPhMxcpeMzTmgg30VOpMDmM mhubbard@HP8600-4.local
The key's randomart image is:
+---[RSA 3072]----+
|    ..           |
| . ..            |
|.Eo+             |
|o B+*    .       |
|.=oXo.. S        |
|o.*o== . .       |
| =ooB++ . .      |
|.==..Bo ..       |
|o..oo. o...      |
+----[SHA256]-----+
```

You can view the keys with:<br/>
```
 ls -l ~/.ssh                                                                                                                        [13:50:58]
.rw------- 2.7k mhubbard 12 May 13:50 id_rsa
.rw-r--r--  577 mhubbard 12 May 13:50 id_rsa.pub
.rw-r--r-- 5.7k mhubbard  5 May 16:21 known_hosts
```

If you are on Windows, you can use putty's puttygen tool. Here is a link with instructions: [Using PuTTYgen on Windows to generate SSH key pairs](https://www.ssh.com/academy/ssh/putty/windows/puttygen)

The keys are just text files. You can open them with any text editor. Here is my public key. I am using zsh with Oh My ZSH installed so the cat puts the key
into a table:

```
cat ~/.ssh/id_rsa.pub                                                                                                                [13:52:49]
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: /Users/mhubbard/.ssh/id_rsa.pub
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCXuTtgxH/a9mnOqkkiGVm2qfHi+01WBkqqRHzPVqOlTh0EadxTqQkAQLpjYK7Kt7cc3t5luohXcPuJ1S8tCXy0lN6eQ0SFyuhtY42/i
       │ Bo5sqLPcHF6ZTVhe+3fDDDlXzGRoJaG7HZNnxI6mb1YVh4s/dEBVyJo8N4HKz+7Tc3L3F67zlME2x7Jwnhx/B1hlBQc5gByR0rqmdemq/DqulJTqRIvkNLwAqpeeRMcwvjc4+1JGM6e8d
       │ 4gP+uo0Mp2Yg1H2c9Wh7G/o3zAJdtbCP3IfpVq+q5WKrFeW/dNR/jdR9obhaPqulANt5r1rZmWrLaPmuUUjiP4vnkgBleMNjR9TYj746gZtSacuf3TTjDmEfqNTpof7TlWLSRGWCXbJY6
       │ q1ZrIbon7Htcvmf9lreex9sntrjcDP4zLTNQmwKVvPuvj307p4e45Fk7ibEDzfImq/vEj/68cLdSnx1uEwlQC+BmTAmyk6Kzry9zuTAAuza71Qm7vnZ+bCwrXIdnKJws= mhubbard@HP
       │ 8600-4.local

```

NOTE: The actual key is one long string.

**On the switch**<br/>
In this example, the user "admin" has been created on the switch using the standard HPE password method. 
You do not need to enter that password when using ssh keys.<br/>

I like to set the terminal length and width. This is optional but if you give it a shot you might like it. 
```
conf t
terminal length 55
terminal width 165
aaa authentication ssh enable public-key
ip ssh public-key manager username admin 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCZ/1dtbHYggNpwAKQazPUwoyfMdJK5POdqudUaNuFBO+hjZRIc6Zl0kPeKLBrJsKruIiH6s35rKPbh0vgWZ4J6chIt6y09WgLi6VD1266bF6aOm8WkdRphHGKgNLF6Xcd317VRDUWGjd1N5ESiG5FNuN9YEz/enuBis58kDz97CBxZ9SrfpF7npG6llLObFtxbjR0sAD08CGtRsOocSN6gsx58dE33jQThdRTV7epKORItiCIouiYTfW/qG1UwkprfxMgZuN6mZkfUK3Wel9ELituip+ThuCQkSbs9BvhFYGZvuBOO1JwsvjwebL6k7nXzvmoLRhT+7juHjvGQlmLpQlcnVDfZVN/3ZmkKyJicGeB43XxKOP6PPjpk7DkFVvsMn8iUKQvzaQFIgaEUbBLrP+yhgHLgawEy2Bv6+ld3ryyYn1pN4mhOebwRTvNdx7ngzne80h6UhmRe0T8wvXll1Umhz1El1cXkw3FwJ46ZdjLVZlF4ksAsK5Cct+ko0UE= mhubbard@HP8600-4.local'
end
wr
```

**Verify**
```
show crypto client-public-key manager

 Configuration and Settings - Client Public Keys


 Manager keys:

 Key Index   : 0
 Username    : admin

 Type of Key : mhubbard@HP8600-4.local ssh-rsa
 Key String  : AAAAB3NzaC1yc2EAAAADAQABAAABgQCZ/1dtbHYggNpwAKQazPUwoyfMdJK5
               POdqudUaNuFBO+hjZRIc6Zl0kPeKLBrJsKruIiH6s35rKPbh0vgWZ4J6chIt6
               y09WgLi6VD1266bF6aOm8WkdRphHGKgNLF6Xcd317VRDUWGjd1N5ESiG5FNuN
               9YEz/enuBis58kDz97CBxZ9SrfpF7npG6llLObFtxbjR0sAD08CGtRsOocSN6
               gsx58dE33jQThdRTV7epKORItiCIouiYTfW/qG1UwkprfxMgZuN6mZkfUK3We
               l9ELituip+ThuCQkSbs9BvhFYGZvuBOO1JwsvjwebL6k7nXzvmoLRhT+7juHj
               vGQlmLpQlcnVDfZVN/3ZmkKyJicGeB43XxKOP6PPjpk7DkFVvsMn8iUKQvzaQ
               FIgaEUbBLrP+yhgHLgawEy2Bv6+ld3ryyYn1pN4mhOebwRTvNdx7ngzne80h6
               UhmRe0T8wvXll1Umhz1El1cXkw3FwJ46ZdjLVZlF4ksAsK5Cct+ko0UE=
```

You can also setup Operator level access. To do that use `ip ssh public-key operator` instead of manager

**log in**<br/>
On Mac/Linux just add the path to the private key. For example, using a switch with IP 10.112.250.40. If you use the default id_rsa key you don't even have to enter the
path, the ssh client will default to it. But I like to create a key and name it for the specific purpose. In this case I named my keys hubbard:
```
ssh -i ~/.ssh/hubbard admin@10.112.250.254                                                                                           [16:29:36]
*******************************************************************************
This system is the property of Michael Hubbard.

            UNAUTHORIZED ACCESS TO THIS DEVICE IS PROHIBITED.

     You must have explicit permission to access this device. All activities
                        performed on this device are logged.

Any violations of access policy will result in disciplinary action.
*******************************************************************************

Enter passphrase for key '/Users/mhubbard/.ssh/hubbard':
Press any key to continue
```
**Note:** You can enable ssh keys and still use username/password. If your client doesn't have your private key, it will just use the username/password created on the switch. The HPE document in the reference section explains how to prevent username/password logins if you have ssh public key enabled.<br/>


On Windows, you are probably using a client like SecureCRT, follow their instructions to add the key to the connection. 
This is usually a simple "Click a button and browse" operation.

**Show Commands**<br/>
show crypto client-public-key [<manager|operator>][keylist-str][babble|fingerprint]

```
sh crypto client-public-key fingerprint

 Manager keys:

0 3072 88:b6:5e:61:b5:2f:0e:23:d6:95:c0:1d:ea:2e:87:d3:
        mhubbard@HP8600-4.local

 Operator keys:
 
----------------------------------------- 
sh crypto client-public-key babble

 Manager keys:

0 3072 xedog-lahid-nadec-zatud-gynun-vohyg-potot-cemib-ganah-valyl-vyxox
        mhubbard@HP8600-4.local

 Operator keys:
 
```

**Replacing or clearing the public-key file**

The client public-key file remains in the switch flash memory even if you erase the startup-config file, reset the switch, or reboot the switch.
Remove the existing client public-key file or specific keys by executing the `clear crypto client-public-key` command. 
This clears the public keys from both management modules. The module that is not active must be in standby mode.

Syntax:
```
clear crypto client-public-key
 key-type              Select the public key-type.
 manager               Select manager public keys.
 operator              Select operator public keys.
```

Deletes the client public-key file from the switch.




**References**
[SSH client public-key authentication notes](https://techhub.hpe.com/eginfolib/networking/docs/switches/YA-YB/15-18/5998-8153_yayb_2530_asg/content/ch08s08.html)



