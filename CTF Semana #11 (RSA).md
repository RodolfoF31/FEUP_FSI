# CTF Semana #10 - (RSA)

For this CTF, we were tasked with decrypting a message encrypted using RSA encryption. The public key (e, n) was provided, along with crucial information indicating that the primes were approximately 2^512 and 2^513. Armed with this knowledge, discovering the original prime numbers used in calculating the private and public keys became relatively straightforward. To achieve this, all that was required was an algorithm to probabilistically verify if a number is prime and a kind of "bruteforce" approach that systematically tested numbers close to 2^512 for primality. Subsequently, we also checked for a prime number close to 2^513 and verified if the multiplication of both primes yielded the provided N.
Code used:

```python
def is_probably_prime(n, k=5):
    """
    Testa se um número é provavelmente primo usando o algoritmo de Miller-Rabin.
    
    Parâmetros:
    - n: O número a ser testado.
    - k: O número de iterações. Quanto maior, mais preciso o teste (padrão é 5).
    
    Retorna:
    - True se n for provavelmente primo, False se for composto.
    """
    if n <= 1:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False

    # Escrever n-1 como 2^r * d
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    # Realizar o teste de Miller-Rabin k vezes
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)

        if x == 1 or x == n - 1:
            continue

        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False  # n é definitivamente composto

    return True  # n é provavelmente primo

def discover_primes(n):
    for p in range(pow(2, 512), pow(2, 512)+5000):
        if (is_probably_prime(p)):
            for q in range(pow(2, 513), pow(2, 513)+5000):
                if (is_probably_prime(q)):
                    if (p*q == n):
                        print("Consegui")
                        return (p,q)

```

With the output of this last function we discovered the two primes and with that knowledge all that was left to do was to discover d like so:

```python
def calcular_d(e, phi_n):
    d = mod_inverse(e, phi_n)
    return d
```

Having obtained the private key 'd', the final step is to utilize the decryption function provided to us. Applying this function to the ciphertext (flag) will unveil the original message:

```python
M = dec(cypher, d, n)

print(M.decode("latin-1"))
```

Output aquired:
<img src="/images/p_q_rsa.png" alt="Output from the execution of the program">

All the code used:
```python
from binascii import hexlify, unhexlify
from sympy import mod_inverse
import random


def is_probably_prime(n, k=5):
    """
    Testa se um número é provavelmente primo usando o algoritmo de Miller-Rabin.
    
    Parâmetros:
    - n: O número a ser testado.
    - k: O número de iterações. Quanto maior, mais preciso o teste (padrão é 5).
    
    Retorna:
    - True se n for provavelmente primo, False se for composto.
    """
    if n <= 1:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False

    # Escrever n-1 como 2^r * d
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    # Realizar o teste de Miller-Rabin k vezes
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)

        if x == 1 or x == n - 1:
            continue

        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False  # n é definitivamente composto

    return True  # n é provavelmente primo

def dec(y, d, n):
    int_y = int.from_bytes(unhexlify(y), "little")
    x = pow(int_y,d,n)
    return x.to_bytes(256, 'little')

def discover_primes(n):
    for p in range(pow(2, 512), pow(2, 512)+5000):
        if (is_probably_prime(p)):
            for q in range(pow(2, 513), pow(2, 513)+5000):
                if (is_probably_prime(q)):
                    if (p*q == n):
                        print("Consegui")
                        return (p,q)

def calcular_d(e, phi_n):
    d = mod_inverse(e, phi_n)
    return d


# Cypher text
cypher = unhexlify("6130653131393432393839623635356263353164656637633330353032323937393031356232666339303364356636323235326435646562613363386436313135616131623934333439653864356161323330326563353039373334666366666639373361663338616230316236396236636533663038383037636637663162633062366237393861343034303930636230353333313039363837633539613437353435323932343237363238326134346230383562363463343031653038346464653066613461353131373932313132626434323239633433343438356563636564326664353266333561633037623362323663656231626161376338343730303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030")

# Prime numbers to discover
p = 0; # Around 2^512
q = 0; # Around 2^513

# Public Data
e = 65537
n = 359538626972463181545861038157804946723595395788461314546860162315465351611001926265416954644815072042240227759742786715317579537628833244985694861279002376076148965114221950891522196203308822438723897396235881555224606126074332672093175387623354263464285679980671440667191096301871260912544465300286011668027

(p, q) = discover_primes(n)


phi_n = (p - 1) * (q - 1)

d = calcular_d(e, phi_n)

print(f"P: {p}")
print(f"Q: {q}")

M = dec(cypher, d, n)

print(M.decode("latin-1"))


```

## How can I use the information I have to infer the values used in the RSA that encrypted the flag? 

To deduce the values used in the RSA encryption of the flag, we can exploit the knowledge that these values are close to specific numbers. For instance, if 'p' is close to \(2^{512}\) and 'q' is close to \(2^{513}\), we can iterate through numbers around these ranges and use the Miller-Rabin algorithm to check if they are prime.

## How can I verify if my inference is correct?

To confirm the correctness of our inference for the prime numbers 'p' and 'q', we need to check if their multiplication equals the 'n' provided in the public key. This step ensures that our deduced values align with the information given in the public key.

## Finally, how can I extract my key from the cryptogram I received?

To extract the private key, we must calculate 'd' by performing the reverse modulus operation between 'e' and 'phi', where 'phi' is calculated as \((p-1) \times (q-1)\). Once the private key is determined, we can use it to decrypt the received cryptogram and reveal the original message.
