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

* Choosing a lower power supply significantly reduced power consumption.
* Divide chip into multiple **voltage domains** (e.g. high for memory, medium for processor, low for I/O.)

##### 5.2.3.1 Voltage Domains

* The gate in $V_{DDL}$ domain cannot directly drive a gate in $V_{DDH}$ domain, otherwise causing short circuit.
* Use **level converter** to cross voltage domains.
* Easiest way to manage is to associate each domain with a large area of the [[3 Chip Planning|floorplan]].
* Another approach is **clustered voltage scaling (CVS)**, in which two supply voltage are used in a single block. E.g. using high voltage for the critical path.

![[Pasted image 20231024213132.png|150]]![[Pasted image 20231024213147.png|200]]
![[Pasted image 20231024213509.png|500]]

##### 5.2.3.2 Dynamic Voltage Scaling (DVS)

* Motivation: Many tasks have uneven workload, requiring different frequency & supply voltage at different time.
* DVS or DVFS reduces the supply voltage to minimum necessary to operate at a given frequency.
* **DVS controller** takes information from Core logic.
	* First decide frequency
	* Then decide supply voltage
* A switching voltage regulator efficiently steps down $V_{\text{in}}$ to a necessary $V_{DD}$.
* Define **rate** to be fraction of maximum performance required to complete the workload.
* Without DVS (linear) > Undithered (stepped) > Dithered > Arbitrary Supply Voltage
* Use discrete voltages.
* A system can **dither** between these voltages to achieve almost as low energy as by using arbitrary supply voltage.
* **Local voltage dithering** let each block operate in different voltages.

![[Pasted image 20231024214119.png|300]]![[Pasted image 20231024214951.png|350]]

#### 5.2.4 Frequency

Use multiple frequency domains

#### 5.2.5 Short-Circuit Current

* Increase as the input edge rates become slower.
* Good to use relatively crisp edge rates.

### 5.3 Static Power

In nanometer processes with low threshold voltages and thin gate oxides, leakage can account for as much as 1/3 of total active power.

#### 5.3.1 Static Power Sources

##### 5.3.1.1 Subthreshold Leakage

* Increase exponentially with temperature, and thus limiting die temperature is important.
* **Stack effect** is the effect that the leakage through two or more series transistors is dramatically reduced by a factor of about 10.

#### 5.3.2 Power Gating

* The easiest way to reduce static current during sleep mode is to turn off the power supply to sleep blocks. (In clock gating, we just block the logic)
* When sleep, the header switch turns OFF.
* In real design, pMOS header power gating is simpler.
* Practical designs use **coarse-grained power gating** where the switch is shared across an entire block.

![[Pasted image 20231024220344.png|300]]

#### 5.3.3 Multiple Threshold Voltages and Oxide Thicknesses

* Maintain performance by using low-$V_t$ transistors on critical paths, and high-$V_t$ transistors on other path.
* 5.3.4 Variable Threshold Voltage
* 5.3.5 Input Vector Control

### 5.4 Energy-Delay Optimization

See [[ECE 470 Computer Architecture#Metrics]]

#### 5.4.1 Minimum Energy

* **Power-delay product (PDP)** is simply the energy for an operation
* The blue lines denotes how many times of minimum energy is used.

![[Pasted image 20231024221400.png|600]]

#### 5.4.2 Minimum Energy-Delay Product

* A popular metric that balances performance and power.
* 5.4.3 Minimum Energy Under a Delay Constraint

### 5.5 Low Power Architectures

* 5.5.1 Microarchitecture
* 5.5.2 Parallelism and Pipelining
* 5.5.3 Power Management Modes

