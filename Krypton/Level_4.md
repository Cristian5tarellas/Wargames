# Level 4

### Level Info

Good job!

You more than likely used some form of FA and some common sense to solve that one.

So far we have worked with simple substitution ciphers. They have also been ‘monoalphabetic’, meaning using a fixed key, and giving a one to one mapping of plaintext (P) to ciphertext (C). Another type of substitution cipher is referred to as ‘polyalphabetic’, where one character of P may map to many, or all, possible ciphertext characters.

An example of a polyalphabetic cipher is called a Vigenère Cipher. It works like this:

If we use the key(K) ‘GOLD’, and P = PROCEED MEETING AS AGREED, then “add” P to K, we get C. When adding, if we exceed 25, then we roll to 0 (modulo 26).

```
P P R O C E E D M E E T I N G A S A G R E E D\
K G O L D G O L D G O L D G O L D G O L D G O\
```

becomes:

```
P 15 17 14 2 4 4 3 12 4 4 19 8 13 6 0 18 0 6 17 4 4 3\
K 6 14 11 3 6 14 11 3 6 14 11 3 6 14 11 3 6 14 11 3 6 14\
C 21 5 25 5 10 18 14 15 10 18 4 11 19 20 11 21 6 20 2 8 10 17\
```

So, we get a ciphertext of:

```
VFZFK SOPKS ELTUL VGUCH KR
```

This level is a Vigenère Cipher. You have intercepted two longer, english language messages (American English). You also have a key piece of information. You know the key length!

For this exercise, the key length is 6. The password to level five is in the usual place, encrypted with the 6 letter key.

Have fun!

## Solution
For this level is better to work directly in a script. In this exercise I will explain the different parts of the script XX used for this level. 

The first part of the script explores the data tha we have (**found1** and **found2**). We load the files and count the letters for each file and in total:
```bash
# Reading the files needed
file[found1]=$(cat found1)
if [ "$?" != 0 ];then
  echo -e "\n ${redColour}[!] There is an error. Probably you don't have the file needed.\n Check${endColour} ${grayColour}$0 -h${endColour} ${redColour}for help${endColour}"
  exit 1
fi
file[found2]=$(cat found2)
if [ "$?" != 0 ];then
  echo -e "\n ${redColour}[!] There is an error. Probably you don't have the file needed.\n Check${endColour} ${grayColour}$0 -h${endColour} ${redColour}for help${endColour}"
  exit 1
fi


echo -e "\n${yellowColour}Information of the files:${endColour}\n"
# Frequency analysis: Letters
for key in ${!file[@]}; do
  # List of different letters and their frequency in the text
  list_f[$key]=$(echo "${file[$key]}" | tr -d ' ' | grep -o . | sort | uniq -c | sort -nr)
  echo -e "${yellowColour}[+]${endColour} ${grayColour}The frequency of letters in${endColour} ${yellowColour}$key${endColour} ${grayColour}is:${endColour}"
  echo ${list_f[$key]}
done

echo -e "\n${yellowColour}[+]${endColour} ${grayColour}The total value for each letter is:${endColour}"
echo "${file[found1]}" "${file[found2]}" | tr -d ' ' | grep -o . | sort | uniq -c | sort -nr | column

```
The output in the screen is the next:
```bash
Information of the files:

[+] The frequency of letters in found1 is:
105 I 94 S 80 R 79 Y 78 K 75 V 73 W 73 L 73 J 70 X 69 M 63 C 60 F 55 E 50 B 45 P 43 O 36 N 35 G 34 T 32 Z 32 D 29 Q 25 H 23 U 19 A
[+] The frequency of letters in found2 is:
162 I 161 R 146 X 145 Y 136 S 122 M 117 C 112 V 107 K 105 J 105 E 104 F 97 L 91 P 87 W 77 N 67 B 66 Q 65 G 63 T 63 D 59 U 57 O 52 H 50 Z 37 A

[+] The total value for each letter is:
    267 I	   224 Y	   187 V	   178 J	   160 W	   117 B	   100 G	    95 D	    77 H
    241 R	   216 X	   185 K	   170 L	   160 E	   113 N	    97 T	    82 Z	    56 A
    230 S	   191 M	   180 C	   164 F	   136 P	   100 O	    95 Q	    82 U

```

In this level we know that we have a key length of 6 words, then we will go directly to know the letters corresponend to the key. To get the key we need to know the common letters with higher frequency in each position of the letter of the key. Once we have the most frequent letters in each possition of the letter of the key
```bash
#Decrypting the key
echo -e "\n\n${greenColour}Decrypting the key${endColour}\n" 
# Total numbers with the three files
# shift_max is the length of the key
for i in $(seq 1 $shift_max);do
avg_letter=[]
  for letter in {A..Z}; do
    num_f1=$(echo "${file[found1]}" |  tr -d ' ' | fold -w$shift_max | cut -c $i | sort | uniq -c | grep $letter | awk '{print $1}')
    if [ -z "$num_f1" ];then num_f1=0;fi
    num_f2=$(echo "${file[found2]}" |  tr -d ' ' | fold -w$shift_max | cut -c $i | sort | uniq -c | grep $letter | awk '{print $1}')
    if [ -z "$num_f2" ];then num_f2=0;fi
    ave_num=$(echo "scale=3; ($num_f1+$num_f2)" | bc)
    avg_letter+="$ave_num $letter\n" 
  done
  #echo -e $avg_letter | sort -rn 
  echo -e "${greenColour}[+]${endColour} ${grayColour} Letter${endColour} ${greenColour}$i${endColour}${grayColour}:${endColour}"
  echo -e $avg_letter | sort -rn | awk '{print $2}' | xargs
  pass_code+="$(echo -e $avg_letter | sort -rn | awk '{print $2}' | xargs | head -c 1)"
done

echo -e "\n${greenColour}[+]${endColour}${grayColour} The most frequent letters are:${endColour} ${greenColour}$pass_code${endColour}"

```

In this part the most important block is:
```bash
for i in $(seq 1 $shift_max);do
avg_letter=[]
  for letter in {A..Z}; do
    num_f1=$(echo "${file[found1]}" |  tr -d ' ' | fold -w$shift_max | cut -c $i | sort | uniq -c | grep $letter | awk '{print $1}')
    if [ -z "$num_f1" ];then num_f1=0;fi
    num_f2=$(echo "${file[found2]}" |  tr -d ' ' | fold -w$shift_max | cut -c $i | sort | uniq -c | grep $letter | awk '{print $1}')
    if [ -z "$num_f2" ];then num_f2=0;fi
    ave_num=$(echo "scale=3; ($num_f1+$num_f2)" | bc)
    avg_letter+="$ave_num $letter\n" 
  done
  #echo -e $avg_letter | sort -rn 
  echo -e "${greenColour}[+]${endColour} ${grayColour} Letter${endColour} ${greenColour}$i${endColour}${grayColour}:${endColour}"
  echo -e $avg_letter | sort -rn | awk '{print $2}' | xargs
  pass_code+="$(echo -e $avg_letter | sort -rn | awk '{print $2}' | xargs | head -c 1)"
done
```
This section is responsible of splitting the text in blocks of length 6 (the same lenght of the key) and explore for each position in a loop from 1 to the length of the key (*shift_max*) the letter with a higher frequency. The output of this part is the following:
```bash
Decrypting the key

[+]  Letter 1:
O K C D Y B R S X V P N Q M W G E L F Z I U A T J H
[+]  Letter 2:
I S X E M V L W R H P J G T Y K C Z F Q O B A U N D
[+]  Letter 3:
C R M Y Q L G P B F J S U K D E T I Z W N H V O A X
[+]  Letter 4:
P E L D Y Z T S O C N Q W X R H M J F V G I B A U K
[+]  Letter 5:
I S E X V M R P L W H J Y G C T K F O Z Q N B A U D
[+]  Letter 6:
R G N F B V E Y U Q S P J L Z T H O C X I W K D A M
[+]  Letter 7:
K G Z U T X O Y N R J I L V E C M S H B Q A W P F D
[+]  Letter 8:
X T M B G K L H E Y Z P V R N I F W U D O S J A Q C
[+]  Letter 9:
L V U H Z Y P S O K J W N M F D T I C B R E A X Q G

[+] The most frequent letters are: OICPIRKXL
```

Now we have to determine the key. For this process we have to know that the letter with the maximum frequency in American English is the **E**. Then we can apply the decypher of the Vigenère method. We have a function in the script that is focused on the decryption:
```bash
function decryption(){
  # Variables
  pass=$1
  key=$2
  pass_trans=""
  #Parameters
  length_abc=26
  len_pass=${#pass}
  # Creating an array to associate the letter to the position
  declare -A abc_id
  declare -a id_abc
  itera=0
  for i in {A..Z};do
    abc_id[$i]=$itera
    id_abc[$itera]=$i
    let itera+=1
  done
  # Repeat key to match the length of the password
  for ((i=0; i<len_pass; i++));do
    key_expanded+=$key
  done
  key_expanded=${key_expanded:0:$len_pass}
  
  for i in $(seq 1 $len_pass);do
    letter_key=$(echo $key_expanded | head -c $i | tail -c 1)
    letter_pass=$(echo $pass | head -c $i | tail -c 1)
    letter_decrypt=$((abc_id[$letter_pass]-abc_id[$letter_key]))
    letter_decrypt=$(($letter_decrypt % $length_abc))
    pass_trans+="${id_abc[$letter_decrypt]}"
  done
  echo "$pass_trans"
}

```

If we use the function to know the key:
```bash
#Decrypting the KEY for the decryption
most_freq_letter="E"
key_codec=$(decryption "$pass_code" "E")

echo -e "${greenColour}[+]${endColour} ${grayColour}The Key is:${endColour} ${greenColour}$key_codec${endColour}"
```
Finally we can repeat the same process to decypher the password for the next level:
```bash
#Decrypting the password
password=$(cat krypton5 | tr -d ' ')
password_decrypted=$(decryption "$password" "$key_codec")

echo -e "\n${purpleColour}Solution${endColour}\n"
echo -e "${purpleColour}[+]${endColour} ${grayColour}The password for the level 5 is:${endColour} ${purpleColour}${password_decrypted}${endColour}"
```
The result that we have in the screen:
```bash

[+] The most frequent letters are: JVIOIC
[+] The Key is: FREKEY

Solution

[+] The password for the level 5 is: CLEARTEXT
```

## Password for next level:

CLEARTEXT

## Extra information:
You can find the script for this level in my repository Scripts: [krypton_04.sh](https://github.com/Cristian5tarellas/Scripts/blob/main/Bash/krypton_04.sh)
