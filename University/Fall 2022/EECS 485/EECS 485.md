# Introduction to Web Systems

1. [[Web Development 485]]
2. [[Distributed Web 485]]
3. [[Web Development Projects 485]]

## L13 OS and Parallelism

**Motivation**

It's slow to handle a single request at a time.

**Processes**

A process is a program in execution.

PID, address space, some threads

**Threads**

Multiple functions in one process run separately.

Separate stack, shared address space (code, global variables).

**Synchronization**

P: Some interleaving  may produce incorrect results.

S: Locks

**Atomic operations**

Python dictionaries, lists are thread-safe.



## L14 Networking

**Motivation**

Distributed systems need a way to communicate.

**IP**

Get a message from one computer to another.

Routers (mostly Linux computers) connect all the computers.

Send packets

**TCP**

Use a sequence number to reassemble packets in order.

Use ACK for missing packets, resend packets without ACK.

P: Fast sender overflows a slow receiver.

S: Receiver tells the sender how many empty buffers are there.

P: Many senders overflow a network router.

S: Decrease congestion window when sender loses a packet.

TCP window: min(congestion_window, receiver_window)

**UDP**

P: TCP can be slow sometimes.

Used in video chat, heartbeat msg, etc.

**Sockets**

OS function (API) that allows applications to use TCP/UDP.

P: Two programs use the network.

S: Select a unique port number.



## L15 Text Analysis for Web Search

**Web search**

Example of Information Retrieval

Query: input

Hits: output

Rank: order of all the outputs

Document: one web page

Collection: all documents

Term: one word in a document

**Boolean retrieval**

Use a term-document incidence table

**Vector space model**

Represent documents as normalized vectors, the dimension is the number of terms.

Rank by sim(doc, query)

**tf-idf**

Term frequency: \#times of occurance

Document frequency: how often a word appear in documents.

$$IDF = N / n_k$$

$$W_{ik} = tf_{ik} \times \log (N/n_k)$$

$$w_{ik} = \frac{W_{ik}}{n_i}$$

#### Assessing rank quality

**Precision-recall curve**

Change of precision and recall when \#return results increases

**Kendall's Tau**

tau = (#pairs_that_agree - \#pairs_that_disagree) / \#total_pairs

**Mean Reciprocal Rank**

How close to the top of the search result is the 1st correct answer?
$$
MRR = \frac{1}{|Q|} \sum_{i = 1}^{|Q|}\frac{1}{rank_i}
$$



## L16 Link Analysis for Web Search

**Document importance**

If people link to it, then it's important.

Web as a directed graph.

**Page Rank algorithm**
$$
PR(A) = \frac{1-d}{N} + d\sum_i \frac{PR(I_i)}{C(i_i)}
$$
$$d$$ is the damping factor.

P: A sink might drain the rank.

S: Add edge from sink to every node.

Score(q, doc) = w * sim(q, doc) + (1-3) * PR(doc)

**HITS algorithm**
$$
auth(p) = \sum_{i = 1}^n hub(i)\\
hub(p) = \sum_{i=1}^n auth(i)
$$
Dependent on the query, need to compute at runtime.

**Search engine optimization**

Improve tf-idf: pages focus on a particular topic



## L17 Scaling Web Search

**Crawlers**

P: How to download the web?

S: Traversal on the link graph

**Deduplication**

Bag of words + shingles (sequence of k words)
$$
J(A, B) = \frac{|A\cap B|}{|A \cup B|}
$$
Compare of two documents takes O(k) time.

**Inverted index construction**

Maps words to docs that contain those words.

**Distributed search architecture**

P: inverted index is too big for one machine

S: Parallel query processing

Segment by document (docid % num_segments) vs. segment by term.



## L18 Scaling Static Pages

**Cloud**

P: How do we scale our web app to millions of users?

S: Rent servers (IaaS)

PaaS: Rent a computer with software installed and maintained.

SaaS: Rent a web app built, hosted, and maintained by others.

**Scaling static pages intro**

Rent many servers in different geographic locations.

**DNS**

Translate domain name to IP addresses

 <img src="./image/17.jpg" style="zoom:30%;" />

**DNS cache poisoning**

Bad info is inserted into a DNS server and cached

**CNDs**

A content delivery networks stores static files at many locations and servers nearby clients.

Website rewrite their URLs to use CDN URSs.

PaaS

 

## L19 Scaling Dynamic Pages

**Multi-process, multi-thread, and asynchronous**

Asynchronous programming + multiple threads + multicore CPU + multiple servers

**Round robin DNS**

There are many servers, use round robin to decide which server to direct the user to.

Load balancer (proxy server) multiple

**Hardware virtualization**

P: program have different environment requirements, and should be isolated, and might need to scale.

S: use virtual machines (hypervisor)

**Containerization**
P: VMs have large memory overhad

S: Use containers that share OS, with its binaries and libraries

 <img src="./image/19.jpg" style="zoom:30%;" />

Stateless, separate storage and computation.

**SOA**

Service-oriented architecture break up web app into services that communicate over a network.



## L20 Scaling Storage

**Distributed network database**

Dynamic pages servers run in containers. AKA Front end server.

Use a distributed DBMS.

**Sharding by content**

Different rows or tables stored on different DB servers.

CP

**Database replication**

A write can access any copy and must be synchronized to other copies.

AP

**CAP Theorem**

Consistency, availability, partition-tolerance, we can only get 2 out of 3.

**Practical database examples**

Relational DB such as PostgreSQL prioritize consistency over availability.

NoSQL such as MongoDB, prioritize availability over consistency.

You can use both to store different information.

**Distributed file system for media uploads**

P: They are large

S: Use a Network (bad, cannot scale) / Distributed (scalable & fault tolerant) file system



## L21 Recommender Systems

**Introduction**

P: Data is inadequate

**User-bases collaborative filtering**

S: find people similar to you, and recommend what they like.

Fin the k nearest neighbors and select the most frequent score.

Weakness: cold start, scalability, sparsity

**Content-bases filtering**

Recommend items similar to what you like

Hybrid filters: combine them together.

**Historical examples**



## L22 Ads and Auctions

#### Auction types and terminology

**Open auction**

Everybody sees the bids

English: open, ascending price

Dutch: open, descending price

**Sealed-bid auction**

Bids are secret

First-price -> not good

Second-price AKA Vickrey

Bidding your true value is always best

**Auction mathematical analysis**

Bidder with highest value wins, pays 2nd-highest value.

Proxy bidding: ebay

Sniping turns it into a Vickrey Auction

**Auctions for selling ads**

If the user clicks the ad, high bidder pays 2nd-price.



## L23 Blockchain

Blockchain is a distributed store of information with no central authority.

**Currency**

Fiat currency is a currency without intrinsic value.

Central bank, e.g. the Federal Reserve, is the bank used by banks.

**Bitcoin**

A decentralized digital fiat currency without a government behind it

A Bitcoin is an information object that represents value, represented as a chain of digital signatures over the transactions that the Bitcoin participated in.

Owner is represented by a public key (AKA Bitcoin Wallet).

Pseudo anonymous as everyone can see every Bitcoin transaction.

**Blockchain**

Collectively build a log of every Bitcoin transaction to prevent double spending

A distributed database with a shared protocol for multiple writers who don't trust each other.

Useful when:

1. Database is shared with multiple writers
2. Users don't trust each other
3. Users don't trust a central authority

**Mining**

Bitcoin miners runs the distributed ledger.

**Altcoins**



## L24 Dark Web

Part of the web accessible only through an anonymous connection (Tor).

What if a router is being listened in?

They know who (source IP) is sending to who (destination IP).

**VPN proxies**

Hide your IP by making you internet traffic appear to come from somewhere else.

Vulnerability: single point of failure, traffic analysis.

**Tor**

The Onion Router can invalidate traffic analysis.

Select a random path for network connection.

Tor encrypts outgoing data multiple times.

**Tor services**

Tor allows users to anonymously publish services.

**Tor browser**

Avoid browser fingerprinting.

**Tor history**







## P5 Search-Engine

If your Map program is more complicated than your Reduce program, consider reorganizing. Transform data in Map, compute in Reduce.
