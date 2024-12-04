# Level 16

### Level Info

The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

### Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Helpful Reading Material

- [Secure Socket Layer/Transport Layer Security on Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [OpenSSL Cookbook - Testing with OpenSSL](https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/index.html)

# Solution

To use SSL/TLS encryption, we will use the ***ncat*** command. This command has many parameters, and if we check the help, we can see that one of the parameters is ***--ssl***.

```sh
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001
dfa
Wrong! Please enter the correct current password.
```
We can see that we connect, and it gives an error when we enter something that is not the correct password.

By entering the correct password, we will obtain the password for the next level:

```sh
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

```
# Password for level 17:

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
