## L22 Censorship and Circumvention

#### Detect and Understand Censorship

How to collect data without deploying or ask people on the ground in censored region (too dangerous).

Use existing hosts as vantage points.

##### Spooky Scan

TCP handshake is SYN (IP_ID: X) + SYN-ACK (IP_ID: Y) + ACK (IP_ID: X+1). However, if we send SYN-ACK, the server will return RST. If we don't complete the third way hand shake, the server would send multiple AYN-ACKs (3 to 5).

![[Pasted image 20230415204705.png]]

![[Pasted image 20230415204828.png]]

#### Safeguard the Consumer VPN Ecosystem

Use VPNalyzer to analyze security of VPN.



## L24 Election cybersecurity

There is no realistic mechanism to fully secure vote casting and tabulation. On the other hand, no credible evidence has been put forth that supports a conclusion that the 2020 election outcome in any state has been altered.

##### Russian Attacks During the 2016 Election

* Targeted political leaks
* Trolling/message amplification
* Attacks on election infrastructure

##### Security Requirements

* **Integrity**: votes are cast as intended, votes are counted as cast (in tension with Ballot Secrecy)
* **Ballot Secrecy**: nobody can figure our how you voted even if you try to prove it to them
* **Voter Authentication**: only authorized voter can cast votes (in tension with enfranchisement)
* **Enfranchisement**: all authorized voters have the opportunity to vote
* **Availability**: the election system is able to produce results in a timely manner

##### Challenges

* Voting complexity: paper Ballot, mail, punch card, DREs with or without WPAT, etc
* Voting machines are vulnerable
* Have privacy vulnerability

**Risk-Limiting Audit**: Hand count enough paper ballots to ensure that if the reported outcome is wrong, then the audit has the high probability of detecting the discrepancy. (But most states won't look at the paper)

##### Internet Voting?

Never try doing this.

* Server-side threats: remote intrusion, insider attacks, supply-chain attacks, DoS
* Client-side threats: coercion, malware, credential theft, imposter sites

##### End-to-End Verifiable Voting

* My vote is cast as I intended
* My vote is counted as cast
* All votes are counted as cast
* No voter can demonstrate how he or she voted to a third party. (count the result without decrypting the data)

##### Defending U.S. Election

Paper Ballots + Post-Election Audits
