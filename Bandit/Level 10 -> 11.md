## Level Goal

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Helpful Reading Material

- [Base64 on Wikipedia](https://en.wikipedia.org/wiki/Base64)
# Solution
Al estar el contenido en base64 utilizaremos el comando ***base64***. Si vemos sus parametros tenemos:
```sh
  -d, --decode          decode data
  -i, --ignore-garbage  when decoding, ignore non-alphabet characters
  -w, --wrap=COLS       wrap encoded lines after COLS character (default 76).Use 0 to disable line wrapping
```
Por lo tanto:
```sh
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
Generando el output de password:
```sh
bandit10@bandit:~$ cat data.txt | base64 -d | awk 'NF{print $NF}'
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

# Password next level:

dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

## Next level:
[[Level 11 -> 12]]