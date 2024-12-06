# Level 29

### Level Goal

There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

Clone the repository and find the password for the next level.

### Commands you may need to solve this level

git

# Solution

By repeating the same process as in the previous level, we obtain the following:

```sh
#CREAMOS DIRECCION TEMPORAL
bandit28@bandit:~$ dir_temp=$(mktemp -d)
bandit28@bandit:~$ cd $dir_temp

#CLONAMOS REPOSITORIO
bandit28@bandit:/tmp/tmp.kZtnEbX94A$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit28/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit28-git@localhost's password: 
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.

#ENTRAMOS EN EL REPOSITORIO CLONADO Y LEEMOS README 
bandit28@bandit:/tmp/tmp.kZtnEbX94A$ ls
repo
bandit28@bandit:/tmp/tmp.kZtnEbX94A$ cd repo
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ ls
README.md
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

```

In this case, the `README` file does not contain the password; instead, it holds the value `xxxxxxx`. What we can do is check the commit history using ***git log***:

```sh
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ git log
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md
```

There are three commits:
- First: Adds the README
- Second: Adds data
- Third: Fixes an info leak

To see the changes made in each commit, we can use ***git show*** with the commit ID we want to view:

```sh
# COMMIT INITIAL: AÑADEN EL README
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ git show 5f7265568c7b503b276ec20f677b68c92b43b712
commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..7ba2d2f
--- /dev/null
#INFORMACION DEL README
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for level29 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <TBD>
+
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ git show 73f5d0435070c8922da12177dc93f40b2285e22a
commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: <TBD>                               # -- SUBSTITUYES POR +-
+- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7    # AÑADEN LA PASSWORD
 
bandit28@bandit:/tmp/tmp.kZtnEbX94A/repo$ git show 8cbd1e08d1879415541ba19ddee3579e80e3f61a
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7   # SUSTITUYEN PASSWORD POR XXXXX
+- password: xxxxxxxxxx
```

We can see that in the second commit, the password is added, and in the last one, it is replaced with `xxxxx`. In both commits, we can find the password for bandit29.

# Password for level 30:

4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
