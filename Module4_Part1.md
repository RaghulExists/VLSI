---
Page 1
---

# Module-4

Subsystem Design and Layout: Some Architectural Issues, Switch Logic, Gate (restoring) Logic, A parity generator, Multiplexers (Data selectors), Design of 4-bit shifter.
(Text 1: 6.1 to 6.3, 6.4.1, 6.4.3, 7.2.2)

Illustration of the Design Processes: Some observations on the design process, Design of an ALU Subsystem, (Text 1: 8.1, 8.3)

---
Page 2
---

## ARCHITECTURAL ISSUES

* All design processes, a logical and systematic approach is essential.
1. Define the requirements (properly and carefully).
2. Partition the overall architecture into appropriate subsystems.
3. Consider communication paths carefully in order to develop sensible interrelationships between subsystems.
4. Draw a floor plan of how the system is to map onto the silicon.
5. Draw suitable (stick or symbolic) diagrams of the leaf-cells of the subsystems.
6. Aim for regular structures so that design is largely a matter of replication.
7. Convert each cell to a layout.
8. Carefully and thoroughly carry out a design rule check on each cell.
9. Simulate the performance of each cell/subsystem.

---
Page 3
---

The whole design process will be greatly assisted if considerable care is taken with:

1. The partitioning of the system so that there are clean and clear subsystems with a minimum interdependence and complexity of interconnection between them.
2. The design simplification within subsystems so that architectures are adopted which allow the exploitation of a cellular design concept.

This allows the system to be composed of relatively few standard cells which are replicated to form highly regular structures.

---
Page 4
---

## SWITCH LOGIC

* Switch logic is based on the 'pass transistor' or on transmission gates.
* This approach is fast for small arrays and takes no static current from the supply rails.
* Thus, power dissipation of such arrays is small since current only flows on switching.
* Switch (pass transistor) logic is similar to logic arrays based on relay contacts in that the path through each switch is isolated from the signal activating the switch.
* In consequence, the designer has a considerable amount of freedom in implementing architectural features compared with bipolar logic-based designs.
* Some examples of switch logic is below

---
Page 5
---

![Pass Transistor Logic: 4-Way Series Pass Transistor Switch]

**Description:**
* **Circuit type:** nMOS Pass Transistor Logic (Series Switch)
* **PMOS transistors:** None
* **NMOS transistors:** Four nMOS pass transistors connected in series.
* **Inputs:** Data input $V_{in}$ applied to the source of the first transistor. Gate control inputs A, B, C, D applied to the consecutive nMOS gates.
* **Outputs:** $V_{out}$ taken from the drain of the final transistor in the series chain.
* **Internal nodes:** Source/drain sharing connections between consecutive pass transistors in the chain.
* **VDD connection:** Not explicitly connected to static VDD; driven dynamically by $V_{in}$.
* **GND connection:** Not explicitly connected to static GND.
* **Pull-up network:** N/A (Switch logic, no static pull-up).
* **Pull-down network:** N/A (Switch logic, no static pull-down).
* **Signal flow:** Signal flows laterally from $V_{in}$ on the left to $V_{out}$ on the right, provided all pass transistors are turned ON by their respective gate signals.
* **Logic operation:** Acts as a 4-input AND function for passing the signal. $V_{out}=V_{in}$ when $A \cdot B \cdot C \cdot D = 1$. The $V_{out}$ logic level will be degraded by threshold voltage ($V_t$) effects if passing a logic 1.

$$
V_{out}=V_{in} \text{ when } A.B.C.D=1
$$
($V_{out}$ logic levels will be degraded by $V_t$ effects)

![Pass Transistor Logic: 8-Way Series Pass Transistor Switch]

**Description:**
* **Circuit type:** nMOS Pass Transistor Logic (Extended Series Switch)
* **PMOS transistors:** None
* **NMOS transistors:** Eight nMOS pass transistors connected in series.
* **Inputs:** Data input $V_{in}$ applied to the source of the first transistor. Gate control inputs A, B, C, D, E, F, G, H applied to the corresponding gates of the 8 pass transistors.
* **Outputs:** $V_{out}$ taken from the drain of the 8th transistor.
* **Internal nodes:** 7 intermediate source-to-drain junctions connecting the pass transistors in series.
* **VDD connection:** N/A
* **GND connection:** N/A
* **Pull-up network:** N/A
* **Pull-down network:** N/A
* **Signal flow:** Linear flow from left ($V_{in}$) to right ($V_{out}$) when all 8 gate controls are HIGH.
* **Logic operation:** $V_{out}=V_{in}$ when $A \cdot B \cdot C \cdot D \cdot E \cdot F \cdot G \cdot H = 1$. The signal experiences severe RC delay and threshold degradation passing through a long chain of 8 nMOS devices.

$$
V_{out}=V_{in} \text{ when } A.B.C.D.E.F.G.H=1
$$
$$
V_{out}=? \text{ when } A.B.C.D.E.F.G.H=0
$$

---
Page 6
---

![Transmission Gate Logic: CMOS 5-way selector]

**Description:**
* **Circuit type:** CMOS Transmission Gate Multiplexer (5-way selector)
* **PMOS transistors:** Five pMOS transistors, one for each transmission gate in the selector, with gates driven by the complemented select signals ($\overline{A}, \overline{B}, \overline{C}, \overline{D}, \overline{E}$).
* **NMOS transistors:** Five nMOS transistors, one for each transmission gate, with gates driven by the true select signals (A, B, C, D, E).
* **Inputs:** Five independent data lines $V_1, V_2, V_3, V_4, V_5$. Five independent true select lines A, B, C, D, E and their complements.
* **Outputs:** A single common output node $V_{out}$.
* **Internal nodes:** None (all paths go directly from an input to the output via a single transmission gate).
* **VDD connection:** N/A (signals driven by input lines).
* **GND connection:** N/A
* **Pull-up network:** N/A (uses bidirectional transmission gates).
* **Pull-down network:** N/A
* **Signal flow:** Parallel inputs $V_1$ through $V_5$ route to a single output $V_{out}$. Only one transmission gate is enabled at a time based on the active select line.
* **Logic operation:** 5-to-1 Multiplexer using transmission gates. $V_{out} = V_1 \cdot A + V_2 \cdot B + V_3 \cdot C + V_4 \cdot D + V_5 \cdot E$, assuming A, B, C, D, and E are mutually exclusive (one-hot encoding). Because transmission gates are used, the output logic levels will *not* be degraded by $V_t$ effects.

CMOS 5-way selector
$V_{out}=V_1.A+V_2.B+V_3.C+V_4.D+V_5.E$ assuming A, B, C, D and E are mutually exclusive
($V_{out}$ logic levels will not be degraded by $V_t$ effects)

---
Page 7
---

## Pass Transistors and Transmission Gates

* Switches and switch logic may be formed from simple n- or p-pass transistors or from transmission gates (complementary switches) comprising an n-pass and a p-pass transistor in parallel as shown in Figure 6.2.
* Transmission gate eliminates the undesirable threshold voltage effects which give rise to the loss of logic levels in pass transistors as indicated in Figure.
* No such degradation occurs with the transmission gate, but more area is occupied, and complementary signals are needed to drive it.
* 'On' resistance, however, is lower than that of the simple pass transistor switches.

---
Page 8
---

![Pass Transistor Logic: Logic Level Degradation in n-switch]

**Description:**
* **Circuit type:** nMOS Pass Transistor passing a Logic 1
* **PMOS transistors:** None
* **NMOS transistors:** One nMOS pass transistor.
* **Inputs:** Data input (I/P) = +5V (Logic 1). Gate control (X) = +5V (Logic 1).
* **Outputs:** Data output (O/P) = $+5V - V_{tp(n)}$ (+4V).
* **Signal flow:** A +5V step input is applied to the source, and the gate is held at +5V.
* **Logic operation:** The nMOS transistor turns off when the source reaches $V_{DD} - V_{tp}$. The output can only rise to $V_{DD} - V_{tp}$ (+4V), demonstrating a degraded logic 1 level.

n-switch - degraded logic 1

![Pass Transistor Logic: Logic Level Degradation in p-switch]

**Description:**
* **Circuit type:** pMOS Pass Transistor passing a Logic 0
* **PMOS transistors:** One pMOS pass transistor.
* **NMOS transistors:** None
* **Inputs:** Data input (I/P) = 0V (Logic 0). Gate control ($\overline{X}$) = 0V (Logic 0).
* **Outputs:** Data output (O/P) = $0V + |V_{tp(p)}|$ (+1V).
* **Signal flow:** A 0V step input is applied to the source, and the gate is held at 0V.
* **Logic operation:** The pMOS transistor turns off when the drain/source drops to $|V_{tp}|$. The output can only fall to $|V_{tp}|$ (+1V), demonstrating a degraded logic 0 level.

p-switch - degraded logic 0

Loss of logic level 1 if the gate of a pass transistor is driven from another pass transistor.

---
Page 9
---

![Transmission Gate Logic: Transmission Gate Schematic and Symbol]

**Description:**
* **Circuit type:** CMOS Transmission Gate
* **PMOS transistors:** One pMOS transistor forming the upper half of the parallel switch structure. Gate driven by $\overline{X} (= 0v)$.
* **NMOS transistors:** One nMOS transistor forming the lower half of the parallel switch structure. Gate driven by $X (= +5v)$.
* **Inputs:** Shared Data input (I/P) connected to both the source of the nMOS and the drain of the pMOS.
* **Outputs:** Shared Data output (O/P) connected to both the drain of the nMOS and the source of the pMOS.
* **Control signals:** True control $X$ drives the nMOS gate; complement control $\overline{X}$ drives the pMOS gate.
* **Internal nodes:** The parallel connection combines the nMOS and pMOS channels.
* **VDD connection:** N/A (acts as a bidirectional switch).
* **GND connection:** N/A
* **Pull-up network:** The pMOS handles pulling the output all the way to $V_{DD}$ (strong logic 1).
* **Pull-down network:** The nMOS handles pulling the output all the way to GND (strong logic 0).
* **Signal flow:** A 0V to +5V transition at I/P propagates completely to O/P when the gate is enabled.
* **Logic operation:** Passes full logic levels (0V or 5V) without threshold degradation because the nMOS passes a strong 0 and the pMOS passes a strong 1. The document also includes the standard "bowtie" symbol for the transmission gate with X at the top and $\overline{X}$ (with an inversion bubble) at the bottom.

Transmission gate - good logic levels
Transmission gate symbol

---
Page 10
---

## Parity Generator

* A parity generator is a digital logic circuit used to detect errors in data transmission by adding an extra bit (called the parity bit) to a binary message.
* What is Parity?
* Parity refers to whether the number of 1s in a binary sequence is:
* total number of 1s is even
    * Even parity
* total number of 1s is odd
    * Odd parity

---
Page 11
---

## Parity Generator

A circuit is to be designed to indicate the parity of a binary number or word.
* Design
* General solution to be obtained
* Even number of 1s at input, $P=1$
* Odd number of 1s at input, $P=0$
* Basic 1-bit cell to be designed
* n-bit parity generator obtained by cascading 1-bit cells

---
Page 12
---

![Structured Logic Design: Parity Generator Basic Block Diagram]

**Description:**
* **Circuit type:** Parity Generator Block Diagram (System Level)
* **Inputs:** A broad bus of parallel inputs: A0, A1, A2, ... An-1, An.
* **Outputs:** P (Parity output), $\overline{P}$ (Complement of Parity output).
* **Signal flow:** The $n+1$ bit input word feeds into the monolithic Parity generator block, which evaluates the total count of '1's across all input bits and produces the Even Parity bit $P$ and its inverse.
* **Logic operation:** Outputs $P=1$ if the input vector contains an even number of 1s. Outputs $P=0$ if the input vector contains an odd number of 1s.

PG- Basic Block diagram
* The requirement is indicated in fig for an (n+1) bit input.
Even number of 1s at input, $P=1$
Odd number of 1s at input, $P=0$

---
Page 13
---

![Structured Logic Design: Parity Generator Structured Design Approach]

**Description:**
* **Circuit type:** Cascaded Parity Generator Architecture
* **Inputs:** Data inputs fed vertically into each cell ($Ai-1, \overline{Ai-1}$, $Ai, \overline{Ai}$, $Ai+1, \overline{Ai+1}$, $Ai+2, \overline{Ai+2}$). Initial cascading inputs from the previous stage: $Pin, \overline{Pin}$.
* **Outputs:** Intermediate cascading outputs ($Pi-1, \overline{Pi-1}$, $Pi, \overline{Pi}$, $Pi+1, \overline{Pi+1}$, $Pi+2, \overline{Pi+2}$). The final stage passes the cumulative parity to the "Next stage".
* **Signal flow:** The architecture breaks the overall parity generator down into 1-bit cells connected in a linear daisy chain (systolic/iterative array). The parity state propagates horizontally from left to right, updated at each stage by the corresponding $Ai$ input bit.
* **Logic operation:** Each "One-bit cell" acts as an XOR/XNOR stage, combining the cumulative parity of the previous bits ($P_{i-1}$) with the current data bit ($A_i$) to produce the updated parity ($P_i$).

PG Structured Design Approach
* A suitable regular structure is set out in fig

---
Page 14
---

![Structured Logic Design: PG Basic One-Bit Cell]

**Description:**
* **Circuit type:** 1-Bit Parity Generator Cell (Block Diagram)
* **Inputs:** Cascading parity inputs from previous stage: $Pi-1, \overline{Pi-1}$ (entering from the left). Current bit inputs: $Ai, \overline{Ai}$ (entering from the bottom).
* **Outputs:** Updated cascading parity to next stage: $Pi, \overline{Pi}$ (exiting to the right).
* **Signal flow:** Calculates the next cumulative parity state based on the previous state and the current input bit.
* **Logic operation:** If $A_i=1$, the parity state toggles ($P_i = \overline{P_{i-1}}$). If $A_i=0$, the parity state remains the same ($P_i = P_{i-1}$). This gives the Boolean XOR function: $P_i = \overline{P_{i-1}}A_i + P_{i-1}\overline{A_i}$.

PG- basic one bit cell
* A standard or basic one -bit cell from which an n-bit parity generator may be formed.
$A_i=1$, Parity is changed, $P_i=\overline{P_{i-1}}$
$A_i=0$, Parity is unchanged, $P_i=P_{i-1}$

$$
P_i=\overline{P_{i-1}}A_i+P_{i-1}\overline{A_i}
$$

---
Page 15
---

![nMOS Logic Circuit: Parity Generator 1-Bit Cell]

**Description:**
* **Circuit type:** Depletion-load nMOS Parity Generator 1-Bit Cell
* **PMOS transistors:** None (nMOS technology).
* **NMOS transistors:** One depletion-mode nMOS acting as a pull-up load (gate tied to its source at node $\overline{P_i}$). Four enhancement-mode nMOS transistors forming the pull-down evaluation network. Two additional enhancement-mode nMOS transistors forming a static inverter to generate $P_i$ from $\overline{P_i}$ (one pull-up depletion, one pull-down enhancement).
* **Inputs:** $P_{i-1}$, $\overline{P_{i-1}}$, $A_i$, $\overline{A_i}$.
* **Outputs:** $\overline{P_i}$ (intermediate node), $P_i$ (final buffered output).
* **Internal nodes:** The evaluation node $\overline{P_i}$ between the depletion pull-up and the logic pull-down network.
* **VDD connection:** Drains of both depletion-mode pull-up transistors.
* **GND connection:** Sources of the bottom-most nMOS evaluation transistors.
* **Pull-up network:** Depletion-mode nMOS transistors.
* **Pull-down network:** Two parallel branches: Branch 1 has $\overline{P_{i-1}}$ in series with $A_i$. Branch 2 has $P_{i-1}$ in series with $\overline{A_i}$.
* **Signal flow:** Inputs configure the pull-down path. If either ($A_i=1$ AND $P_{i-1}=0$) OR ($A_i=0$ AND $P_{i-1}=1$) is true, the pull-down network conducts, driving $\overline{P_i}$ LOW. The static inverter then flips this to drive $P_i$ HIGH.
* **Logic operation:** Implements $P_i = \overline{P_{i-1}}A_i + P_{i-1}\overline{A_i}$.

![Stick Diagram: Parity Generator 1-Bit Cell nMOS]

**Description:**
* **VDD rail:** Blue horizontal line at the top.
* **GND rail:** Blue horizontal line at the bottom.
* **Polysilicon lines:** Red vertical lines for the gate inputs ($A_i, \overline{P_{i-1}}, P_{i-1}, \overline{A_i}$). Red connections for the depletion pull-up gates tied to the n-diffusion nodes.
* **n-diffusion:** Green lines forming the pull-down network paths. The layout clearly shows two parallel n-diffusion branches between the $\overline{P_i}$ node and GND, crossed by the poly gate lines.
* **p-diffusion:** N/A (nMOS process).
* **Metal layers:** Blue lines used for routing the output $\overline{P_i}$ over to the input of the inverter stage, and for VDD/GND rails.
* **Contacts:** Black dots indicating connections between metal and diffusion, or metal and poly.
* **Depletion Implant:** Yellow boxes surrounding the pull-up transistors, indicating they are depletion-mode devices.
* **Signal routing:** The inputs cross the n-diffusion vertically. The intermediate $\overline{P_i}$ node is routed via metal to the gate of the inverter's pull-down transistor to generate $P_i$.

PG - nmos circuit and Stick diagram

$$
P_i=\overline{P_{i-1}}A_i+P_{i-1}\overline{A_i}
$$

---
Page 16
---

![Stick Diagram: Parity Generator 1-Bit Cell CMOS]

**Description:**
* **VDD rail:** Blue horizontal line at the top.
* **GND rail:** Blue horizontal line at the bottom.
* **Polysilicon lines:** Red vertical lines for the input signals $A_i, \overline{P_{i-1}}, P_{i-1}, \overline{A_i}$. These lines cross both the p-diffusion (top) and n-diffusion (bottom).
* **n-diffusion:** Green horizontal path at the bottom near GND forming the n-block.
* **p-diffusion:** Yellow horizontal path at the top near VDD forming the p-block.
* **Metal layers:** Blue horizontal lines routing intermediate signals, connecting the p-block and n-block outputs to form $\overline{P_i}$, and connecting to the inverter stage.
* **Contacts:** Black dots showing connections between metal and diffusions/poly.
* **Signal routing:** A demarcation line (dashed) separates the p-devices from the n-devices. The complex gate logic output is routed to a standard CMOS inverter (right side) to produce the true $P_i$ output.

![CMOS Logic Circuit: Parity Generator 1-Bit Cell]

**Description:**
* **Circuit type:** Standard Complementary CMOS Parity Generator Cell (XNOR/XOR stage)
* **PMOS transistors:** Four pMOS transistors forming the pull-up p-block network. Arranged in a complex series-parallel combination to implement the complement of the pull-down logic. Two additional pMOS for the inverter (wait, the drawing shows standard CMOS but handwritten loops indicate transistor placement logic for Euler paths).
* **NMOS transistors:** Four nMOS transistors forming the pull-down n-block. Arranged in two parallel branches, each with two transistors in series. One additional nMOS for the output inverter.
* **Inputs:** $A_i, \overline{A_i}, P_{i-1}, \overline{P_{i-1}}$.
* **Outputs:** Intermediate $\overline{P_i}$, Final buffered $P_i$.
* **Internal nodes:** The node connecting the p-block and n-block.
* **VDD connection:** Top of the p-block.
* **GND connection:** Bottom of the n-block.
* **Pull-up network:** pMOS network implementing the dual of the XOR function to properly pull-up $\overline{P_i}$ to $V_{DD}$ when the XOR conditions are false.
* **Pull-down network:** nMOS network with one branch having $A_i$ and $\overline{P_{i-1}}$ in series, and the other having $\overline{A_i}$ and $P_{i-1}$ in series.
* **Signal flow:** Inputs modulate the complementary networks. The output of the complex gate $\overline{P_i}$ is inverted by a static CMOS inverter to output $P_i$.
* **Logic operation:** The n-block evaluates $P_i = \overline{P_{i-1}}A_i + P_{i-1}\overline{A_i}$. The diagram includes red arrows indicating Euler paths (a common layout optimization technique) drawn over the schematic to map it to the stick diagram efficiently.

PG-CMOS circuit and stick diagram

$$
P_i = \overline{P_{i-1}}A_i + P_{i-1}\overline{A_i}
$$

---
Page 17
---

* When converting stick diagram to layout
* boundary is set so that no design rule violations occur when cells are butted together
* avoid wastage of area

---
Page 18
---

![Multiplexer: 4:1 MUX Block Diagram]

**Description:**
* **Inputs:** Four data input lines $I_0, I_1, I_2, I_3$.
* **Select lines:** Two select control lines $S_1, S_0$.
* **Output:** One output line $f$.
* **Logic operation:** Routes one of the four data inputs to the output based on the binary value of the select lines.

Multiplexer(data selectors)

| $S_1$ | $S_0$ | $I_0$ | $I_1$ | $I_2$ | $I_3$ | f |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | X | X | X | 0 |
| 0 | 0 | 1 | X | X | X | $I_0$ |
| 0 | 1 | X | 0 | X | X | 0 |
| 0 | 1 | X | 1 | X | X | $I_1$ |
| 1 | 0 | X | X | 0 | X | 0 |
| 1 | 0 | X | X | 1 | X | $I_2$ |
| 1 | 1 | X | X | X | 0 | 0 |
| 1 | 1 | X | X | X | 1 | $I_3$ |

$$
f=\overline{S_1}\overline{S_0}I_0+\overline{S_1}S_0I_1+S_1\overline{S_0}I_2+S_1S_0I_3
$$

---
Page 19
---

![Stick Diagram: 4:1 MUX nMOS]

**Description:**
* **VDD rail:** N/A (pass transistor switch matrix).
* **GND rail:** N/A.
* **Polysilicon lines:** Four vertical red lines representing the select control signals $S_1, \overline{S_1}, S_0, \overline{S_0}$.
* **n-diffusion:** Four horizontal green/yellow lines for the data inputs $I_0, I_1, I_2, I_3$ passing through the matrix.
* **p-diffusion:** N/A.
* **Metal layers:** Output collection node routing the four row outputs together to $f$.
* **Contacts:** Black dots showing where active pass transistors are placed by crossing poly over diffusion.
* **Signal routing:** Forms a crossbar-like structure where specific combinations of vertical select lines activate two series pass transistors per row to allow a data line to propagate to $f$.

![nMOS Logic Circuit: 4:1 MUX]

**Description:**
* **Circuit type:** 4:1 Multiplexer using nMOS Pass Transistor Logic
* **PMOS transistors:** None.
* **NMOS transistors:** Eight nMOS pass transistors arranged in a 4x2 matrix.
* **Inputs:** Four horizontal data lines $I_0, I_1, I_2, I_3$.
* **Select lines:** Vertical gate control lines $S_1, \overline{S_1}, S_0, \overline{S_0}$.
* **Output:** A single common output node $f$ tying all four rows together.
* **Internal nodes:** Intermediate nodes between the two pass transistors in each row.
* **Pull-up network:** N/A.
* **Pull-down network:** N/A.
* **Signal flow:** Signals propagate from left ($I_X$) to right ($f$).
    * Row 0 ($I_0$) passes through transistors gated by $\overline{S_1}$ and $\overline{S_0}$.
    * Row 1 ($I_1$) passes through transistors gated by $\overline{S_1}$ and $S_0$.
    * Row 2 ($I_2$) passes through transistors gated by $S_1$ and $\overline{S_0}$.
    * Row 3 ($I_3$) passes through transistors gated by $S_1$ and $S_0$.
* **Logic operation:** Implements the MUX function: $f = \overline{S_1}\overline{S_0}I_0 + \overline{S_1}S_0I_1 + S_1\overline{S_0}I_2 + S_1S_0I_3$. This topology uses simple nMOS series pass transistors, which will degrade the logic 1 levels.

4:1 MUX-NMOS circuit & stick diagram
$$
f=\overline{S_1}\overline{S_0}I_0+\overline{S_1}S_0I_1+S_1\overline{S_0}I_2+S_1S_0I_3
$$

---
Page 20
---

![Transmission Gate Logic: 4:1 MUX CMOS Circuit]

**Description:**
* **Circuit type:** 4:1 Multiplexer using CMOS Transmission Gates
* **PMOS transistors:** Eight pMOS transistors, one for each transmission gate in the 4x2 array.
* **NMOS transistors:** Eight nMOS transistors, one for each transmission gate.
* **Inputs:** Four data input lines $I_0, I_1, I_2, I_3$.
* **Select lines:** Complementary pairs for each control stage ($S_1, \overline{S_1}$ and $S_0, \overline{S_0}$) driving the transmission gates.
* **Output:** A single common output node $f$ combining the four branches.
* **Internal nodes:** Intermediate nodes between the two transmission gates in each row branch.
* **Signal flow:** Same logical flow as the nMOS version, but each switch is replaced by a full transmission gate to ensure no voltage drop.
    * $I_0$ path gated by TGs controlled by $\overline{S_1}$ and $\overline{S_0}$.
    * $I_1$ path gated by TGs controlled by $\overline{S_1}$ and $S_0$.
    * $I_2$ path gated by TGs controlled by $S_1$ and $\overline{S_0}$.
    * $I_3$ path gated by TGs controlled by $S_1$ and $S_0$.
* **Logic operation:** Full swing, non-degraded multiplexing of 4 inputs to 1 output.

![Stick Diagram: 4:1 MUX CMOS]

**Description:**
* **VDD rail:** Blue horizontal line at top and bottom.
* **GND rail:** N/A explicitly, but standard CMOS rails imply upper VDD and lower GND.
* **Polysilicon lines:** Vertical red lines representing $S_1, \overline{S_1}, S_0, \overline{S_0}$. These cross the horizontal diffusions to form the transistor gates.
* **n-diffusion:** Horizontal black/dark green lines in the lower half (below the dashed demarcation line) forming the nMOS switches.
* **p-diffusion:** Horizontal yellow lines in the upper half forming the pMOS switches.
* **Metal layers:** Blue vertical wire on the far right collecting the outputs of all paths into a single node $f$.
* **Contacts:** Black dots indicating connections between the diffusion paths and the vertical metal collection wire.
* **Signal routing:** Input signals $I_0, I_1, I_2, I_3$ are fed into the left side of BOTH the p-diffusion and n-diffusion tracks. The vertical poly lines act as gates for both n and p devices simultaneously to form the physical transmission gates.

4:1 MUX- CMOS circuit & stick diagram
$$
f=\overline{S_1}\overline{S_0}I_0+\overline{S_1}S_0I_1+S_1\overline{S_0}I_2+S_1S_0I_3
$$

---
Page 21
---

![Stick Diagram: 4:1 MUX Layout Variations]

**Description:**
* **Circuit type:** Further layout variations of the 4:1 CMOS MUX stick diagram shown on the previous page.
* **Polysilicon lines:** Red vertical lines ($S_1, \overline{S_1}, S_0, \overline{S_0}$).
* **n-diffusion:** Black/dark green horizontal segments (bottom).
* **p-diffusion:** Yellow horizontal segments (top).
* **Metal layers:** Blue lines handling the complex interconnects. The top diagram shows a basic parallel combination. The bottom diagram shows a highly optimized topology where intermediate metal routing (blue zig-zags) is used to connect p-diffusion segments and n-diffusion segments efficiently, minimizing area and capacitance.
* **Signal routing:** Shows how the physical layout designer maps the transmission gate schematic onto a dense silicon footprint using interlaced metal and diffusion segments.

---
Page 22
---

![Stick Diagram: 4:1 MUX Formal Stick Diagram]

**Description:**
* **VDD rail:** Blue horizontal line at the top, labeled $V_{DD}$.
* **GND rail:** Blue horizontal line at the bottom, labeled $V_{SS}$.
* **Polysilicon lines:** Four red vertical lines representing the select inputs $S_1, \overline{S_1}, S_0, \overline{S_0}$.
* **p-diffusion:** Four yellow horizontal lines above the dashed demarcation line. Inputs $I_0, I_1, I_2, I_3$ enter from the left.
* **n-diffusion:** Four green horizontal lines below the dashed demarcation line. Inputs $I_0, I_1, I_2, I_3$ also enter these from the left (often strapped to the p-diffusion inputs via metal, not explicitly shown).
* **Metal layers:**
    * Blue horizontal lines representing VDD and VSS rails.
    * Blue lines connecting the drains of the corresponding p-diffusion and n-diffusion paths.
    * Blue vertical line on the right side collecting all branches to the final output node (O/P).
* **Contacts:** Black dots showing connections between the active diffusion regions and the metal interconnects.
* **Vias:** Black 'X' marks indicating connections between poly and metal, or diffusion and metal. Here, they connect specific points to VDD or VSS, perhaps for substrate taps or tie-offs.
* **Device placement:** pMOS devices are strictly segregated to the top half, nMOS to the bottom half, separated by a distinct Demarcation line, enforcing standard CMOS layout rules.

---
Page 23
---

## Design of a 4-bit shifter

| | | | | |
|---|---|---|---|---|
| **SR-O** | 1 | 0 | 1 | 1 |
| **SL-0** | 1 | 0 | 1 | 1 |
| **SR-1** | 1 | 1 | 0 | 1 |
| **SL-1** | 0 | 1 | 1 | 1 |
| **SR-2** | 1 | 1 | 1 | 0 |
| **SL-2** | 1 | 1 | 1 | 0 |
| **SR-3** | 0 | 1 | 1 | 1 |
| **SL-3** | 1 | 1 | 0 | 1 |

---
Page 24
---

## Design of a 4-bit shifter

| | | | | |
|---|---|---|---|---|
| **SR-0** | 1 | 0 | 1 | 1 |
| **SL-0** | 1 | 0 | 1 | 1 |
| **SR-1** | 1 | 1 | 0 | 1 |
| **SL-1** | 0 | 1 | 1 | 1 |
| **SR-2** | 1 | 1 | 1 | 0 |
| **SL-2** | 1 | 1 | 1 | 0 |
| **SR-3** | 0 | 1 | 1 | 1 |
| **SL-3** | 1 | 1 | 0 | 1 |

---
Page 25
---

## Design of a 4-bit shifter

* The n-bit shifter should shift the incoming data by up to $(n-1)$ places in right shift or left shift direction
* The shifts should be on an 'end-around' basis
* Shifter must have
* Input from a four-line parallel data bus
* Four output lines for the shifted data
* Means of transferring input data to output lines with any shift from 0 to 3 bits inclusive

---
Page 26
---

![Crossbar Switch: 4x4 Cross Bar Switch]

**Description:**
* **Circuit type:** 4x4 nMOS Crossbar Switch Matrix
* **PMOS transistors:** None.
* **NMOS transistors:** 16 nMOS pass transistors arranged in a 4x4 grid (labeled $SW_{00}$ to $SW_{33}$).
* **Input lines:** Four vertical data lines $in_0, in_1, in_2, in_3$ feeding into the sources of the nMOS transistors in each respective column.
* **Output lines:** Four horizontal data lines $out_0, out_1, out_2, out_3$ taking signals from the drains of the nMOS transistors in each respective row.
* **Switching structure:** Every intersection between a vertical input line and a horizontal output line contains an nMOS pass transistor.
* **Routing paths:** The diagram has light blue arrows tracing an example routing configuration.
    * $in_0$ routes to $out_3$ via $SW_{03}$.
    * $in_1$ routes to $out_0$ via $SW_{10}$.
    * $in_2$ routes to $out_1$ via $SW_{21}$.
    * $in_3$ routes to $out_2$ via $SW_{32}$.
* **Logic operation:** By activating the gate of specific transistors ($SW_{ij}$), any input can be routed to any output. The example configuration implements a logical shift.

4x4 Cross Bar Switch
out3 out2 out1 outo

---
Page 27
---

## 4x4 Cross Bar Switch

* Any input line can be connected to any output line
* If all switches activated then all outputs short circuited to all inputs
* Too many control signals-complexity undesirable

The following table:

| | Out3 | Out2 | Out1 | Out0 |
|---|---|---|---|---|
| **SLO** | SW33 | SW22 | SW11 | SW00 |
| **SRO** | In3 | In2 | In1 | In0 |
| **SL1** | SW23 | SW12 | SW01 | SW30 |
| **SR3** | In2 | In1 | In0 | In3 |
| **SL2** | SW13 | SW02 | SW31 | SW20 |
| **SR2** | In1 | In0 | In3 | In2 |
| **SL3** | SW03 | SW32 | SW21 | SW10 |
| **SRI** | In0 | In3 | In2 | In1 |

The following table:

| | Out3 | Out2 | Out1 | Out0 |
|---|---|---|---|---|
| **SLO** | SW33 | SW22 | SW11 | SW00 |
| **SRO** | 0 | 1 | 1 | 1 |
| **SL1** | SW23 | SW12 | SW01 | SW30 |
| **SR3** | 0 | 1 | 1 | 1 |
| **SL2** | SW13 | SW02 | SW31 | SW20 |
| **SR2** | 1 | 1 | 1 | 0 |
| **SL3** | SW03 | SW32 | SW21 | SW10 |
| **SR1** | 0 | 1 | 1 | 1 |

---
Page 28
---

![Barrel Shifter: 4x4 Barrel Shifter Schematic]

**Description:**
* **Circuit type:** 4x4 Barrel Shifter (nMOS Pass Transistor Matrix)
* **PMOS transistors:** None.
* **NMOS transistors:** 16 nMOS pass transistors.
* **Input lines:** Data inputs $in_0, in_1, in_2, in_3$ enter vertically from the bottom. Notice that the vertical lines wrap around, forming a barrel architecture.
* **Output lines:** Data outputs $out_0, out_1, out_2, out_3$ exit horizontally to the right.
* **Control signals:** Shift control lines $sh_0, sh_1, sh_2, sh_3$. Unlike the full crossbar which requires 16 independent control lines, the barrel shifter ties the gates of transistors diagonally.
* **Switching structure:**
    * Activating $sh_0$ connects $in_0 \rightarrow out_0, in_1 \rightarrow out_1, in_2 \rightarrow out_2, in_3 \rightarrow out_3$ (Shift by 0).
    * Activating $sh_1$ connects $in_0 \rightarrow out_1, in_1 \rightarrow out_2, in_2 \rightarrow out_3, in_3 \rightarrow out_0$ (Shift by 1).
    * And so on.
* **Routing paths:** The hardwired diagonal gate connections strictly enforce the rotational shifting behavior, vastly simplifying control logic compared to a full crossbar switch.

4x4 Barrel Shifter

---
Page 29
---

![Barrel Shifter: 4x4 Barrel Shifter Signal Flow]

**Description:**
* **Circuit type:** 4x4 Barrel Shifter (Signal routing illustration)
* **Input lines:** Bottom inputs $in_0=1, in_1=1, in_2=0, in_3=1$.
* **Output lines:** Right outputs $out_0=1, out_1=0, out_2=1, out_3=1$.
* **Control signals:** Shift control line $sh_1$ is activated (circled in green). $sh_0, sh_2, sh_3$ are inactive.
* **Routing paths:**
    * Blue arrow traces $in_0 (1) \rightarrow out_1 (1)$. (Wait, the diagram shows $out_0=1$. Let's trace carefully: $sh_1$ is active. The transistor connected to $in_3$ on the bottom right routes up and left to $out_0$. So $in_3 \rightarrow out_0$. The transistor connected to $in_0$ routes to $out_1$. So $in_0 \rightarrow out_1$. The transistor connected to $in_1$ routes to $out_2$. So $in_1 \rightarrow out_2$. The transistor connected to $in_2$ routes to $out_3$. So $in_2 \rightarrow out_3$.)
    * Blue/Red arrows physically trace these activated paths through the enabled diagonal set of pass transistors.
* **Logic operation:** Demonstrates a left shift by 1 position (or a right shift by 3). Input vector [1 0 1 1] (reading $in_3$ to $in_0$) becomes [0 1 1 1] at the output.

4x4 Barrel Shifter

---
Page 30
---

* Gates connected in staircase fashion
* 4 shift control signals sufficient for $4\times4$ shifter which must be mutually exclusive in active state
* Structure is of high regularity and generality
* Even though logic 1 levels at the output are degraded, they are passed through restoring logic
* The arrangement produces output at a faster rate as there is only one n-type switch between an input and the corresponding output line

The following table:

| | Out3 | Out2 | Out1 | Out0 |
|---|---|---|---|---|
| **SLO** | | | Sho | |
| **SRO** | In3 | In2 | In1 | In0 |
| **SL1** | | | Sh1 | |
| **SR3** | In2 | In1 | In0 | In3 |
| **SL2** | | | Sh2 | |
| **SR2** | In1 | In0 | In3 | In2 |
| **SL3** | | | Sh3 | |
| **SR1** | In0 | In3 | In2 | In1 |

The following table:

| | Out3 | Out2 | Out1 | Out0 |
|---|---|---|---|---|
| **SLO** | | | Sho | |
| **SRO** | 0 | 1 | 1 | 1 |
| **SL1** | | | Sh1 | |
| **SR3** | 0 | 1 | 1 | 1 |
| **SL2** | | | Sh2 | |
| **SR2** | 1 | 1 | 1 | 0 |
| **SL3** | | | Sh3 | |
| **SRI** | 0 | 1 | 1 | 1 |

---
Page 31
---

![Stick Diagram: 4x4 Barrel Shifter]

**Description:**
* **VDD rail:** N/A (Switch matrix).
* **GND rail:** N/A.
* **Polysilicon lines:** Vertical red lines representing the inputs $in_0, in_1, in_2, in_3$. Note that in this layout interpretation, inputs are applied to the vertical poly lines instead of diffusion, which differs from previous schematics. Wait, looking closer: The labels $in_0, in_1, in_2, in_3$ are on the horizontal metal lines (blue) on the left. The labels $out_0, out_1, out_2, out_3$ are on horizontal metal lines on the left as well. The control lines $Shift_0, Shift_1, Shift_2, Shift_3$ are vertical poly lines (red).
* **n-diffusion:** Green regions acting as the pass transistors.
* **Metal layers:** Horizontal blue lines carrying input data and output data.
* **Contacts:** Black dots where the input metal drops down to the source of an n-diffusion transistor, and where the drain of the n-diffusion transistor drops down to the output metal line.
* **Signal routing:** A very dense, regular array. The diagonal 'staircase' connection of the barrel shifter is implemented by shifting the physical location of the contacts for each column (shift control line), effectively rotating the connection between the horizontal input buses and the horizontal output buses.

Stick diagram for 4X4 barrel shifter
etc.

---
Page 32
---

thank You