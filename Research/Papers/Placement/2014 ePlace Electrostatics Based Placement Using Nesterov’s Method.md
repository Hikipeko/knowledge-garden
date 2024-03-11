---
year: 2014
author: J Lu, P Chen, CC Chang, L Shan, DJH Huang, CC Teng, CK Cheng
citation: 61
---
[[2015 ePlace Electrostatics-Based Placement Using Fast Fourier Transform and Nesterov’s Method]] is a better version.

1. **MIP** ePlace quadratically minimize the total wirelength at the mixed-size initial placement.
2. **MGP** populates extra area with dummy fillers, and co-optimizes all the objects.
3. **MLG**
4. **CGP**
5. **CDP** the detailed placer in "Unified Analytical Global Placement for Large-Scale Mixed-Size Circuit Designs" is used to legalize the standard-cell layout.

![[Pasted image 20231028111838.png|500]]
![[Pasted image 20231028112516.png|500]]

#### Macro Legalization

![[Pasted image 20231029211826.png|500]]

* At each iteration of the outer loop, update the cost function as $f_{mLG}(\mathbf v) = W(\mathbf v) + \mu_D D(\mathbf v) + \mu_O O_m(\mathbf v)$
* $D$ is the total standard-cell area covered by macros, $O$ is macro overlap.
* **Objective** is to minimize $W(\mathbf v) + \mu_D D(\mathbf V)$, set $\mu_D = W(\mathbf v) / D(\mathbf v)$.
* **Constraint** is $O(v) = 0$, we set $\mu_O$ as penalty factor and multiplied by $\kappa$ at each mLG iteration.
* Inner loop is simulated annealing.

#### Standard-Cell Global Placement

* Same with mGP except for fixed macros.

