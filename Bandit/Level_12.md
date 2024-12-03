# Level 12

### Level Info

The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

### Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Helpful Reading Material

- [Rot13 on Wikipedia](https://en.wikipedia.org/wiki/ROT13)

# Solution

This level focuses on the Caesar cipher. In this case, it's a rotation of 13 letters. The total number of positions that can be rotated is 25, which is the number of letters in the alphabet.

Rotation 00: abcdefghijklmnopqrstuvwyz  
Rotation 13: nopqrstuvwyzabcdefghijklm

Manually, we can use the ***tr*** command:
```sh
bandit11@bandit:~$ cat data.txt 
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4

bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
Therefore:
```sh
bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]' | awk 'NF{print $NF}'
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

- We can also use the website [https://rot13.com/](https://rot13.com/) for this.

# Password for level 12:

7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
