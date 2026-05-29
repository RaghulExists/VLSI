---
Page 1
---

# MODULE -4

---
Page 2
---

![Datapath Diagram: 4-Bit Data Path for Processor]

**Description:**
* **Registers:** 4-bit registers block.
* **Buses:** Two 4-bit buses (A and B) connecting the 4-bit registers to the 4-bit ALU. A 4-bit bus (S) connecting the ALU to the 4-bit end around shifter. A feedback bus connecting the shifter output back to the Input/output port and 4-bit registers.
* **ALU:** 4-bit ALU block taking inputs A, B, and Carry in. Outputs S and Carry out.
* **Shifters:** 4-bit end around shifter block.
* **Adders:** Located inside the 4-bit ALU block (implied).
* **Multiplexers:** Implied inside the shifter and input/output routing.
* **Control signals:** Direction control (for Input/output port), Control (for registers), Clock (for registers), Control (for ALU), Clock (for ALU), Shift control (for 4-bit end around shifter).
* **Data flow directions:** Bidirectional between Input/output port and 4-bit registers. Unidirectional from registers to ALU (A, B). Unidirectional from ALU to shifter (S). Feedback from shifter back to the beginning of the datapath.
* **Interconnections:** Synchronized routing across the processor components for parallel data manipulation.

**Design of an ALU subsystem**

Discuss the 4 Bit data path for processor with block diagram

---
Page 3
---

The heart of the ALU is a 4-bit adder circuit and it is this which we will actually design, indicating later how it may be readily adapted to subtract and perform logical operations. Obviously, a 4-bit adder must take the sum of two 4-bit numbers, and it will be seen we have assumed that all 4-bit quantities are presented in parallel form and that the shifter circuit has been designed to accept and shift a 4-bit parallel sum from the ALU.

Let us now specify that the sum is to be stored in parallel at the output of the adder from where it may be fed through the shifter and back to the register array. Thus, a single 4-bit data bus is needed from the adder to the shifter and another 4-bit bus is required from the shifted output back to the register array (since the shifter is merely a switch array with no storage capability). As far as the input to the adder is concerned, the two 4-bit parallel numbers to be added are to be presented in parallel on two 4-bit buses. We can also decide on some of the basic aspects of system timing at this stage and will assume clock phase $\phi_{1}$ as being the phase during which signals are fed along buses to the adder input and during which their sum is stored at the adder output. Thus clock signals are required by the ALU as shown. The shifter is unclocked but must be connected to four shift control lines. It is also necessary to provide a 'carry out' signal from the adder and, in the general case, to provide for a possible 'carry in' signal, as indicated in Figure 8.1.

---
Page 4
---

| Ak | 1 | 1 | 0 | 1 |
|---|---|---|---|---|
| **Bk** | **1** | **0** | **0** | **1** |

**Design of a 4-bit Adder**

Illustrate the design of 4-bit adder with relevant equations for Sum & Carry.

The following table:

| Ck-1 | Ak | Bk | Sk | Ck |
|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 1 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 0 | - | - |

---
Page 5
---

* $S_{k}=\overline{A_{k}}\overline{B_{k}}C_{k-1}+\overline{A_{k}}B_{k}\overline{C_{k-1}}+A_{k}\overline{B_{k}}\overline{C_{k-1}}+A_{k}B_{k}C_{k}$
* $S_{k}=(\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}})\overline{C_{k-1}}+(\overline{A_{k}}\overline{B_{k}}+A_{k}B_{k})C_{k-1}$
* $S_{k}=(A_{k}\oplus B_{k})\overline{C_{k-1}}+(\overline{A_{k}\oplus B_{k}})C_{k-1}$
* $S_{k}=H_{k}\overline{C_{k-1}}+\overline{H_{k}}C_{k-1}$
* $C_{k}=\overline{A_{k}}B_{k}C_{k-1}+A_{k}\overline{B_{k}}C_{k-1}+A_{k}B_{k}\overline{C_{k-1}}+A_{k}B_{k}C_{k}$
* $C_{k}=(\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}})C_{k-1}+(C_{k-1}+\overline{C_{k-1}})A_{k}B_{k}$
* $C_{k}=(A_{k}\oplus B_{k})C_{k-1}+A_{k}B_{k}$
* $C_{k}=H_{k}C_{k-1}+A_{k}B_{k}$
* where $H_{k}=A_{k}\oplus B_{k}=\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}}$
* $0\le k\le n-1$

The following table:

| Inputs | | | Outputs | |
|---|---|---|---|---|
| **I/PA** | **I/P B** | **Previous carry** | **New Carry** | **Sum** |
| **Ak** | **Bk** | **Ck-1** | **Sk** | **Ck** |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

---
Page 6
---

* If $A_{k}=B_{k}$, then $S_{k}=C_{k-1}$

| Inputs | | | Outputs | |
|---|---|---|---|---|
| **I/PA** | **I/P B** | **Previous carry** | **New Carry** | **Sum** |
| **Ak** | **Bk** | **Ck-1** | **Sk** | **Ck** |
| 0 | 0 | 0 | 0 | 0 |

* If $A_{k}=B_{k}$, then $C_{k}=A_{k}=B_{k}$

| 0 | 0 | 1 | 1 | 0 |
|---|---|---|---|---|
| 0 | 1 | 0 | 1 | 0 |

* If $A_{k}=B_{k}$ $C_{k}=1$ when $A_{k}=B_{k}=1$

| 0 | 1 | 1 | 0 | 1 |
|---|---|---|---|---|
| 1 | 0 | 0 | 1 | 0 |

* and $C_{k}=0$ when $A_{k}=B_{k}=0$

| 1 | 0 | 1 | 0 | 1 |
|---|---|---|---|---|
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

---
Page 7
---

**Adder Element Requirements**

* If $A_{k}=B_{k}$, then $S_{k}=C_{k-1}$

| Inputs | | | Outputs | |
|---|---|---|---|---|
| **I/PA** | **I/P B** | **Previous carry** | **New Carry** | **Sum** |

* If $A_{k}\ne B_{k}$ then $S_{k}=\overline{C_{k-1}}$

| **Ak** | **Bk** | **Ck-1** | **Sk** | **Ck** |
|---|---|---|---|---|

* If $A_{k}=B_{k}$, then $C_{k}=A_{k}=B_{k}$

| 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|
| 0 | 0 | 1 | 1 | 0 |

* If $A_{k}=B_{k}$ $C_{k}=1$ when $A_{k}=B_{k}=1$

| 0 | 1 | 0 | 1 | 0 |
|---|---|---|---|---|

and $C_{k}=0$ when $A_{k}=B_{k}=0$

| 0 | 1 | 1 | 0 | 1 |
|---|---|---|---|---|
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |

* If $A_{k}\ne B_{k}$ then $C_{k}=C_{k-1}$

| 1 | 1 | 0 | 0 | 1 |
|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 1 |

---
Page 8
---

![Standard Adder Cell: Standard Adder Element]

**Description:**
* **Circuit type:** Standard Adder Cell Block Diagram
* **Inputs:** Ak (Operand A bit k), Bk (Operand B bit k), Carry in $Ck-1$ (Previous carry).
* **Outputs:** Sk (Sum bit k), Carry out $Ck$ (New carry).
* **Internal nodes:** Encapsulated within the "Adder element" block.
* **Signal flow:** Top input receives previous carry ($Ck-1$). Left inputs receive Ak and Bk. Output Sum ($Sk$) is generated to the right. Output Carry ($Ck$) propagates to the bottom.
* **Logic operation:** Computes full adder Boolean logic: $Sk = Ak \oplus Bk \oplus Ck-1$ and $Ck = (Ak \cdot Bk) + (Ck-1 \cdot (Ak \oplus Bk))$.

A standard adder element
Discuss a Standard Adder element.

---
Page 9
---

![Multiplexer: MUX based Adder Element]

**Description:**
* **Circuit type:** Multiplexer-Based Adder Element Block Diagram
* **Inputs:** $C_{k-1}$ tied to port '0', $\overline{C_{k-1}}$ tied to port '1'.
* **Select lines:** Select line $S_0$.
* **Output:** $Sk$ (Sum output).
* **Signal flow:** The select line $S_0$ determines whether $C_{k-1}$ or its inverse $\overline{C_{k-1}}$ is routed to the output $Sk$.
* **Logic operation:**
    When $S_0 = 0$ (meaning $A_k = B_k$), $Sk = C_{k-1}$.
    When $S_0 = 1$ (meaning $A_k \ne B_k$), $Sk = \overline{C_{k-1}}$.

Implementing Adder using Multiplexer

The following table:

| Inputs | | | Outputs | |
|---|---|---|---|---|
| **I/PA** | **I/P B** | **Previous carry** | **New Carry** | **Sum** |
| **Ak** | **Bk** | **Ck-1** | **Sk** | **Ck** |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

MUX based Adder element
$S_{0}=0$ when $A_{k}=B_{k}$
$S_{0}=1$ when $A_{k}\ne B_{k}$
$S_{0}=0$ i.e. $A_{k}=B_{k}$ then $C_{k}=A_{k}=B_{k}$
$S_{0}=1$ i.e. $A_{k}\ne B_{k}$ then $C_{k}=C_{k-1}$

---
Page 10
---

$A_{k}=B_{k}$ 0 MUX based

The following table:

| Inputs | | | Outputs | |
|---|---|---|---|---|
| **I/PA** | **I/P B** | **Previous carry** | **Sum** | **New Carry** |
| **Ak** | **Bk** | **Ck-1** | **Sk** | **Ck** |
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

Adder element
Select line
$S_{0}=0$ when $A_{k}=B_{k}$
$S_{0}=1$ when $A_{k}\ne B_{k}$
$S_{0}=0$ i.e. $A_{k}=B_{k}$ then $C_{k}=A_{k}=B_{k}$
$S_{0}=1$ i.e. $A_{k}\ne B_{k}$ then $C_{k}=C_{k-1}$

---
Page 11
---

![Circuit Diagram: Pass Transistor Adder Logic]

**Description:**
* **Circuit type:** Pass Transistor Logic / Transmission Gate based Adder Network
* **PMOS/NMOS transistors:** The handwritten schematic illustrates a generic pass-transistor grid (often implemented as complementary transmission gates in CMOS, or simple nMOS pass-transistors).
* **Inputs:**
    Horizontal signal lines: $A_k$, $\overline{A_k}$, $B_k$, $\overline{B_k}$.
    Vertical signal lines routing the carry: $C_{k-1}$.
* **Outputs:** $C_k$, $\overline{S_k}$ which is then inverted to $S_k$.
* **Internal nodes:** Control junctions where horizontal lines intersect vertical rails with pass transistors configured to implement the Boolean sum and carry functions.
* **VDD/GND connection:** Not explicitly tied to static VDD/GND rails, uses inputs as driving signals (characteristic of pass-transistor logic).
* **Pull-up/Pull-down network:** N/A, uses pass-transistor switching arrays instead of standard pull-up/pull-down networks.
* **Signal flow:** The primary inputs $A_k$ and $B_k$ (and their complements) gate the columns, selectively passing $C_{k-1}$ or its complement $\overline{C_{k-1}}$ to the Sum line ($S_k$), and generating the appropriate New Carry ($C_k$) down through the network grid.
* **Logic operation:** A structured layout implementation of a full adder. The multiplexing conditions derived earlier ($S_{0}=0$ for $A_{k}=B_{k}$ and $S_{0}=1$ for $A_{k}\ne B_{k}$) are physically instantiated here using pass switches to route the carry bits into sum and next-carry outputs.

Explain Multiplexer based adder logic with stored and buffered sum output.

---
Page 12
---

![Adder Architecture: 4-Bit Ripple Carry Adder]

**Description:**
* **Sum generation:** Individual $S_0, S_1, S_2, S_3$ bits are generated from each corresponding Adder element.
* **Carry generation:** Each adder element generates a carry output ($C_0, C_1, C_2, C_3$).
* **Carry propagation:** The carry ripples from the carry-out of stage $k$ to the carry-in of stage $k+1$. The first stage receives $C_{-1}$, and the final stage produces $C_3$.
* **Inputs:** Data bit pairs ($A_0, B_0$), ($A_1, B_1$), ($A_2, B_2$), ($A_3, B_3$) and initial carry $C_{-1}$.
* **Outputs:** Sum bits $S_0, S_1, S_2, S_3$ and final carry $C_3$.
* **Interconnections:** Two vertical bus lines for $V_{DD}$ and GND cross through all adder elements to provide power.

Design of 4 Bit Adder
Design a 4 Bit Adder by cascading four adder elements

---
Page 13
---

Implementing ALU functions with an adder

Implement ALU function with an adder for EXOR, EXNOR & OR operation.

Switching of carry line between 0 and 1 gives rise to 4 basic logical operations
* Arithmetic functions:
    $(A+B)$ & (A-B)
* Logic functions:
* $S_{k}=H_{k}\overline{C_{k-1}}+\overline{H_{k}}C_{k-1}$
* Case 1:
    $C_{k-1}=0$
    AND, OR, EX-OR & EX-NOR
* $S_{k}=H_{k}=\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}}=EX-OR$

---
Page 14
---

EX-NOR Operation
* $S_{k}=H_{k}\overline{C_{k-1}}+\overline{H_{k}}C_{k-1}$
Logic functions:
AND, OR, EX-OR & EX-NOR
* Case 2: $C_{k-1}=1$
* $S_{k}=\overline{H_{k}}=\overline{A_{k}}\overline{B_{k}}+A_{k}B_{k}=EX-NOR$

---
Page 15
---

AND Operation
Logic functions:
AND, OR, EX-OR & EX-NOR
* $C_{k}=H_{k}C_{k-1}+A_{k}B_{k}$
* Case 1:
    $C_{k-1}=0$
* $C_{k}=A_{k}B_{k}=AND$

---
Page 16
---

OR operation
* $C_{k}=H_{k}C_{k-1}+A_{k}B_{k}$
Logic functions:
AND, OR, EX-OR & EX-NOR
* Case 2:
    $C_{k-1}=1$
* $C_{k}=H_{k}+A_{k}B_{k}$
* $C_{k}=(\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}})+A_{k}B_{k}$
* $C_{k}=\overline{A_{k}}B_{k}+A_{k}$
$C_{k}=A_{k}+B_{k}=OR$

---
Page 17
---

![ALU Architecture: 4-Bit ALU Subsystem]

**Description:**
* **Registers:** N/A (Adder logic elements shown).
* **Buses:** Implied routing for input vectors A and B.
* **ALU:** Cascaded 4-bit ALU built from generic adder elements.
* **Adders:** Four individual bit-slice adder elements (Bit 0, Bit 1, Bit 2, Bit 3).
* **Control signals:** Carry in ($C_{-1}$ or similar) acts as the operation mode selector (0 or 1).
* **Data flow directions:** Inputs $A_k, B_k$ enter left-to-right. Carry propagates top-to-bottom ($C_0 \rightarrow C_1 \rightarrow C_2 \rightarrow C_3$). Outputs $S_0, S_1, S_2, S_3$ exit to the right. Power lines ($V_{DD}$ and $V_{SS}$) route through all cells vertically.
* **Interconnections:** Serial carry chain connects sequential adder stages.

FIGURE 8.12 4-bit ALU.

$S_{k}=H_{k}\overline{C_{k-1}}+\overline{H_{k}}C_{k-1}$
Case 1: $C_{k-1}=0$
$S_{k}=H_{k}=\overline{A_{k}}B_{k}+A_{k}\overline{B_{k}}=EX-OR$
* $C_{k}=H_{k}C_{k-1}+A_{k}B_{k}$
* Case 1:
    $C_{k-1}=0$
* $C_{k}=A_{k}B_{k}=AND$

---
Page 18
---

Explain the working principle of pseudo nMOS logic.

![Pseudo nMOS Logic: Inverter Cascade]

**Description:**
* **Circuit type:** Pseudo nMOS Logic
* **PMOS transistors:** Used exclusively as an active pull-up load, with the gate permanently tied to $V_{SS}$ (Ground) so it is always ON.
* **NMOS transistors:** Used in the pull-down n-block.
* **Inputs:** A, B, C (shown in a generic n-block representation). $V_{in}$ shown on the gate of the pull-down nMOS.
* **Outputs:** Z (from the n-block diagram) and $V_{out}$ (from the cascaded inverter diagram).
* **Internal nodes:** Connection between the PMOS drain and the n-block drain.
* **VDD connection:** Connected to the source of the PMOS transistor.
* **GND connection:** Connected to the source of the bottom NMOS transistor(s).
* **Pull-up network:** A single PMOS device acting as a resistive load.
* **Pull-down network:** A standard NMOS logic tree (e.g., an n-block or a single NMOS for an inverter).
* **Signal flow:** Input $V_{in}$ drives the NMOS gate. If NMOS is OFF, PMOS pulls output HIGH. If NMOS is ON, it overrides the weak PMOS to pull the output LOW.
* **Logic operation:** Ratioed logic. The logic function is evaluated by the n-block. The PMOS serves merely as a static pull-up replacement for depletion-mode loads found in older nMOS technologies.

---
Page 19
---

if we replace the depletion mode pull-up transistor of the standard nMOS circuits with a p-transistor with gate connected to Vss, we have a structure similar to the nMOS equivalent.
This approach to logic design is illustrated by the three-input Nand gate in Figure1.
Fig 2 which is a pseudo-nMOS inverter is being driven by another similar inverter, and we consider the conditions necessary to produce an output voltage of Vinv for an identical input voltage. As for the nMOS analysis, we consider the conditions for which $V_{inv}=V_{dd}/2.$
At this point the n-device is in saturation (i.e. $0 < V_{gsn} - V_{tn} < V_{dsn}$) and the p-device is operating in the resistive region (i.e. $0 < V_{dsp} < V_{gsp} - V_{tp}$).
Equating currents of the n-transistor and the p-transistor, and by suitable rearrangement of the resultant expression, we obtain

$$
V_{inv}=V_{in}+\frac{(2\mu_{p}/\mu_{n})^{1/2}[(-V_{DD}-V_{in})V_{dp}-V_{dip}^{2}]^{1/2}}{(Z_{p.u.}/Z_{p.d.})^{1/2}}
$$

$Z_{p.u.}=L_{p}/W_{p}$
$Z_{p.d.}=L_{n}/W_{n}$
$V_{inv}=0.5V_{DD}$
$V_{tn}=|V_{tp}|=0.2V_{DD}$
$V_{DD}=5~V$
$\mu_{n}=2.5~\mu_{p}$
$\frac{Z_{p.u.}}{Z_{p.d.}}=\frac{3}{1}$

---
Page 20
---

![CMOS Logic Circuit: Dynamic CMOS 3-Input NAND Gate]

**Description:**
* **Circuit type:** Dynamic CMOS Logic
* **PMOS transistors:** 1 Precharge transistor (labeled 'p') connected to $V_{DD}$.
* **NMOS transistors:** 3 Evaluation transistors in the n-block (inputs A, B, C) in series with 1 clocked foot transistor (labeled 'n') connected to $V_{SS}$.
* **Inputs:** A, B, C (logic inputs), and $\phi$ (clock).
* **Outputs:** Z (dynamic node).
* **Internal nodes:** Node between precharge p-MOS and the top of the n-block (Z). Node between the bottom of the n-block and the clocked n-MOS foot.
* **VDD connection:** Source of the precharge p-MOS.
* **GND connection:** Source of the clocked n-MOS.
* **Pull-up network:** A single p-MOS transistor controlled by the clock signal $\phi$.
* **Pull-down network:** Series n-MOS logic block (implementing NAND) stacked above a clocked evaluate n-MOS.
* **Signal flow:** When $\phi = 0$ (Precharge), the p-MOS turns ON, pulling Z to $V_{DD}$. The evaluation n-MOS is OFF, preventing static power dissipation. When $\phi = 1$ (Evaluate), the p-MOS turns OFF, and the bottom n-MOS turns ON. If A=B=C=1, Z discharges to $V_{SS}$.
* **Logic operation:** Dynamic 3-input NAND gate.
* **Clock signals:** $\phi$ drives both the precharge and the evaluation phase.

![Timing Diagram: 4-Phase Clocks]

**Description:**
* **Signal names:** $\phi_1, \phi_2, \phi_3, \phi_4$ (primary phases) and derived clocks $\phi_{12}, \phi_{23}, \phi_{34}, \phi_{41}$.
* **Clock edges:** Overlapping clock edge generation to establish cascaded dynamic pipeline evaluation states.
* **State transitions:** Shows multi-phase clocking schemes used to mitigate charge sharing and race conditions in dynamic cascaded structures.

Dynamic CMOS Logic
(a) Schematic
(b) Possible $4\phi$ and derived clocks

---
Page 21
---

![Transmission Gate Logic: Dynamic Gate with TG Output]

**Description:**
* **Circuit type:** Dynamic Logic with Transmission Gate Latch
* **PMOS transistors:** Precharge p-MOS driven by $\phi_{12}$. p-MOS in the transmission gate driven by $\overline{\phi_{23}}$.
* **NMOS transistors:** n-block evaluation transistors, clocked foot n-MOS driven by $\phi_{12}$. n-MOS in the transmission gate driven by $\phi_{23}$.
* **Inputs:** Logic inputs A, B, C. Evaluate clock $\phi_{12}$. TG latch clock $\phi_{23}$.
* **Outputs:** Z (latched output).
* **Internal nodes:** Dynamic precharged node before the transmission gate.
* **VDD connection:** Source of precharge p-MOS.
* **GND connection:** Source of foot evaluate n-MOS.
* **Pull-up structures:** Clocked p-MOS precharge device.
* **Pull-down structures:** Logic n-block + clocked evaluate n-MOS.
* **Signal propagation paths:** Evaluates during $\phi_{12}$. Result propagates to output Z strictly during the active window of the transmission gate clock $\phi_{23}$.
* **Operation:** A pipeline strategy that holds the evaluated state while the subsequent stage evaluates to avoid race conditions.

TABLE 6.1 Dynamic logic types and sequences

| Gate type | Evaluate clock | Transmission gate clock | Allowable next types |
|---|---|---|---|
| Type 1 | $\phi_{12}$ | $\phi_{23}$ | Types 2 or 3 |
| Type 2 | $\phi_{23}$ | $\phi_{34}$ | Types 3 or 4 |
| Type 3 | $\phi_{34}$ | $\phi_{41}$ | Types 4 or 1 |
| Type 4 | $\phi_{41}$ | $\phi_{12}$ | Types 1 or 2 |

---
Page 22
---

The actual logic (see Figure 6.1l(a) for the schematic arrangement) is implemented in the inherently faster nMOS logic (the n-block); a p-transistor is used for the non-time-critical precharging of the output line 'Z' so that the output capacitance is charged to Vdd during the off period of the clock signal <ø>. During this same period the inputs are applied to the n-block and the state of the logic is then evaluated during the on period of the clock when the bottom n-transistor is turned on.

Note the following:
1.  Charge sharing may be a problem unless the inputs are constrained' not to change during the on period of the clock.
2.  Single phase dynamic logic structures cannot be cascaded since, owing to circuit delays, an incorrect input to the next stage may be present when evaluation begins, so that its output is inadvertently discharged and the wrong output results.

One remedy is to employ a four-phase clock in which the actual signals used are the derived clocks $\phi_{12}, \phi_{23}, \phi_{34},$ and $\phi_{41}$ as illustrated in Figure 6.1l(b). The basic circuit of Figure 6.ll(a) is modified by the inclusion of a transmission gate as in Figure 6.ll(c), the function of which is to sample the output during the 'evaluate' period and to hold the output state while the next stage logic evaluates. For this strategy to work, the next stage must operate on overlapping but later clock signals. Clearly, since there are four different derived clock signals which are used in sequential pairs

---
Page 23
---

![Domino Logic: CMOS Domino Gate]

**Description:**
* **Circuit type:** CMOS Domino Logic
* **PMOS transistors:** 1 clocked precharge p-MOS on the dynamic node. 1 p-MOS inside the static inverter.
* **NMOS transistors:** n-block logic network. 1 clocked foot n-MOS. 1 n-MOS inside the static inverter.
* **Inputs:** Logic inputs to the n-block, clock signal $\phi$.
* **Outputs:** Z (from the static inverter).
* **Internal nodes:** Dynamic precharge node located between the p-MOS precharge device and the n-block.
* **VDD connection:** Source of the precharge p-MOS and source of the static inverter's p-MOS.
* **GND connection:** Source of the evaluate foot n-MOS and source of the static inverter's n-MOS.
* **Pull-up network:** Clock-controlled precharge p-MOS.
* **Pull-down network:** n-block logic tree + clocked evaluate n-MOS.
* **Signal flow:** Clock $\phi=0$: dynamic node is precharged to $V_{DD}$, so Z is driven to $V_{SS}$ (LOW). Clock $\phi=1$: evaluation occurs. If n-block conducts, dynamic node discharges, driving Z HIGH.
* **Logic operation:** Non-inverting Domino logic gate. The static inverter ensures a strictly $0 \rightarrow 1$ transition during evaluation, enabling single-phase cascading without race conditions.

CMOS Domino Logic
This modified arrangement allows for the cascading of logic structures using only a single phase clock.
This requires a static CMOS buffer in each logic gate.

---
Page 24
---

* The following remarks will help to place this type of logic in the scheme of things:
* 1. Such logic structures can have smaller areas than conventional CMOS logic.
* 2. Parasitic capacitances are smaller so that higher operating speeds are possible.
* 3. Operation is free of glitches since each gate can make only one '1' to '0' transition.
* 4. Only non-inverting structures are possible because of the presence of the inverting buffer.
* 5. Charge distribution may be a problem and must be considered.

---
Page 25
---

![Clocked CMOS (C2MOS): Clocked Logic Gates]

**Description:**
* **Circuit type:** Clocked CMOS (C2MOS) Logic
* **PMOS transistors:** (Left Diagram): 2-input p-block (series A and B). 1 clocked p-MOS driven by $\overline{\phi}$. (Right Diagram): 1 logic p-MOS, 1 clocked p-MOS driven by $\overline{\phi}$.
* **NMOS transistors:** (Left Diagram): 2-input n-block (parallel A and B). 1 clocked n-MOS driven by $\phi$. (Right Diagram): 1 logic n-MOS, 1 clocked n-MOS driven by $\phi$.
* **Inputs:** Logic inputs A, B, and clock signals $\phi, \overline{\phi}$.
* **Outputs:** Z (for both circuits).
* **Internal nodes:** The evaluation path is only complete when the clocked transistors in the middle of the stack are activated.
* **VDD connection:** Top of the p-block network.
* **GND connection:** Bottom of the n-block network.
* **Pull-up network:** p-block logic network stacked with a clock-enabled p-MOS.
* **Pull-down network:** clock-enabled n-MOS stacked with an n-block logic network.
* **Signal flow:** Output Z isolates (high impedance) when clocks are inactive ($\phi=0, \overline{\phi}=1$). Output Z is driven by the logic blocks only when $\phi=1, \overline{\phi}=0$.
* **Logic operation:** (a) 2-input Clocked NOR gate. (b) Clocked Inverter.

Clocked CMOS logic
(a) $2~I/P$ Nor gate
(b) Inverter

---
Page 26
---

Clocked CMOS logic
* The logic is implemented in both n-and p-transistors in the form of a pull-up p-block and a complementary n-block pull down structure.
* (Figure 6.12(a)), as for the inverter-based CMOS logic discussed earlier. However, the logic in this case is evaluated (connected to the output) only during the on period of the clock.
* As might be expected, a clocked inverter circuit forms part of this family of logic as shown in Figure 6.12(b).
* Owing to the extra transistors in series with the output, slower rise-times and fall-times can be expected.

---
Page 27
---

![n-p CMOS Logic: NORA Logic Pipeline]

**Description:**
* **Circuit type:** N-P CMOS Logic (NORA - No Race logic).
* **PMOS transistors:** Stage 1: Precharge p-MOS. Stage 2: p-block logic network. Stage 3: Precharge p-MOS.
* **NMOS transistors:** Stage 1: n-block logic network. Stage 2: Precharge n-MOS. Stage 3: n-block logic network.
* **Inputs:** Logic inputs feeding the n-blocks and p-blocks. Clock signals $\phi$ and $\overline{\phi}$.
* **Outputs:** Cascaded outputs feeding the next dynamic logic block.
* **VDD connection:** Source of precharge p-MOS devices and logic p-blocks.
* **GND connection:** Source of logic n-blocks and precharge n-MOS devices.
* **Pull-up structures:** Alternates between a single precharge p-MOS (in $\phi$-stages) and a complex p-block (in $\overline{\phi}$-stages).
* **Pull-down structures:** Alternates between a complex n-block (in $\phi$-stages) and a single precharge n-MOS (in $\overline{\phi}$-stages).
* **Signal propagation paths:** Evaluated signal cascades directly from an n-block stage into a p-block stage, then back to an n-block stage without any intervening static inverters.
* **Clock signals:** Clock phases rigidly alternate to prevent evaluation overlap.

N-P CMOS Logic
FIGURE 6.14 n-p CMOS logic.
etc

---
Page 28
---

* This is another variation of basic dynamic logic arrangement, in which the actual logic blocks are alternately 'n' and 'p' in a cascaded structure.
* The precharge and evaluate transistors are fed from the clock and clockbar ($\overline{\phi}$) alternately, and clearly the functions of the top and bottom transistors also alternate, between precharge and evaluate.