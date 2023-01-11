## 1 Computer Networks and the Internet

![[Pasted image 20230110170836.png|500]]

> A small portion of the Internet. Each node represents an IP address and edge represents the delay between nodes.

### 1.1 What is the Internet?

#### 1.1.1 A Nuts-and-Bolts Description

The Internet is the system of billions of **hosts** (aka end system e.g. PCs, smartphones) connected together by a network of **communication links** and **packet switches**, using the TCP/IP protocol suite. It is the infrastructure that enables communication between hosts.

End systems access the Internet through **Internet Service Providers (ISPs)**.

Internet standards are developed by the Internet Engineering Task Force (IETF), and the standards documents are called **requests for comments (RFCs)**.

#### 1.1.2 A Services Description

The Internet is an infrastructure that provides services to applications through the **socket** interface.

#### 1.1.3 Protocol

A protocol defines the format and the order of messages exchanged between communication entities, as well as the action taken on the transmission and receipt of a message.



### 1.2 The Network Edge

**Cable Internet access** is the most prevalent choice. Fiber optics connect the cable head end to neighborhood-level junctions, from which traditional coaxial cable is then used to reach individual houses. It is a *shared broadcast medium*, which means every packet sent by the head end travels to every home.

Cable Internet access requires a **modem** that connects to the home PC through an Ethernet port. 



### 1.3 The Network Core

#### 1.3.1 Packet Switching

Mainly routers and link-layer switches.

#### 1.3.3 A Network of Networks

The end devices can access the Internet through an ISP, and the access ISPs themselves must be interconnected. This is done by creating *a network of networks*, but how? We approximate the real Internet step by step.

1. Interconnect all of the access ISPs with a single global transit ISP.
2. Many interconnected global tier-1 ISPs with regional ISPs connected to one of them.
3. Multi-tier hierarchy, e.g. in China city ISP -> provincial ISP -> national ISP.
4. **PoP**: a group of routers. **Multi-home**: ISP connects to multiple providers ISPs. **IXP**: Internet exchange point, a meeting point where multiple ISPs can **peer** (connect together).
5. **Content-provider networks**: e.g. the google data centers are separate from the public Internet. It directly peers with lower-tier ISPs.

![[Pasted image 20230109210922.png]]



### 1.4 Delay, Loss and Throughput

#### 1.4.1 Delay

**Processing delay** is the time required to examine the packet's header and determine where to direct.

**Queuing delay** is the time a packet stays in the queue before being processed.

**Transmission delay** is the time required to receive the full packet.

**Propagation delay** is the time that a packet travels from a router to another.



### 1.5 Protocol Layers and Their Service Models

#### 1.5.1 Layered Architecture

Each protocol belongs to one of the layers. We are interested in the services that a layer offers to the layer above, called **service model**. A protocol layer can be implemented in software, in hardware, or both. When taken together, the protocols of the various layers are called the **protocol stack**.

**Application layer** includes HTTP, SMTP, etc.

**Transport layer** includes TCP and UPD.

**Network layer** is responsible for moving a packet from one host to another.

**Link layer** moves a packet from one node to the next node in the route, e.g. Ethernet, WiFi.

**Physical layer** moves the individual bits within the frame from one node to the next.

![[Pasted image 20230109215008.png]]

#### 1.5.2 Encapsulation

Each layer is an encapsulation.



### 1.6 Networks Under Attack

#### Malware

**Virus** is malware that require some form of user interaction to infect the user's device, e.g. email link.

**Worm** is malware that can enter a device without any explicit user interaction.

#### DoS Attack

Vulnerability attack, bandwidth flooding, connection flooding.
