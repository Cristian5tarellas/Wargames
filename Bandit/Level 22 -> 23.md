## Level Goal

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Commands you may need to solve this level

cron, crontab, crontab(5) (use “man 5 crontab” to access this)

# Solution
Repetimos el mismo proceso que en el nivel anterior para ver que esta realizando el script en este caso:
```sh
bandit22@bandit:~$ cd /etc/cron.d

bandit22@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat

bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null

bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:/etc/cron.d$ 
```
##### Que hace el script?
```sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
Tiene dos variables:
- **myname**: bandit23
- **mytarget**: hash
En la última linea copia la contraseña de bandit23 en un archivo que lo mete en /tmp/ y el nombre del archivo es el hash creado. 

Por lo tanto:
```sh
#AVERIGUAMOS EL VALOR DE MY TARGE YA QUE SABEMOS EL VALOR DE MYNAME
bandit22@bandit:/usr/bin$ mytarget=$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)
#COMPROBAMOS EL RESULTADO
bandit22@bandit:/usr/bin$ echo $mytarget
8ca319486bfbbc3663ea0fbe81326349
#VEMOS LOS PERMISOS DEL ARCHIVO CREADO Y LO LEEMOS
bandit22@bandit:/usr/bin$ ls -l /tmp/$mytarget
-rw-rw-r-- 1 bandit23 bandit23 33 Aug 29 14:17 /tmp/8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/usr/bin$ cat /tmp/$mytarget
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

# Password next level:

0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

## Next level:
[[Level 23 -> 24]]