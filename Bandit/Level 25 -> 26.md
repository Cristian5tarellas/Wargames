## Level Goal

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use Powershell to `ssh` into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

## Commands you may need to solve this level

ssh, cat, more, vi, ls, id, pwd

# Solution
Si nos intentamos conectar con la key sale lo siguiente:
```sh
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p 2220
.
.
.
  Enjoy your stay!

  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to localhost closed.
```
Para ver que ocurre podemos ver el archivo /etc/passwd para ver que ejecuta bandit26 al iniciar sesion:
```sh
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
andit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
bandit25@bandit:~$ cat /home/bandit26/text.txt
cat: /home/bandit26/text.txt: Permission denied
```
Ejecuta el archivo **showtext** que es un script bash que ejecuta el comando **more** en el archivo text.txt que no tenemos acceso. 

Al estar con el comando **more** podemos usar para entrar en las opciones antes de que se cierre la conexión. Para eso tenemos que NO visualizar todo el contenido. Como? Reduciendo el tamaño de la ventana:
```sh
  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
:set shell=/bin/bash     
```
En modo **more** con la tecla **V** podemos entrar en modo visual y con dos puntos podemos inyectar comandos. Por ejemplo ***set*** para definir una variable que en este caso será una bash.
Ejecutamos la variable y :
```sh
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
:shell
bandit26@bandit:~$ 
```
Ya estamos en una bash:
```sh
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ cat text.txt 
  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
```
Podemos ver que el texto era el banner.
Ahora Tenemos un binario **bandit27-do**. Ya hemos trabajado con este binario. Por lo tanto Podemos conseguir acceso a bandit27.

```sh
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

# Password level 27:

upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

## Next level:
[[Level 26 -> 27]]