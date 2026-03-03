---
date: 2026-02-06
tags:
  - cryptography
---
# RSA
---
RSA is a public-key encryption algorithm.

RSA Variables:
- `p` and `q` are large prime numbers
- `n` is the product of p and q
- The public key is `n` and `e`
- The private key is `n` and `d`
- `m` is used to represent the plaintext
- `c` represents the ciphertext

`ϕ(n) = n - p - q + 1`
Select e such that e is relatively prime to ϕ(n)
Select d where `e x d = 1 mod ϕ(n)`

# Diffie-Hellman Key Exchange
---
![../../PNG PDF JPEG/attachment-02102026-1.png](../../PNG%20PDF%20JPEG/attachment-02102026-1.png)
1. Alice and Bob agree on the **public variables**: a large prime number _p_ and a generator _g_, where 0 < _g_ < _p_. These values will be disclosed publicly over the communication channel. Although insecurely small, we will choose _p_ = 29 and _g_ = 3 to simplify our calculations.
2. Each party chooses a private integer. As a numerical example, Alice chooses _a_ = 13, and Bob chooses _b_ = 15. Each of these values represents a **private key** and must not be disclosed.
3. It is time for each party to calculate their **public key** using their private key from step 2 and the agreed-upon public variables from step 1. Alice calculates _A_ = _g^a_ mod _p_ = 313 mod 29 = 19 and Bob calculates _B_ = _g^b_ mod _p_ = 315 mod 29 = 26. These are the public keys.
4. Alice and Bob send the keys to each other. Bob receives _A_ = _g^a_ mod _p_ = 19, i.e., Alice’s public key. And Alice receives _B_ = _g^b_ mod _p_ = 26, i.e., Bob’s public key. This step is called the **key exchange**.
5. Alice and Bob can finally calculate the **shared secret** using the received public key and their own private key. Alice calculates _B__a_ mod _p_ = 2613 mod 29 = 10 and Bob calculates _A^b_ mod _p_ = 1915 mod 29 = 10. Both calculations yield the same result, _g^ab_ mod _p_ = 10, the shared secret key.

# PGP and GPG
---
Pretty Good Privacy (PGP) implements encryption for encrypting files, performing digital signing, and more.
GPG is commonly used in email to protect the confidentiality of the email messages. It can also be used to sign an email address and confirm its integrity.

*If keys are passphrase protected, use John the Ripper gpg2john to attempt to crack it.*
