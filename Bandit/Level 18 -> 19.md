## Level Goal

The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

## Commands you may need to solve this level

ssh, ls, cat

# Solution
Con ssh podemos inyectar comandos directamente al servidor. Esto hace que se ejecute los comandos inyectados antes de usar el archivo .bashrc 
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
Teniendo acceso en este modo podemos encontrar la contraseña del siguiente nivel según los objetivos:
```sh
pwd
/home/bandit18
ls
readme
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

# Password next level:

cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8


## Next Level:
[[Level 19 -> 20]]
