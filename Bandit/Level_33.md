# Level 33

### Level Info

After all this `git` stuff, it’s time for another escape. Good luck!

### Commands you may need to solve this level

sh, man

# Solution

Upon entering level 32, we find the following:

```sh

WELCOME TO THE UPPERCASE SHELL
>> export TERM=xterm
sh: 1: EXPORT: Permission denied
>> whoami
sh: 1: WHOAMI: Permission denied
>> 
```

It displays everything in uppercase and gives an error. But we are in a shell, specifically a ***sh*** shell. If it can only be in uppercase, we can check if the environment variables work:

```sh
>> $TERM
sh: 1: xterm-kitty: Permission denied
>> $SHELL
WELCOME TO THE UPPERCASE SHELL
```

It seems to recognize it. Then we can use the symbol `$` to retrieve the shell. If we have a script, we can use the parameters `$0`, `$1`, and `$2`:
- **$0**: refers to the script (outputs the script name).
- **$1**: the first parameter you add to the script.
- **$2**: the second parameter you add to the script.

If the script **ejemplo.sh** is as follows:

```bash
#!/bin/bash
echo "Script name: $0"
echo "First parameter: $1"
echo "Second parameter: $2"


```sh
#!/bin/bash
echo "$0, $1, $2"
```

The output is:

```sh
./ejemplo.sh param_1 param_2
#OUTPUT
ejemplo.sh param_1 param_2
```

While in a terminal, we can try `$0` to see if it returns the terminal:
```sh
>> $0
$ whoami
bandit33
$ bash
bandit33@bandit:~$ 
```

**Note:** We are bandit33, but we are in a restricted shell, possibly a sh shell, which could impact how we use specific commands or parameters.


```sh
bandit33@bandit:~$ pwd
/home/bandit32
```

Therefore, if we go to our home directory, we will see the following:

```sh
bandit33@bandit:~$ cd /home/bandit33

bandit33@bandit:/home/bandit33$ ls
README.txt

bandit33@bandit:/home/bandit33$ cat README.txt 
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
