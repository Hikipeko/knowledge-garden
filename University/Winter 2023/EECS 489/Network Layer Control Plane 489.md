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
where $\min_v$ is taken over all of $x$'s neighbors. The solution to the Bellman-Ford equation provides the entries in node $x$'s forwarding table.




















