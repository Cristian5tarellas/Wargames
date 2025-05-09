# Leviathan Level 0 → Level 1

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=lkYtVl5rXjg&ab_channel=Gabahack)

> This video shows my full process solving (in Spanish) Level 0 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

We can access the **Leviathan0** account using the connection details provided in the [Leviathan wargame introduction](https://overthewire.org/wargames/leviathan/).

---

## 🔍 Initial Inspection

After logging in, we list the contents of the home directory:

```bash
leviathan0@gibson:~$ ls
leviathan0@gibson:~$ ll
total 24
drwxr-xr-x  3 root       root       4096 Sep 19 07:07 ./
drwxr-xr-x 83 root       root       4096 Sep 19 07:09 ../
drwxr-x---  2 leviathan1 leviathan0 4096 Sep 19 07:07 .backup/
-rw-r--r--  1 root       root        220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root       3771 Mar 31  2024 .bashrc
-rw-r--r--  1 root       root        807 Mar 31  2024 .profile
```

We notice a hidden directory .backup/ that we can access and read:

```bash
leviathan0@gibson:~$ cd .backup/
leviathan0@gibson:~/.backup$ ll
total 140
drwxr-x--- 2 leviathan1 leviathan0   4096 Sep 19 07:07 ./
drwxr-xr-x 3 root       root         4096 Sep 19 07:07 ../
-rw-r----- 1 leviathan1 leviathan0 133259 Sep 19 07:07 bookmarks.html
```

Let's inspect the contents of bookmarks.html. Since it's a large file, we can search for the word password:
```bash
leviathan0@gibson:~/.backup$ cat bookmarks.html | grep "password"
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is 3QJ3TgzHDq" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

We can see the password: 3QJ3TgzHDq

## Solution
Password for leviathan1:

3QJ3TgzHDq

