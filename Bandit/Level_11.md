# Level 11

### Level Info

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

### Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Helpful Reading Material

- [Base64 on Wikipedia](https://en.wikipedia.org/wiki/Base64)

# Solution

Since the content is in base64, we will use the ***base64*** command. If we check its parameters, we have:
```sh
  -d, --decode          decode data
  -i, --ignore-garbage  when decoding, ignore non-alphabet characters
  -w, --wrap=COLS       wrap encoded lines after COLS character (default 76).Use 0 to disable line wrapping
```

Therefore:
```sh
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

Generating the output of the password:
```sh
bandit10@bandit:~$ cat data.txt | base64 -d | awk 'NF{print $NF}'
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

# Password for level 12:

dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
