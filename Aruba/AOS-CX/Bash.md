# Configuring the .bashrc file for easier use and safety

To drop into the Linux shell type `start-shell` and press [enter]</br>

Use `ls -la` do list the files in the home directory:

```
8325:~$ ls -la
total 8
drwxr-xr-x 3 admin administrators 100 Feb  5 10:36 .
drwxr-xr-x 5 root  root           100 Feb  5 10:36 ..
-rwxr-xr-x 1 admin administrators 410 Nov  3 13:07 .bashrc
-rwxr-xr-x 1 admin administrators 241 Nov  3 13:07 .profile
drwx------ 2 admin administrators  40 Feb  5 10:36 .ssh
```

The shell uses Bash so the .bashrc files contains the configurations for Bash. </br>

In Linux the `.` in front of a files designates it as a hidden file. That is the reason we used `ls -la` so display the files.</br>

The`-la` means - Show files in long format and include hidden files. </br>

To list the contents of the .bashrc file, type `cat .bashrc` [enter] </br>

```
8325:~$ cat .bashrc 
# ~/.bashrc: executed by bash(1) for non-login shells.

export PS1='\h:\w\$ '
umask 022

# You may uncomment the following lines if you want `ls' to be colorized:
# export LS_OPTIONS='--color=auto'
# eval `dircolors`
# alias ls='ls $LS_OPTIONS'
# alias ll='ls $LS_OPTIONS -l'
# alias l='ls $LS_OPTIONS -lA'
#
# Some more alias to avoid making mistakes:
# alias rm='rm -i'
# alias cp='cp -i'
# alias mv='mv -i'
```
Notice the comments in the .bashrc file suggest removing the `#` symbol to "uncomment" them.</br>

For better or worse, the only editor I have found is vim. If you are not familiar with vim it will be frustrating at first. </br>

To open the .bashrc file type `vim .bashrc` [enter]

```
# ~/.bashrc: executed by bash(1) for non-login shells.
  
export PS1='\h:\w\$ '
umask 022

# You may uncomment the following lines if you want `ls' to be colorized:
# export LS_OPTIONS='--color=auto'
# eval `dircolors`
# alias ls='ls $LS_OPTIONS'
# alias ll='ls $LS_OPTIONS -l'
# alias l='ls $LS_OPTIONS -lA'
#
# Some more alias to avoid making mistakes:
# alias rm='rm -i'
# alias cp='cp -i'
# alias mv='mv -i'
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
".bashrc" 16L, 410C                                           1,1           All
```
To edit the file, press the `i` key. This puts vim into "insert mode". </br>

You will see -- INSERT -- at the bottom of the file. 

## Search and replace the `#` symbol</br>

Press the `ESC` key to exit insert mode.</br>

Type `:%s/#//gc` [enter]</br>

The screen will change and the `#` symbols will be highlighted in yellow. The first `#` symbol is necassary so enter `n`. </br>

The next `#` is also necassary as that is a comment line. Type `n` one more time. </br>

Enter `y` on the symbols that are not spacers or comments. </br>

Your .bahsrc file should look like this when complete.

```
# ~/.bashrc: executed by bash(1) for non-login shells.
  
export PS1='\h:\w\$ '
umask 022

# You may uncomment the following lines if you want `ls' to be colorized:
 export LS_OPTIONS='--color=auto'
 eval `dircolors`
 alias ls='ls $LS_OPTIONS'
 alias ll='ls $LS_OPTIONS -l'
 alias l='ls $LS_OPTIONS -lA'
#
# Some more alias to avoid making mistakes:
 alias rm='rm -i'
 alias cp='cp -i'
 alias mv='mv -i'
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
~                                                                               
8 substitutions on 8 lines                                    16,1          All
```

## Add a few more aliases
```
alias mnt="mount | awk -F' ' '{ printf \"%s\t%s\n\",\$1,\$3; }' | column -t | egrep ^/dev/ | sort"
#View only mounted drives
alias lt='ls --human-readable --size -1 -S --classify'
#count files in a directory
alias count='find . -type f | wc -l'
alias ipen0='ip addr show eth0 | grep "inet\|ether\|brd";ip route | grep eth0'
```

Press the `i` key to enter "insert mode" then move to the end of the line `alias mv='mv -i'` and press [enter]</br.

Paste the additional aliases into the file. Press `ESC` to exit "insert mode"<</br>

Type `:wq` [enter] to save and exit. <br>

## Verify your changes

```8325:~$ cat .bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.

export PS1='\h:\w\$ '
umask 022

# You may uncomment the following lines if you want `ls' to be colorized:
 export LS_OPTIONS='--color=auto'
 eval `dircolors`
 alias ls='ls $LS_OPTIONS'
 alias ll='ls $LS_OPTIONS -l'
 alias l='ls $LS_OPTIONS -lA'
#
# Some more alias to avoid making mistakes:
 alias rm='rm -i'
 alias cp='cp -i'
 alias mv='mv -i'
 alias mnt="mount | awk -F' ' '{ printf \"%s\t%s\n\",\$1,\$3; }' | column -t | egrep ^/dev/ | sort"
 #View only mounted drives
 alias lt='ls --human-readable --size -1 -S --classify'
 #count files in a directory
 alias count='find . -type f | wc -l'
 alias ipen0='ip addr show eth0 | grep "inet\|ether\|brd";ip route | grep eth0'
 ```
**To test them out**</br>

First we have to reload bash using:</br>
`exec bash`

Then we can try the alias `l` </br>

```
8325:~$ exec bash
8325:~$ l
total 28
-rw-r--r-- 1 admin administrators 4117 Feb  5 11:02 ..bashrc.un~
-rw------- 1 admin administrators   70 Feb  5 11:03 .bash_history
-rwxr-xr-x 1 admin administrators  733 Feb  5 11:02 .bashrc
-rwxr-xr-x 1 admin administrators  410 Nov  3 13:07 .bashrc~
-rwxr-xr-x 1 admin administrators  241 Nov  3 13:07 .profile
drwx------ 2 admin administrators   40 Feb  5 10:36 .ssh
-rw------- 1 admin administrators 1721 Feb  5 11:02 .viminfo
```

We can see that it is working as just typing `l` listed the hidden files in long format!

## Create .inputrc and setup incremental history</br>
This is a very useful feature that will save a lot of time if you do much work in the treminal.</br>

I used this article to create the `.inputrc` file 
[Incremetnal History](https://codeinthehole.com/tips/the-most-important-command-line-tip-incremental-history-searching-with-inputrc/)</br>

We will use vim again

`vim .inputrc`

Then press `i` for "insert mode". Paste the following into the file:

"\e[A": history-search-backward 
"\e[B": history-search-forward 
"\e[C": forward-char
"\e[D": backward-char

Press `ESC` to exit "insert mode", then `:wq` to write and quit.</br>

Now let's see if file exits

```
8325:~$ l
total 36
-rw-r--r-- 1 admin administrators 4117 Feb  5 11:02 ..bashrc.un~
-rw-r--r-- 1 admin administrators  523 Feb  5 11:09 ..inputrc.un~
-rw------- 1 admin administrators   70 Feb  5 11:03 .bash_history
-rwxr-xr-x 1 admin administrators  733 Feb  5 11:02 .bashrc
-rwxr-xr-x 1 admin administrators  410 Nov  3 13:07 .bashrc~
-rw-r--r-- 1 admin administrators  109 Feb  5 11:09 .inputrc
-rwxr-xr-x 1 admin administrators  241 Nov  3 13:07 .profile
drwx------ 2 admin administrators   40 Feb  5 10:36 .ssh
-rw------- 1 admin administrators 2433 Feb  5 11:09 .viminfo

8325:~$ cat .inputrc 
"\e[A": history-search-backward 
"\e[B": history-search-forward 
"\e[C": forward-char
"\e[D": backward-char
```
It does exist and has the correct lines in it.</br>

Now Bash will parse the history file and save us a lot of time.

To see your complete history file type `history`[enter]

```
8325:~$ history
    1  ls -la
    2  cat .bashrc 
    3  vim .bashrc 
    4  vim .bashrc 
    5  cat .bashrc
    6  l
    7  exec bash
    8  l
    9  vim .inputrc
   10  l
   11  cat .inputrc 
   12  history
   ```

That is useful, but with the .inputrc file active, you can just type a couple characters and press the up arrow.</br>

They history will be searched and only entries that match the characters you entered will be matched.</br>
   
It will take some time for the history to populate. Just keep working at the terminal and after a short time you </br>
will be able to take advantage of the incremental history feature.










