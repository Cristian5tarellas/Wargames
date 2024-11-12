# Level 1

### Level Info

The password for level 2 is in the file ‘krypton2’. It is ‘encrypted’ using a simple rotation. It is also in non-standard ciphertext format. When using alpha characters for cipher text it is normal to group the letters into 5 letter clusters, regardless of word boundaries. This helps obfuscate any patterns. This file has kept the plain text word boundaries and carried them to the cipher text. Enjoy!

# Solution

In **/krypton/krypton1** we have the file krypton2 that contains the password:
```sh
krypton1@bandit:/krypton/krypton1$ cat krypton2 
YRIRY GJB CNFFJBEQ EBGGRA
```

It is codified with rot13. We can use https://rot13.com/ or we can use the function **tr**.
```sh
krypton1@bandit:/krypton/krypton1$ cat krypton2 | tr 'A-Z' 'N-ZA-M'
LEVEL TWO PASSWORD ROTTEN
```
If we replace the letter A by the 13th letter, we have that A should be N.  

# Password for level 2:

ROTTEN

## Extra information
In this exercise I create the following script (brute_ceasar.sh). Basically check the connection ssh for each decrypted password for all possible combinations (26). In addition you can find extra options/functions to play with the ceasar cipher. However, with normal configurations of the firewall you could have problems in a ssh server (This is not the case for krypton.labs.overthewire.org)

You can find this script in my github: [Kryton_01](https://github.com/Cristian5tarellas/Scripts/tree/main/Bash)
