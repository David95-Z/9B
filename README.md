#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>
#include <ctype.h>

long int p, q, n, t, e, d, i, j; // Key variables
long int temp[100], m[100], ct[100], pt; // Message and cipher storage

char msg[100];  // Message to encrypt/decrypt

// Function prototypes
int isPrime(long int num);
long int calculateD(long int e);
void encrypt();
void decrypt();

int main() {
    printf("RSA Encryption and Decryption\n");

    // Step 1: Enter prime numbers p and q
    printf("\nEnter the first prime number (p): ");
    scanf("%ld", &p);
    if (!isPrime(p)) {
        printf("Invalid input. Please enter a prime number.\n");
        return 1;
    }

    printf("Enter the second prime number (q): ");
    scanf("%ld", &q);
    if (!isPrime(q) || p == q) {
        printf("Invalid input. Please enter two different prime numbers.\n");
        return 1;
    }

    // Step 2: Calculate n and t (Euler's Totient Function)
    n = p * q;
    t = (p - 1) * (q - 1);

    // Step 3: Choose an encryption key e such that 1 < e < t and gcd(e, t) = 1
    printf("Enter an encryption key (e): ");
    scanf("%ld", &e);
    if (e <= 1 || e >= t || (t % e == 0)) {
        printf("Invalid encryption key.\n");
        return 1;
    }

    // Step 4: Calculate the decryption key d
    d = calculateD(e);

    // Step 5: Get the message to encrypt (convert it to uppercase)
    printf("Enter the message to encrypt (letters only): ");
    scanf("%s", msg);

    // Convert the message to uppercase
    for (i = 0; msg[i] != '\0'; i++) {
        msg[i] = toupper(msg[i]);
    }

    // Step 6: Perform encryption and decryption
    encrypt();
    decrypt();

    return 0;
}

// Function to check if a number is prime
int isPrime(long int num) {
    for (i = 2; i <= sqrt(num); i++) {
        if (num % i == 0)
            return 0;  // Not prime
    }
    return 1;  // Prime
}

// Function to calculate d (the decryption key)
long int calculateD(long int e) {
    long int d = 1;
    while ((d * e) % t != 1) {
        d++;
    }
    return d;
}

// Function to encrypt the message
void encrypt() {
    printf("\nEncrypted Message: ");
    for (i = 0; msg[i] != '\0'; i++) {
        pt = msg[i] - 'A' + 1;  // Convert char to number (A=1, B=2, ..., Z=26)
        ct[i] = 1;
        // Encrypt using the formula: ct = (pt^e) % n
        for (j = 0; j < e; j++) {
            ct[i] = (ct[i] * pt) % n;
        }
        printf("%c", ct[i] + 'A' - 1);  // Convert number back to character
    }
    printf("\n");
}

// Function to decrypt the message
void decrypt() {
    printf("\nDecrypted Message: ");
    for (i = 0; msg[i] != '\0'; i++) {
        pt = ct[i];
        long int decryptedValue = 1;
        // Decrypt using the formula: pt = (ct^d) % n
        for (j = 0; j < d; j++) {
            decryptedValue = (decryptedValue * pt) % n;
        }
        printf("%c", decryptedValue + 'A' - 1);  // Convert number back to character
    }
    printf("\n");
}
