#### 9.1 Basics

* Front end synthesize high-level design description into connected gates + wires (netlist of gates and wires).
* Back end (optimally) turns these into IC masks (called **layout**) to build real chips.

##### Topics in Layout

* Technology Mapping: from synthesized logic to real gates
* Placement: arrange gates on surface of chips, optimally
* Routing: connect wires to all the gates on chip
* Timing Analysis: how fast is mapped/placed/routed logic?

##### Standard Cell Library

* A standard cell is a basic logic gat5e or small logic element.
* A set of allowed logic elements.
* A cell is kind of an abstraction.
* In order technology 130nm CMOS, all cells have same height, while cells can have different widths.
* In real designs, small cells dominate.
##### Placer

* Input: Netlist of gates and wires
* Output: Exact location of each gate
* Goal: able to **route** (connect) all wires, while minimizing expected wirelength.

#### 9.2 Wirelength Estimation

##### Simple Model

* All gates are exactly the same size.
* Each grid slot can hold 1 gate.
* Net aka. wire

##### Half-Perimeter Wirelength

![[Pasted image 20230818182154.png]]

* AKA Bounding Box (BBOX)
* Put smallest bounding box around all gates
* Add width and height of the BBOX
* Easy to calculate
* Always a lower bound
* Most nets are short

#### 9.3 Simple Iterative Improvement Placement

* Start with a random placement
* Randomly exchange two gates
* Keep the change if wirelength decreases
* Problem: easy to stuck in local minimum

#### 9.4-9.5 Simulated Annealing Placement

![[Pasted image 20230818182847.png]]

* 模拟退火算法
* With a slowly reduced temperature $T$.
* If a swap makes wirelength worse, randomly accept it with probability $P(\Delta L, T)$.
* Inefficient for large circuits (1M+ gates)

```algorithm
T = hot
frozen = false
while (!frozen) {
	for (s = 1; s < M * #gates; s++) {
		Swap Gi and Gj
		Calculate delta = change of L
		if (delta < 0)
			accept
		else {
			if (uniform_random() < exp(-delta/T))
				accept
			else
				undo
		}
	}
	if (L is still decreasing)
		T = 0.9 * T
	else
		frozen = true
}
return final replacement
```

#### 9.6-9.7 Quadratic Wirelength Model

* Represent position of each gate as $(x_i, y_i)$
* Represent cost as $F(x_1, x_2, \dots, y_1, y_2, \dots)$
* Model cost as squared length of distance between points.
* For k-point net, weight by $1/(k-1)$.
* Solve by setting the derivative equal to 0 and solve for $x$ and $y$.

##### Calculation


![[Pasted image 20230818202459.png]]


1. Build the connectivity matrix $C$
2. Build $A$ matrix with $C$
3. Set $b_x[i] = w_i \cdot x_i$

* Solve $Ax= b_x$ and $Ay=b_y$
* Easy to solve for large matrix

#### 9.8-9.9 Recursive Partitioning

![[Pasted image 20230818203135.png]]

* Problem: Gates are all concentrated near the center
* Put N/2 gates in one region, and the other N/2 gates in the other, vertically and then horizontally.
* Problem: How do we keep the gates assigned to left side actually in the left side? 
* Solution: Use Pseudo-pads
* Can use [[2 Netlist and System Partitioning]] for better partition

![[Pasted image 20230818204043.png]]
![[Pasted image 20230818204054.png]]
![[Pasted image 20230818204105.png]]
![[Pasted image 20230818204123.png]]

* Legalization: need to force gates in precise rows for final result
* One easy way is by annealing
* Modern placers are all analytical

