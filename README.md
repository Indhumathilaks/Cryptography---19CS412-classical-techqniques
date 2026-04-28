# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
PROGRAM:
```
CaearCipher.
#include <stdio.h>
#include <stdlib.h>
 
// Function to perform Caesar Cipher encryption void caesarEncrypt(char *text, int key) {
   for (int i = 0; text[i] != '\0'; i++) { char c = text[i];
// Check if the character is an uppercase letter 
    if (c >= 'A' && c <= 'Z') {
    text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
    }
// Check if the character is a lowercase letter
    else if (c >= 'a' && c <= 'z') {
        text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
    }
// Ignore non-alphabetic characters
    }
}

// Function to perform Caesar Cipher decryption 
void caesarDecrypt(char *text, int key) {
// Decryption is the same as encryption with a negative key 
caesarEncrypt(text, -key);
}

int main() {
char message[100]; // Declare a character array to store the message int key;

printf("Enter the message to encrypt: ");
fgets(message, sizeof(message), stdin); // Read input from the user printf("Enter the Caesar Cipher key (an integer): ");
scanf("%d", &key); // Read the key from the user
// Encrypt the message using the Caesar Cipher caesarEncrypt(message, key); printf("Encrypted Message: %s", message);
// Decrypt the message back to the original
 
caesarDecrypt(message, key); printf("Decrypted Message: %s", message); return 0;
}
```

## OUTPUT:

Simulating Caesar Cipher

<img width="1621" height="839" alt="image" src="https://github.com/user-attachments/assets/d664d343-eaf8-4491-af16-38f91166bcf9" />


Input :indhumathi
Encrypted Message : lqgkxpdwkl
Decrypted Message : indhumathi

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_TEXT_LENGTH 100

// Function to prepare text for encryption
void prepare_text(char *text) {
    int length = strlen(text);
    for (int i = 0; i < length; i++) {
        text[i] = tolower(text[i]);
    }

    // Replace spaces and 'j' with 'i'
    for (int i = 0; i < length; i++) {
        if (text[i] == ' ') {
            for (int j = i; j < length; j++) {
                text[j] = text[j + 1];
            }
            length--;
            i--;
        }
        if (text[i] == 'j') {
            text[i] = 'i';
        }
    }

    // Add 'x' between identical consecutive letters
    for (int i = 0; i < length - 1; i++) {
        if (text[i] == text[i + 1]) {
            for (int j = length; j > i + 1; j--) {
                text[j] = text[j - 1];
            }
            text[i + 1] = 'x';
            length++;
        }
    }

    // If text length is odd, append 'z'
    if (length % 2 != 0) {
        text[length] = 'z';
        text[length + 1] = '\0';
    }
}

// Function to generate the Playfair Cipher key table
void generate_key_table(char *key, char key_table[5][5]) {
    int key_len = strlen(key);
    int used[26] = {0};
    int idx = 0;

    // Add key characters to the table
    for (int i = 0; i < key_len; i++) {
        if (!used[key[i] - 'a']) {
            used[key[i] - 'a'] = 1;
            key_table[idx / 5][idx % 5] = key[i];
            idx++;
        }
    }

    // Add remaining characters of the alphabet
    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (ch != 'j' && !used[ch - 'a']) {
            key_table[idx / 5][idx % 5] = ch;
            idx++;
        }
    }
}

// Function to find the position of a letter in the key table
void find_position(char key_table[5][5], char letter, int *row, int *col) {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (key_table[i][j] == letter) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Function to encrypt the plain text
void encrypt(char *plain_text, char key_table[5][5], char *cipher_text) {
    int len = strlen(plain_text);
    for (int i = 0; i < len; i += 2) {
        char a = plain_text[i];
        char b = plain_text[i + 1];

        int row1, col1, row2, col2;
        find_position(key_table, a, &row1, &col1);
        find_position(key_table, b, &row2, &col2);

        if (row1 == row2) {  // Same row
            cipher_text[i] = key_table[row1][(col1 + 1) % 5];
            cipher_text[i + 1] = key_table[row2][(col2 + 1) % 5];
        } else if (col1 == col2) {  // Same column
            cipher_text[i] = key_table[(row1 + 1) % 5][col1];
            cipher_text[i + 1] = key_table[(row2 + 1) % 5][col2];
        } else {  // Different row and column
            cipher_text[i] = key_table[row1][col2];
            cipher_text[i + 1] = key_table[row2][col1];
        }
    }
}

// Function to decrypt the cipher text
void decrypt(char *cipher_text, char key_table[5][5], char *plain_text) {
    int len = strlen(cipher_text);
    for (int i = 0; i < len; i += 2) {
        char a = cipher_text[i];
        char b = cipher_text[i + 1];

        int row1, col1, row2, col2;
        find_position(key_table, a, &row1, &col1);
        find_position(key_table, b, &row2, &col2);

        if (row1 == row2) {  // Same row
            plain_text[i] = key_table[row1][(col1 - 1 + 5) % 5];
            plain_text[i + 1] = key_table[row2][(col2 - 1 + 5) % 5];
        } else if (col1 == col2) {  // Same column
            plain_text[i] = key_table[(row1 - 1 + 5) % 5][col1];
            plain_text[i + 1] = key_table[(row2 - 1 + 5) % 5][col2];
        } else {  // Different row and column
            plain_text[i] = key_table[row1][col2];
            plain_text[i + 1] = key_table[row2][col1];
        }
    }
}

int main() {
    char key[MAX_TEXT_LENGTH], plain_text[MAX_TEXT_LENGTH], cipher_text[MAX_TEXT_LENGTH];
    char key_table[5][5];

    printf("Playfair Cipher\n");

    printf("Enter the key: ");
    fgets(key, MAX_TEXT_LENGTH, stdin);
    key[strcspn(key, "\n")] = 0;  // Remove the newline character

    printf("\nEnter the plaintext: ");
    fgets(plain_text, MAX_TEXT_LENGTH, stdin);
    plain_text[strcspn(plain_text, "\n")] = 0;  // Remove the newline character

    prepare_text(plain_text);  // Prepare the text

    generate_key_table(key, key_table);  // Generate the key table

    encrypt(plain_text, key_table, cipher_text);  // Encrypt the plain text
    printf("\nCipher Text: %s\n", cipher_text);

    decrypt(cipher_text, key_table, plain_text);  // Decrypt the cipher text
    printf("\nDecrypted Text: %s\n", plain_text);

    return 0;
}
```


## OUTPUT:
<img width="1605" height="844" alt="image" src="https://github.com/user-attachments/assets/e11e2a5f-950b-4c41-b2f8-dbbf8fa04f47" />

Key text: crypto
Plain text: indhumathi
Cipher text: hqbivlerik

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:

```
#include <stdio.h>
 #include <string.h>
 int main() {
 unsigned int a[3][3] = {{6, 24, 1}, {13, 16, 10}, {20, 17, 15}};
 unsigned int b[3][3] = {{8, 5, 10}, {21, 8, 21}, {21, 12, 8}};
 int i, j, t = 0;
 unsigned int c[3], d[3];
 char msg[4]; // buffer for exactly 3 characters plus null terminator
 printf("Enter plain text (3 letters): ");
scanf("%3s", msg); // ensure input is limited to 3 characters
 // Ensure the message has exactly 3 characters
 if (strlen(msg) != 3) {
 printf("Error: The plain text must be exactly 3 letters.\n");
 return 1;
 }
 // Convert plain text to numerical values (A=0, B=1, ..., Z=25)
 for (i = 0; i < 3; i++) {
 c[i] = msg[i]- 'A';
 printf("%d ", c[i]); // display numerical representation of characters
 }
 // Encrypt the message using matrix 'a'
 for (i = 0; i < 3; i++) {
 t = 0;
 for (j = 0; j < 3; j++) {
 t += a[i][j] * c[j];
 }
 d[i] = t % 26; // mod 26 for alphabet range
 }
 // Output encrypted cipher text
 printf("\nEncrypted Cipher Text: ");
 for (i = 0; i < 3; i++) {
 printf("%c", d[i] + 'A');
 }
 // Decrypt the message using matrix 'b'
 for (i = 0; i < 3; i++) {
 t = 0;
for (j = 0; j < 3; j++) {
 t += b[i][j] * d[j];
 }
 c[i] = t % 26; // mod 26 for alphabet range
 }
 // Output decrypted cipher text
 printf("\nDecrypted Cipher Text: ");
 for (i = 0; i < 3; i++) {
 printf("%c", c[i] + 'A');
 }
 getchar(); // Use getchar() to wait for input
 return 0;
 }
```

## OUTPUT:

Simulating Hill Cipher

<img width="1614" height="841" alt="image" src="https://github.com/user-attachments/assets/abdf1e77-1285-4964-9c91-a5788bc296b0" />


Input Message : CSK
Encrypted Message : MYC
Decrypted Message : CSK

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
```
#include <stdio.h>
 #include <ctype.h>
 #include <string.h>
 #include <stdlib.h>
 void encipher();
 void decipher();
int main() {
 int choice;
 while (1) {
 printf("\n1. Encrypt Text");
 printf("\t2. Decrypt Text");
 printf("\t3. Exit");
 printf("\n\nEnter Your Choice: ");
 scanf("%d", &choice);
 if (choice == 3)
 return 0; // Proper exit from main()
 else if (choice == 1)
 encipher();
 else if (choice == 2)
 decipher();
 else
 printf("Please Enter a Valid Option.\n");
 }
 }
 void encipher() {
 unsigned int i, j;
 char input[50], key[10];
 printf("\n\nEnter Plain Text: ");
 scanf("%s", input);
 printf("\nEnter Key Value: ");
 scanf("%s", key);
 printf("\nResultant Cipher Text: ");
for (i = 0, j = 0; i < strlen(input); i++, j++) {
 if (j >= strlen(key)) {
 j = 0; // Reset key index if it exceeds the key length
 }
 printf("%c", 65 + (((toupper(input[i])- 65) + (toupper(key[j])- 65)) % 26));
 // Encryption formula
 }
 printf("\n"); // New line after output
 }
 void decipher() {
 unsigned int i, j;
 char input[50], key[10];
 int value;
 printf("\n\nEnter Cipher Text: ");
 scanf("%s", input);
 printf("\nEnter the Key Value: ");
 scanf("%s", key);
 printf("\nDecrypted Plain Text: ");
 for (i = 0, j = 0; i < strlen(input); i++, j++) {
 if (j >= strlen(key)) {
 j = 0; // Reset key index if it exceeds the key length
 }// Decryption formula
 value = (toupper(input[i])- 65)- (toupper(key[j])- 65);
 if (value < 0) {
 value += 26; // Correct the negative wrap-around in alphabet
 }
printf("%c", 65 + (value % 26));
 }
 printf("\n"); // New line after output
 }
```

## OUTPUT:
OUTPUT :

Simulating Vigenere Cipher


Input Message : SecurityLaboratory
Encrypted Message : NMIYEMKCNIQVVROWXC Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

PROGRAM:
#include<stdio.h> #include<string.h> #include<stdlib.h> main()
{
int i,j,len,rails,count,code[100][1000]; char str[1000];
printf("Enter a Secret Message\n"); gets(str);
len=strlen(str);
printf("Enter number of rails\n"); scanf("%d",&rails); for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
code[i][j]=0;
}
}
count=0; j=0;
while(j<len)
{
if(count%2==0)
{
for(i=0;i<rails;i++)
{
//strcpy(code[i][j],str[j]);
code[i][j]=(int)str[j]; j++;
}

}
else
{
 
for(i=rails-2;i>0;i--)
{
code[i][j]=(int)str[j]; j++;
}
}

count++;
}

for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
if(code[i][j]!=0) printf("%c",code[i][j]);
}
}
printf("\n");
}
## OUTPUT:
OUTPUT:
Enter a Secret Message wearediscovered
Enter number of rails 2
waeicvrderdsoee
## RESULT:
The program is executed successfully
