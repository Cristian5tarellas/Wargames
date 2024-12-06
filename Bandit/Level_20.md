#Level 20 

### Level Info

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

### Helpful Reading Material

- [setuid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)

# Solution

In this case, we need to take advantage of the SUID permission.

```sh
bandit19@bandit:~$ ls -l bandit20-do 
-rwsr-x--- 1 bandit20 bandit19 14880 Jul 17 15:57 bandit20-do
```

If we execute it, we can observe that we use a command as another user:

```sh
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
```
For example the command **whoami**:

```sh
bandit19@bandit:~$ whoami
bandit19
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
```

Therefore, we can open a bash. However, we need to specify that we are using SUID permissions with the **-p** parameter:

```sh
bandit19@bandit:~$ ./bandit20-do bash -p
bash-5.2$ whoami
bandit20
bash-5.2$ cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

# Password for level 21:

0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
