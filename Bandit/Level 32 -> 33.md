## Level Goal

After all this `git` stuff, it’s time for another escape. Good luck!

## Commands you may need to solve this level

sh, man

# Solution
En este caso al entrar en el nivel 32 nos encontramos con lo siguiente:
```sh

WELCOME TO THE UPPERCASE SHELL
>> export TERM=xterm
sh: 1: EXPORT: Permission denied
>> whoami
sh: 1: WHOAMI: Permission denied
>> 
```
Lo pone todo en mayúscula y da error. Pero estamos en una shell, de hecho es un sh. Si solo se puede en mayúscula podemos ver si las environment variable funcionan:
```sh
>> $TERM
sh: 1: xterm-kitty: Permission denied
>> $SHELL
WELCOME TO THE UPPERCASE SHELL
```
Parece que lo reconoce. Entonces podemos usar el símbolo $ para poder recuperar la shell. Si tenemos un script y podemos los parámetros $0, $1, $2:
- **$0**: hace referencia al script (output nombre del script).
- **$1**: parámetro 1 que añades al script.
- **$2**: parámetro 2 que añades al script.

Si el script **ejemplo.sh** es el siguiente:
```sh
#!/bin/bash
echo "$0, $1, $2"
```

El output será:

```sh
./ejemplo.sh param_1 param_2
#OUTPUT
ejemplo.sh param_1 param_2
```

Estando en una terminal podemos probar $0 por si nos devuelve la terminal:
```sh
>> $0
$ whoami
bandit33
$ bash
bandit33@bandit:~$ 
```
OJO que shomos bandit33. Pero estamos en:
```sh
bandit33@bandit:~$ pwd
/home/bandit32
```
Por lo tanto si vamos a nuestro directorio personal veremos lo siguiente:
```sh
bandit33@bandit:~$ cd /home/bandit33

bandit33@bandit:/home/bandit33$ ls
README.txt

bandit33@bandit:/home/bandit33$ cat README.txt 
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!