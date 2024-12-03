# Level 4

### Level Info

The password for the next level is stored in a hidden file in the inhere directory.

### Commands you may need to solve this level
ls , cd , cat , file , du , find

# Solution

We can list the hidden files/directories and then open the file, once we know the name:
```bash
bandit3@bandit:~$ pwd
/home/bandit3

bandit3@bandit:~$ ls -l
total 4
drwxr-xr-x 2 root root 4096 Jul 17 15:57 inhere

bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Jul 17 15:57 .
drwxr-xr-x 3 root    root    4096 Jul 17 15:57 ..
-rw-r----- 1 bandit4 bandit3   33 Jul 17 15:57 ...Hiding-From-You

bandit3@bandit:~/inhere$ file ...Hiding-From-You 
...Hiding-From-You: ASCII text

bandit3@bandit:~/inhere$ cat ...Hiding-From-You 
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

# Password for level 5:

2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
