# Level 5

### Level Info

The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

### Commands you may need to solve this level

[ls](https://man7.org/linux/man-pages/man1/ls.1.html) , [cd](https://man7.org/linux/man-pages/man1/cd.1p.html) , [cat](https://man7.org/linux/man-pages/man1/cat.1.html) , [file](https://man7.org/linux/man-pages/man1/file.1.html) , [du](https://man7.org/linux/man-pages/man1/du.1.html) , [find](https://man7.org/linux/man-pages/man1/find.1.html)

# Solution

We explore the directories and check the files in the `inhere` directory. We use the `file` command to determine the type of each file. We can identify that only one of them is an *ASCII text*, indicating it is a text file.


```sh
bandit4@bandit:~$ pwd
/home/bandit4

bandit4@bandit:~$ ls -l
total 4
drwxr-xr-x 2 root root 4096 Jul 17 15:57 inhere

bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls -l
total 40
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file00
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file01
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file02
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file03
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file04
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file05
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file06
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file07
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file08
-rw-r----- 1 bandit5 bandit4 33 Jul 17 15:57 -file09

bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

Our target is *file07*. Therefore, to create a command line specific to this task, we can filter the `file` output for the text *text* and open the file directly:

```sh
bandit4@bandit:~/inhere$ file ./* | grep text | awk '{print $1}' FS=':' | xargs cat
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```
# Password for level 6:

4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
