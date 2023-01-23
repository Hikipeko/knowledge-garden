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
