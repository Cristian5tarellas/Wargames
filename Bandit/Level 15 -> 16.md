## Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

## Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

## Helpful Reading Material

- [Secure Socket Layer/Transport Layer Security on Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [OpenSSL Cookbook - Testing with OpenSSL](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html)

# Solution
Para usar la encriptación ssl/tls utilizaremos la función ***ncat***. Este comando tiene muchos parámetros, si buscamos en la ayuda vemos que un parámetro es ***--ssl***.
```sh
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001
dfa
Wrong! Please enter the correct current password.
```
Podemos ver que conectamos y nos da error al meter algo que no es la contraseña correcta.

Añadiendo la contraseña correcta tendremos el password para el siguiente nivel:
```sh
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

```
# Password next level:

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
## Next level:
[[Level 16 -> 17]]