### L24

#### CPA-Secure RSA

The "textbook" RSA encryption is not CPA secure since the encryption function is deterministic. Also, note that the security of RSA requires the input $y = {\sf RSA}_{N,e}(x)$ to be uniform random. If the adversary can choose from any $x$, then there's no guarantee that $y$ is chosen uniformly random.

To fix this problem, we let the encryptor choose a random $x$ and use $H(x)$ to "one time pad" the message $m$, where $H$ is a hash function. Informally, since $x$ is chosen uniformly random, ${\sf RSA}_{N,e}$ is a bijection, $y= {\sf RSA}_{N,e}(x)$ is also uniform random. Now we satisfies the **RSA hardness assumption**, and we are confident that an adversary cannot recover $x$ from $y$.

**Secure RSA Encryption Scheme

* Enc: on input public key $e, N$ and message $m$, choose a uniform $x \in {\mathbb Z}_N^*$ and output the ciphertext $c=( y={\sf RSA}_{N,e}(x) = x^e \bmod N, H(x) \oplus m)$.
* Dec: on input secret key $d, N$ and ciphertext $c=(y,p)$, compute $x = {\sf RSA}_{N,d}(y) = y^d \bmod N$ and output $m = H(x) \oplus p$.

**CPA Security**

We require the hash function $H$ to "practically behaves" like a uniform random function (a.k.a. random oracle). Intuitively, if $H$ has an uneven distribution, an adversary in CPA game might manipulate $m_1$ and $m_2$ and distinguish the ciphertext by observing the difference between $H(x_1) \oplus m_1$ and $H(x_2) \oplus m_2$. On the other hand, if the RSA assumption holds and $H$ is a "random-oracle", then the above scheme is CPA secure.

#### Digital Signatures

Digital signature is a way of authenticity in public key setting. A big difference from the symmetric message authentication is that there's no shared secret key $k$.

##### Model

* ${\sf Gen}(1^n)$: output a (public) verification key $vk$ and a (secret) signing key $sk$.
* ${\sf Sign}(sk, m)$: Given signing key $sk$ and message $m$, output a signature $\sigma$.
* ${\sf Ver}(vk, m, \sigma)$: Given verification key $vk$, message $m$, purported signature $\sigma$, accept or reject.

**Correctness**: we require that $\forall (vk, sk) \gets {\sf Gen}(1^n), \forall m, {\sf Ver}(vk, m, {\sf Sign}(sk, m)) = \text{accept}$ .

##### Security

An signature scheme $\Pi = ({\sf Gen}, {\sf Sign}, {\sf Ver})$ is unforgeable under chosen message attack (UFCMA) if for any p.p.t. forger $\mathbb F$,
$$
\mathbf{Adv}_\Pi^{\sf CMA}({\mathbb F}) = \Pr_{(vk,sk) \gets{\sf Gen}(1^n)} ({\mathbb F}^{{\sf Sign}_{sk}(\cdot)}(1^n, vk) \text{ forges})= \text{negl}(n),
$$
where "forges" means output $(m^*, \sigma^*)$ such that ${\sf Ver}(vk, m^*, \sigma^*) = \text{accept}$ and $m^*$ wasn't a query to the $\sf Sign$ oracle.

##### Textbook RSA

First let's see the broken "textbook version" RSA which we should never use in practice.

* ${\sf Gen}(1^n)$: run $(N, e, d) \gets {\sf GenRSA}(1^n)$, output $vk=(N,e),sk=(N,d)$
* ${\sf Sign}(sk=(N,d, m\in {\mathbb Z}^*_N)$: output $\sigma = {\sf RSA}_{N,d}(m) = m^d \bmod N$.
* ${\sf Ver}(vk=(N,d), m\in {\mathbb Z}^*_N, \sigma\in {\mathbb Z}^*_N)$: Accept if $m={\sf RSA}_{N,e}(\sigma) = \sigma^e \bmod N$, else reject.

There are two trivial attacks.

1. Query to get $(m, \sigma)$, and forge $(\sigma, m)$
2. Query to get $(m_1, \sigma_1), (m_2, \sigma_2)$ and forge $(m_1 \cdot m_2, \sigma_1 \cdot \sigma_2)$.

##### Secure RSA Signature

![[Pasted image 20230411200901.png]]

The fundamental problem of "textbook RSA" is that the adversary knows the fact that some message $m$ is mapped to $\sigma$ such that $\sigma = m^d \bmod N$. In order to construct a secure signature, we must prevent these $(m, m^d)$ pair from leaking. To achieve this, we can apply $\sf RSA$ to $H(m)$ instead of directly on $m$.

**Scheme**

* ${\sf Gen}(1^n)$: run $(N, e, d) \gets {\sf GenRSA}(1^n)$, output $vk=(N,e),sk=(N,d)$
* ${\sf Sign}(sk=(N,d, m\in {\mathbb Z}^*_N)$: output $\sigma = H(m)^d \bmod N$.
* ${\sf Ver}(vk=(N,d), m\in {\mathbb Z}^*_N, \sigma\in {\mathbb Z}^*_N)$: Accept if $H(m)=\sigma^e \bmod N$, else reject.

**UFCMA Security**

Again, we require the hash function $H$ to "practically behaves" like a uniform random function (a.k.a. random oracle). As a result, $H(m)$ is uniform random, and we satisfies the requirement of RSA assumption. If the RSA assumption holds and $H$ is a "random-oracle", then the above scheme is UFCMA secure.
