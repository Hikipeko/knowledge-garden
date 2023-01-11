## 3.1 Introduction and Transport-Layer Services

Transport-layer protocols are implemented in the end systems.

On the sending side, the transport layer converts the application-layer message into transport-layer packets, know as transport-layer **segments** by breaking the messages into smaller chunks and adding a transport-layer header. 

### 3.1.1 Relation Between Transport and Network Layers

 <img src="./image/3.1.1.PNG" style="zoom:60%;" />

Transport-layer protocol provides logical communication between processes.

Network-layer protocol provides logical communication between hosts.

### 3.1.2 Overview of the Transport Layer in the Internet

**UDP**

Unreliable, connectionless.

Services: process-to-process data delivery & error checking.

**TCP**

Reliable (correct & order), connection-oriented.

Congestion control: prevent TCP connection from swamping the links and route with an excessive amount of traffic.

**IP**

The network-layer protocol, IP, is a best-effort delivery service.

Unreliable.

## 3.2 Multiplexing and Demultiplexing

Extend the host-to-host delivery service to process-to-process delivery.

**Demultiplexing**

Deliver the data in a transport-layer segment to the correct socket.

**Multiplexing**

Gather data chunks at source host from different sockets and encapsulate with header information.

Requirements: unique identifiers & each segment have fields indicating the socket to be delivered

Source port number field & Destination port number field.

**Port**

Range from 0 to 65535 (0 to 1023 are **well-know port numbers**).

### 3.2.1 Connectionless M & D

M: The transport layer in Host A create a segment including the source port number & destination port number besides the original data.

D: The transport layer in Host B examines the destination port number and delivers the segment to its socket.

### 3.2.2 Connection-Oriented M & D

Arriving TCP segments with different source IP address or source port numbers will be directed to two different sockets. (contrast with UDP)

The newly created connection socket is identified by:

1. Source port umber.
2. IP address of source host.
3. Destination port number.
4. Its own IP address.

The arriving segments match all four values will be demultiplexed to this socket.

## 3.3 UDP

**Reasons of choosing UDP**

1. Finer application-level control over what data is sent, and when.
2. No connection establishment, no delay.
3. No connection state, no need for state information.
4. Smaller packet header overhead.

### 3.3.1 UDP Segment Structure

 <img src="./image/3.3.1.PNG" style="zoom:65%;" />

### 3.3.2 UDP Checksum

The sender performs the 1s complement of the sum of al the 16-bit words in the segment.

The receiver add all the 16-bit words and the checksum and check the sum to be 1111 1111 1111 1111.

No way to recover.

## 3.4 Principles of Reliable Data Transfer

 <img src="./image/3.4.PNG" style="zoom:70%;" />

### 3.4.1 Building a Reliable Data Transfer Protocol

**Bits in a packet may be corrupted**

Use positive/negative acknowledgements to indicate whether the data has been received correctly.

**ARQ (Automatic Repeat reQuest) protocol**

1. Error detection.
2. Receiver feedback. ACK/NAK
3. Retransmission.

Problem: what if the ack packet is corrupted?

Solution: the sender resend the packet when it receives a garbled ACK.

Problem: how does receiver know the packet is a new packet or a retransmission?

Solution: Add **sequence number** field to the data packet (0/1).

Problem: the underlying channel can lose packets.

The sender wait for some time for the ACK, otherwise resend the data packet.

The time is chosen by the sender.

 <img src="./image/3.4.1.PNG" style="zoom:80%;" />

 <img src="./image/3.4.1.1.PNG" style="zoom:80%;" />

### 3.4.2 Pipelined Reliable Data Transfer Protocols

The **utilization** of the sender (the fraction of time the sender is actually busy sending bits into the channel) is  very low using stop-and-wait protocol.

 <img src="./image/3.4.2.PNG" style="zoom:80%;" />

### 3.4.3 Go-Back_N

In a **Go-Back-N protocol**, the sender is allowed to transmit  multiple packets without waiting for an acknowledgment.

 <img src="./image/3.4.3.PNG" style="zoom:50%;" />

N: window size.

A sliding-window protocol.

 <img src="./image/3.4.3.2.PNG" style="zoom:60%;" />

 <img src="./image/3.4.3.3.PNG" style="zoom:60%;" />

If a timeout occurs, the sender resends all packets that have been sent but not acknowledged.

The sender discards out-of-order packets.

Problem: a single packet error can cause GBN to retransmit a large number of packets.

### 3.4.4 Selective Repeat (SR)

 <img src="./image/3.4.4.PNG" style="zoom:60%;" />

 <img src="./image/3.4.4.1.PNG" style="zoom:50%;" />

 <img src="./image/3.4.4.2.PNG" style="zoom:50%;" />

As sequence number may be reused, some care must be taken to guard against duplicate packets.

## 3.5 TCP

### 3.5.1 The TCP Connection

Processes must send some preliminary segments to establish the parameters of the ensuing data transfer.

TCP provides **full-duplex services**. (双向传输)

**Establish a connection**

Three-way handshake

1. The client process informs the client transport layer that it wants to establish a connection to (serverName, serverPort).
2. The client first sends a special TCP segments; the server responds with a second TCP segments; the client finally responds again with a third special segment.

**Sending data**

**MSS** 

Maximum segment size: maximum amount of data can be placed in a segment.

**MTU** 

Maximum transmission unit: largest link-layer frame.

1. TCP sender receives the data to the connection's send buffer.
2. TCP sender constantly grab chunks of data from the send buffer, add a TCP header to form **TCP segments**, and send to the network layer.
3. TCP receiver receives the segment on the receive buffer.

 <img src="./image/3.5.1.PNG" style="zoom:50%;" />

### 3.5.2 TCP Segment Structure

 <img src="./image/3.5.2.PNG" style="zoom:50%;" />

**Header length field**

Indicate the length of the TCP header in 32-bit words (20 bytes if option field is empty).

**Options field**

Used when a sender and receiver negotiate the MSS.

**Flag field**

ACK: whether the value in the acknowledgement field is valid.

PSH: the receiver should pass the data to the upper layer immediately.

URG: data is marked as "urgent" by the sending-side.

**Sequence Number**

The sequence number for a segment is the byte-stream number of the first byte in the segment.

 <img src="./image/3.5.2.1.PNG" style="zoom:60%;" />

The first segment gets sequence number 0, the second segment gets 1000.

**Acknowledgement Number**

The sequence number of the next byte the receiver is expecting from sender.

TCP is **cumulative acknowledgements** as it only acknowledges bytes up to the first missing byte in the stream.

The receiver have two choices for out-of-order segments:

1. Discard.
2. Keep and wait for the missing bytes.

#### Telnet

Port: 23.

Application-layer protocol for remote login.

Not encrypted.

Each character typed by the user is sent to the remote host, sent back to the client, and displayed on the user's screen.

 <img src="./image/3.5.2.2.PNG" style="zoom:60%;" />

### 3.5.3 Round-Trip Time Estimation and Timeout

TCP use timeout mechanism to recover from lost segments.

#### Estimating the Round-Trip Time (RTT)

**SampleRTT**

The amount of time between when the segment is send and when an acknowledge for it is received.

Fluctuate from segment to segment.

**EstimatedRTT**
$$
\text{EstimatedRTT} = (1-\alpha) \cdot  \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT}
$$
The recommended value of $\alpha$ is 0.125.

**DevRTT**

Estimate of variability of the RTT.
$$
\text{DevRTT} = (1-\beta) \cdot  \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}|
$$
The recommended value of $\beta$ is 0.25.

**Timeout Interval**
$$
\text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}
$$

### 3.5.4 Reliable Data Transfer

The recommended TCP timer management procedures use only a single retransmission timer for all the not yet acknowledged segments.

**Simplified TCP sender**

```pseudocode
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

**Doubling the Timeout Interval**

Each time TCP  retransmits, it sets the next timeout interval to twice the previous value.

The timeout interval is set according to EstimatedRTT and DevRTT in the other two situations.

#### Fast Retransmit

 <img src="./image/3.5.4.PNG" style="zoom:60%;" />

If one segment is lost, there will likely be many back-to-back duplicate ACKs.

If the TCP sender receives three duplicate ACKs for the same data, the TCP sender performs a **fast retransmit**, retransmitting the missing segment before the timer expires.

#### Go-Back-N or Selective Repeat?

TCP  is similar to GBN in the sense that it only maintain the smallest sequence number of a transmitted but unacknowledged byte (SendBase) and the sequence number of the next byte to be sent (NextSeqNum).

However, many TCP implementations will buffer correctly received but out-of-order segments.

### 3.5.5 Flow Control

The host of TCP connection set aside a receive buffer for the connection.

The receive buffer can overflow if the sender send data too quickly.

TCP provides a **flow-control service** to solve overflow problem.

Match the sending speed against the reading speed.

**Receive window (rwnd)**

How much free buffer space is available at the receiver.

LastByteRead: the number of the last byte in the data stream read from the buffer from the client.

LastByteRcvd: the number of the last byte in the data stream that has arrived from the network and has been place in the receive buffer.
$$
\text{LastByteRcvd} - \text{LastByteRead} \leq \text{RcvBuffer}\\
\text{rwnd} = \text{RcvBuffer} - (\text{LastByteRcvd} - \text{LastByteRead})
$$
**Sender side**
$$
\text{LastByteSent} - \text{LastByteAcked} \leq \text{rwnd}
$$
Problem: when the buffer becomes full, the sender will stop to send message, and will not be informed when the buffer is cleared.

Solution: the receiver send segment with one data byte when the receive window is zero.

### 3.5.6 TCP Connection Management

**Establishment of TCP connection**

1. The client send a TCP segment with **SYN** set to 1 and a randomly chosen **sequence number** as client_isn.
2. The server extracts the SYN segment, allocates the TCP buffers, and send a connection-granted segment (**SYNHACK segment**) with SYN bit set to 1, acknowledgment field set to client_isn + 1, sequence number to be a randomly chosen value server_isn.
3. The client receives the SYNACK segment and allocate buffer for the connection. Then client then send another segment with acknowledgment field to be server_isn + 1, SYN set to 0. This segment may contain client data.

 <img src="./image/3.5.6.PNG" style="zoom:60%;" />

**Closing a TCP connection**

1. The client sends a special TCP segment with **FIN** bit set to 1.
2. The server sends an acknowledgment segment in return.
3. The server sends its own shutdown segment with FIN bit set to 1.
4. The client acknowledges the server's shutdown.

 <img src="./image/3.5.6.1.PNG" style="zoom:60%;" />

If a host receives a TCP segment whose port numbers or source IP address do not match with any of the ongoing sockets in the host, it sends a special reset segment to the source with **RST** bit set to 1.



## 3.6 Principles of Congestion Control

### 3.6.1 The Causes and the Costs of Congestion

1. Large queuing delays are experienced as the packet-arrival rate nears the link capacity.
2. The packets will be dropped when arriving to an already-full buffer, and therefore the sender must perform retransmissions to compensate for dropped packets due to buffer overflow. The sender may time out prematurely and retransmit a packet that has been delayed in the queue but not yet lost.
3. When a packet is dropped along a path, the transmission capacity at upstream links to forward that packet is wasted.

 

### 3.6.2 Approaches to Congestion Control

**End-to-end congestion control** (default)

The network layer provides no explicit support to the transport layer for congestion-control purposes.

TCP segment loss is taken as indication of network congestion, and TCP decreases its window size accordingly.

**Network-assisted congestion control**

Routers provide explicit feedback to the sender/receiver regarding the congestion state of the network.

Two ways of feedback:

1. Direct feedback: sent from a router to the sender.
2. A router updates a field in a packet flowing from sender to receiver to indicate congestion.



## 3.7 TCP Congestion Control

Have each sender limit the rate at which it sends traffic into its connection as a function of perceived network congestion.

**How to limit transmission rate**

TCP use a variable **congestion window (cwnd)** to impose a constraint on the rate at which a TCP sender can send traffic into the network.
$$
\text{LastByteSent} - \text{LastByteAcked} \leq \min(\text{cwnd}, \text{rwnd})
$$
The sending rate is roughly cwnd/RTT.

**Detection**

Receipt of three duplicate ACKs.

**Adjust transmission rate**

TCP takes the arrival of in-order acknowledgments as an indication of low traffic.

It increases cwnd according to the arriving rate of acknowledgments.

**TCP guiding principles**

1. A lost segment implies congestion, and the TCP sender's rate should be decreases.
2. A acknowledged segment indicates that the network is fine, and the sender's rate can be increased.
3. Bandwidth probing: incrementally ask for more and more bandwidth.

#### TCP congestion-control algorithm

**Slow Start**

The value of cwnd is typically initialize to 1 MSS (e.g. 500 bytes), and increased by 1 MSS every time a transmitted segment is acknowledged.

 <img src="./image/3.7.1.PNG" style="zoom:60%;" />

**Cases of ending**

1. Loss event happens (indicated by a timeout). cwnd is set to 1, **ssthresh (slow start threshold)** is set to cwnd/2.
2. Directly tied to the value of ssthresh.
3. Three duplicate ACKs are detected.

**Congestion Avoidance**

TCP increases the value to cwnd by a single MSS ever RTT.

Stop when:

1. Timeout: enter slow start state.
2. Three duplicate ACKs: half the cwnd, update ssthresh as half of ssthresh, enter fast recovery.

**Fast Recovery**

The value of cwnd is increased by 1 MSS for every duplicate ACK received for the missing segment that caused TCP to enter the fast-recovery state. 

 <img src="./image/3.7.2.PNG" style="zoom:80%;" />

**TCP congestion control: retrospective**

Theoretical analyses showed that TCP's congestion-control algorithm serves as a distributed asynchronous-optimization algorithm that results in several important aspects of user and network performance being simultaneously optimized.

**Macroscopic description of TCP throughput**

0.75 * W / RTT, W is the value of the window size when loss occurs.
$$
\text{throughput} \approx \frac{1.22 \cdot MSS}{RTT \cdot \sqrt L}
$$
**TCP over high-bandwidth paths**

### 3.7.1 Fairness

Assume 2 TCP connections pass through a bottleneck link with transmission rate R bps, the throughput of both connection will finally stabilize to R/2.

 <img src="./image/3.7.1.1.PNG" style="zoom:70%;" />

**Fairness and UCP**

From the perspective of TCP, the UDP are not fair -- they do not cooperate with the other connections nor adjust their transmission rates appropriately.

**Fairness and parallel TCP connections**

Fairness problem may still exist as a single app can use multiple parallel TCP connections.

### 3.7.2 Explicit Congestion Notification (ECN)

Network-assisted congestion control.

The Service field of the IP datagram header are used for ECN. 

One bit is used by a router to indicate congestion.

Another bit is set by the host to indicate the sender and receiver are ECN-capable.
