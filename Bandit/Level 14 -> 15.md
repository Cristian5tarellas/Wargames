## Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

## Commands you may need to solve this level

ssh, telnet, nc, openssl, s_client, nmap

## Helpful Reading Material

- [How the Internet works in 5 minutes (YouTube)](https://www.youtube.com/watch?v=7_LPdttKXPc) (Not completely accurate, but good enough for beginners)
- [IP Addresses](https://computer.howstuffworks.com/web-server5.htm)
- [IP Address on Wikipedia](https://en.wikipedia.org/wiki/IP_address)
- [Localhost on Wikipedia](https://en.wikipedia.org/wiki/Localhost)
- [Ports](https://computer.howstuffworks.com/web-server8.htm)
- [Port (computer networking) on Wikipedia](https://en.wikipedia.org/wiki/Port_(computer_networking))

# Solution

Para conectarnos a un sevidor con un puerto determinado podemos usar  netcat (***nc***). Simplemente indicando el servidor y el puerto. En este caso sería: `nc 127.0.0.1 30000`indicando localhost con la dirección 127.0.0.1 y  el puerto que nos pide (30000).
```sh
bandit14@bandit:~$ nc 127.0.0.1 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

```
Añadiendo la contraseña de bandit14 nos proporciona la contraseña de bandit15.

# Password next level:

8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

## Next level:
[[Level 15 -> 16]]