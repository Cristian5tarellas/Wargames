# Level 15

### Level Info

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

### Commands you may need to solve this level

ssh, telnet, nc, openssl, s_client, nmap

### Helpful Reading Material

- [How the Internet works in 5 minutes (YouTube)](https://www.youtube.com/watch?v=7_LPdttKXPc) (Not completely accurate, but good enough for beginners)
- [IP Addresses](https://computer.howstuffworks.com/web-server5.htm)
- [IP Address on Wikipedia](https://en.wikipedia.org/wiki/IP_address)
- [Localhost on Wikipedia](https://en.wikipedia.org/wiki/Localhost)
- [Ports](https://computer.howstuffworks.com/web-server8.htm)
- [Port (computer networking) on Wikipedia](https://en.wikipedia.org/wiki/Port_(computer_networking))

# Solution

To connect to a server on a specific port, we can use netcat (***nc***). Simply specify the server and the port. In this case, it would be: `nc 127.0.0.1 30000`, indicating localhost with the address 127.0.0.1 and the requested port (30000).

```sh
bandit14@bandit:~$ nc 127.0.0.1 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

```
Adding the password for bandit14 provides us with the password for bandit15.


# Password for level 16:

8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
