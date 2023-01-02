### 22 Introduction to Cryptography

##### Authority

Proving one's identity

##### Privacy/Confidentiality

Ensuring that no one can read the message except the intended receiver

##### Integrity

Guarantee that the received message has not been altered in any way

##### Kerckhoff's Principle

A cryptosystem should be secure, even if everything about the system, except the key, is public known.

**Computational/conditional security**

To learn any information about the secret message, Eve will have to solve an [[Complexity 376#NP-Hard|NP-Hard]] problem.

#### 22.1 Review of Modular Arithmetic

**T169**

Let $n$ be a positive integer and $a$ be an element of $\mathbb{Z}_n^+$. An inverse of $a$ modulo $n$ is an element $b \in \mathbb{Z}_n^+$ such that $a \cdot b \equiv 1 \pmod{n}$.

An inverse of $a$, denoted as $a^{-1}$, exists modulo $n$ is and only if $a$ and $n$ are coprime.

#### 22.2 One-time Pad

* The key is a random string at least as long as the plaintext.
* The key is pre-shared between the two parties.
* The key is only used once.

The only cryptosystems that provides information-theoretic security.



### 23 Diffie-Hellman Key Exchange

##### Symmetric-Key Algorithm

Algorithms that use the same key for both encryption and decryption.

**Key exchange problem**

We want two parties to establish a shared key even if all their communication is over an insecure channel.

#### Diffie-Hellman Protocol

Each parties has its own private key to generate a public key. Recovering the private key would require to solve an NP-Hard problem.

**Generator**

![[Pasted image 20221220004408.png]]

Every prime number has at least one generator.

![[Pasted image 20221220004802.png]]

If Eve wants to recover one of the private key, she has to solve the Discrete Logarithm.

**Discrete Logarithm**

Given $g^i \equiv x \pmod{q}, q, g, x$, compute $i$.

The brute-force algorithm is exponential, and the *discrete logarithm assumption* states there's no efficient algorithm exists.



### 24 RSA

**Fermat's Little Theorem**

If $p$ is a prime number, then $a^{p-1} \equiv 1 \pmod{p}$  for $1 \leq a \leq p-1$.

**Extension**

$$
a^{1 + k \phi(n)} \equiv a \pmod{n}
$$
where $\phi{n}$ is Euler's totient function, whose value is the number of elements in $\mathbb{Z}_n^+$ that are coprime to $n$.

#### RSA Encryption

![[Pasted image 20221220010816.png]]
![[Pasted image 20221220010828.png]]

Eve has to factor $n = pq$, and the *factorization hardness assumption* state that there's no efficient algorithm for it. Only Alice can decrypt, however, anyone can pretend to be Bob! That's why we need RSA signature.

#### 24.1 RSA Signature

Privacy and confidentiality are solved, what about authentication and integrity?

![[Pasted image 20221220011922.png]]

The pair $m, m^d \pmod{n}$ acts as a *certificate* that the sender knows the secrete key. Only Alice can send this message, everyone else can decrypt it. Thus if the certificate is valid, it must be Alice's signature!

But wait, what if there's an man in the middle attack, for example Eve pretends to be Alice and send her public key to Bob?

##### Certificate

We need a way to verify that the public key truly belongs to Alice. We need a person, say Susan that works at the company's certificate authority center. Susan can create a certificate for Alice simply by signing Alice's public key as well as some information about Alice.

Now Bob can verify Alice's public key by checking the certificate. 

What happens if a website doesn't have a certificate?

Anyone else can make a website looks the same and cheat the users.





