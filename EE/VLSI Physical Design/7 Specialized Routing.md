Some types of design, e.g. analog circuits and printed circuit boards cannot warrant the separation into global and detailed routing.

### 7.1 Area Routing

#### 7.1.1 Introduction

**Goals** is to route all the nets in the design

* without global routing
* within the given layout space
* meeting all geometric and electrical design rules
* minimize total length and number of vias
* minimize total area of wiring and number of routing layers
* minimize circuit delay
* avoid harmful capacitive coupling

#### 7.1.2 Net Ordering

* **MBB** is the minimum bounding box
* **AR** is the aspect ratio, $\max(W/L, L/W)$
* **Rule 1** Route large AR first.
* **Rule 2** For two nets, the one whose pins are contained in another nets' MBB is routed first.
* **Rule 3** Route based on the number of pins inside MBB, from small to large.

![[Pasted image 20231023160501.png|500]]
![[Pasted image 20231023160514.png|500]]

### 7.2 Non-Manhattan Routing

Octilinear Steiner Tree Algorithm generalize rectilinear Steiner trees by allowing segments that extend in eight directions.

### 7.3 Clock Routing

Once the clock signal entry points and sinks are known, **clock tree routing** generates a clock tree for each clock domain of the circuit.

