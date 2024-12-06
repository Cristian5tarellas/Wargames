# Level 28

### Level Goal

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

### Commands you may need to solve this level

git

# Solution

The first step is to clone the provided repository using ***git clone***.  
**Note:** Always create a temporary directory so that everything we do can be deleted when the session is closed.

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
Keep in mind that if the repository is hosted on a specific port, you need to specify it in the server address, not in the path:
- `bandit27-git@localhost:2220/home/bandit27-git/repo`

Once we have the repository and access it, it only contains a file called `README`, which holds the password for the next level:

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

# Password for level 29:

Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
