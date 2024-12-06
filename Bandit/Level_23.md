# Level 23

### Level Info

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

### Commands you may need to solve this level

cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution

We repeat the same process as in the previous level to see what the script is doing in this case:

```sh
bandit22@bandit:~$ cd /etc/cron.d

bandit22@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat

bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null

bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:/etc/cron.d$ 
```
##### What does the script do?

```sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
It has two variables:
- **myname**: bandit23
- **mytarget**: hash

In the last line, it copies the password for bandit23 into a file, which is placed in /tmp/, and the file name is the created hash.

Therefore:

```sh
#AS WE KNOW THE VALUE OF MYNAME, WE FIND OUT THE VALUE OF MYTARGET
bandit22@bandit:/usr/bin$ mytarget=$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)

#WE CHECK THE RESULT
bandit22@bandit:/usr/bin$ echo $mytarget
8ca319486bfbbc3663ea0fbe81326349

#WE OBSERVE THE FILE PERMISSIONS AND WE READ IT
bandit22@bandit:/usr/bin$ ls -l /tmp/$mytarget
-rw-rw-r-- 1 bandit23 bandit23 33 Aug 29 14:17 /tmp/8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/usr/bin$ cat /tmp/$mytarget
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

# Password for level 24:

0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
