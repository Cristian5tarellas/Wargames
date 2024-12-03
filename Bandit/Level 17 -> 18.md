## Level Goal

There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

## Commands you may need to solve this level

cat, grep, ls, diff

# Solution
En este caso utilizamos la función ***diff*** para comparar dos archivos. 
```sh
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```
Por lo tanto la contraseña es x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO.

El comando ***diff*** compara strings, pero tiene muchas funciones. Tener en cuenta si queremos ver diferencias entre archivos, ***diff*** puede ser la primera opción que indagar.

# Password next level:

x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

## Next Level:
[[Level 18 -> 19]]