# 4 The Network Layer: Control Plane

## 4.1 Overview of Network Layer

### 4.1.1 Forwarding and Routing

Role: move packets from a sending host to a receiving host.

**Forwarding**

When a packet arrives at a router's input link, it must move the packet to the appropriate output link.

Implemented in hardware.

**Routing**

The network layer use routing algorithms to determine the route or path taken by packets as they flow from a sender to a receiver.

Implemented in software.

**Forwarding table**

The value stored in the forwarding table entry for those values indicates the outgoing link interface at that router to which that the packet is to be forward.

 <img src="./image/4.2.PNG" style="zoom:70%;" />

**Control Plane: The Traditional Approach**
The routers interact with each other to compute the forward tables.

**Control Plane: The SDN Approach**

Remote controller computes and distributes the forwarding tables to be used by each and every router.

 <img src="./image/4.3.PNG" style="zoom:70%;" />

### 4.1.2 Network Service Model

The network service model defines the characteristics of end-to-end delivery of packets between sending and receiving hosts.

**Possible services**

Guaranteed delivery, guaranteed delivery with bounded delay, in-order packet delivery, guaranteed minimal bandwidth, security.
