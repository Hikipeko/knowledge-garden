##### Random Generation

1. Collect a pool of high-entropy data. Some hardware support this, e.g. Intel processors uses thermal noise.
2. From these data, generate a sequence of nearly independent and unbiased bits

We can obtain a uniform sequence even if the original bits are biased. We output 0 if we see 10, and output 1 if we see 01. 11 and 00 trigger not output. Some cryptography level PRG:

* C (Linux): `#include <sys/random.h>`
* Python: `import secrets; data = secrets.randbits(256)`
* JavaScript: `const array = new Uint8Array(32);self.crypto.getRandomValues(array)`

### 2.1 Definitions

$K$ (random variable for the key) and $M$ (random variable for the message) are required to be independent.

##### Perfect Secrecy 2.3

The adversary know anything except for the private key. He should not gain any knowledge about the plaintext by observer the ciphertext.

An encryption schema with message space $\mathcal M$ is **Shannon secret** if for every probability distribution for $\mathcal M$, every message $m \in \mathcal M$, and every ciphertext $c \in \mathcal C$ for which $\text{Pr}[C=c]>0,$

$$
\text{Pr} [M = m | C = c] = \text{Pr} [M = m].
$$

*Try to prove it.* Shannon secrecy is equivalent to **perfect secrecy**: for every $m, m' \in \mathcal M$, and every $c \in \mathcal C$, we have

$$
\text{Pr} [{\sf Enc}_K(m) = c] = \text{Pr} [{\sf Enc}_K(m') =c].\quad (2.1)
$$

##### Perfectly Indistinguishability

It is also equivalent to **perfectly indistinguishability**, which means an adversary with unbounded computation power can do no better than random guess of which one of the two messages (specified by the adversary) are encrypted given the ciphertext.

### 2.2 The One-Time Pad

The key is uniformly chosen from the key space. Both encryption and decryption are $c := k \oplus m,\, m := k \oplus c$.

The one-time pad encryption schema is perfectly secret. This can easily be proved by using (2.1).

##### Limitations

* The key has to be as long as the message.
* One key can be used only once. Note that $c\oplus c' = m \oplus m'$.

### 2.3 Limitations of Perfect Secrecy

##### Theorem 2.11

*Try to prove it.* For any perfectly secret encryption scheme with message space $\mathcal M$ and key space $\mathcal K$, then $|\mathcal K| \geq |\mathcal M|$.
