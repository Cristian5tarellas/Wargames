# Level 9

### Level INfo

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

### Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Helpful Reading Material

- [Piping and Redirection](https://ryanstutorials.net/linuxtutorial/piping.php)

# Solution
For this case, we will use functions like ***uniq***. In its help, we can see different parameters:

```sh
Mandatory arguments to long options are mandatory for short options too.

  -c, --count    prefix lines by the number of occurrences
  
  -d, --repeated  only print duplicate lines, one for each group
.
.
.
  -i, --ignore-case     ignore differences in case when comparing

  -s, --skip-chars=N    avoid comparing the first N characters
  
  -u, --unique          only print unique lines
  
  -z, --zero-terminated     line delimiter is NUL, not newline
  
  -w, --check-chars=N   compare no more than N characters in lines
```

For this case, we will use the ***-u*** parameter (only print unique lines).

**IMPORTANT:** It works with the ***sort*** command.

```sh
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

# Password for level 10:

4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
