## L1 Introduction

##### Goals For the Course

1. The Security Mindset
2. Applied Cryptography
3. Network Security
4. Host/Application Security
5. Security in Context

#### The Security Mindset

Security studies how systems behave in the presence of an **adversary**, an intelligence that actively tries to cause the system to misbehave.

The *security mindset* encourages thinking about how attackers could cause system to fail, in order to head off problems before they are exploited.

##### Think Like An Attacker

* Look for weakest links
* Identify assumptions security depends on
* Think outside the box (e.g. software could be attacked via hardware vulnerabilities)

##### Think as a Defender

* Security polices: what properties are we trying to enforce (confidentiality, integrity, etc.)
* Threat modeling: who are we defending against
* Assessing risk and selecting countermeasures

#### Course Mechanics

Complete a brief quiz before the next lecture. Contribute on Piazza.

* Participation 5%: complete quiz before the next lecture + contribute on Piazza
* Projects 45%: 5 projects in total
* Lab 5%: submit via Autograder.io (==Strick lateness policy==)
* Midterm 15% 2/23 19:00-20:30 in person
* Final 30% 4/21 19:00-21:00 in person, cover the entire course

##### Security Ethics

Don't be evil, practice attacks only on systems we give you to test.



## L2 Message Integrity

Message integrity wants to ensure that attackers cannot modify messages without being detected.

#### Message Verifier

1. Alice computes verifier $v = f(m)$
2. Alice sends $(m, v)$
3. Bob verifies that $v' = f(m')$

We want f to be easily computed by Alice and Bob, not Mallory.

**Random function (RF)** is a giant look up table, given an random (256 bits) string for any possible input m.

##### Pseudorandom Function

See [[Private-Key (Symmetric) Cryptography 475#3.5.1 Pseudorandom Functions and Permutations|pseudorandom function]] from EECS 475.

1. Let $f$ be a secure PRF known to everyone
2. Alice and Bob (not Mallory) shared a private key $k$
3. Alice sends $(m, v = f_k(m))$
4. Bob verifies $v' = f_k(m')$

#### Cryptographic Hashes

##### Preimage Resistance

Given output $h$, hard to find input $m$ s.t. $H(m) = h$.

##### Second-Preimage Resistance

Given input $m_1$, hard to find input $m_2$ s.t. $H(m_1) = H(m_2)$.

##### Collision Resistance

Hard to find any $m_1, m_2$ s.t. $H(m_1) = H(m_2)$.

Collisions have been found in MD5 and SHA-1. Now SHA-256 is widely used.



## L3 Randomness and Pseudorandomness

#### SHA-256

Built from a compression function $h$, the input is (256 bits, 512 bits), and the output is 256 bits. It uses the **Merkle-Damgård construction**. It pads the bit length of m in 8 bytes.

![[Pasted image 20230118230921.png]]

##### Length Extension Attack

The attacker can calculate $H(x||\text{pading}||s)$.

![[Pasted image 20230118231232.png]]

##### HMAC-SHA256

Prevents length extension attack, and we can treat it as a PRF.
$$
\text{HMAC}_k(m) = H(k\oplus c_1 || H(k\oplus c_2 || m))
$$

#### Pseudorandom

##### True Randomness

Output of a physical process that is inherently unpredictable (e.g. quantum observation).

##### Pseudorandom Generator (PRG)

See [[Private-Key (Symmetric) Cryptography 475#3.3.1 Pseudorandom Generators|pseudorandom generator]] and [[Introduction and Classical Cryptography 475#Random Generation|generate random stream]] from EECS 475.


We can build a PRG from a PRF by defining $g_k() := f_k(0) || f_k(1) || \dots$



## L4 Confidentiality

We want to keep the message secret form an eavesdropper.

See [[Introduction and Classical Cryptography 475#1.3 Historical Ciphers|historical ciphers]] and [[Introduction and Classical Cryptography 475#2.2 The One-Time Pad|one-time pad]] from EECS 475.

#### Stream Cipher

See [[Private-Key (Symmetric) Cryptography 475#3.6.1 Stream Ciphers|stream cipher]]. E.g. ChaCha20

#### Block Cipher

See [[Private-Key (Symmetric) Cryptography 475#3.6.3 Block Ciphers|block cipher]].

#### AES

Advanced encryption standard, the most commonly used block cipher. 128 bits x key length -> 128 bits. 10, 12, or 14 rounds for key length 128 bits, 192 bits, or 256 bits. Each round:

1. Non-linear substitution for each type thru a lookup table
2. Shift rows
3. Linear-mix columns by multiplying a constant invertible matrix
4. Key addition: XOR

##### PKCS7

A padding method which adds n bytes of value n.

Problem: how do we encrypt message longer than 128 bits?

##### Cipher-Block Chaining (CBC) Mode

![[Pasted image 20230122163058.png]]

Chains ciphertexts to obscure later ones. We have to send IV.

##### Counter (CTR) Mode

![[Pasted image 20230125163143.png]]

Generate from $k$ a keystream and a unique **nonce**. Encryption and decryption are simply XOR. It doesn't require padding, but we cannot reuse nonce for same $k$ (otherwise the adversary can XOR the two ciphertext).



## L5 Combining Confidentiality and Integrity

Problem: many encryption methods are malleable. E.g. CBC mode suffers from padding oracle attack, and CTR mode suffers from the flip of bits.

##### Authenticated Encryption (AE)

To combine encryption and MAC, we have two approaches:

1. Generically compose encryption and MAC
2. Build "all-in-one" primitive that does both

* $c = {\sf Enc}_k(p)$
* $p/\text{fail} = {\sf Dec}_k(c)$

**Security definition**

1. If $b=0$ then $G() = E_k(), H() = D_k()$ or fail for previous $E()$ outputs
2. If $b=1$ then $G() = \text{random bits}, H() = \text{Return fail}$
3. Give Mallory $G()/H()$ oracles
4. Mallory guess $b$ in polynomial time

We say that AE is **secure** if Mallory can't do meaningfully better than random guessing.

![[Pasted image 20230125162431.png]]

**Cryptographic Doom Principle**

If you perform any cryptographic operations before verifying the MAC, it will somehow inevitably lead to doom.

##### Padding Oracle Attack

Happens when the server tells the attacker whether or not the padding is correct. E.g. the server checks padding before checking the message integrity.

![[Pasted image 20230122164645.png]]

##### AE with Associated Data (AEAD)

![[Pasted image 20230125162926.png]]

AES-GCM is a widely-used example.

##### Galois/Counter Mode (GCM)

![[Pasted image 20230125163307.png|400]]

Although it violates principle of key separation, we can prove it's ok to do this.



## L6 Key Exchange and Public-Key Cryptography

##### Diffie-Hellman Key Exchange

We need a shared private key for most of the encryption algorithms: [[Cryptography 376#Diffie-Hellman Protocol|Diffie-Hellman key exchange]]. However, it is prone to man in the middle attack. We can defend MITMs with digital signatures. Elliptic curves cryptography (ECC) is more commonly used nowadays, which is faster, and (hoped) harder to break.

##### Forward Secrecy

Even if our key is stolen, we don't want our adversary to break our previous ciphertexts. We use D-H to generate a temporary session key.

##### RSA Public-Key Cryptography

See [[Cryptography 376#24 RSA|RSA]] from EECS 376. This algorithm is problematic for real use. RSA can be used for public-key encryption, authentication (digital signature), or both. E.g. Alice first encrypts the message with Bob's public key, and then signs it with her private key. However, ==signatures can be forged on random message==, and textbook RSA is dangerously insecure.

##### A Toy Secure Channel Protocol

![[Pasted image 20230204160931.png]]
