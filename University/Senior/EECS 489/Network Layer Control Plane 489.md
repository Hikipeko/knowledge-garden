### 5.1 Introduction

* **Per-router control**: Forwarding and routing function are contained by each router. Routers communicate with each other to compute the routing table.
* **Logically centralized control**: A centralized controller interacts with a control agent (CA) in routers and mange their routing table. This approach is becoming more and more prevalent (>75%)



### 5.2 Routing Algorithms

Goal: determine good paths from sender to receiver. We use undirected graph $G = (N, E)$ to formulate routing problem, denote $c(x,y)$ as the cost of passing an edge. We want to find a path having least cost.

* **Centralized routing algorithm**: compute using complete, global knowledge about the network, also referred to as link-state algorithms.
* **Decentralized routing algorithms**: calculation is carried out in an iterative, distributed manner by the routers. More suitable for per-router control.

#### 5.2.1 The Link-State (RS) Routing Algorithm

First, all the routers broadcast the cost information through a **link-state broadcast**. Then each router runs [[Graphs 281#Dijkstra's Algorithm]].

Problem is that the cost function is dynamic, and may cause path oscillations. A possible solution is to ensure that not all routers run RS at the same time.

#### 5.2.2 The Distance-Vector (DV) Routing Algorithm

DV is iterative, asynchronous, and distributed. Each node exchange information with its neighbors. It is based on Bellman-Ford equation 
$$d_x(y) = \min_v{c(x,v) + d_v(y)},$$
where $\min_v$ is taken over all of $x$'s neighbors. The solution to the Bellman-Ford equation provides the entries in node $x$'s forwarding table. Suppose $v'$ is $x$'s best neighbor when sending to $y$. After sending packets to all its neighbors, $x$ with finally set $v'$ as next-hop router for destination $y$.

Each node $x$ maintains the following routing information:

* For each neighbor $v$, the cost $c(x,v)$.
* Its distance vector $D_x = [D_x(y): y \in N]$ containing $x$'s estimation of its cost to all destinations.
* The distance vector of each of its neighbors $D_v$.

Each node sends a copy of its distance vector to its neighbors. On receiving a new distance vector from a neighbor, the node updates its own distance vector as follows:
$$
D_x(y) = \min_v\{c(c,v) + D_v(y)\} \quad \text{for each node }y\in N
$$
If the distance vector is changed during the update, it will send the updated distance vector to each of its neighbors. Finally, $D_x(y)$ will converge to $d_x(y)$. The loop will end until a link cost changes.

![[Pasted image 20230322113536.png|550]]

##### Link-Cost Changes and Link Failure

When a node detects a change in the link cost, it updates its distance vector. If there's a change in the cost of the least-cost path, it informs its neighbors.

**Routing loop**

![[Pasted image 20230322123310.png|500]]

In the second scenario, when $c(x,y)$ changes from 4 to 60, suppose $y$ detect this change, it will update $D_y(x) = c(y,z) + D_z(x) = 5$, which leads to a route $y \to z \to y \to x$. Node $z$ will do the similar calculation. Finally a **routing loop** forms between $y$ and $z$. This is also referred to as the count-to-infinity problem.

##### Adding Poisoned Reverse

Simple solution: if $z$ routes through $y$ to get to destination $x$, then $z$ will advertise to $y$ that $D_z(x) = \infty$.

1. $y$ detects the cost change, updates $D_y(x) = 60$, and propagates to $z$.
2. $z$ updates $D_z(x)=50$ using the direct link , and propagates to $y$.
3. $y$ updates $D_y(x) = 51$.

However, loops involving three or more nodes will not detected by the poisoned reverse technique. Besides, DV algorithm is not so robust. A single malfunctioning router might be flooded with traffic and cause severe problems



### 5.3 Intra-AS Routing in the Internet: OSPF

The above algorithms have two practical problems:

1. **Scale**: the larger the scale, the larger the communication overhead.
2. **Administrative autonomy**: hard to enforce a standard algorithm in all routers.

Solution: organize routers into **autonomous systems (ASs)**. Each AS consists of group of routers that are under the same administrative control, running the same algorithm. E.g. routers inside an ISP. An AS is identified by autonomous system number (ASN). The routing algorithm running within an AS is called an *intra-autonomous system routing protocol*.

##### Open Shortest path First (OSPF)


























