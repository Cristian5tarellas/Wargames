# Level 30

### Level Info

There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

Clone the repository and find the password for the next level.

### Commands you may need to solve this level

git

# Solution

We begin with the standard procedure to clone the repository and check the information it contains:

```sh
bandit29@bandit:~$ dir_temp=$(mktemp -d)
bandit29@bandit:~$ cd $dir_temp
bandit29@bandit:/tmp/tmp.a7OVYRQX8V$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit29/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit29-git@localhost's password: 
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit29@bandit:/tmp/tmp.a7OVYRQX8V$ ls
repo
bandit29@bandit:/tmp/tmp.a7OVYRQX8V$ cd repo/
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ ls
README.md
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

In this case, there is no password information. We check if there are any commits:


```sh
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ git log
commit efa5bd803f8335e5e5e9da5c4c7c876aefc9f8b4 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:31 2024 +0000

    fix username

commit 5a53eb83a43bac1f0b4e223e469b40ef68a4b6e6
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:31 2024 +0000

    initial commit of README.md
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ git show efa5bd803f8335e5e5e9da5c4c7c876aefc9f8b4
commit efa5bd803f8335e5e5e9da5c4c7c876aefc9f8b4 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:31 2024 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.
 
 ## credentials
 
-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>
 
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ git show 5a53eb83a43bac1f0b4e223e469b40ef68a4b6e6
commit 5a53eb83a43bac1f0b4e223e469b40ef68a4b6e6
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:31 2024 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..2da2f39
--- /dev/null
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for bandit30 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <no passwords in production!>
+
```

The commits only include the creation of the file and a user change. The password is in production. Typically, in GitHub, the *master branch* contains all the final and tested work, while you can have different *branches* for ongoing tasks. To check the branches, we use ***git branch***. In this case, we use the **-a** parameter to view all the branches:

```sh
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ git branch -a
  a
  master
* sploits-dev
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ git checkout dev
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ ls
code  README.md
```

To switch from one branch to another, we use the command ***git checkout*** *branch_name*.

Finally, if we read the README file we can get the password:

```sh
bandit29@bandit:/tmp/tmp.a7OVYRQX8V/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

# Password for level 31:

qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
