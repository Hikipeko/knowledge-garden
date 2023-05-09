Hash functions provides a way to deterministically map a long input string to a shorter string, called a **digest**. We would want it to be extremely hard to find a collision.

### 6.1 Definitions

#### 6.1.1 Collision Resistance

We consider keyed hash functions $H^k$, where $k$ is public.

**Definition 6.1** A **hash function** is a pair of ppl. algorithms. $\sf Gen$ outputs a key $k$. $H$ is a deterministic algorithm that takes as input a key $k$ and a string $x \in \{0,1\}^*$ and outputs a string $H^k(x) \in \{0,1\}^{l(n)}.$

If the input length is fixed, we call $H$ a compression function.

**Definition 6.2** A hash function is **collision resistant** if for all ppl. adversary, the probability of finding $x, x'$ such that $x \neq x' \land H(x) = H(x')$ is negligible.

In real world, we use **unkeyed** hash functions, where the key is never changed. Although this seems to be insecure, it's collision resistant as long as collision pairs are not found.

#### 6.1.2 Weaker Notions of Security

* **Second-preimage resistance**: given $x$, it's hard to find $x' \neq x$ s.t. $H(x') = H(x)$.
* **Preimage resistance**: given $y = H(x)$ ($x$ is not given), it's hard to find $H(x') = H(x)$. We allow $x' = x$.



### 6.2 The Merkle-Damgård Transform

Fixed-length compression is easy, but how to we extend it to arbitrary length input? 

**Construction 6.3** Let $({\sf Gen}, h)$ be a compression function for input of length $n + n' > 2n$ with output length $n$. Fix $l < n'$ and $IV \in \{0,1\}^n$. Define our $H$ on input $x\in \{0,1\}^*$ of length $L<2^l$ as follows:

1. Append a 1 to $x$, followed by enough zeros so that the length of the resulting string is $l$ less than a multiple of $n'$. Then append $L$ encoded as an $l$-bit string. Parse the string as sequence of $n'$-bit blocks $x_1, ..., x_B$.
2. Set $z_0 = IV$.
3. For $i = 1, ..., B$, compute $z_i := h(z_{i-1}||x_i)$.
4. Outputs $z_B$.

**Theorem 6.4** If $({\sf Gen}, h)$ is collision resistant, then so is $({\sf Gen}, H)$.

We show that a collision in $H$ yields a collision in $h$. If $H(x) = H(x')\land x \neq x'$. Denote $x = x_1||x_2||\cdots|| x_B$ and $x' = x'_1||x'_2|| \cdots ||x'_{B'}$. Let $z_0, z_1, .., z_B$ be the intermediate results.


1. If $L \neq L'$, then $x_B \neq x'_{B'}$, and we find a collision $z_{B-1} || x_B$ and $z'_{B'-1} || x'_{B'}$.
2. If $L = L'$, then there exists such $x_i \neq x_i'$, and we find a collision.



### 6.3 Message Authentication Using Hash Functions

#### 6.3.1 Hash-and-MAC

**Construction 6.5** Let $\Pi = ({\sf Mac}, {\sf Vrfy})$ be a MAC for messages of length $l(n)$, and let ${\cal H} = ({\sf Gen}_H, H)$ be a hash function with output length $l(n)$. Construct a MAC $\Pi'$ as follows:

* **Gen'**: choose uniform $k \in \{0,1\}^n$ and run $s = {\sf Gen}_H(1^n)$, outputs $(k,s)$.
* **Mac'**: output $t \gets {\sf Mac}_K(H^s(m))$
* **Vrfy'**: canonical.

**Theorem 6.6** If $\Pi$ is a secure MAC for message of length $l(n)$ and $\cal H$ is collision resistant, then 6.5 is a secure MAC for arbitrary length message.

![[Pasted image 20230320125518.png]]

#### 6.3.2 HMAC

Combining two things is not easy. Can we construct a MAC or PRF directly from a hash function?

**Construction 6.7** Let $H$ be a hash function constructed by applying the Merkle-Damgård transform. Fix distinct constants ${\sf opad}, {\sf ipad}\in \{0,1\}^{n'}$. Define $\sf Mac$ as
$$
t := H^s((k\oplus {\sf opad})||H^s((k \oplus {\sf ipad})||m)).
$$

![[Pasted image 20230320131617.png|500]]

We can view this construction as a inner hash plus an outer MAC. To prove that this construction is secure, we have to further rely on that using same $k$ doesn't cause problem. We have to assume 
$$
G^s(k) := h^s(IV||(k\oplus{\sf ipad})) || h^s(IV||(k\oplus {\sf opad})) = k_{in} || k_{out}
$$
is a PRG.

**Theorem 6.8** Assume $G^s$ is a PRG, $\tilde \Pi^s$ is a secure fixed-length MAC, and $h$ is collision resistant. Then HMAC is a secure MAC.

Why we need a inner secret key, as collision resistant scheme doesn't require a secret key? Because this allows security of HMAC to be based on a weakly collision resistant $H$. E.g. MD5.
