## Level Goal

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

## Commands you may need to solve this level

[ls](https://man7.org/linux/man-pages/man1/ls.1.html) , [cd](https://man7.org/linux/man-pages/man1/cd.1p.html) , [cat](https://man7.org/linux/man-pages/man1/cat.1.html) , [file](https://man7.org/linux/man-pages/man1/file.1.html) , [du](https://man7.org/linux/man-pages/man1/du.1.html) , [find](https://man7.org/linux/man-pages/man1/find.1.html) , [grep](https://man7.org/linux/man-pages/man1/grep.1.html)

# Solution
Igual que en el nivel anterior [[Level 5 -> 6]], utiizamos la función ***find*** con la info proporcionada. Como el archivo está en cualquier sitio del servidor lo aplicaremos sobre el directorio raíz (/).
```sh
bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```
No olvidar que al no ser root tendremos muchos permisos denegados. Usaremos *2>/dev/null* para quitarnos todos los errores.

Una vez localizado el archivo, podemos comprobar si se es legible (entendible) y lo leemos:
```sh
bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null | xargs file
/var/lib/dpkg/info/bandit7.password: ASCII text

bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c -readable 2>/dev/null | xargs cat
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

# Password next level:

morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

## Next level:
[[Level 7 -> 8]]]