### 5.1 Introduction

The application and architectural level are where the major impact on power dissipation can be made. So You must optimize power in a top-down manner, from the problem definition downward. Or you will be doomed to fail.

#### 5.1.1 Definition

* **Instantaneous power** $P(t)$
* Energy $E$
* Average power $P_{\text{avg}} = E / T$

#### 5.1.2 Examples

* The energy stored in capacitor is $E_C = \frac12 C_LV_{DD}^2$
* The energy delivered from the power supply is $CV_{DD}^2$
* Only half of energy is stored in the capacitor, the other half is dissipated in the pMOS.
* The small blip of current is called **short-circuit current**, when both transistors are in linear region.
* When $V_{in}$ rises, the energy on the capacitor is dumped to GND through nMOS.
* **Dynamic power** $P_{\text{switching}} = \alpha CV_{DD}^2f$, where $\alpha$ is the **activity factor**.
* Clock has activity factor $\alpha = 1$, most data has maximum activity factor of 0.5.
* Static CMOS logic has activity factor close to 0.1.

![[Pasted image 20231022155232.png|650]]

#### 5.1.3 Sources of Power Dissipation

##### Dynamic Dissipation

* Charge and discharging load capacitance
* Short-circuit current, normally less than 10% of the whole.
* $P_{\text{dynamic}} = P_{\text{switching}}+P_{\text{short circuit}}$

##### Static Dissipation

* [[2 MOS Transistor Theory#2.4.4.1 Subthreshold Leakage]] through OFF transistors
* [[2 MOS Transistor Theory#2.4.4.2 Gate leakage]] through gate dielectric
* [[2 MOS Transistor Theory#2.4.4.3 Junction Leakage]] from source/drain diffusions
* Contention current in ratioed circuits
* $P_{\text{static}} = (I_{\text{sub}} + I_{\text{gate}}+I_{\text{junct}} + I_{\text{contention}})V_{DD}$
* **Active power** is the power consumed while the chip is doing useful work.
* **Standby power** is the power when the chip is idle.

### 5.2 Dynamic Power

**Effective capacitance** is the true capacitance multiplies by the activity factor.

#### 5.2.1 Activity Factor

##### 5.2.1.1 Clock Gating

* Turn off the not used block by stopping the clock.
* Often, however, clock gating signals are some of the most critical paths of the chip.

![[Pasted image 20231022162344.png|250]]

##### 5.2.1.2 Switching Probability

* We can estimate the activity factors by analyzing the probability that each node is 1.
* $\alpha_i = \bar P_i P_i$

##### 5.2.1.3 Glitches

* Gates sometimes make spurious transitions called **glitches** when inputs do not arrive simultaneously.
* Cause extra power dissipation, and can raise the activity power to above 1.

#### 5.2.3 Voltage

* Divide 
