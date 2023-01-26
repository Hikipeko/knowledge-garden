## 3 Private-Key Encryption

### 3.1 Computational Security

We want to relax the secrecy we want to achieve:

* Security is only guaranteed against efficient adversaries that run for some feasible amount of time.
* Adversaries can succeed with a very small probability.

#### 3.1.1 The Concrete Approach

A schema is $(t, \epsilon)-\sf{secure}$ if any adversary running for time at most $t$ succeeds in breaking the schema with a probability at most $\epsilon$.

A computational hard schema with key length 128 will take 10 billion years to break.

#### 3.1.2 The Asymptotic Approach

We use an integer-valued **security parameter** to parameterize both cryptographic schemes and involved parties (honest party and attackers). Temporally we can view the security parameter as the length of the private key, and view running time and success probability as a function of security parameter.

* Efficient adversaries: randomized algorithms running in time ${\sf poly}(n)$, see [[Complexity 376#P|P]].
* Negligible success probability: smaller than any inverse ${\sf poly}(n)$ for $n > N$.

A scheme is **secure** if any PPT (probabilistic polynomial-time) adversary succeeds in breaking the scheme with at most negligible probability.

Within this approach, the honest party can tune the security parameter to some desired level.

##### Randomized Algorithm

Randomized algorithms are assumed to have access to a sequence of unbiased, independent random bits. We gives the attackers this power.



### 3.2 Defining Computationally Secure Encryption

##### Private-Key Encryption Scheme

1. **Generation** $\sf Gen$: a probabilistic algorithm $k \leftarrow {\sf Gen}(1^n)$, $|k| > n$.
2. **Encryption** $\sf Enc$: input a key $k$ and a message $m$ and outputs a ciphertext $c \leftarrow {\sf Enc}_k(m)$
3. **Decryption** $\sf Dec$: input a key $k$ and a ciphertext $c$ and outputs a plaintext $m = {\sf Dec}_k(c)$ or an error $\bot$.

If ${\sf Enc}_k$ is only defined for message $m \in \{0,1\}^{l(n)}$, then it's a fixed-length private-key encryption scheme for message of length $l(n)$.

#### 3.2.1 The Basic Definition of Security

We made two modifications to the [[Introduction and Classical Cryptography#Perfectly Indistinguishability|perfectly indistinguishability]]. We consider only adversaries running in polynomial time, and we let them to have negligible advantage. Another difference is that we view the running time and success probability as functions of $n$.

##### Definition 3.8 EAV-Security (Indistinguishability)

Let a private-key encryption scheme $\Pi = ({\sf Gen, Enc, Dec})$, an adversary $\mathcal A$, and a value $n$ for the security parameter, the indistinguishability experiment ${\sf PrivK}_{{\mathcal A}, \Pi}^{\sf eav}(n)$:

1. $\mathcal A$ is given input $1^n$, and outputs a pair of messages $m_0, m_1$ having the same length.
2. $k \leftarrow {\sf Gen}(1^n)$, $b$ is a uniformly chose from $\{0,1\}$, $c \leftarrow {\sf Enc}_k(m_b)$ is computed and given to $\mathcal A$. We refer to $c$ as the challenge ciphertext.
3. $\mathcal A$ outputs a bit $b'$.
4. The output of the experiment is defined to be ${\sf PrivK}_{{\mathcal A}, \Pi}^{\sf eav}(n) = 1$ if $b' = b$, and 0 otherwise.

If for all probabilistic polynomial-time adversaries $A$ there is a negligible function $\sf negl$ such that for all $n$,
$$
\Pr[{\sf PrivK}_{{\mathcal A}, \Pi}^{\sf eav}(n) = 1] \leq \frac{1}{2} + {\sf negl}(n).
$$



### 3.3 Constructing and EAV-Secure Encryption Scheme

#### 3.3.1 Pseudorandom Generators

A pseudorandom generator $G$ is an efficient, deterministic algorithm which transfers a short uniform string (seed) into a longer "uniform-looking" output string.

Let $\sf Dist$ be a *distribution* on $l$-bit strings, we say $\sf Dist$ is pseudorandom if any adversary cannot efficiently distinguish between a string samples from $\sf Dist$ and a uniform string of length $l$.

##### Definition 3.14

For any PPT algorithm $D$, there is a negligible function $\sf negl$ such that
$$
|\Pr[D(G(s)) = 1] - \Pr[D(r) = 1]| \leq {\sf negl}(n),
$$
where the first probability is taken over uniform choice of $s \in \{0,1\}^n$, and the second probability is taken over uniform choice of $r \in \{0,1\}^{l(n)}$. $l(n)$ is called the expansion factor of $G$.






