#### Why Distribute Systems?

**Performance**

Aggregate performance of many computers and can be faster than any single machine.

**Reliability**

Can provide continuous service, even if some computers fail.

A distributed system is a collection of distinct processes that:

1. are spatially separated
2. communicate with each other by exchanging messages
3. have non-negligible communication delay
4. do not share fate
5. have separate physical clocks (that cannot compare)

Use network to share data

**Microkernels**

OS structure that separates OS functionality into several server processes, each in its own address space.

#### Ordering Events in Distributed Systems

Events can be:

* local
* send
* receive

We use $\to$ to denote "happened-before" relation. A partial order.

##### Lamport clock

Define a function $LC$ such that:

$$
p \to q \Rightarrow LC(p) < LC(q)
$$

We can generate a total order, ties broken by unique process ID.

#### Client-Side Caching

P: if we use RPC each time for reading, the server might be overloaded.

S: cache it, the client creates additional copy.

P: there might be inconsistency

S: use states: invalid, shared, exclusive

##### State Machine for Cached Copy

![[Pasted image 20221224152345.png]]

TBA
