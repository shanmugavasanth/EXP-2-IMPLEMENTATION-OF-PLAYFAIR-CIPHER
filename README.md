## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
   
## EXAMPLE:

![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.




Program:

```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define SIZE 100


void toLowerCase(char str[]) {
    for (int i = 0; str[i]; i++)
        if (str[i] >= 'A' && str[i] <= 'Z')
            str[i] += 32;
}


int removeSpaces(char str[]) {
    int count = 0;
    for (int i = 0; str[i]; i++)
        if (str[i] != ' ')
            str[count++] = str[i];
    str[count] = '\0';
    return count;
}


int preparePlainText(char str[]) {
    int len = strlen(str);
    for (int i = 0; i < len; i++)
        if (str[i] == 'j') str[i] = 'i';
    if (len % 2 != 0) {
        str[len++] = 'z'; 
        str[len] = '\0';
    }
    return len;
}


void createKeyMatrix(char key[], char matrix[5][5]) {
    int i, j, k;
    int used[26] = {0}; 
    used['j' - 'a'] = 1; 
    char temp[25];
    int index = 0;

  
    for (i = 0; key[i]; i++) {
        if (!used[key[i] - 'a']) {
            temp[index++] = key[i];
            used[key[i] - 'a'] = 1;
        }
    }

 
    for (i = 0; i < 26; i++)
        if (!used[i])
            temp[index++] = i + 'a';

  
    index = 0;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            matrix[i][j] = temp[index++];
}


void searchMatrix(char matrix[5][5], char a, char b, int pos[4]) {
    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++) {
            if (matrix[i][j] == a) { pos[0] = i; pos[1] = j; }
            if (matrix[i][j] == b) { pos[2] = i; pos[3] = j; }
        }
}


void encryptPlayfair(char str[], char matrix[5][5], int len) {
    int pos[4];
    for (int i = 0; i < len; i += 2) {
        searchMatrix(matrix, str[i], str[i + 1], pos);

        if (pos[0] == pos[2]) { 
            str[i] = matrix[pos[0]][(pos[1] + 1) % 5];
            str[i + 1] = matrix[pos[0]][(pos[3] + 1) % 5];
        } else if (pos[1] == pos[3]) { 
            str[i] = matrix[(pos[0] + 1) % 5][pos[1]];
            str[i + 1] = matrix[(pos[2] + 1) % 5][pos[1]];
        } else { 
            str[i] = matrix[pos[0]][pos[3]];
            str[i + 1] = matrix[pos[2]][pos[1]];
        }
    }
}

int main() {
    char plaintext[SIZE], key[SIZE];
    char matrix[5][5];

  
    printf("Enter the plain text: ");
    fgets(plaintext, SIZE, stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0'; 

   
    printf("Enter the keyword: ");
    fgets(key, SIZE, stdin);
    key[strcspn(key, "\n")] = '\0';

    toLowerCase(plaintext);
    toLowerCase(key);
    removeSpaces(plaintext);
    removeSpaces(key);


    createKeyMatrix(key, matrix);

  
    int len = preparePlainText(plaintext);

  
    encryptPlayfair(plaintext, matrix, len);

    printf("Cipher Text: %s\n", plaintext);

    return 0;
}
```



## Output:

<img width="358" height="158" alt="image" src="https://github.com/user-attachments/assets/a69cc36c-74ff-4cd9-bdc6-e76113ae89ce" />

## RESULT:

The program was executed successfully.
