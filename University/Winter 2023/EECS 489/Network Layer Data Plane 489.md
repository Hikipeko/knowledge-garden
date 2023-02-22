### 4.1 Overview of Network Layer

Service: move packets from a sending host to a receiving host.

#### 4.1.1 Forwarding and Routing

**Forwarding**

When a packet arrives at a router's input link, it must move the packet to the appropriate output link. Implemented in hardware.

**Routing**

The network layer use routing algorithms to determine the route or path taken by packets as they flow from a sender to a receiver. Implemented in software.

**Forwarding table**

![[Pasted image 20230217210024.png|550]]

The value stored in the forwarding table entry for those values indicates the outgoing link interface at that router to which that the packet is to be forward.

**Control Plane: The Traditional Approach**

The routing algorithms determines the content of the routers' forwarding tables. The routers interact with each other to compute the forward tables.

**Control Plane: The SDN Approach**

Remote controller computes and distributes the forwarding tables to be used by each physical routers. Routers perform forwarding only.

#### 4.1.2 Network Service Model

The network service model defines the characteristics of end-to-end delivery of packets between sending and receiving hosts.

**Possible services**

Guaranteed delivery, guaranteed delivery with bounded delay, in-order packet delivery, guaranteed minimal bandwidth, security are not provided by IP. IP only provides best-effort service.

> forwarding = switching
> packet switch = link-layer switches (layer 2) + routers (layer 3)



### 4.2 What's Inside a Router

![[Pasted image 20230217212759.png]]

* **Input ports** perform physical layer termination, link layer interoperation, and network layer decision of output port. Control packets are forwarded to the routing processor.
* **Switching fabric** connects the router's input ports to its output ports.
* **Output ports** perform physical layer and link layer function to transmit the packets to the next router.
* **Routing processor** performs control-plane functions.

Input ports, output ports, and switching fabric are almost always implemented in hardware for efficiency reasons. On the other hand, the control plane functions doesn't require extremally-high efficiency, and is usually implemented in software and executed on the routing processor.

#### 4.2.1 Input Port Processing and Destination-Based Forwarding

![[Pasted image 20230217214915.png]]

The lookup table is either set by the routing processor or received from a remote SDN controller. The router matches a prefix of the packet's destination. It uses the **longest prefix matching rule**, that is, it finds the longest matching entry in the table for forwarding when there's more than one matches. Further, this "match plus action" abstraction is performed in many networked devices.

#### 4.2.2 Switching Fabric

Switching can be accomplished in a number of ways.

![[Pasted image 20230217222622.png]]

* **Switching via memory**. The switching is controlled by processor. Some modern routers have processors on each line card to perform this lookup and storing of the packet into the appropriate memory location.
* **Switching via a bus**. The input port pre-pend a switch-internal label indicating the output port. At a single time, one packet is sent to all the output ports, and only the port that matches the label keep the packet.
* **Switching via an interconnection network**. Each vertical bus intersects each horizontal bus as a cross point, which can be opened or closed by the switch fabric controller. Blocking only happens if packets from multiple input ports goes to the same output port.

#### 4.2.3 Output Port Processing

Selecting (i.e., scheduling) and de-queueing packets for transmission.

#### 4.2.4 Where Does Queuing Occur

##### Input Queueing

Happens when the switching fabric transmission rate is lower than the packet arriving rate. **Head-of-the-line (HOL) blocking** can also happen when a packet is blocked by another packet at the head of the line.

![[Pasted image 20230217225032.png]]

##### Output Queueing

Happens when the fabric transmission rate is faster than the output sending rate. When the buffer is full or almost full, the router might:

* Drop a packet before full
* Setting the congestion bits

###### How Much Buffering Is Enough?

Research show that a good choice is $B= RTT \cdot C / \sqrt{N}$, where $RTT$ is the average round trip time, $C$ is the link capacity, and $N$ is the number of independent TCP flows. Note that larger buffer can lead to longer delay.

###### Bufferbloat

Imagine if it takes 20ms to transmit a packet inside the router, and RRT is 200ms. Say 25 packets are sent when t = 0. When the first ACK arrives at t = 200ms, there're still 5 packets in the queue, while the host start to send the 21st packet. This packet will have 300ms delay.

#### 4.2.5 Packet Scheduling

* FIFO (First-in-First-Out)
* [[Process 482#Priority]]
* [[Process 482#Round Robin]]
* 

**Weighted Fair Queuing (WFQ)** is a generalized form of round robin. It is a work-conserving queuing discipline that the link won't be idle as long as there're packets waiting to be transmitted. in WFQ, each class is assigned with a weight, and different classes receive a differential amount of service.



### 4.3 The Internet Protocol (IP)

#### 4.3.1 IPv4 Datagram Format

![[Pasted image 20230220210029.png]]

* **Version number**: IP protocol version of the datagram.
* **Header length**: typically 20
* **Type of service**: e.g. indicate a datagram is real-time
* **Datagram length** in bytes
* **Identifier, flags, fragmentation offset**: used when a large IP datagram is broken into smaller ones
* **Time-to-live**: decremented by one each processed by a router, and 0 is dropped
* **Protocol**: indicates the transport layer protocol
* **Header checksum**: only for the IP header. Routers typically discard error datagrams.
* **Source & Destination IP address**
* **Options**: used rarely
* **Data**: the actual payload

#### 4.3.2 IPv4 Addressing

**Interface** is the boundary between the host (also the router) and the physical link, so that hosts and routers can send and receive IP datagrams. An IP address is associated with the interface rather than the machine.

IP address is often in dotted-decimal notation.

![[Pasted image 20230221130930.png]]

**Subnet** is the interconnection of one router interface with multiple host interfaces. For example, 223.1.1.0/24 is a subnet, where the /24 is the **subnet mask**, indicating that the leftmost 24 bits of the 32-bit quantity define the subnet address.

**Classless Interdomain Routing (CIDR)**

The Internet's address assignment strategy. It divides IP address a.b.c.d/x into two parts, where x indicates the number of bits in the first part, referred to as the **prefix**. The prefix is used for routing outside an organization, and the second part is used for devices within the organization.

##### Obtaining a Block of Addresses

* Get from an ISP
* An ISP can get from ICANN

##### Obtaining a Host Address

* The IP addresses of routers are typically configured by administrators.
* The **Dynamic Host Configuration Protocol (DHCP)** automatically assigns an IP address to a host.

![[Pasted image 20230221140152.png]]

Ideally each subnet will have a DHCP server. DHCP is a client-server protocol：

1. DHCP server discovery: the newly connected host look for a DHCP server by broadcasting.
2. DHCP server offers: the DHCP responds by broadcasting.
3. DHCP request: the client chooses from IP addresses offered by the server.
4. DNCP ACK.