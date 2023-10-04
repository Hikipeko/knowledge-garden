### 12.1 Basics

* Logic-side tools must estimate delays through unplaced logic, *Static timing analysis*
* Layout tools must estimate delays through placed logic, *Interconnect delay analysis*

## Logic-Level Timing

### 12.2 Basic Assumptions & Models

* We want to know whether the netlist satisfy the timing requirement.
* Assume design is synchronous: longest delay through CL must be shorter than one clock cycle
* Pin-to-Pin Delay: each gate has a fixed delay.
* **Static Timing Analysis**: ignore logic, use only topological analysis

![[Pasted image 20231003200355.png|400]]

### 12.3 SAT Delay Graph, ATs, RATs, and Slacks

* Vertices are wires and edges are gates
* Add Source/Sink nodes
* Use additional nodes for interconnect delay
* For each node, we find the worst delay to the node along any path
* **Arrival Time** AT(n) = latest time the signal can become stable
	* The max delay from all predecessors
* **Required Arrival Time** RAT(n) = latest time the signal is allowed to become stable
	* The min delay from all successors
* **Slack** Slack(n) = RAT(n) - AT(n), amount of timing margin for the signal, negative is bad
* Can increase delay at node with positive slack, which saves power

![[Pasted image 20231003201308.png|250]]
![[Pasted image 20231003201415.png|500]]
![[Pasted image 20231003201624.png|400]]

### 12.5 Computing ATs, RATs, Slacks, and Worst Paths

##### Topological Sorting

```algorithm
L = Empty list contain the sorted elements
S = Set of all nodes with no incoming edges
while S is not empty do
	remove a node n from S
	insert n into L
	for each outgoing edge (n, m)
		remove edge (n, m)
		if m has no incoming edges then
			add m to S
if G has edge then
	return error (cyclic)
else 
	return L
```

##### Computing ATs

```algorithm
Order = Topological-Sort(V)
AT(src) = 0
for each (n in Order) do
	AT(n) = - infinity
	for each p in Pred(n) do
		AT(n) = max(AT(n), AT(p) + delta(p,n))
```

##### Using Slack for Path Reporting

* The computation for RATs and Slacks are then trivial
* We want to enumerate the N worst paths
* Store in the heap `<partial_path, delay_of_path, slack_of_final_node>`, indexed on Slack value
* Partial path with worst slack is always searched first
* Similar to [[11 Routing#Maze Routing]], expand until N worst paths are reported

## Interconnect Timing

### 12.6 Electrical Models of Wire Delay

* Model = Electrical Circuit: Delay comes from electrical loading
* Interconnect must be modeled and analyzed as a circuit
* Interconnect is large compared to devices
* Metal wires has capacitance to silicon substrate
* **Pi model** accounts for the resistance R and the capacitance C
* Replace every straight wire segment with pi model ad get a **RC Tree**

![[Pasted image 20231003204129.png|400]]
![[Pasted image 20231003204320.png|500]]
![[Pasted image 20231003204425.png|500]]
![[Pasted image 20231003204848.png|400]]

### 12.7 The Elmore Delay Model

* **Root** of tree is where signal is input, leaves are the driven outputs
* Voltage source + resistor as input at root
* At each resistor, do $\tau = tau + R \cdot \sum (\text{all downstream capacitors})$

![[Pasted image 20231003205642.png|600]]

### 12.8 Elmore Delay Examples

* For SAT: Can take a real routed wire and build a good delay model for STA.
* For placement: Get quick delay estimate for a wire shape