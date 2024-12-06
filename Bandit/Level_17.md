# Level 17

### Level Info

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

### Commands you may need to solve this level

ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Helpful Reading Material

- [Port scanner on Wikipedia](https://en.wikipedia.org/wiki/Port_scanner)

# Solution

The server has a route if the ports are open, even if you cannot directly access them. However, it is possible to send empty strings to those routes:
- The route: **/dev/tcp/ip_address/port**

```sh
bandit16@bandit:~$ echo ' ' > /dev/tcp/127.0.0.1/22
bandit16@bandit:~$ echo $?
0                                                 ### CÓDIGO DE ESTADO EXITOSO
bandit16@bandit:~$ echo ' ' > /dev/tcp/127.0.0.1/23
-bash: connect: Connection refused
-bash: /dev/tcp/127.0.0.1/23: Connection refused
bandit16@bandit:~$ echo $?
1                                                ### CÓDIGO DE ESTADO ERRONEO
```

Therefore, we can find the open ports by checking which ones do not return an error. To do this, we will create a script (***PortScan_vC.sh***) that determines which ports (within a specified range) are open, and then we will identify which of those ports are SSL:

```sh
#!/bin/bash
# Scanning and determining the ssl ports.
# In this script we determine the opened ports in a localhost and which of the ports correspond to ssl

#Colours
greenColour="\e[0;32m\033[1m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"
endColour="\033[0m\e[0m"

# Ctrl+C
function ctrl_c(){
  echo -e "\n\n ${redColour}[!] Saliendo ...${endColour} \n"
  exit 1
}
trap ctrl_c INT

# Range of Ports to scan
echo -e "\nSelect the range of port to be scanned:"
# Initial Port
echo -e "\n${blueColour}Initial port:${endColour}"
read ini_port
#Last Port
echo -e "\n${blueColour}Last port:${endColour}"
read end_port

# Checking correct range of ports
if [[ $ini_port -ge $end_port ]]; then
    echo -e "\n${redColour}[!]The initial port cannot be bigger or equal than the last port...${endColour}"
    exit 1
fi

# Determining the opened ports
port_opened=()                                                          # Variable to save the opened port
for i in $(seq $ini_port $end_port); do
  (echo ' ' > /dev/tcp/127.0.0.1/$i) &>/dev/null && port_opened+=("$i")
done

# Checking if there are opened ports
if [[ $port_opened -eq 0 ]]; then
        echo -e "\n${yellowColour}[!] There are no opened ports in the range provided${endColour}\n"
        exit 0
fi
echo -e "\n${purpleColour}[+] The opened ports are:${endColour} ${port_opened[@]}"

# Finding out SSL connection using ncat
port_ssl=()                                                             # Variable to save the ssl ports
for port in "${port_opened[@]}"; do
  (ncat --ssl -zv 127.0.0.1 $port) &>/dev/null && port_ssl+=("$port")
done

echo -e "\n${greenColour}[+] The ssl ports are:${endColour} ${port_ssl[@]}\n"

```

The output:

```sh
Select the range of port to be scanned:

Initial port:
31000

Last port:
32000

[+] The opened ports are: 31046 31518 31691 31790 31960

[+] The ssl ports are: 31518 31790

```

We only need to test the SSL ports to find the one that leads to the next step, using the password for bandit16:

```sh
bandit16@bandit:/tmp/tmp.5qVqpr4Awa$ ncat --ssl 127.0.0.1 31518
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx


^C
bandit16@bandit:/tmp/tmp.5qVqpr4Awa$ ncat --ssl 127.0.0.1 31790
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

```

Therefore, we save the key in a file and use it to log into bandit17.

**NOTE**: If you don't change the file permissions:

```sh
bandit16@bandit:/tmp/tmp.5qVqpr4Awa$ ssh -i id_rsa.key bandit17@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes 
Could not create directory '/home/bandit16/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit16/.ssh/known_hosts).

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server on port 22, which is not intended.

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0664 for 'id_rsa.key' are too open.
```

Therefore:

```sh
bandit16@bandit:/tmp/tmp.5qVqpr4Awa$ chmod 600 id_rsa.key 
bandit16@bandit:/tmp/tmp.5qVqpr4Awa$ ssh -i id_rsa.key bandit17@localhost -p 2220
```

Now we simply search for the password in the /etc/bandit_pass directory:
```sh
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
```

# Password for level 18:

EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
