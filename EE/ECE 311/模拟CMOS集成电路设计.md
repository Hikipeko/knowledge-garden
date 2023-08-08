### 1 Introduction to Analog Design

#### 1.1 Why Analog

##### 1.1.1 Sensing and Processing Signals

* Many electronic systems sense a signal and process and extract information from it, e.g. a digital camera senses the light and extract an image.
* Q: why don't we do all the job in the digital world?
* A: power, etc

##### 1.1.2 When Digital Signals Become Analog

![[Pasted image 20230730150606.png|500]]

* 高频信号在传输时容易衰减，因此增强高频信号
* 1.1.3 Analog Design is in Great Demand

##### 1.1.4 Analog Design Challenges

* Transistor imperfections
* Declining supply voltage
* Power consumption
* Circuit complexity
* PVT variations (fabrication process, supply voltage, ambient temperature)

#### 1.2 Why Integrated

Driven primarily by the memory and microprocessor market, integrated-circuit technologies  have also embraced analog design, affording a complexity, speed, and precision that would be impossible to achieve using discrete implementations.

#### 1.3 Why CMOS

* CMOS gates dissipated power only during switching and required very few devices.
* Possibility of placing both analog and digital circuits on the same chip.
* Device scaling and continuous speed improvement.
* Operate with low supply voltages (1V vs 2V for bipolar circuits).

#### 1.4 Why This Book

* Engineer's hat: quick and intuitive understanding of a large circuit
* Mathematician's hat: quantifying effects in a circuit
* Artist's hat: inventing new circuit topologies

#### 1.5 Levels of Abstraction

![[Pasted image 20230730152907.png|500]]



### 2 Basic MOS Device Physics

#### 2.1 General Considerations

##### 2.1.1 MOSFET as a Switch

* $V_G = H$, short
* $V_G = 0$, open

##### 2.1.2 MOSFET Structures

![[Pasted image 20230730153847.png|500]]

* Source is the terminal that provides the charge carriers.
* Assume the the substrate of NMOS transistors is connected to most negative supply (GND).
* Arrow is the source.

![[Pasted image 20230730155623.png]]

#### 2.2 MOS I/V Characteristics

##### 2.2.1 Threshold Voltage

![[Pasted image 20230730155703.png|250]]![[Pasted image 20230730155716.png|250]]

* $C_{ox}$ is the gate-oxide capacitance per unit area.

##### 2.2.2 Derivation of I/V Characteristics

![[Pasted image 20230730161104.png]]

* When $V_{DS} \ll 2(V_{GS} - V_{TH})$, we are in deep triode region, where the MOS is a voltage-controlled resistance.
* In saturation region, the inversion layer stops at $x \leq L$, and we say the channel is pinched off. 夹断 This will lead to channel length modulation.
* For PMOS, $V_{GS}, V_{DS}, V_{GS} - V_{TH}$ are negative.

![[Pasted image 20230730161401.png]]

See [[2 MOS Transistor Theory#2.2 Long-Channel I-V Characteristics]]

##### 2.2.3 MOS Transconductance

* Usually defined in the saturation region

$$
g_m = \mu_nC_{ox}\frac{W}{L}(V_{GS} - V_{TH}) = \frac{2I_D}{V_{GS} - V_{TH}} = \sqrt{2\mu_n C_{ox}\frac{W}{L}I_D} \quad (2.18-2.20)
$$

![[Pasted image 20230731145552.png]]

#### 2.3 Second-Order Effects

##### Body Effect

* When a voltage $V_{sb}$ is applied between the source and body, it increases the amount of charge required to invert the channel, and thus increases $V_{TH}$.
* Can be leveraged to adjust $V_{TH}$.
* Usually undesirable as it complicates the design of analog circuits.

$$
V_{TH} = V_{TH0} + \gamma(\sqrt{2\phi_F+V_{SB}} - \sqrt{2\phi_F})\quad (2.23)
$$

##### Channel-Length Modulation

$$
I_D \approx \frac12\mu_nC_{ox} \frac{W}{L}(V_{GS} - V_{TH})^2(1+\lambda V_{DS})\quad (2.27)
$$

* Use this bias to drive a current? The dependence is too weak. Drive current by $V_{GS} - V_{TH}$.

###### Subthreshold Conduction

* $V_{GS} <V_{TH}$, there's still some current.
* $I_D = I_0 \exp{\frac{V_{GS}}{\xi V_T}}$

![[Pasted image 20230730170409.png|500]]

#### 2.4 MOS Device Models

##### 2.4.1 MOS Device Layout

![[Pasted image 20230730171049.png|500]]

##### 2.4.3 MOS Small-Signal Model

We derive the small-signal model by producing a small increment in one bias parameter and calculating the resulting increment in other bias parameters.

![[Pasted image 20230731143154.png]]

* $V_{DS}$ on $I_{DS}$, we have channel length modulation.
	* A current source proportional to port voltage is equivalent to a resistor, so the length modulation can be modeled as$r_O$.
	* $r_O$ limits the maximum voltage gain of most amplifiers.
* $V_{BS}$ on $I_{DS}$, we have body effect.
	* In most cases, $V_{BS} < 0$
	* Typically $\eta = 0.25$

$$
\frac{\partial I_D}{\partial V_{DS}}=\frac{1}{r_O} = \frac12\mu_nC_{ox} \frac{W}{L}(V_{GS} - V_{TH})^2\cdot \lambda \approx I_D \cdot \lambda \quad (2.46)$$
 $$g_{mb} = \frac{\partial I_D}{\partial V_{BS}} = g_m \eta, \eta = \frac{\gamma}{2\sqrt{2\Phi_F + V_{SB}}} \quad(2.53)$$

###### PMOS Small-Signal Model

* Positive $\Delta V_{GS}$ produces a positive $\Delta I_D$, the same as NMOS.

### 3 Single-Stage Amplifiers

#### 3.1 Applications

Mobile phone, laptop, digital camera, etc.

#### 3.2 General Considerations

* Idea: $y_(t) = \alpha_0 + \alpha_1 x(t)$. Reality: $y_(t) = \alpha_0 + \alpha_1 x(t) + \alpha_2 x^2(t) + \cdots + \alpha_n x^n(t)$.
* Power dissipation, supply voltage, linearity, noise, maximum voltage swings

#### 3.3 Common-Source Stage

##### 3.3.1 Common-Source Stage with Resistive Load

![[Pasted image 20230730145223.png]]

![[Pasted image 20230731152240.png]]

* $A_v = -g_m (r_O ||R_D)\quad (3.10,3.17)$
* Since $g_m$ varies with the input signal, the gain is not stable.
* We want the gain equation to be a weak function of signal-dependent parameters such as $g_m$.
* Increase $A_v$ by increasing $W/L$ or $V_{RD}$ or decreasing $I_D$.
* Gain limited by intrinsic gain $-g_m r_O$.

##### 3.3.2 CS Stage with Diode-Connected Load

![[Pasted image 20230731153641.png|150]]

* Difficult to fabricate resistors with tightly-controlled values or a reasonable physical size.
* Replace $R_D$ with a MOS transistor.
* MOSFET can operate as a small-signal resistor if gate and drain are shorted.
* Always in saturation.
* Impedance is $(1/g_m)||r_O \approx 1/g_m$.
* Impedance is $(1/g_m)||(1/g_{mb})||r_O \approx 1/g_m$ with body effect.
* Looking into the source of a MOSFET, we see $1/g_m$.
* The gain is independent of the bias currents and voltages, implying a constant gain!
* If we use PMOS on the top, we can avoid body effect since bulk is connected to $V_{DD}$.
* Channel-length modulation is significant in today's CMOS technology, 3.4.5.

$$
A_v = -\sqrt{\frac{(W/L)_1}{(W/L)_2}}\frac{1}{1+\eta}\quad (3.34)
$$
$$
A_v = -g_{m1}(\frac{1}{g_{m2}||r_{O1}||r_{O2}}) \quad (3.45)
$$
![[Pasted image 20230731161218.png]]

##### 3.3.3 CS Stage with Current-Source Load

![[Pasted image 20230731162538.png]]

* Increase $R_D$ leads to large dc drop, limiting the output voltage swing.
* The top PMOS act as a current source.
* $A_v = -g_{m1}(r_{O1}||r_{O2}) \quad (3.46)$
* Top MOS cannot act as an amplifier.

##### 3.3.4 CS Stage with Active Load

![[Pasted image 20230731165104.png]]
$$
A_v = -(g_{m1}+g_{m2})(r_{O1}||r_{O2})\quad (3.48)
$$

* Bias current is a strong function of PVT (e.g. threshold voltage).

##### 3.3.5 CS Stage with Triode Load

* MOS deep triode region behave as a resistor, serving as the load.

##### 3.3.6 CS Stage with Source Degeneration

![[Pasted image 20230731170505.png]]

* A fraction of change in $V_{in}$ appears across the resistor rather than as the gate-source override, thus leading to a smoother variation of $I_D$.
* The linearization is obtained at the cost of lower gain.
* In (3.62) the denominator is "the resistance seen in the source path. The numerator is the resistance seen at the drain.
* We view the magnitude of gain as the resistance seen at the drain node divided by the total resistance in the source path.

$$
\begin{align}
G_m &= \frac{g_m}{1 + g_mR_S}\quad (3.55)\\
A_v &= -G_mR_D \quad (3.56, 3.57)\\
G_m &= \frac{g_m r_O}{R_S + [1+(g_m+g_{mb})R_S)]r_O} \quad (3.61)\\
A_v &= -\frac{R_D}{\frac{1}{g_m}+R_S} \quad (3.62)\\
R_{out} &= R_D||R_S +r_O + (g_m + g_{mb})R_Sr_O \quad (3.79)
\end{align}
$$
![[Pasted image 20230731171512.png]]

**Lemma**

In a linear circuit, the voltage gain is equal to $G_mR_{out}$, where $G_m$ denotes the transconductance of the circuit when the output is shorted to ground, and $R_{out}$ represents the output resistance of the circuit when the input voltage is set to zero.

![[Pasted image 20230731175649.png]]

#### 3.4 Source Follower

