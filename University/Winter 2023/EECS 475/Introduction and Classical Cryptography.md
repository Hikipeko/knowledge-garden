## 1 Introduction

### 1.1 Cryptography and Modern Cryptography

Cryptography has gone from an "art" to a science.



### 1.2 The Setting of Private-Key Encryption

**Private-key setting** is that the communication parties share some secret information in advance.

![[Pasted image 20230113180050.png]]

##### The syntax of encryption

We have a message space $\mathcal{M}$ along with three procedure.

1. **Generation** $\sf Gen$: a probabilistic algorithm outputs key $k$ according to some distribution
2. **Encryption** $\sf Enc$: input a key $k$ and a message $m$ and outputs a ciphertext $c = {\sf Enc}_k(m)$
3. **Decryption** $\sf Dec$: input a key $k$ and a ciphertext $c$ and outputs a plaintext $m = {\sf Dec}_k(c)$

**Correctness requirement**: ${\sf Dec}_k({\sf Enc}_k(m)) = m$

**Key space** is the set of all possible keys output by $\sf Gen$, denoted $\mathcal K$. Typically assumed to choose a key uniformly from the key space.

##### Kerckhoffs' principle

The cipher method must not be required to be secret, and it must be able to fall into the hand of the enemy without inconvenience. Public encryption algorithms has many benefits:

1. has a public view checking for weakness
2. ensures compatibility.



### 1.3 Historical Ciphers

##### Caesar's cipher

Shift the alphabet $k$ places. Trivial to recover.

##### Mono-alphabetic substitution cipher

A permutation of the alphabet. Can be attacked by statistical properties of the English language.

##### Vigenère cipher

Shift a chunk by a string. e.g. hello -> igmnp if key is 12. It can also be attacked by statistics.



### 1.4 Principles of Modern Cryptography

We want rigorous proof that a given construction is secure.

#### 1.4.1 Formal Definitions

How can we rigorously prove security without its definition?

E.g. the attacker should gain no additional information about the plaintext given the cyphertext.

##### Threat Model

* Ciphertext-only attack: only has ciphertext
* Known-plaintext attack: knows some (plaintext, ciphertext) pairs
* Chosen-plaintext attack: knows ciphertext of any plain text the attack wants
* Chosen-ciphertext attack: additionally knows plaintext of any ciphertext

#### 1.4.2 Precise Assumptions

We should clearly state our assumptions during the proof of security. Many security proofs relay on some not yet proven assumptions (e.g. factorize a semiprime is NP-hard).

#### 1.4.3 Proofs of Security

We want to provide a rigorous proof that a construction satisfies a given definition under certain assumptions.

#### 1.4.4 Provable Security and Real-World Security

We should be careful about the assumptions, and make sure the definition meets our real-world need. Provable security doesn't necessarily imply security of that schema in the real world.



## 2 Perfectly Secret Encryption

##### Random Generation

1. Collect a pool of high-entropy data. Some hardware support this, e.g. Intel processors uses thermal noise
2. From these data, generate a sequence of nearly independent and unbiased bits

We can obtain a uniform sequence even if the original bits are biased. We output 0 if we see 10, and output 1 if we see 01. 11 and 00 trigger not output.

### 2.1 Definitions

$K$ (random variable for the key) and $M$ (random variable for the message) are required to be independent.

##### Perfect Secrecy 2.3

The adversary know anything except for the private key. He should not gain any knowledge about the plaintext by observer the ciphertext.

An encryption schema with message space $\mathcal M$ is *perfectly secret* if for every probability distribution for $\mathcal M$, every message $m \in \mathcal M$, and every ciphertext $c \in \mathcal C$ for which $\text{Pr}[C=c]>0$:

$$
\text{Pr} [M = m | C = c] = \text{Pr} [M = m].
$$

TODO