EX.NO:3.Hill Cipher

AIM:
To develop a simple C program to implement Hill Cipher.

DESIGN STEPS:
Step 1:
Design of Hill Cipher algorithnm

Step 2:
Implementation using C or pyhton code

Step 3:
Testing algorithm with different key values. 

PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define SIZE 2 // Using a 2x2 key matrix
#define MOD 26
int determinant(int key[SIZE][SIZE]) {
    return (key[0][0] * key[1][1] - key[0][1] * key[1][0]) % MOD;
}
int modInverse(int num, int mod) {
    num = (num % mod + mod) % mod;
    for (int i = 1; i < mod; i++) {
        if ((num * i) % mod == 1)
            return i;
    }
    return -1; // No modular inverse exists
}
void inverseMatrix(int key[SIZE][SIZE], int inverseKey[SIZE][SIZE]) {
    int det = determinant(key);
    int detInverse = modInverse(det, MOD);
    if (detInverse == -1) {
        printf("Inverse does not exist. Key is not valid.\n");
        exit(1);
    }
    inverseKey[0][0] = (key[1][1] * detInverse) % MOD;
    inverseKey[0][1] = (-key[0][1] * detInverse) % MOD;
    inverseKey[1][0] = (-key[1][0] * detInverse) % MOD;
    inverseKey[1][1] = (key[0][0] * detInverse) % MOD;
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (inverseKey[i][j] < 0)
                inverseKey[i][j] += MOD;
        }
    }
}
void encrypt(char plaintext[], int key[SIZE][SIZE], char ciphertext[]) {
    int length = strlen(plaintext);
    if (length % SIZE != 0) {
        plaintext[length] = 'X'; // Padding with 'X'
        plaintext[length + 1] = '\0';
        length++;
    }
    for (int i = 0; i < length; i += SIZE) {
        int vector[SIZE], result[SIZE] = {0};
        for (int j = 0; j < SIZE; j++)
            vector[j] = plaintext[i + j] - 'A';
        for (int j = 0; j < SIZE; j++)
            for (int k = 0; k < SIZE; k++)
                result[j] += key[j][k] * vector[k];
        for (int j = 0; j < SIZE; j++)
            ciphertext[i + j] = (result[j] % MOD) + 'A';
    }
    ciphertext[length] = '\0'; // Null terminate the string
}
void decrypt(char ciphertext[], int key[SIZE][SIZE], char plaintext[]) {
    int length = strlen(ciphertext);
    int inverseKey[SIZE][SIZE];
    inverseMatrix(key, inverseKey); // Get inverse matrix
    for (int i = 0; i < length; i += SIZE) {
        int vector[SIZE], result[SIZE] = {0};
        for (int j = 0; j < SIZE; j++)
            vector[j] = ciphertext[i + j] - 'A';
        for (int j = 0; j < SIZE; j++)
            for (int k = 0; k < SIZE; k++)
                result[j] += inverseKey[j][k] * vector[k];
        for (int j = 0; j < SIZE; j++)
            plaintext[i + j] = (result[j] % MOD) + 'A';
    }
    plaintext[length] = '\0'; // Null terminate the string
}
int main() {
    int key[SIZE][SIZE] = { {3, 3}, {2, 5} }; // Example key matrix
    char plaintext[100], ciphertext[100];
    printf("Enter uppercase plaintext (A-Z only, no spaces): ");
    scanf("%s", plaintext);
    encrypt(plaintext, key, ciphertext);
    printf("Encrypted Text: %s\n", ciphertext);
    return 0;
}

```

OUTPUT:
![image](https://github.com/user-attachments/assets/28478e15-1223-4176-b948-653b076e7be8)


RESULT:
The program is executed successfully
