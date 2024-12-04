# Level 14

### Level Info

The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on

### Commands you may need to solve this level

ssh, telnet, nc, openssl, s_client, nmap

### Helpful Reading Material

- [SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)

# Solution
Given that we have a private key, we can assume it is the private key for the *bandit14* user. To do this, we can connect to that user using the private key file with the command ***ssh -i***.

```sh
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
```

**NOTE:** The server uses port 2220 for the SSH connection. If you don't specify it, it will automatically try to connect via port 22 and give an error. To determine the SSH connection's port, you can read the file ***`/etc/ssh/sshd_config`***. The ***Ports*** section will indicate the port being used.

Once inside, we read the password from the specified file:

```sh
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

**Comment**: If a public key is generated from our private key, we can place it in the **.ssh** folder in a file called **authorized_keys**. However, to do this, we need access to and the ability to write to the **.ssh** folder of the user on the server we want to connect to.

# Password next level:

MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
