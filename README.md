# Implementation of Diffie-Hellman Algorithm

## AIM
To implement key exchange between users using the Diffie-Hellman algorithm.

---

## ALGORITHM
1. Select a large prime number `P` and a primitive root `G` of `P` (publicly known).  
2. Alice selects a private key `a` and computes her public key `x = (G^a) mod P`.  
3. Bob selects a private key `b` and computes his public key `y = (G^b) mod P`.  
4. Alice and Bob exchange their public keys (`x` and `y`).  
5. Alice computes the secret key as `ka = (y^a) mod P`.  
6. Bob computes the secret key as `kb = (x^b) mod P`.  
7. Both `ka` and `kb` will be the same, forming a shared secret key for secure communication.

---

## PROGRAM

```c
#include <stdio.h>

long long int mod_exp(long long int base, long long int exp, long long int mod) {
    long long int result = 1;
    while (exp > 0) {
        if (exp % 2 == 1)
            result = (result * base) % mod;
        exp = exp >> 1; 
        base = (base * base) % mod;
    }
    return result;
}

int main() {
    long long int P, G, a, b, x, y, ka, kb;

    printf("\n********* Diffie-Hellman Key Exchange Algorithm **********\n\n");

    printf("Enter a prime number P: ");
    scanf("%lld", &P); 
    printf("The value of P: %lld\n", P);

    printf("Enter a primitive root G for P: ");
    scanf("%lld", &G); 
    printf("The value of G: %lld\n\n", G);

    printf("Enter the private key for Alice (a): ");
    scanf("%lld", &a);
    x = mod_exp(G, a, P);
    printf("The public key for Alice (x = G^a mod P): %lld\n", x);

    printf("Enter the private key for Bob (b): ");
    scanf("%lld", &b);
    y = mod_exp(G, b, P); 
    printf("The public key for Bob (y = G^b mod P): %lld\n\n", y);

    ka = mod_exp(y, a, P);
    kb = mod_exp(x, b, P); 

    printf("Shared secret key for Alice (ka = y^a mod P): %lld\n", ka);
    printf("Shared secret key for Bob (kb = x^b mod P): %lld\n", kb);

    if (ka == kb) {
        printf("\nDiffie-Hellman Key Exchange successful. Both parties share the same key.\n");
    } else {
        printf("\nError: The keys for Alice and Bob do not match.\n");
    }

    return 0;
}
```
---
## OUTPUT
<img width="3837" height="1838" alt="Screenshot 2025-10-24 220931" src="https://github.com/user-attachments/assets/427a62a1-42ec-4ca3-89d8-2decba08b04c" />





