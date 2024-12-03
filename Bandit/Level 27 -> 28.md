## Level Goal

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

## Commands you may need to solve this level

git

# Solution
Lo primero de todo será clonar el repositorio proporcionado con ***git clone***. 
**OJO:** Siempre tenemos que hacer un directorio temporal para que todo lo que hagamos pueda ser borrado al cerrar sesión.

```sh
# CREACION DE DIRECCION TEMPORAL
bandit27@bandit:~$ mktemp -d
/tmp/tmp.0FWfzZ0WKz
bandit27@bandit:~$ cd /tmp/tmp.0FWfzZ0WKz

# CLONANDO REPOSITORIO
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit27/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit27-git@localhost's password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
Receiving objects: 100% (3/3), 286 bytes | 286.00 KiB/s, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
```
Hay que tener en cuenta que si el repositorio se encuentra por algún puerto en particular hay que indicarlo en la dirección del servidor, no en la ruta:
- bandit27-git@localhost:**2220**/home/bandit27-git/repo

Una vez tenemos el repositorio y entramos solo tiene un file llamado README, el cual contiene el password del siguiente nivel:
```sh
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz$ ls
repo
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz$ cd repo/
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz/repo$ ls
README
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz/repo$ file README 
README: ASCII text
bandit27@bandit:/tmp/tmp.0FWfzZ0WKz/repo$ cat README 
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

# Password next level:

Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

## Next level:

[[Level 28 -> 29]]