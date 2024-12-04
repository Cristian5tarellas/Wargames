# Level 18

### Level Info

There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

### Commands you may need to solve this level

cat, grep, ls, diff

# Solution

In this case, we use the ***diff*** function to compare two files.

```sh
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```
Therefore, the password is x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO.

The ***diff*** command compares strings, but it has many functions. If you want to see differences between files, ***diff*** can be the first option you explore.

# Password next level:

x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

