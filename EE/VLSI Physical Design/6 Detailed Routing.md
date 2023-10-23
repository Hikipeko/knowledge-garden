* Assign route segments of signal nets to specific routing tracks, vias, and metal layers.
* Must account for additional manufacturing rules.

### 6.1 Terminology

* **Channel routing** is where pins are located on opposite sides of the channel
* Switchbox routing
* Over-the-cell (OTC) routing
* **Horizontal constraint** exists between two nets if their horizontal segments overlap when placed on the same track.
* **Vertical constraint** exist between two nets if they have pins in the same column.

![[Pasted image 20231022193046.png|550]]
![[Pasted image 20231022193141.png|550]]
![[Pasted image 20231022193452.png|550]]

### 6.2 Horizontal and Vertical Constraint Graphs

* Predict the minimum number of tracks that are required
* Detect potential routing conflicts
* **Zone Representation** use $S(col)$ to denote the set of nets that pass through column $col$.
* **Horizontal constraint graph** $HCG(V,E)$ use edge to represent horizontal constraint.
* **Vertical constraint graph** $VCG(V,E)$ is similar to [[3 Chip Planning#3.3 Terminology]]

![[Pasted image 20231022193828.png|550]]
![[Pasted image 20231022194101.png|550]]

### 6.3 Channel Routing Algorithm

#### 6.3.1 Left-Edge Algorithm

```algorithm
Left-Edge(netlist)

HCG = HCG(netlist)
VCG = VCG(netlist)
curr_track = {}
curr_track_num = 1
while (netlist is not empty)
	curr_nets = VCG.roots()
	for (net in curr_nets)
		if (Try-Assign-To-Track(net, curr_track, curr_track_num))
			VCG.remove(net)
			HCG.remove(net)
	curr_track += 1
```

#### 6.3.2 Dogleg Routing

* Introduced to deal with cycles in VCG, a **dogleg** can be introduced.
* Can help reduce total number of tracks.

![[Pasted image 20231022195506.png|500]]

### 6.4 Switchbox Routing

### 6.5 OTC and Gcell Routing Algorithms

* Cells predominately use only Poly and Metal1 for internal routing
* Higher metal layers are typically used for OTC routing

#### 6.5.1 OTC Routing Methodology

1. Select nets that will be routed outside the channel
2. Route these nets in the OTC area
3. Route the remaining nets

#### 6.5.2 OTC and Gcell Routing Algorithms

More recent detailed routers are less OTC specific but focus on certain aspects of the modern routing challenge, mainly to address issues arising with advanced technology nodes.

### 6.6 Modern Challenges in Detailed Routing

* Via defects
* Interconnect defects
* Antenna-induced defects 