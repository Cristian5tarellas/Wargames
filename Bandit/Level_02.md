# Level 2
### Level Info

The password for the next level is stored in a file called - located in the home directory

### Commands you may need to solve this level
ls , cd , cat , file , du , find

Helpful Reading Material
- Google Search for “dashed filename”
- Advanced Bash-scripting Guide - Chapter 3 - Special Characters

# Solution

Be cautious with files that start with a hyphen, as many functions use the hyphen to indicate parameters for the commands being executed. The best approach is to specify the absolute path of the file.


```bash
bandit1@bandit:~$ pwd
/home/bandit1

bandit1@bandit:~$ ls -l
total 4
-rw-r----- 1 bandit2 bandit1 33 Jul 17 15:57 -

bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

# Password for level 3:

263JGJPfgU6LtdEvgfWU1XP5yac29mFx
