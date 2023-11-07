### 8.1 Introduction

See from [[7 Specialized Routing#Useful Skew]]. 

* **Setup constraint** $t_{\text{cycle}} \geq t_{\text{combDelay}} + t_{\text{setup}}+t_{\text{skew}}$
* **Hold-time constrain** $t_{\text{combDelay}}  \geq t_{\text{hold}}+t_{\text{skew}}$
* See [[12 Timing#12.3 SAT Delay Graph, ATs, RATs, and Slacks]]

**Timing closure** is the process of satisfying timing constraints through layout optimization and netlist modifications.

### 8.1 Timing Analysis and Performance Constraints

#### 8.2.1 Static Timing Analysis

* See [[12 Timing]]
* **Gate node convention** has one node per logic gate.
* **Pin node convention** Each node in the DAG is an input pin or output pin. Can represent different delay for different pins.
* **Critical nets** are signals that have negative slack.
* **Current Practice** Separate timing are performed for **rise delay** and **fall delay**.
* **Signal integrity** extensions to STA consider changes in delay due to switching activity on neighboring wires of the path.
* Challenges
	* The assumption of a clock: STA is not applicable to asynchronous context
	* All paths are sensitizable: there are **false paths**

![[Pasted image 20231105133709.png|500]]
![[Pasted image 20231105134424.png|500]]

#### 8.2.2 Delay Budgeting with the Zero-Slack Algorithm

Chicken-egg-dilemma: timing optimization requires knowledge of delay and thus wirelength, while wirelengths are unknown until routing.

Solution: **timing budgets** are used to establish delay and wirelength constraint for each net, thereby guiding placement and routing to a timing-correct result.

##### Zero-Slack Algorithm

* Seeks to decrease positive slacks of all nodes to zero by increasing $t(v), t(e)$ the delay of gates and output wires respectively.
* **Timing budget** $TB(v) = t(v) + t(e)$ should not be exceeded during placement and routing.
* Another approach to satisfying the timing budget is rebudgeting.
* Algorithm
	1. Determine the initial slacks
	2. Select a node $v_{min}$ with minimum positive slack
	3. Find a path of nodes that dominates slack($v_{min}$)
	4. Evenly distribute the slack by increasing $TB(v)$ for each $v$ in path.

```algorithm
Input: timing graph G(V, E)
Output: timing budgets TB for each v
do
	(AAT, RAT, slack) = STA(G)
	for each (v8 in V)
		TB[vi] = delay(vi) + delay(ei)
	slack_min = infinity
	for each (v in V)
		if (slack[v] < slack_min) and (slack[v] > 0)
			slack_min = slack[v]
			v_min = v
	if (slack_min is not infinity)
		path = v_min
		Add-to-Front(path, Backward-Path(v_min, G))
		Add-to-Back(path, Forward-Path(v_min, G))
		for (i = 1 to |path|)
			node = path[i]
			TB[node] = TB[node] + s
while (slack_min is not infinity)
```

##### Early-Mode Analysis

* To satisfy the hold-time constraint.
* **Earliest actual arrival time** $AAT(v)= \min \{AAT(u) + t(u,v)\}$
* **Required arrival time EM** $RAT_{EM}(v) = \max\{RAT_{EM}(u) - t(v,u)\}$
* $slack_{EM}(v) = AAT_{EM}(v) - RAT_{EM}(v) \geq 0$.

##### Near-Zero-Slack Algorithm

* Decreases $TB(v)$ by decreasing $t(v)$ or $t(e)$.
* Since $t(v), t(e)$ cannot be negative, node slack may not necessarily all becomes zero.
* This may violates the late mode analysis.
* Often used to confirm that early-mode constraints are satisfied.

### 8.3 Timing-Driven Placement

* **Path-based**
	* Seeks to shorten or speed up entire timing-critical paths rather than individual nets.
	* Does not scale to large, modern designs.
* In practice, some industrial flows do not incorporate timing-driven methods during initial placement since timing information is typically not very accurate until locations are available.

#### 8.3.1 Net-Based Techniques

##### Net Weighting

* Assigns priority to critical nets during placement.
* **Static net weights** are computed before placement.
	* E.g. $w = (1- slack/t)^\alpha$, where $t$ is the longest path delay.
* **Dynamic net weights** are computed during placement iterations, depending on the timing information.
	* Can be more effective in practice.

##### Delay budgeting

* Assigns upper bounds to the timing of individual nets.
* E.g. using ZSA.

#### 8.3.2 Embedding STA into Linear Programs for Placement

* Minimize a function of slack, subject to 
* **Physical constraints**
* **Timing constraints**