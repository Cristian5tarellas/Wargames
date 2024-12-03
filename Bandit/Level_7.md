# Level 7

### Level Info

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

### Commands you may need to solve this level

[ls](https://man7.org/linux/man-pages/man1/ls.1.html) , [cd](https://man7.org/linux/man-pages/man1/cd.1p.html) , [cat](https://man7.org/linux/man-pages/man1/cat.1.html) , [file](https://man7.org/linux/man-pages/man1/file.1.html) , [du](https://man7.org/linux/man-pages/man1/du.1.html) , [find](https://man7.org/linux/man-pages/man1/find.1.html) , [grep](https://man7.org/linux/man-pages/man1/grep.1.html)

# Solution

Similar to the previous level, we use the ***find*** command with the provided information. Since the file can be anywhere on the server, we will apply it to the root directory (/).

```sh
bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```
Don't forget that, since we are not root, we will encounter many permission denials. We will use *2>/dev/null* to suppress all the errors.

Once the file is located, we can check if it is readable (understandable) and then read it:
```sh
bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null | xargs file
/var/lib/dpkg/info/bandit7.password: ASCII text

bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null | xargs cat
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

# Password for level 8:

morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
