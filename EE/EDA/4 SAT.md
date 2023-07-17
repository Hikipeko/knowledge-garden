#### 4.1 Part 1

* For function $F$, find an assignment of the variables s.t. $F() = 1$, or prove that there are no such assignments.
* For [[2 Computational Boolean Algebra#2.4 Application to Logic Network Repair|Network Repair]], it is an SAT problem for all the $d$ values.
* Standard SAT form: Conjunctive Normal Form (CNF)

![[Pasted image 20230716180606.png|500]]

##### Status of Clauses

* Conflicting: = 0
* Satisfied: = 1
* Unresolved

##### Solve SAT

* Decision: select a variable, assign its value, and simplifies CNF
* Deduction: iteratively simplifies the newly simplified clauses

#### 4.2 Boolean Constraint Propagation

* A way of deduction
* **Unit Clause Rule**: a clause is **unit** if it has one unassigned literal
* Unit clause has exactly one way to be satisfied, the choice is an **implication**
* Name: DPLL algorithm
* SOTA: MiniSAT, CHAFF, GRASP

#### 4.3 Using SAT for Logic

Are two boolean functions $F, G$ the same? SAT for $F \bar\oplus G$.

##### From Gates to CNF

1. Assign variable for each wire
2. Write out gate consistency function for each gate
3. Product them up

![[Pasted image 20230716182050.png]]

![[Pasted image 20230716182036.png|500]]

