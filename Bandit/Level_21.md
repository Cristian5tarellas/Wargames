# Level 21

### Level Info

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

### Commands you may need to solve this level

ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

# Solution
We will use terminal 1 for the binary file and terminal 2 to listen on the specified port. Both terminals have two scenarios: case 1, where an incorrect value is provided, and case 2, where the correct value is provided.

TERMINAL 1:
```sh
bandit20@bandit:~$ ls
suconnect
# CASE 1: FAIL
bandit20@bandit:~$ ./subconnect 5656
-bash: ./subconnect: No such file or directory
bandit20@bandit:~$ ./suconnect 5656
Read: Hello
ERROR: This doesnt match the current password!

# CASE 2: CORRECT
bandit20@bandit:~$ ./suconnect 5656
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
bandit20@bandit:~$ 
```

TERMINAL 2:
```sh
# CASE 1: FAIL
bandit20@bandit:~$ ncat -nvlp 5656
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Listening on [::]:5656
Ncat: Listening on 0.0.0.0:5656
Ncat: Connection from 127.0.0.1:51054.
Hello
FAIL!

# CASE 2: CORRECT
bandit20@bandit:~$ ncat -nvlp 5656
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Listening on [::]:5656
Ncat: Listening on 0.0.0.0:5656
Ncat: Connection from 127.0.0.1:57174.
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

# Password for level 22:

EeoULMCra2q0dSkYj561DX7s1CpBuOBt
