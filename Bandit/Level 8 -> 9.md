## Level Goal

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Helpful Reading Material

- [Piping and Redirection](https://ryanstutorials.net/linuxtutorial/piping.php)

# Solution
Para este caso utilizaremos funciones como ***uniq***. En su ayuda podemos ver diferentes parámetros:
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

Para este caso, utilizaremos el parámetro ***-u*** (only print unique lines). 

**IMPORTANTE:** Funciona con el comando ***sort***.
```sh
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

# Password next level:

4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

## Next Level:
[[Level 9 -> 10]]