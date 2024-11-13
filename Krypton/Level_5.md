# Level 5

### Level Info

FA can break a known key length as well. Lets try one last polyalphabetic cipher, but this time the key length is unknown. Note: the text is writen in American English

Enjoy.

## Solution
For this level, we can modify the script from the previous level to adapt it for the new files we must use, including an additional file, **found3**. We’ll follow a similar process as before, but with an important change: the length of the key is unknown.

If you used the script [krypton_04.sh](https://github.com/Cristian5tarellas/Scripts/blob/main/Bash/krypton_04.sh), you’ll recognize that it includes a process to determine the key length. The part of the script responsible for this process is shown below:
```bash
# Determining the length of the key
echo -e "\n\n${blueColour}Determining the length of the key:${endColour}\n"
# Defining an array for the dictionary
declare -a abc=()
for il in {A..Z};do
  abc+=("$il")
done
echo -e "${blueColour}[+]${endColour} ${grayColour}The letters that we use come from the American English:${endColour}"
echo "${abc[@]}"

# Exploring the problable length of the key
max_length=12
echo -e "${blueColour}[+]${endColour} ${grayColour}Checking the problable length of the key:${endColour}"
echo -e "\t${grayColour}· The maximum value of coincident letters will correspond to the length of the key${endColour}"
echo -e "\t${grayColour} · The maximum length is $max_length letters${endColour}"

declare -a text1=($( cat found1 | tr -d ' ' | fold -w1))
declare -a text2=($( cat found2 | tr -d ' ' | fold -w1))
declare -a text3=($( cat found3 | tr -d ' ' | fold -w1))

shift_max=0
top_coincidence=0
for j in $(seq 1 $max_length);do
count_coincidence=0
  # For found1
  for i in $(seq 0 $((${#text1[@]}-$((1+$j)))));do
    letter_1="${text1[$i]}"               # Original position letter
    letter_2="${text1[$i+j]}"               # Shift position letter
    if [ "$letter_1" == "$letter_2" ];then
      let count_coincidence+=1
    fi
  done
  # For found2
  for i in $(seq 0 $((${#text2[@]}-$((1+$j)))));do
    letter_1="${text2[$i]}"               # Original position letter
    letter_2="${text2[$i+j]}"               # Shift position letter
    if [ "$letter_1" == "$letter_2" ];then
      let count_coincidence+=1
    fi
  done
  # For found3
  for i in $(seq 0 $((${#text3[@]}-$((1+$j)))));do
    letter_1="${text3[$i]}"               # Original position letter
    letter_2="${text3[$i+j]}"               # Shift position letter
    if [ "$letter_1" == "$letter_2" ];then
      let count_coincidence+=1
    fi
  done

  if [ "$count_coincidence" -gt "$top_coincidence" ];then
    top_coincidence=$count_coincidence
    shift_max=$j
  fi
  echo -e "\t${blueColour}[-]${endColour} ${grayColour}Shift of length${endColour} ${blueColour}$j${endColour}${grayColour}: $count_coincidence${endColour}"
done
echo -e "\n${blueColour}[+]${endColour} ${grayColour}Key length:${endColour} ${blueColour}$shift_max${endColour}"
```
This script identifies groups of letters with the most frequent repetitions across all three files (**found1**, **found2**, and **found3**). For each letter position in a potential key, the script records the count of the highest-frequency letter. The parameter _max_length_ limits the maximum length of the key for consideration.

When you run this script, you’ll see the output as follows:
```bash
Information of the files:

[+] The frequency of letters in found1 is:
91 L 85 X 83 R 79 V 74 P 71 S 71 C 70 G 67 M 65 I 63 E 60 Z 59 A 58 Y 56 T 55 K 51 B 49 N 48 O 40 H 35 U 31 Q 31 D 30 W 30 F 28 J
[+] The frequency of letters in found3 is:
103 Y 98 L 93 S 90 P 88 R 88 K 80 V 78 X 78 M 78 E 71 I 71 G 70 C 68 O 66 T 58 B 57 Z 55 A 52 H 50 W 49 N 47 U 47 F 47 D 44 J 33 Q
[+] The frequency of letters in found2 is:
92 L 90 Y 86 E 84 R 82 X 79 G 78 P 77 S 64 A 63 M 58 O 58 K 57 U 57 H 56 Z 56 V 54 I 53 C 49 B 48 T 47 J 47 F 43 N 42 Q 39 D 37 W

[+] The total value for each letter is:
    281 L	   245 X	   227 E	   208 M	   190 I	   173 Z	   149 H	   124 F	   117 D
    255 R	   242 P	   220 G	   201 K	   178 A	   170 T	   141 N	   119 J	   106 Q
    251 Y	   241 S	   215 V	   194 C	   174 O	   158 B	   139 U	   117 W


Determining the length of the key:

[+] The letters that we use come from the American English:
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
[+] Checking the problable length of the key:
	· The maximum value of coincident letters will correspond to the length of the key
	· The maximum length is 12 letters
	[-] Shift of length 1: 198
	[-] Shift of length 2: 142
	[-] Shift of length 3: 169
	[-] Shift of length 4: 167
	[-] Shift of length 5: 183
	[-] Shift of length 6: 204
	[-] Shift of length 7: 195
	[-] Shift of length 8: 199
	[-] Shift of length 9: 294
	[-] Shift of length 10: 208
	[-] Shift of length 11: 180
	[-] Shift of length 12: 217

[+] Key length: 9


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
[+] The key is: KEYLENGTH

Solution

[+] The password for the level 6 is: RANDOM
```

## Password for level 6:

RANDOM

## Extra info:
The script used for this level is: [krypton_05.sh](https://github.com/Cristian5tarellas/Scripts/blob/main/Bash/krypton_05.sh)
If you want to check all the scripts, they are located in my repository Scripts: [Scripts_Repository](https://github.com/Cristian5tarellas/Scripts)

