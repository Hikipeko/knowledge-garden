### 5.1 Chosen-Ciphertext Attacks and CCA-Security

An adversary causes a receiver to decrypt ciphertexts that the adversary generates.

CCA-security additionally gives the attacker the $\sf Dec$ oracle, except for the challenge message.

### 5.2 Authenticated Encryption

We have authentication schema, we have encryption schema, but how do we combine?

#### 5.2.1 Defining Authenticated Encryption

The **unforgeable** encryption experiment ${\sf EncForge}_{{\cal A}, \Pi}(n)$:

1. $\sf Gen$ generates key $k$.
2. $\cal A$ is given oracle ${\sf Enc}_k(\cdot)$. It eventually outputs a ciphertext $c$. Let $m:={\sf Dec}_k(c)$.
3. $\cal A$ succeeds if and only if $m \neq \bot$ and $m$ is not queried before.

**Definition 5.2** A private-key encryption scheme is **unforgeable** if for all ppl. $\cal A$, its advantage is negligible.

**Definition 5.3** A private-key encryption scheme is an **authenticated encryption (AE)** scheme if it is *CCA-secure and unforgeable*.

See [[Security Fundamentals 388#AE with Associated Data (AEAD)]].



### 5.3 AE Schemes

#### 5.3.1 Generic Constructions

Not all combination works. Secure cryptographic tools can be combined in such a way that the result is insecure. Let $\Pi_E = ({\sf Enc}, {\sf Dec})$ be a CPA-secure encryption scheme, and let $\Pi_M({\sf Mac}, {\sf Vrfy})$ be a strongly secure MAC.

**Encrypt and authenticate** 

* Since the tag segment is deterministic, the scheme is not CPA-secure. Also the tag might leak information about the plaintext (say just append a copy of the original message).
* The scheme might not be unforgeable. The ciphertext can have extra useless parts that one can "tweak" without causing failure.

**Authenticate then encrypt**

* The scheme might not be unforgeable due to the same reason.
* The scheme might not be CPA-secure because the length of tag might be different.

**Construction 5.6 Encrypt then authenticate**

* **Gen'**: output independent, uniform $k_E, k_M \in \{0,1\}^n$.
* **Enc'**: $c \gets {\sf Enc}_{k_E}(m), t \gets {\sf Mac}_{k_M}(c)$, output ciphertext $\langle c,t \rangle$.
* **Dec'**: First check ${\sf Vrky}_{k_M}(c) \stackrel{?}{=} t$, then output ${\sf Dec}_{k_E}(c)$ or $\bot$. 

**Theorem 5.7** Construction 5.6 is an authenticated encryption scheme.
