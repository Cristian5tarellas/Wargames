## Level Goal

The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Helpful Reading Material

- [Rot13 on Wikipedia](https://en.wikipedia.org/wiki/ROT13)

# Solution

Este nivel se centra en el cifrado césar. En este caso es una rotación de 13 letas. Total de posiciones que se puede rotar es 25, es decir el número de letras del abecedario.

Rotación 00: abcdefghijklmnopqrstuvwyz 
Rotación 13: nopqrstuvwyzabcdefghijklm

De manera manual podemos utilizar el comando ***tr***:
```sh
bandit11@bandit:~$ cat data.txt 
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4

bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
Por lo tanto:
```sh
bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]' | awk 'NF{print $NF}'
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

- También podemos utilizar la página https://rot13.com/
# Password next level:

7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

## Next level:
[[Level 12 -> 13]]