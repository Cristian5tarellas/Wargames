# Level 31

### Level Info

There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

Clone the repository and find the password for the next level.

### Commands you may need to solve this level

git

# Solution

First we clone the repository:

```sh
bandit30@bandit:~$ dir_temp=$(mktemp -d)
bandit30@bandit:~$ cd $dir_temp
bandit30@bandit:/tmp/tmp.jqjsFUwrZy$ git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit30/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit30/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit30-git@localhost's password: 
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
bandit30@bandit:/tmp/tmp.jqjsFUwrZy$ ls
repo
bandit30@bandit:/tmp/tmp.jqjsFUwrZy$ cd repo/
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ ls
README.md
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ cat README.md README.md 
just an epmty file... muahaha
just an epmty file... muahaha
```

we check the commits:

```sh
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ git log
commit 60410f42e05023128098dc1f6991c75e6ae02e47 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:34 2024 +0000

    initial commit of README.md
```

We only have the file creation. Then, let's check the branches:

```sh
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

We only have the master branch, and there is nothing extra. What do we have left?

We can check the tags.

### Tags:
Like most VCSs, Git has the ability to tag specific points in a repository’s history as being important. Typically, people use this functionality to mark release points (`v1.0`, `v2.0` and so on). In this section, you’ll learn how to list existing tags, how to create and delete tags, and what the different types of tags are.

To list the *tags*, we can use ***git tag***:

```sh
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ git tag
secret
bandit30@bandit:/tmp/tmp.jqjsFUwrZy/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

# Password for level 32:

fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
