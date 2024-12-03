## Level Goal

The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on

## Commands you may need to solve this level

ssh, telnet, nc, openssl, s_client, nmap

## Helpful Reading Material

- [SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)

# Solution
Al tener una llave privada podemos suponer que es la llave privada del usuario 
*bandit14*.  Para ello nos podemos conectar a ese usuario con el archivo de la llave privada usando el código ***ssh -i***.

```sh
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
```
**OJO:** El servidor usa el puerto 2220 para la conexión ssh. Si no lo especifícas automáticamente intentará conectarse por el puerto 22 y te dará error. Para saber que puerto usa la conexión ssh podemos leer el archivo ***`/etc/ssh/sshd_config`***. En la sección ***Ports*** te indicará el puerto que se está usando.

Una vez dentro, leemos el password del archivo indicado:
```sh
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

**Comentario**: En el caso de tener una llave publica generada de nuestra llave privada podemos colocar la llave pública en la carpeta **.ssh** en un archivo llamado **authorized_keys**. Sin embargo para eso necesitamos acceso y poder escribir a la carpeta .ssh del usuario en el servidor que nos queremos conectar.

# Password next level:

MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

## Next level:
[[Level 14 -> 15]]