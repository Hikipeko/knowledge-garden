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

We made two modifications to the [[1 Introduction 475#Perfectly Indistinguishability|perfectly indistinguishability]]. We consider only adversaries running in polynomial time, and we let them to have negligible advantage. Another difference is that we view the running time and success probability as functions of $n$.

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

Do pseudorandom generators exist? We don't know!

#### 3.3.2 Proofs by Reduction

![[Pasted image 20230207121817.png]]

To prove a scheme $\Pi$ is secure, we want to transform any efficient adversary $\cal A$ that breaks $\Pi$ into an efficient algorithm $\cal A'$ that solves $\sf X$.

#### 3.3.3 EAV-Security from a Pseudorandom Generator
 
We construct an encryption scheme $\Pi$. First we use $G$ to generate a pseudorandom string from the secret key. Then we use the pseudorandom string for both encryption and decryption (XOR). We want to prove that if $G$ is a pseudorandom generator, then $\Pi$ is EAV-secure.

The key idea is to construct a distinguisher $D$ using $\cal A$:

1. Run ${\cal A}(1^n)$ to obtain $m_0, m_1$
2. Choose a uniform bit $b \in \{0,1\}$. Set $c = w \oplus m_b$.
3. Give $c$ to $\cal A$ and obtain output $b'$. Output 1 if $b' = b$, and output 0 otherwise.



### 3.4 Stronger Security Notions

#### 3.4.1 Security for Multiple Encryptions

EAV-security only ensures a single message is sent securely, what if the communication parties need to send more than one messages?

#### 3.4.1 Chosen-Plaintext Attacks and CPA-Security

##### CPA Game

The textbook definition is slightly different from the class definition (in 3.4.3). The adversary is given a single $c_b$. However, $\cal A$ have oracle access to ${\sf Enc}_k$.

Think of the WW2, the US navy deciphered AF by sending a message "Midway is low on fresh water".

##### Definition 3.21

A scheme $\Pi$ has **indistinguishable encryptions under a chosen-plaintext attack (aka CPA-secure)** if all p.p.t $A$ has negligible advantage in the CPA game against $\Pi$.
$$
\Pr[{\sf PrivK}_{{\mathcal A}, \Pi}^{\sf eav}(n) = 1] \leq \frac{1}{2} + {\sf negl}(n).
$$

##### Theorem 3.20

There does not exist a CPA-secure encryption with a deterministic and stateless ${\sf Enc}_k$. Just think of first input $(m_0, m_0)$, and then input $(m_0, m_1)$.

#### 3.4.3 CPA-Security for Multiple Encryptions

![[Pasted image 20230209125805.png]]

A scheme $\Pi$ is **CPA-secure for multiple encryptions** if all p.p.t $A$ has negligible advantage in the CPA game against $\Pi$.

CPA-secure for multiple encryptions implies CPA-secure since ${\sf Enc}(k(m) = {\sf LR}_{k,b}(m,m)$. More interestingly, the other way around is also true, and thus these two definitions are equivalent.



### 3.5 Constructing a CPA-Secure Encryption Scheme

#### 3.5.1 Pseudorandom Functions and Permutations

We cared about keyed function $F(k,x) : \{0,1\}^* \times \{0,1\}^* \to  \{0,1\}^*$ that are efficient. For simplicity, we assume that the length of key, input, output are the same as the security parameter $n$.

Let ${\sf Func}_n$ be the set of all functions that maps an n-bit strings to n-bit strings. We can view each function as a lookup table, and thus the size of ${\sf Func}_n$ is $2^{n\cdot2^n}$. A keyed function $F$ incudes a distribution on functions of ${\sf Func}_n$. We say that $F$ is *pseudorandom* if the function $F_k$ chosen uniformly from $F$ is indistinguishable from a function chosen uniformly from ${\sf Func}_n$.

##### Definition 3.24

![[Pasted image 20230212174327.png]]

An efficient, length preserving, keyed function $F$ is a **pseudorandom function** if for all p.p.t. distinguisher $D$:
$$
\left| \Pr [D^{F_k(\cdot)}(1^n) = 1] - \Pr [D^{u(\cdot)}(1^n) = 1] \right| \leq {\sf negl}(n),
$$
where the first probability is taken over uniform choice of $k \in \{0,1\}^n$, and the second probability is taken over uniform choice of $u \in {\sf Func}_n$.

We can easily create a [[3 Private-Key Encryption 475#3.3.1 Pseudorandom Generators|pseudorandom generator]] with a pseudorandom function by just concatenating $G(s) = F_s(1) || F_s(2) || \cdots || F_s(l)$. More interestingly, the other direction is also true. If the expansion factor is large enough, we can use $G$ to construct a lookup table. If this is not the case, the expansion is more complicated.

##### Pseudorandom Permutations

${\sf Perm}_n \subset {\sf Func}_n$ denotes the set of all permutations on $\{0,1\}^n$, the size is $(2^n)!$. An permutation $F_k$ is pseudorandom if any decider cannot efficiently distinguish between $F_k$ and a uniform permutation.

An efficient, length preserving, keyed permutation $F$ is a **pseudorandom permutation** if for all p.p.t. distinguisher $D$:
$$
\left| \Pr [D^{F_k(\cdot),F_k^{-1}(\cdot)}(1^n) = 1] - \Pr [D^{u(\cdot),u^{-1}(\cdot)}(1^n) = 1] \right| \leq {\sf negl}(n).
$$
We additionally give the distinguisher the oracle access to the inverse of the permutation.

#### 3.5.2 CPA-Security from a Pseudorandom Function

##### Construction 3.28

* Gen: on input $1^n$, output $k$ chosen uniformly from the key space.
* Enc: on input $k$ and $m$, choose a uniform $r \in \{0,1\}^n$ and output the ciphertext $c=\langle r,m\oplus F_k(r) \rangle$.
* Dec: on input $k$ and $c$, output $m = c \oplus F_k(r)$.

##### Theorem 3.29

If $F$ is a pseudorandom function, then the above construction is a CPA-secure, fixed-length private-key encryption scheme for message of length $n$.

Note that in the proof on the textbook, the definition of CPA-security and the construction of the decider is different from those in class. Here we presents the in-class proof which is more intuitive.

![[Pasted image 20230214160014.png]]

We should notice that if we have duplicate $x$, the adversary can gain extra information. We should take this into consideration in our proof.
$$
\begin{align*}
\Pr[D_0^{u(\cdot)}=1]&=\Pr[D_0^{u(\cdot)}=1|\text{no repeat}]\cdot \Pr[\text{no repeat}]\\
&+ \Pr[D_0^{u(\cdot)}=1|\text{repeat}]\cdot \Pr[\text{repeat}]\\
&\leq\Pr[D_0^{u(\cdot)}=1|\text{no repeat}] + \Pr[\text{repeat}]\\
&\leq \Pr[{\cal A}=1|\text{hybrid}] + \frac{q(n)^2}{2^{n+1}}\\
&\leq \Pr[{\cal A}=1|\text{hybrid}] + \text{negl}(n)
\end{align*}
$$
![[Pasted image 20230214162758.png]]



### 3.6 Modes of Operation and Encryption in Practice

We are now interested in how building blocks such as PRG and PRP are instantiated in real world through stream cipher and block ciphers.

#### 3.6.1 Stream Ciphers

A stream cipher is a pair of deterministic algorithms (Init, Next) where:

* **Init** takes as input a seed $s$ and an optional **initialization vector IV**, and outputs some initial state $st$.
* **Next** takes as input the current state $st$ and outputs a bit $y$ along with a updated state $st'$.

Based on Init and Next, we define **GetBits** which returns $\langle y, st_l \rangle$ where $y$ is the $l$-bit string and $st_l$ is the current state. We also define
$$
G^l(s) := {\sf GetBits}_1({\sf Init}(s), 1^l).
$$
We say the stream cipher is **secure** if $G^l$ is a PRG for any polynomial $l$.

##### Construction 3.30

Let $F$ be a PRF, we define a stream cipher:

* **Init**: on input $s \in \{0,1\}^n$ and $IV \in \{0,1\}^{3n/4}$, output $st = (s, IV, 0)$
* **Next**: on input $st= (s, IV, i+1)$, output $y := F_s(IV || i)$ and updated state $st' = (s, IV, i+1)$

Pros: simple, parallel, and no padding.

#### 3.6.2 Stream-Cipher Modes of Operation

##### Synchronized Mode

The sender and the receiver keeps a mutual state, so it is a **stateful** encryption. Used when messages arrive in order and no messages are lost.

##### Unsynchronized Mode

**Construction 3.31** The sender choose uniform $IV \in \{0,1\}^n$ and outputs the ciphertext
$$
\langle IV, {\sf GetBits}_1({\sf Init}(k, IV), 1^{|m|})\oplus m\rangle.
$$

#### 3.6.3 Block Ciphers

A.k.a. [[3 Private-Key Encryption 475#Pseudorandom Permutations|pseudorandom permutation]].

##### Electronic Code Book (ECB) Mode
![[Pasted image 20230217192538.png|400]]

It's deterministic and is not CPA-secure or EAV-secure. Never use it in practice.

##### Cipher Block Chaining (CBC) Mode

![[Pasted image 20230217192734.png|400]]

**Theorem 3.32** If $F$ is a PRP, then CBC mode is CPA-secure. Even if a same IV is repeated, the consequence is not catastrophic.

![[Pasted image 20230217193048.png|500]]

Chained CBC mode is vulnerable to CPA. Imagine if the attacker choose $m_4 = c_3 \oplus m_1 \oplus IV$.

##### Output Feedback (OFB) Mode

![[Pasted image 20230217193518.png|400]]

Being a unsynchronized block cipher, OFB mode works quite similar to synchronized stream cipher. It is CPA-secure if $F$ is a PRF (not necessarily a PRP).

##### Counter (CTR) Mode

![[Pasted image 20230217194129.png|400]]

Similar to OFB, but can be paralleled.

**Theorem 3.33** If $F$ is a PRF, then CTR mode is CPA-secure.
