# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
#include <stdio.h>

typedef struct
{
    long long x, y;
} Point;

long long mod(long long a, long long p)
{
    return ((a % p) + p) % p;
}

long long mod_inverse(long long a, long long p)
{
    a = mod(a,p);

    long long t = 0, new_t = 1;
    long long r = p, new_r = a;

    while(new_r != 0)
    {
        long long q = r / new_r;

        long long temp = new_t;
        new_t = t - q * new_t;
        t = temp;

        temp = new_r;
        new_r = r - q * new_r;
        r = temp;
    }

    if(r > 1)
        return -1;

    if(t < 0)
        t += p;

    return t;
}

Point add_points(Point P, Point Q, long long a, long long p)
{
    Point R;

    if(P.x==-1 && P.y==-1)
        return Q;

    if(Q.x==-1 && Q.y==-1)
        return P;

    if(P.x==Q.x && mod(P.y+Q.y,p)==0)
    {
        Point inf={-1,-1};
        return inf;
    }

    long long m;

    if(P.x==Q.x && P.y==Q.y)
    {
        m = mod(
            ((3*P.x*P.x+a)*
            mod_inverse(2*P.y,p)),
            p);
    }
    else
    {
        m = mod(
            ((Q.y-P.y)*
            mod_inverse(Q.x-P.x,p)),
            p);
    }

    R.x = mod((m*m-P.x-Q.x),p);
    R.y = mod((m*(P.x-R.x)-P.y),p);

    return R;
}

Point scalar_multiplication(Point P,
                            long long k,
                            long long a,
                            long long p)
{
    Point result={-1,-1};

    while(k>0)
    {
        if(k%2==1)
        {
            result=
            add_points(result,P,a,p);
        }

        P=add_points(P,P,a,p);

        k/=2;
    }

    return result;
}

int main()
{
    long long p,a,b;

    Point G;

    long long adarsh_private_key;
    long long bob_private_key;

    Point adarsh_public_key;
    Point bob_public_key;

    Point adarsh_shared_secret;
    Point bob_shared_secret;

    printf("ELLIPTIC CURVE CRYPTOGRAPHY\n");

    printf("\nEnter prime number p: ");
    scanf("%lld",&p);

    printf("Enter a and b: ");
    scanf("%lld %lld",&a,&b);

    printf("Enter Base Point G(x y): ");
    scanf("%lld %lld",&G.x,&G.y);

    printf("Enter Adarsh private key: ");
    scanf("%lld",&adarsh_private_key);

    printf("Enter Bob private key: ");
    scanf("%lld",&bob_private_key);

    adarsh_public_key =
    scalar_multiplication(
        G,
        adarsh_private_key,
        a,
        p);

    bob_public_key =
    scalar_multiplication(
        G,
        bob_private_key,
        a,
        p);

    printf("\nAdarsh Public Key=(%lld,%lld)",
           adarsh_public_key.x,
           adarsh_public_key.y);

    printf("\nBob Public Key=(%lld,%lld)",
           bob_public_key.x,
           bob_public_key.y);

    adarsh_shared_secret =
    scalar_multiplication(
        bob_public_key,
        adarsh_private_key,
        a,
        p);

    bob_shared_secret =
    scalar_multiplication(
        adarsh_public_key,
        bob_private_key,
        a,
        p);

    printf("\n\nAdarsh Shared Secret=(%lld,%lld)",
           adarsh_shared_secret.x,
           adarsh_shared_secret.y);

    printf("\nBob Shared Secret=(%lld,%lld)",
           bob_shared_secret.x,
           bob_shared_secret.y);

    if(adarsh_shared_secret.x ==
       bob_shared_secret.x &&
       adarsh_shared_secret.y ==
       bob_shared_secret.y)
    {
        printf("\n\nKey Exchange Successful");
    }
    else
    {
        printf("\n\nKey Exchange Failed");
    }

    return 0;
}
```

## Output:
<img width="1384" height="774" alt="image" src="https://github.com/user-attachments/assets/ff2023c4-7fdb-4d14-95ab-26765829fa82" />


## Result:
The program is executed successfully

