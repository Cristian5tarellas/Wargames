# Level 1

### Level Info

The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### Commands you may need to solve this level

[ls](https://man7.org/linux/man-pages/man1/ls.1.html) , [cd](https://man7.org/linux/man-pages/man1/cd.1p.html) , [cat](https://man7.org/linux/man-pages/man1/cat.1.html) , [file](https://man7.org/linux/man-pages/man1/file.1.html) , [du](https://man7.org/linux/man-pages/man1/du.1.html) , [find](https://man7.org/linux/man-pages/man1/find.1.html)

**TIP:** Create a file for notes and passwords on your local machine!

Passwords for levels are _not_ saved automatically. If you do not save them yourself, you will need to start over from bandit0.

Passwords also occassionally change. It is recommended to take notes on how to solve each challenge. As levels get more challenging, detailed notes are useful to return to where you left off, reference for later problems, or help others after you’ve completed the challenge.

# Solution

When we access a machine, check the TERM variable. To make Ctrl+l work (to clear the terminal), we want to use TERM=xterm. Therefore, we redefine the TERM variable:

```sh
bandit0@bandit:~$ export TERM=xterm
```

We look for the readme file in the directory, check its type, confirm that it is a text file, and then read it:

```sh
bandit0@bandit:~$ pwd
/home/bandit0

bandit0@bandit:~$ ls -l
total 4
-rw-r----- 1 bandit1 bandit0 437 Jul 17 15:57 readme

bandit0@bandit:~$ file readme
readme: ASCII text

bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

# Password for level 2:

ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
