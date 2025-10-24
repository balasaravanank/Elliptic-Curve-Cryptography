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

typedef struct { long long x, y, inf; } Point;

Point add(Point p, Point q, long long P, long long A) {
    if(p.inf) return q; if(q.inf) return p;
    long long m = (p.x==q.x) ? ((3*p.x*p.x + A)/(2*p.y)) : ((q.y-p.y)/(q.x-p.x));
    long long xr = (m*m - p.x - q.x) % P;
    long long yr = (m*(p.x - xr) - p.y) % P;
    return (Point){xr, yr, 0};
}

Point mul(Point p, long long k, long long P, long long A){
    Point r = {0,0,1};
    while(k){ if(k&1) r = add(r, p, P, A); p = add(p, p, P, A); k >>= 1; }
    return r;
}

int main() {
    long long P=23, A=1;           // small demo curve
    Point G={9,7,0};                // generator point
    long long dA=5, dB=7;           // private keys
    Point QA = mul(G,dA,P,A);       // Alice public key
    Point QB = mul(G,dB,P,A);       // Bob public key

    Point S1 = mul(QB,dA,P,A);      // Alice computes shared secret
    Point S2 = mul(QA,dB,P,A);      // Bob computes shared secret

    printf("Alice shared secret: (%lld,%lld)\n",S1.x,S1.y);
    printf("Bob shared secret: (%lld,%lld)\n",S2.x,S2.y);
}

```
---
## OUTPUT
<img width="3839" height="1702" alt="Screenshot 2025-10-24 222240" src="https://github.com/user-attachments/assets/88f01624-c7ab-493c-a153-c479e9922c60" />


## RESULT
The Diffie-Hellman key exchange algorithm has been successfully simulated, with correct execution of the program and verification of the results.



