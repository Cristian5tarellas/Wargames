# Level 3

### Level Info

The password for the next level is stored in a file called spaces in this filename located in the home directory

### Commands you may need to solve this level
ls , cd , cat , file , du , find

### Helpful Reading Material
- Google Search for “spaces in filename”

# Solution
In this case, it's best to enclose the name in quotes so the ***cat*** command does not interpret it as multiple files. Alternatively, we can use an asterisk to match everything that comes after the first letter.
```bash
bandit2@bandit:~$ ls -l
total 4
-rw-r----- 1 bandit3 bandit2 33 Jul 17 15:57 spaces in this filename

bandit2@bandit:~$ cat "spaces in this filename"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```
# Password for level 4:

MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

