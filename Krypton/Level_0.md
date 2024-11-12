# Krypton Level 0
## Level Info

Welcome to Krypton! The first level is easy. The following string encodes the password using Base64:

```
S1JZUFRPTklTR1JFQVQ=
```

Use this password to log in to krypton.labs.overthewire.org with username krypton1 using SSH on port 2231. You can find the files for other levels in /krypton/

# Solution
In base64, the equal sign (`=`) is used for padding. That is, the equal sign does not own an index and is not involved in the encoding of data. By and large, the padding character ensures that the length of Base64 value is a multiple of 4 bytes and it is always appended at the end of the output.
In this case we have to decode the password:
```sh
❯ echo "S1JZUFRPTklTR1JFQVQ" | base64 -d
KRYPTONISGREAT    
```

# Password for level 1:

KRYPTONISGREAT
