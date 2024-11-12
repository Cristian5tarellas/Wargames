# Level 3

### Level Info

Well done. You’ve moved past an easy substitution cipher.

The main weakness of a simple substitution cipher is repeated use of a simple key. In the previous exercise you were able to introduce arbitrary plaintext to expose the key. In this example, the cipher mechanism is not available to you, the attacker.

However, you have been lucky. You have intercepted more than one message. The password to the next level is found in the file ‘krypton4’. You have also found 3 other files. (found1, found2, found3)

You know the following important details:

- The message plaintexts are in American English (*** very important) - They were produced from the same key (*** even better!)

Enjoy.

## Solution
Following the two important details we can think that the key is organised by the frequency of the letters in the documents: found3, found2, found1.
If we want to determine the frequency of each letter we can do it in one line:
```sh
❯ for i in {A..Z}; do seq1=$(cat found1 | grep -o . |grep "$i" | wc -l); seq2=$(cat found2 | grep -o . | grep "$i" | wc -l); seq3=$(cat found3 | grep -o . | grep "$i" | wc -l);freq=$(($seq1+$seq2+$seq3));echo $freq "$i"; done | sort -rn | awk 'NF{print $NF}' | tr -d '\n' | xargs

SQJUBNGCDZVWMYTXKELAFIORHP
```

However I suggest to make a script to understand step by step:
```sh
# Inside the loop we count the number of the appearence for each letter
for i in {A..Z}; do 
	seq1=$(cat found1 | grep -o . | grep "$i" | wc -l)
	seq2=$(cat found2 | grep -o . | grep "$i" | wc -l) 
	seq3=$(cat found3 | grep -o . | grep "$i" | wc -l)
	freq=$(($seq1+$seq2+$seq3)) # This is the total with the three files
	echo $freq "$i" # The output of the loop is the number and the letter
done | sort -rn | awk 'NF{print $NF}' | tr -d '\n' | xargs 
SQJUBNGCDZVWMYTXKELAFIORHP
```

The ouput of the loop (`echo $freq $i`) will have this format: 
```
34 A
87 B
.
.
.
9 Z
```
Then, we apply `| sort -rn | awk 'NF{print $NF}' | tr -d '\n' | xargs`:
- sort -rn: we sort the output using the numbers in a revers form
- awk 'NF{print $NF}': we take the letters once it is sorted.
- tr -d '\n': we remove the jump and we will have all letters organised in a line
- xargs: the ouput is the argument. If we don't do it we will see and the end of the output the symbol % because we are removing the last element doesn't have '\n'.

Now that we have the Frequency of letters of our texts, we can compare with the frequency of letters in American English: EATSORNIHCLDUPYFWGMBKVXQJZ

IMPORTANT: If you check now the table with the frequency of the letters in Wikipedia, it is not the same, we have some changes. This wargame is not updated.

Then we have a possible key:
```sh
SQJUBNGCDZVWMYTXKELAFIORHP -> EATSORNIHCLDUPYFWGMBKVXQJZ
```
If we apply this key to the krypton4 file:
```sh
❯ cat krypton4 | tr 'SQJUBNGCDZVWMYTXKELAFIORHP' 'EATSORNIHCLDUPYFWGMBKVXQJZ'
WELLD ONETH ELEVE LFOUR PASSW ORDIS BRUTE
```

## Password for level 4:

BRUTE

## Extra information
For this level there is a script in the repository Scripts:[krypton_03.sh](https://github.com/Cristian5tarellas/Scripts/blob/main/Bash/krypton_03.sh)
The script is called **krypton_03.sh** and it shows all the process step by step.
