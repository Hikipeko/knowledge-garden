### 3.1 Introduction and Transport-Layer Services

Transport-layer protocols are implemented in the end systems, not in routers.

The transport layer converts the application-layer message into transport-layer packets, know as transport-layer **segments** by breaking the messages into smaller chunks and adding a transport-layer header. 

#### 3.1.1 Relation Between Transport and Network Layers

Transport-layer protocol provides logical communication between processes.

Network-layer protocol provides logical communication between hosts.

#### 3.1.2 Overview of the Transport Layer in the Internet

##### UDP

Unreliable and connectionless, lightweight. It only provides process-to-process data delivery and error checking.

##### TCP

**Connection-oriented service**: after exchanging transport-layer control information (handshaking), a TCP connection exists between the sockets.

Reliable (correct & order). Congestion control: prevent TCP connection from swamping the links and route with an excessive amount of traffic.

##### IP

The network-layer IP protocol is a best-effort delivery service which tries to transmit data as much as possible. It's unreliable.



### 3.2 Multiplexing and Demultiplexing

![[Pasted image 20230120233421.png]]

##### Demultiplexing

Deliver the data in a transport-layer segment (from the network layer) to the correct socket.

##### Multiplexing

Gather data chunks at source host from different sockets and encapsulate with header information.

Requirements: unique identifiers & each segment has a source port number field and a destination port number field.

##### Port

Port number ranges from 0 to 65535. We should avoid using **well-know port numbers** (0 to 1023) when developing new applications.

##### Connectionless M & D

M: The transport layer in Host A create a segment including the source port number & destination port number besides the original data.

D: The transport layer in Host B examines the destination port number and delivers the segment to its socket.

##### Connection-Oriented M & D

Arriving TCP segments with different source IP address or source port numbers will be directed to two different sockets. (contrast with UDP)

A TCP socket connection is identified by (Source IP, source port number, destination IP, destination port). The arriving segments match all four values will be demultiplexed to this socket.

![[Pasted image 20230120235341.png]]



### 3.3 UDP

Almost like the application is directly talking with IP.

**Reasons of choosing UDP**

1. Finer application-level control over what data is sent, and when.
2. No connection establishment, and thus less delay (DNS).
3. No connection state, no need for state information.
4. Smaller packet header overhead (8 vs 20).

HTTP3 is based on UDP.

#### 3.3.1 UDP Segment Structure

![[Pasted image 20230121010818.png]]

#### 3.3.2 UDP Checksum

The sender performs the 1s complement of the sum of all the 16-bit words in the segment. The receiver add all the 16-bit words and the checksum and check the sum to be 1111 1111 1111 1111.

Even if we know data is invalid, we have no way to recover.

##### End-End Principle

Functions placed at the lower levels may be redundant when compared to the cost of providing them at the higher level.



### 3.4 Principles of Reliable Data Transfer

![[Pasted image 20230121133707.png]]

#### 3.4.1 Building a Reliable Data Transfer Protocol

##### Reliable Data Transfer over a Perfectly Reliable Channel

The sender just make data into a packet and call `udt_send()`. The receiver just receives the packet, extract the data, and delivered to the socket.

##### Bits Errors

Use positive/negative acknowledgements to indicate whether the data has been received correctly.

**ARQ (Automatic Repeat reQuest) protocol**

1. Error detection, e.g. check sum
2. Receiver feedback: ACK/NAK
3. Retransmission

Problem: what if the ack packet is corrupted?

Solution: the sender resend the packet when it receives a garbled ACK.

Problem: how does receiver know the packet is a new packet or a retransmission?

Solution: Add **sequence number** field to the data packet (0/1).

![[Pasted image 20230121135841.png]]

![[Pasted image 20230121140554.png]]

##### A Lossy Channel with Bit Errors

The sender wait for some time for the ACK, otherwise resend the data packet. The time is chosen by the sender.

![[Pasted image 20230121144206.png]]

#### 3.4.2 Pipelined Reliable Data Transfer Protocols

Problem: the above implementation is slow! The **utilization** of the sender (the fraction of time the sender is actually busy sending bits into the channel) is  very low using stop-and-wait protocol. Solution: use the **pipelining** technique.

#### 3.4.3 Go-Back-N

In a **Go-Back-N protocol**, the sender is allowed to transmit multiple packets without waiting for an acknowledgment.

![[Pasted image 20230121151025.png]]

N is the window size, which is the maximum allowing number for not yet ACK packets. GBN is a **sliding-window protocol**.

![[Pasted image 20230123120021.png]]

If a timeout occurs, the sender resends all packets that have been sent but not acknowledged. The sender discards out-of-order packets. Uses **cumulative acknowledgement**. This state machine can be implemented be **event-based programming**.

Problem: a single packet error can cause GBN to retransmit a large number of packets.

#### 3.4.4 Selective Repeat (SR)

![[Pasted image 20230123123644.png]]

##### Sender's Action

* *Data received from above*: if the next sequence number is within the sender's window, then packetize and send the packet.
* *Timeout*: each packet has a timer (can be simplified), and we resent the packet during timeout.
* *ACK received*: mark the packet as received. Move the window if send base == sequence number.

##### Receiver's Action

* *Packet with sequence number within \[base, base+N-1\]*: buffer the page and send ACK (not matter duplicate or not). If base == sequence number, send data to socket and move the window.
* *Packet with sequence number within \[base-N, base -1\]*: send ACK.
* *Otherwise*: ignore the packet.

As sequence number may be reused, some care must be taken to guard against duplicate packets.



### 3.5 TCP

Processes must send some preliminary segments to establish the parameters of the ensuing data transfer. TCP provides **full-duplex services**. (双向传输)

#### 3.5.1 The TCP Connection

##### Three-way handshake

1. The client process informs the client transport layer that it wants to establish a connection to (serverName, serverPort).
2. The client first sends a special TCP segments; the server responds with a second TCP segments; the client finally responds again with a third special segment.

##### Sending data

**MSS** maximum segment size is the maximum amount of data can be placed in a segment.

**MTU** maximum transmission unit is largest link - layer frame. Typically MSS = MTU - 40.

1. TCP sender receives the data to the connection's send buffer.
2. TCP sender constantly grab chunks of data from the send buffer, add a TCP header to form **TCP segments**, and send to the network layer.
3. TCP receiver receives the segment on the receive buffer.

#### 3.5.2 TCP Segment Structure

![[Pasted image 20230124164911.png]]

**Header length field**

Indicate the length of the TCP header in 32-bit words (20 bytes if option field is empty).

**Options field**

Used when a sender and receiver negotiate the MSS.

**Flag field**

ACK: whether the value in the acknowledgement field is valid.

PSH: the receiver should pass the data to the upper layer immediately.

URG: data is marked as "urgent" by the sending-side.

##### Sequence Number and Acknowledgement Number

![[Pasted image 20230124191724.png]]

TCP views data as an unstructured, but ordered, stream of bytes.

The sequence number for a segment is the ==byte-stream number== of the first byte in the segment, not the number of segments. The first segment gets sequence number 0, the second segment gets 1000.

The ACK number is the sequence number of the next byte the receiver is expecting from sender. TCP is **cumulative acknowledgements** as it only acknowledges bytes up to the first missing byte in the stream.

The receiver have two choices for out-of-order segments:

1. Discard
2. Keep and wait for the missing bytes

##### Telnet

![[Pasted image 20230124192717.png]]

Application-layer protocol for remote login. The data is not encrypted, so SSH is more commonly used.

Each character typed by the user is sent to the remote host, sent back to the client, and displayed on the user's screen.

#### 3.5.3 Round-Trip Time Estimation and Timeout

TCP use timeout mechanism to recover from lost segments.

##### Estimating the Round-Trip Time (RTT)

**Sample RTT** is the amount of time between when the segment is send and when an acknowledge for it is received. It fluctuates from segment to segment.

**Estimated RTT** is a running average of sample RTT.
$$
\text{EstimatedRTT} = (1-\alpha) \cdot  \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT}
$$
The recommended value of $\alpha$ is 0.125.

**Dev RTT** is the estimation of variability of the RTT.
$$
\text{DevRTT} = (1-\beta) \cdot  \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}|
$$
The recommended value of $\beta$ is 0.25.

##### Setting and Managing the Retransmission Timeout Interval

**Timeout Interval**
$$
\text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}
$$

#### 3.5.4 Reliable Data Transfer

The recommended TCP timer management procedures use only a single retransmission timer for all the not yet acknowledged segments.

**Simplified TCP sender**

```c
while (true) {
	switch (event) {
	
		event: data received from app above
			if (timer not running)
				start timer
			pass segment to IP
			NextSeqNum += length(data)
			
		event: timer timeout
			retransmit not-yet-acknowledged segment with smallest sequence number
			start timer
			
		event: ACK received, with ACK value fo y
			if (y > SendBase) {
				sendBase = y
				cnt = 0
				if (exist currently not-yet-acknowledgeed segments) {
					start timer
				}
			} else {
				// a duplicate ACK
				if (++cnt == 3) {
					resend segment with sequence number y
				}
			}
	}
}
```

##### Doubling the Timeout Interval

During the timeout (TCP  retransmits) event, sets the next timeout interval to twice the previous value. The timeout interval is set according to Estimated RTT and Dev RTT in the other two situations.

##### Fast Retransmit

![[Pasted image 20230124195744.png]]

If one segment is lost, there will likely be many back-to-back duplicate ACKs.

If the TCP sender receives three duplicate ACKs for the same data, the TCP sender performs a **fast retransmit**, retransmitting the missing segment before the timer expires.

##### Go-Back-N or Selective Repeat?

TCP  is similar to GBN in the sense that it only maintain the smallest sequence number of a transmitted but unacknowledged byte (SendBase) and the sequence number of the next byte to be sent (NextSeqNum).

However, many TCP implementations will buffer correctly received but out-of-order segments.

#### 3.5.5 Flow Control

The host of TCP connection set aside a receive buffer for the connection. The receive buffer can overflow if the sender send data too quickly. TCP provides a **flow-control service** to eliminate the probability of this overflow. It tries to match the sending speed against the reading speed.

TCP sender maintains a **Receive window (rwnd)**.

How much free buffer space is available at the receiver.

`LastByteRead`: the number of the last byte in the data stream read from the buffer from the client.

`LastByteRcvd`: the number of the last byte in the data stream that has arrived from the network and has been place in the receive buffer.
$$
\begin{align*}
\text{LastByteRcvd} - \text{LastByteRead} \leq \text{RcvBuffer}\\
\text{rwnd} = \text{RcvBuffer} - (\text{LastByteRcvd} - \text{LastByteRead})
\end{align*}
$$
**Sender side**
$$
\text{LastByteSent} - \text{LastByteAcked} \leq \text{rwnd}
$$
Problem: when the buffer becomes full, the sender will stop to send message, and will not be informed when the buffer is cleared.

Solution: the receiver send segment with one data byte when the receive window is zero.

#### 3.5.6 TCP Connection Management

##### Establishment of TCP connection

![[Pasted image 20230125131043.png]]

1. The client send a TCP segment with **SYN** set to 1 and a randomly chosen **sequence number** as `client_isn`.
2. The server extracts the SYN segment, allocates the TCP buffers, and send a connection-granted segment (**SYNHACK segment**). SYN bit is set to 1, acknowledgment field is set to `client_isn + 1`, and sequence number is a randomly chosen value `server_isn`.
3. The client receives the SYNACK segment and allocate buffer for the connection. The client then sends another segment with `acknowledgment = server_isn + 1`, SYN set to 0. This segment may contain client data in the segment payload.

##### Closing a TCP connection

![[Pasted image 20230125131121.png]]

1. The client sends a special TCP segment with **FIN** bit set to 1.
2. The server sends an acknowledgment segment in return.
3. The server sends its own shutdown segment with FIN bit set to 1.
4. The client acknowledges the server's shutdown.

If a host receives a TCP segment whose port numbers or source IP address do not match with any of the ongoing sockets in the host, it sends a special reset segment to the source with **RST** bit set to 1. Use **Nmap** to scan the ports.

##### SYN Flood Attack

* Attack: send a large number of TCP SYN segments without completing the third handshake step
* Defense: use SYN cookies. 



### 3.6 Principles of Congestion Control

#### 3.6.1 The Causes and the Costs of Congestion

![[Pasted image 20230125135123.png]]

![[Pasted image 20230125135147.png]]

1. Large queuing delays are experienced as the packet-arrival rate nears the link capacity (3.44).
2. The packets will be dropped when arriving to an already-full buffer, and therefore the sender must perform retransmissions to compensate for dropped packets due to buffer overflow (b).
3. The sender may timeout prematurely and retransmit a packet that has been delayed in the queue but not yet lost (c).
4. When a packet is dropped along a path, the transmission capacity at upstream links to forward that packet is wasted.



#### 3.6.2 Approaches to Congestion Control

##### End-to-end congestion control

The network layer provides no explicit support to the transport layer for congestion-control purposes.

TCP segment loss is taken as indication of network congestion, and TCP decreases its window size accordingly.

##### Network-assisted congestion control

Routers provide explicit feedback to the sender/receiver regarding the congestion state of the network. Two ways of feedback:

1. Direct feedback: sent from a router to the sender.
2. A router updates a field in a packet flowing from sender to receiver to indicate congestion.



### 3.7 TCP Congestion Control

#### 3.7.1 Classic TCP Congestion Control

Have each sender limit the rate at which it sends traffic into its connection as a function of perceived network congestion.

**How to limit transmission rate**

TCP use a variable **congestion window (cwnd)** to impose a constraint on the rate at which a TCP sender can send traffic into the network.
$$
\text{LastByteSent} - \text{LastByteAcked} \leq \min(\text{cwnd}, \text{rwnd})
$$
The sending rate is roughly cwnd/RTT.

**TCP guiding principles**

1. A lost segment (Time out or receipt of three duplicate ACKs) implies congestion, and the TCP sender's rate should be decreases.
2. A acknowledged segment indicates that the network is fine, and the sender's rate can be increased.
3. Bandwidth probing: incrementally ask for more and more bandwidth until a loss event.

##### TCP Congestion-Control Algorithm

![[Pasted image 20230214213246.png]]

###### Slow Start

The value of cwnd is typically initialize to 1 MSS (e.g. 1460bytes), and increased by 1 MSS every time a transmitted segment is acknowledged. This leads to an exponential increase of the transmission rate.

###### Congestion Avoidance

TCP increases cwnd by a single MSS every RTT until a packet loss

###### Fast Recovery

The value of cwnd is increased by 1 MSS for every duplicate ACK received for the missing segment that caused TCP to enter the fast-recovery state. 

**TCP congestion control: retrospective**

Theoretical analyses showed that TCP's congestion-control algorithm serves as a distributed asynchronous-optimization algorithm that results in several important aspects of user and network performance being simultaneously optimized.

**TCP Cubic**

![[Pasted image 20230214221350.png]]

TCP Cubic uses a cubic function instead of linear for recovery, and has recently gained wide deployment.

**Macroscopic description of TCP throughput**

0.75 * W / RTT, W is the value of the window size when loss occurs.
$$
\text{throughput} \approx \frac{1.22 \cdot MSS}{RTT \cdot \sqrt L}
$$
**TCP over high-bandwidth paths**

#### 3.7.2 Network-Assisted

#####  Explicit Congestion Notification

At the network layers, two bits are used for ECN. One is set by the router to indicate congestion. Another bit is to inform the routers that the sender and receivers are ECN-capable. The DCCP transport layer protocol provides a congestion-control UDP-like services that utilizes ECN.

##### Delay-based Congestion Control

The uncongested throughput rate would be ${\sf cwnd}/RTT_{min}$, so if the actual sender-measure throughput is significantly less than this value, the sender will decrease its sending rate.

#### 3.7.3 Fairness

Assume 2 TCP connections pass through a bottleneck link with transmission rate R bps, the throughput of both connection will finally stabilize to R/2.

![[Pasted image 20230214223621.png]]

**Fairness and UCP**

From the perspective of TCP, the UDP are not fair -- they do not cooperate with the other connections nor adjust their transmission rates appropriately.

**Fairness and parallel TCP connections**

Fairness problem may still exist as a single app can use multiple parallel TCP connections.



### 3.8 Evolution of Transport-Layer Functionality

TCP is evolving constantly in the past few years, and there are so many variations. They are so different that their only common feature is that they use the same TCP segment formant.

##### QUIC: Quick UDP Internet Connections

![[Pasted image 20230214230517.png]]

An application-layer protocol designed to improve performance of transport-layer service for secure HTTP. It accounts for 8.7% of Internet traffic.

* Connection-oriented
* Secure: packets are encrypted
* Streams: allows several different application-level "streams" to be multiplexed through a single QUIC connection.
* Reliable
* Congestion control: based on TCP Reno



