# Implementation of Elliptic Curve Cryptography (ECC)

## AIM
To implement key exchange between users using Elliptic Curve Cryptography (ECC) and demonstrate the shared secret computation.

---
## THEORY
Elliptic Curve Cryptography (ECC) is a form of public key cryptography based on the algebraic structure of elliptic curves over finite fields.  

- Private key: an integer `d`  
- Public key: a point `Q = d * G` on the curve  
- Shared secret: `S = d * Q_other`  

The shared secret allows secure communication without transmitting the private keys.

---

## ALGORITHM
1. Choose a prime modulus `P` and curve parameter `A` for the elliptic curve `y^2 = x^3 + Ax + B (mod P)`.  
2. Select a generator point `G(x, y)` on the curve.  
3. Alice selects a private key `dA` and computes her public key `QA = dA * G`.  
4. Bob selects a private key `dB` and computes his public key `QB = dB * G`.  
5. Alice computes shared secret `S1 = dA * QB`.  
6. Bob computes shared secret `S2 = dB * QA`.  
7. Both `S1` and `S2` are equal, forming the shared secret key.

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



