## Level Goal

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

## Helpful Reading Material

- [setuid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)

# Solution
En este caso tenemos que aprovecharnos del permiso SUID.
```sh
bandit19@bandit:~$ ls -l bandit20-do 
-rwsr-x--- 1 bandit20 bandit19 14880 Jul 17 15:57 bandit20-do
```
Si lo ejecutamos:
```sh
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
```
Ejecutamos un comando como otro usuario:
```sh
bandit19@bandit:~$ whoami
bandit19
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
```
Por lo tanto podemos abrir una bash. Sin embargo necesitamos decir que usamos permisos SUID con el parámetro **-p**:
```sh
bandit19@bandit:~$ ./bandit20-do bash -p
bash-5.2$ whoami
bandit20
bash-5.2$ cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

# Password next level:

0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

## Next Level:
[[Level 20 -> 21]]