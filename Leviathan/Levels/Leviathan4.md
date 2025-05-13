# Leviathan Level 4

You can watch the walkthrough for this level here:  
[![YouTube](https://img.shields.io/badge/YouTube-Walkthrough-red?logo=youtube)](https://www.youtube.com/watch?v=vsL3IP_jPL4&list=PLCsnC_Nj6V52jf-QzPWo4-KprAGSn5kxa&index=3&ab_channel=Gabahack)

> This video shows my full process solving (in Spanish) Level 4 from scratch, including the obstacles and mistakes I faced along the way. Some walkthroughs might be longer or shorter depending on the complexity of the level or how quickly I find the solution.

---


## 🔍 Exploration

We begin by inspecting the current directory. Since it's empty, we check for hidden files and folders. We find a hidden directory named .trash containing a binary named bin:

```bash
leviathan4@gibson:~$ ls
leviathan4@gibson:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile  .trash
leviathan4@gibson:~$ cd .trash/
leviathan4@gibson:~/.trash$ ls -l
total 16
-r-sr-x--- 1 leviathan5 leviathan4 14940 Apr 10 14:23 bin
```

When we execute the binary, it prints what appears to be binary-encoded text:

```bash
leviathan4@gibson:~/.trash$ ./bin
00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010 
```

## 💣 Exploitation

The binary output is a string of binary numbers, each representing a character in ASCII. We can decode it using perl. In the walkthrough, I explain how this command works and how to use it to convert binary data to readable text:

```bash
leviathan4@gibson:~/.trash$ echo "00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010" | perl -pe '$_=pack"B*", join("",split)'
0dyxT7F4QD
```

## 🔐 Password for Leviathan 5
0dyxT7F4QD
