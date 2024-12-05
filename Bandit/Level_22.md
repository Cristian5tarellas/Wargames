# Level 22

### Level Info

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

### Commands you may need to solve this level

cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution

The path that schedules the cron files is `/etc/cron.d/`. There, we can see what is running on the server.

```sh
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls -l
total 24
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jul 17 15:57 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8 14:38 e2scrub_all
-rwx------ 1 root root  52 Jul 17 15:59 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
```

This is from the root, meaning we can see all the server cron files. Now, let's focus on the next level: bandit22.

```sh
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

What it's doing: It runs the script `/usr/bin/cronjob_bandit22.sh` every minute and at reboot (@reboot).

```sh
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The script gives the following permissions to `chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`: Owner (rw=), Group (r), Others (r).

```sh
bandit21@bandit:/etc/cron.d$ ls -l /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
-rw-r--r-- 1 bandit22 bandit22 33 Aug 29 14:06 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The owner and the group are bandit22. As bandit21, we are considered "others", so we can only read it.

```sh
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

# Password for level 23:

tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
