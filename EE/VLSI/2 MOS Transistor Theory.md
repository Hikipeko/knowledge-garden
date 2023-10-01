### 2.1 Introduction

* The body is grounded and a voltage is applied to the gate.
* Three modes: accumulation, depletion, and inversion.

![[Pasted image 20230719114352.png|400]]

* **Cutoff**: $V_{gs} < V_{t}$, junctions are zero-biased or reversed biased, the transistor is OFF, $I_{ds} \approx 0$.
* **Linear**: $V_{gs} > V_{t}$, an inversion region of electrons called the **channel** connects the source and drain, turning the transistor ON, creating a conducting path. 
	* $V_{ds} < V_{gs} - V_{t}$
	* When a small positive potential $V_{ds}$ is applied, current $I_{ds}$ flows form drain to source.
	* $I_{ds}$ is controlled by the gate voltage and $V_{ds}$.
* **Saturation**: $V_{gs} > V_{t}, V_{ds} > V_{gs} - V_{t}$, the channel is no longer inverted near the drain and becomes pinched off (夹断), $I_{ds}$ is controlled only by the gate voltage.

![[Pasted image 20230719114812.png|550]]

* The pMOS transistor operates in just the opposite fashion.
* Body is tied to a high potential so that the two junctions are normally reverse-biased.
* Majority carriers flow from their source to their drain.

![[Pasted image 20230719120730.png]]



### 2.2 Long-Channel I-V Characteristics

![[Pasted image 20230929120422.png|250]]

* Model the gate as parallel plate capacitor with capacitance proportional to area over thickness.
* The current is the total amount of charge in the channel divided by the time required to cross.
* $I_{\text{on}} = \frac \beta2(V_{DD} - V_t)$ 

$$\begin{align}
Q_{\text{channel}} &= C_g(V_{gc}-V_t)\\
C_g &= k_{ox} \epsilon_0\frac{WL}{t_{ox}}\\
v &= \mu E\\
E &= \frac{V_{ds}}{L}\\
I_{ds} &= \frac{Q_{\text{channel}}}{L/v}\\
&= \mu C_{ox}\frac WL (V_{gs} - V_t - \frac 12 V_{ds} ) V_{ds}\\
&= \beta(V_{GT} - V_{ds}/2)V_{ds}
\end{align}$$


![[Pasted image 20230719145948.png]]

$\beta = \mu C_{ox} \frac WL;V_{GT} = V_{gs} - Vt$.

* Mobility of holes < electrons, thus pMOS transistors provide less current than nMOS, and is slower.
* Just remember: current flows from source to drain in a pMOS transistor

![[Pasted image 20230719150451.png]]



### 2.3 C-V Characteristics

#### 2.3.1 Simple MOS Capacitance Models

* $C_g = C_{ox}WL = C_\text{permicron}W$, where permicron use $L$ as constant for a particular process.
* **Parasitic capacitor**: capacitance between source and drain, which is useless

### 2.4 Non-ideal I-V Effects

![[Pasted image 20230719155206.png]]

#### 2.4.1 Mobility Degradation and Velocity Saturation

##### Mobility Degradation

High voltage at the gate attracts the carriers to the edge of the channel, causing collisions with the oxide interface that slow the carrier (vertical).

![[Pasted image 20230719155758.png|400]]![[Pasted image 20230719155823.png]]

##### Velocity Saturation

* High $E$ leads to non-linear increase of $v$
* After velocity saturation (tell the difference between saturation region), $I_{ds}$ grows linearly rather than quadratically with $V_{gs}$.

![[Pasted image 20230929133306.png|400]]
![[Pasted image 20230929141305.png|500]]
![[Pasted image 20230929141952.png|300]]

#### 2.4.2 Channel Length Modulation

* The p-n junction between the drain and body forms a depletion region, which shortens the channel length to $L_{\text{eff}} = L - L_d$.
* Generally unimportant for qualitatively understanding of digital circuits.
* Can be modeled as

$$I_{ds} = \frac \beta 2 V_{GT}^2(1+\frac{V_{ds} }{V_A})$$


#### 2.4.3 Threshold Voltage Effects

[[模拟CMOS集成电路设计#Body Effect]]

##### Drain-Induced Barrier Lowering

* A high $V_{ds}$ creates an electric field that can decrease $V_t = V_{to} - \eta V_{ds}$.
* **DIBL coefficient: $\eta \approx 100 \text{mV/V}$
* Cause $I_{ds}$ continue to increase with $V_{ds}$ in the saturation region.
* Normally not a problem at nominal supply voltages.
* Can be very important in sub-threshold region.

##### Short Channel Effect

#### 2.4.4 Leakage

Even OFF transistors leaks small amount of current.

##### 2.4.4.1 Subthreshold Leakage

![[Pasted image 20230929201109.png|400]]

* The "cut-off" region is a mathematical abstraction, "weak inversion".
* Weak inversion region is useful for low power designs.
* Small leakage between source and drain when the device is OFF.
* Saturate when $V_{ds} > 100\text{mV}$.
* Highly dependent on threshold voltage.

$$
I_{DS} = I_{OFF}\cdot \exp(\frac{V_{gs}-V_t+\eta V_{ds} - k\gamma V_{sb}}{n\phi_t}) (1-\exp(-V_{ds}/\phi_t))
$$

##### 2.4.4.2 Gate leakage

* From gate to body, caused by quantum tunneling
* Important for 90nm technologies and below.

##### 2.4.4.3 Junction Leakage

* Reverse biased diode have currents
* $I_{\text{junc}} = I_s \cdot \exp(V_{db}/\phi_t - 1)$
* TODO

#### 2.4.5 Temperature Dependency



### 2.5 DC Transfer Characteristics

#### 2.5.1 Static CMOS Inverter DC Characteristics

![[Pasted image 20230719163702.png|600]]

![[Pasted image 20230719163726.png|500]]

#### 2.5.3 Noise Margin

* Logic '1' and '0' are not necessarily exactly VDD and GND.
* We use the following definition to determine the margin with which we can operate.

![[Pasted image 20230719170835.png|300]]![[Pasted image 20230719171018.png|300]]


