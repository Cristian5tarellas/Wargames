# Leviathan Level 6

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=K65OkckFCCs&t=127s&ab_channel=Gabahack)

> This video shows my full process solving (in Spanish) Level 4 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---

## 🔍 Exploration

We begin by inspecting the current directory. We find an executable with SUID permissions:

```bash
leviathan6@gibson:~$ ls -l
total 16
-r-sr-x--- 1 leviathan7 leviathan6 15036 Apr 10 14:23 leviathan6
leviathan6@gibson:~$ file leviathan6
leviathan6: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=e16f2bca0c05d56ab07cd7c95324355952bb3cc5, for GNU/Linux 3.2.0, not stripped
```

When executed without arguments or with a wrong 4-digit code:

```bash
leviathan6@gibson:~$ ./leviathan6 
usage: ./leviathan6 <4 digit code>
leviathan6@gibson:~$ ./leviathan6  2345
Wrong
```

From the strings, we observe:

```bash
leviathan6@gibson:~$ strings leviathan6 
....
usage: %s <4 digit code>
/bin/sh
Wrong
....
```
This suggests that providing the correct code may spawn a shell.

## 💣 Exploitation

Since the binary expects a 4-digit code, and there’s no obvious vulnerability or output to analyze, a brute-force attack is the most practical solution:

```bash
leviathan6@gibson:~$ for i in $(seq -w 0 9999); do ./leviathan6 "$i" | grep -v "Wrong"; done
whoami
leviathan7
cat /etc/leviathan_pass/leviathan7
qEs5Io5yM8
```
💡 In the walkthrough, I figure out how to optimize the brute-force loop for faster execution and output filtering.

## 🔐 Password for Last Leviathan Level (7)
qEs5Io5yM8

# 🎉 Final Message
After logging in as leviathan7, we find the final message:

```bash
leviathan7@gibson:~$ ls
CONGRATULATIONS
```
🎉 Congratulations! You've completed all levels of the Leviathan wargame.
