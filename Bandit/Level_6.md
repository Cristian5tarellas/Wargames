# Level 6

### Level Info

The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

### Commands you may need to solve this level

[ls](https://man7.org/linux/man-pages/man1/ls.1.html) , [cd](https://man7.org/linux/man-pages/man1/cd.1p.html) , [cat](https://man7.org/linux/man-pages/man1/cat.1.html) , [file](https://man7.org/linux/man-pages/man1/file.1.html) , [du](https://man7.org/linux/man-pages/man1/du.1.html) , [find](https://man7.org/linux/man-pages/man1/find.1.html)

# Solution
For this task, we will use the ***find*** command, utilizing its parameters to narrow the search based on the given characteristics.
```sh
bandit5@bandit:~$ find inhere/ -size 1033c -readable ! -executable
inhere/maybehere07/.file2
```
To negate or do the opposite, we use the exclamation mark **!**.

Now that we know the file, we can use the same ***find*** command to read the file.
```sh
bandit5@bandit:~$ find inhere/ -size 1033c -readable ! -executable | xargs file
inhere/maybehere07/.file2: ASCII text, with very long lines (1000)

bandit5@bandit:~$ find inhere/ -size 1033c -readable ! -executable | xargs cat
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

# Password for level 7:

HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

