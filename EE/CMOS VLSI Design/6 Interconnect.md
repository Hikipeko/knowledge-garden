### 6.1 Introduction

* The wires linking transistors together are called **interconnect**.
* Cause a large faction of RC delay and switching energy.

#### 6.1 Wire Geometry

* The wire has width $d$, length $l$, thickness $t$, spacing of $s$ from their neighbors, and have a dielectric of height $h$ between the below conduction layer.
* **Pitch** = $w + s$.
* **Aspect ratio** is $t/w$.

#### 6.1.2 Example: Intel Metal Stacks

* Top-level metal is usually used for power and clock distribution due to its low resistance.

![[Pasted image 20231026223348.png|600]]

### 6.2 Interconnect Modeling

* L-model, $\pi$-model, and T-model.
* $\pi$-model is the most popular one, 3-5 segments are sufficient to achieve high accuracy.

![[Pasted image 20231026223456.png|400]]

#### 6.2.1 Resistance

$$
R = \rho \frac{l}{wt},\; R=R_\square \frac lw,\; R_\square = \rho/t
$$

* $R_\square$ is the **sheet resistance**.
* Contacts and vias also have a resistance. Typical values are 2-20 $\Omega$.

