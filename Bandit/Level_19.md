# Level 19

### Level Info

The password for the next level is stored in a file called readme in the home directory. Unfortunately, someone has modified the file .bashrc to log you out when you log in with SSH.

### Commands you may need to solve this level

ssh, ls, cat

# Solution

With SSH, we can inject commands directly into the server. This causes the injected commands to be executed before the .bashrc file is used.

```sh
❯ sshpass -p 'x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO' ssh bandit18@bandit.labs.overthewire.org -p 2220 bash
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

whoami
bandit18
```

Having access in this way, we can find the password for the next level based on the objectives:

```sh
pwd
/home/bandit18
ls
readme
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

# Password for level 20:

cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

