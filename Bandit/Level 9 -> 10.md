## Level Goal

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

## Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

# Solution
Para este nivel utilizaremos la función ***string***. Si nos fijamos tenemos el siguiente archivo:
```sh
bandit9@bandit:~$ file data.txt 
data.txt: data

bandit9@bandit:~$ cat data.txt 
ɖw����#���zQ��grm�H�0%��b���2ck���7l�1���X8�_��#����@�5H�XaP�0ٿ����֗�#mB}hF�4����ڐ�c�4��s%�5��P>�1�#w�^)��a ���
�|諭�<�������k�!�G�#�rP��2���3���/��	��.S���4��˒�P�����m!(�Y��s���^�1����(Q�Еt�~�� ��ݰ���o8�}�"I��2Ӥ��і�-E�r�4]����.�n����qt�g�8��Wq{�$�"���M~^u-�o�m�c]�/��ʔ�E��������0J9^�.�;64.3��,F~V��~!�
         3^���������r��G~n�xדS��Lș�wC�=�B�o�eډ��"̺��5�6��v���1�ky�0G�8\�[o��T�IY.j�Jy��+��o"�h�2ՙ^�$��
         C�����T�2!}�GϏ��lE�Suc���e�0�,����%�>��f�H����<ؒ��{y�
```
El archivo tiene partes no legibles. Sin embargo hay partes que sí. Para solo identificar esas partes usamos el comando ***string***:
```sh
bandit9@bandit:~$ strings data.txt 
#mB}hF
3%A<.
~^u-
0J9^
;64.3
,F~V
IY.j
T yb
@2Sv
=aA"f
\a!;========== the
 &iQ
L2@F
^^Tdj
PWAF=1
[0uTN
.
.
.

U,k,
t`"N6R
)$,h
~"O>
%@Lq:
uR.}
```
Tenemos que filtrar por los simbolos === :

```sh
bandit9@bandit:~$ strings data.txt | grep ====
\a!;========== the
========== passwordf
========== isc
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

Tenemos la contraseña localizada. Ahora solo generamos el output que nos de directamente la contraseña a usar:
```sh
bandit9@bandit:~$ strings data.txt | grep ==== | tail -n 1 | awk 'NF{print $NF}'
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

# Password next level:

FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

## Next Level:
[[Level 10 -> 11]]