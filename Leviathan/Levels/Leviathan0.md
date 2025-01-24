# Leviathan Level 0 → Level 1

We can have access to Leviathan0 with the information in the introduction. 

## Inspection
If we explore the directory:

```bash
leviathan0@gibson:~$ ls
leviathan0@gibson:~$
leviathan0@gibson:~$ ll
total 24
drwxr-xr-x  3 root       root       4096 Sep 19 07:07 ./
drwxr-xr-x 83 root       root       4096 Sep 19 07:09 ../
drwxr-x---  2 leviathan1 leviathan0 4096 Sep 19 07:07 .backup/
-rw-r--r--  1 root       root        220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root       3771 Mar 31  2024 .bashrc
-rw-r--r--  1 root       root        807 Mar 31  2024 .profile
```
We observe hidden files. If check the owner and group section we see that we can check the directory `.backup/`. And it has a file that we can read. 
```bash
leviathan0@gibson:~$ cd .backup/
leviathan0@gibson:~/.backup$ ls
bookmarks.html
leviathan0@gibson:~/.backup$ ll
total 140
drwxr-x--- 2 leviathan1 leviathan0   4096 Sep 19 07:07 ./
drwxr-xr-x 3 root       root         4096 Sep 19 07:07 ../
-rw-r----- 1 leviathan1 leviathan0 133259 Sep 19 07:07 bookmarks.html
leviathan0@gibson:~/.backup$
```

We can read it, however it has a lot of text. Then we can try to filter by the word password:
```bash
leviathan0@gibson:~/.backup$ cat bookmarks.html | grep "password"
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is 3QJ3TgzHDq" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

We can see the password: 3QJ3TgzHDq

## Solution
Password next level:

3QJ3TgzHDq

