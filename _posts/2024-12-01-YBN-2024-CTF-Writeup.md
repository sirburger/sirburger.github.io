---
layout: post
title: "YBN 2024 CTF Writeup: Crushing Hero Challenges with Code and Creativity"
subtitle: "A Teen's Journey Through the YBN 2024 Capture The Flag Competition"
author: "Junhui"
header-style: text
tags:
  - Python
  - Cryptography
  - CTF
  - RSA
  - Fermat's Little Theorem
  - Number Theory
  - Cybersecurity
  - Problem Solving
---

> 🚀 **YBN 2024 CTF**: Diving into cryptography, security, and a whole lot of fun!  
> Join me as I walk you through my journey tackling the Hero challenges.

---

## 🌟 The Adventure Begins: YBN 2024 CTF

Recently, I joined my first CTF with my team, Red Watermelons Zanily Jump Hilarious Ninjas Warmly. We managed to place in the Top 15, which I’d say is pretty solid for our first time competing together. I thought it’d be fun to share my write-up for three crypto challenges we tackled during the event.

---

## 🦸 Hero 1: Cracking the Crypto Code

### 🔍 Challenge Overview

**Hero 1** was all about cryptography with a twist. The challenge provided a `chal.py` script that encrypted a flag using a mix of primes and modular arithmetic. My mission? **Decrypt the flag** hidden within the ciphertext.

### 📜 The Code Breakdown

Here’s the `chal.py` script we were given:

```python
from Crypto.Util.number import getPrime, bytes_to_long

flag = b"YBN24{????????????????????????}"

p = getPrime(256)
q = getPrime(256)
y = getPrime(256)
e = getPrime(64)
c = getPrime(32)

try:
    a = int(eval(input("a: ")))
    b = int(eval(input("b: ")))

    assert a > 0
except:
    quit()

g = q * e
n = ((a) ** (b + c)) * p * q * y

enc = pow(bytes_to_long(flag), e, n)

ct = enc * y

print("g = {}".format(g))
print("n = {}".format(n))
print("ct = {}".format(ct))
```

### 🧠 Solving the Puzzle

**Step 1: Analyze the Given Values**

- `g = q * e`
- `n = ((a) ** (b + c)) * p * q * y`
- `ct = enc * y` where `enc = pow(flag, e, n)`

**Step 2: Leverage the Greatest Common Divisor (GCD)**

Using the `gcd` function, I extracted the primes:

```python
y = gcd(ct, n)
q = gcd(g, n)
p = n // (q * y)
```

**Step 3: Rebuild the Private Key**

Calculate the totient `φ(n)` and the private key `d`:

```python
r = (p - 1) * (q - 1) * (y - 1)
e = g // q
d = pow(e, -1, r)
```

**Step 4: Decrypt the Flag**

Finally, decrypt the ciphertext to reveal the flag:

```python
enc = ct // y
pt = pow(enc, d, n)
print(long_to_bytes(pt).decode())
```

### 🏆 The Victory

Running the `solve.py` script gave me the flag:

**`YBN24{RS4_mu1t!prim3_f4ct0r1ng}`**

---

## 🦸‍♂️ Hero 2: Tackling Powers and Primes

### 🔍 Challenge Overview

**Hero 2** amped up the complexity by introducing powers of 2 and larger primes. The `chal.py` script looked like this:

```python
from Crypto.Util.number import getPrime, bytes_to_long

flag = b"YBN24{????????????????????????????????????}"

p = getPrime(256)
q = getPrime(256)
y = getPrime(256)
e = getPrime(64)
c = getPrime(12)

try:
    a = int(input("a: "))
    b = int(input("b: "))
    
    assert a > 1
    assert b > 0 and b < c

except:
    quit()

g = q * e
n = ((a) ** (b + c)) * p * q * y

enc = pow(bytes_to_long(flag), e, n)

ct = enc * y

print("g = {}".format(g))
print("n = {}".format(n))
print("ct = {}".format(ct))
```

### 🧠 Cracking the Code

**Step 1: Extracting Primes with GCD**

Just like Hero 1, I used `gcd` to find `y` and `q`:

```python
y = gcd(ct, n)
q = gcd(g, n)
p = n // (q * y)
```

**Step 2: Handling Powers of 2**

The modulus `n` included a power of 2:

```python
k = 0
while p % 2 == 0:
    p //= 2
    k += 1
```

**Step 3: Rebuilding the Private Key**

Calculate the totient `φ(n)` considering the power of 2:

```python
phi = (y - 1) * (p - 1) * (q - 1) * (2**k - 2**(k-1))
d = inverse(e, phi)
```

**Step 4: Decrypting the Flag**

Decrypt the ciphertext:

```python
enc = ct // y
pt = pow(enc, d, n)
print(long_to_bytes(pt).decode())
```

### 🏆 The Victory

Running the `solve.py` script revealed the flag:

**`YBN24{RS4_mu1t!prim3_f4ct0r1ng_bu7_m0r3_:D}`**

---

## 🦸‍♀️ Hero 3: Mixing Randomness with Primes

Got it! Let’s update the **Hero 3** section to include the explanation about Fermat’s Little Theorem. Here’s the revised write-up:

---

## Hero 3  

This one felt more chaotic, with randomness and constraints thrown in, but at its core, the challenge was cracked using **Fermat’s Little Theorem**. Here’s the challenge:

```python
from Crypto.Util.number import getPrime, bytes_to_long
import random

flag = b"YBN24{????????????????????????????????????????}"

try:
    p1 = getPrime(64) 
    p2 = getPrime(512)
    e = getPrime(32)

    c = str(random.randint(0,999999999))
    a = input("a: ")
    b = input("b: ")

    check = ' 01qwertyuiopasdfghjklzxcvbnm%^&*()+-/><'

    for char in a:
        assert char not in check and char.isascii()
    
    for char in b:
        assert char not in check and char.isascii()

    a = eval(a + c)
    b = eval(b)
    d = pow(a, b, getPrime(128))
    z = pow(p1, p2-d, p2)

    ct = pow(bytes_to_long(flag)*z, e, p2)

    print("e =", e)
    print("p2 =", p2)
    print("ct =", ct)

except:
    quit()
```

### Cracking It with Fermat’s Little Theorem  

The key to solving this challenge lies in understanding Fermat’s Little Theorem, which states:  

> If `p` is a prime and `a` is any integer not divisible by `p`, then:  
> ```  
> a^(p-1) ≡ 1 (mod p)  
> ```  

Here, `p2` is prime, so its totient, `φ(p2)`, simplifies to `p2 - 1`. Using Fermat’s theorem, we can compute the private key and decrypt the ciphertext directly. The randomness in `a`, `b`, and `c` doesn’t actually matter for the decryption process since the modular arithmetic with `p2` dominates everything.

### Steps to Solve  

1. Compute the totient of `p2`:
   ```python
   phi = p2 - 1
   ```

2. Derive the private key using the modular inverse:
   ```python
   d = inverse(e, phi)
   ```

3. Decrypt the ciphertext:
   ```python
   pt = pow(ct, d, p2)
   print(long_to_bytes(pt).decode())
   ```

### Flag  

Running the script gave me:  
**`YBN24{r0und1ng_up_w1th_f3rm4t's_l1ttl3_th30r3m}`**

---
### 🏆 Finally I got it

By recognizing the prime structure and applying Fermat’s Little Theorem, the challenge became manageable. Even though `a` and `b` had constraints, they were distractions in the grand scheme of things. The problem ultimately boiled down to standard RSA math.  

---

## 🎉 Wrapping It All Up

Cracking the Hero challenges in the **YBN 2024 CTF** was an awesome ride! Each challenge taught me something new about cryptography, modular arithmetic, and the importance of understanding the underlying math in security. From Hero 1’s straightforward prime extraction to Hero 3’s intricate mix of randomness and primes, it was a blast putting my skills to the test.

---

## ❤️ Thanks for Joining the Journey!

A huge shoutout to everyone who participated in the YBN 2024 CTF and supported me through these challenges. Whether you're a seasoned CTF competitor or just starting out, remember that persistence and curiosity are your best tools. Keep learning, keep hacking, and most importantly, have fun!

Stay tuned for more write-ups and adventures in the world of cybersecurity! 🎉🔐

---
