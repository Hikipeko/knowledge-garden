# Introduction to Computer Security

[course link](https://eecs388.org/)

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

We choose $f$ from a family of $2^n$ functions. We say the function family is secure if no effective algorithm can tell the difference between a function chosen randomly from the RRF family and a true random function.

1. Let $f$ be a secure PRF know to everyone
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





