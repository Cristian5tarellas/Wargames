# Level 8

### Level Info

The password for the next level is stored in the file **data.txt** next to the word **millionth**

### Commands you may need to solve this level

[man](https://manpages.ubuntu.com/manpages/noble/man1/man.1.html), grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

# Solution

For this task, we will use the filtering function ***grep*** on the output of the *data.txt* document. We will use different methods to extract the password.
```sh
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

bandit7@bandit:~$ cat data.txt | grep "millionth" | awk '{print $2}'
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

bandit7@bandit:~$ cat data.txt | grep "millionth" | awk 'NF{print $NF}'
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

bandit7@bandit:~$ cat data.txt | grep "millionth" | rev | awk '{print $1}' | rev
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

bandit7@bandit:~$ cat data.txt | grep "millionth" | xargs | cut -d ' ' -f 2
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

bandit7@bandit:~$ cat data.txt | grep "millionth" | xargs | tr ' ' '\n' | tail -n 1
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

# Password for level 8:

dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
