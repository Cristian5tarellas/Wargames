# Level 13

### Level Info

The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

### Commands you may need to solve this level

grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

### Helpful Reading Material

- [Hex dump on Wikipedia](https://en.wikipedia.org/wiki/Hex_dump)

# Solution

**Note**: We have copied the file to our PC to avoid creating files on the server and having to delete them.

This level has two parts. The first teaches us how to work with hexadecimal files, and the second teaches us how to automatically decompress files.

## S1) Hexadecimal Files:

To work with hexadecimal files, we will use the ***xxd*** command.

```sh
❯ cat data.txt | xxd -r | sponge data.txt

❯ file data.txt
data.txt: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577
```
We read the hexadecimal file and translated it by overwriting it in the same file using the ***sponge*** function. With the ***xxd*** command and the ***-r*** parameter, we translate from hexadecimal.

As we can see, we have a *gzip* file.

## S2) Automatic Decompression
For this part, we will create a script that identifies the file to decompress and recursively decompresses it until it retrieves the text file containing the password.

**Script:**
```sh
#!/bin/bash

# Determinar el archivo a descomprimir
file2deccom=$(ls | xargs file | grep compressed | xargs | awk -F: '{print $1}')

# Creamos un loop hasta que encontremos/descomprimamos un archivo que contenga texto
while file $file2deccom | grep -v "text" ; do
  echo -e "\n [+] File to decompress: ${file2deccom}"
  next_file=$(7z l $file2deccom | tail -n 3 | head -n 1 | awk 'NF{print $NF}')
  7z x $file2deccom &>/dev/null 
  file2deccom=$next_file
done

# Mostramos el archivo que contiene el password
echo -e "\n [+] The file with the password is ${file2deccom}\n"
# Mostramos el password
echo -e "\n The password is: $(cat $file2deccom | awk 'NF{print $NF}')\n"

```

In this way, we determine the password for this case.

```sh
❯ ./descompresor_file.sh
data.txt: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577

 [+] File to decompress: data.txt
data2.bin: bzip2 compressed data, block size = 900k

 [+] File to decompress: data2.bin
data2: gzip compressed data, was "data4.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 20480

 [+] File to decompress: data2
data4.bin: POSIX tar archive (GNU)

 [+] File to decompress: data4.bin
data5.bin: POSIX tar archive (GNU)

 [+] File to decompress: data5.bin
data6.bin: bzip2 compressed data, block size = 900k

 [+] File to decompress: data6.bin
data6: POSIX tar archive (GNU)

 [+] File to decompress: data6
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 49

 [+] File to decompress: data8.bin

 [+] The file with the password is data9.bin


 The password is: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```
# Password next level:

FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
