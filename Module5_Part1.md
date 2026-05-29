---
Page 1
---

DRAM (Dynamic Random Access Memory)

It is a type of RAM which stores each bit of data in a memory cell, which consist of a small capacitor and a MOSFET.
→ Capacitor is used as a memory element. The capacitor can either be charged or discharged; these two states are called 0 & 1.
The electric charge on capacitors gradually leaks away, which may cause loss of data. To prevent this DRAM requires an external memory refresh circuit which periodically rewrites the data in the capacitors, restoring them to their original charge.

**1T DRAM**

![Memory Cell: 1T DRAM]

**Description:**
* **Storage node(s):** Single capacitor ($C_S$) serving as the storage element.
* **Access transistor(s):** Single NMOS pass transistor.
* **Word line(s):** Word line (WL) connected to the gate of the NMOS transistor.
* **Bit line(s):** Bit line (BL) connected to the drain of the NMOS transistor.
* **Read path:** When WL is asserted HIGH, the NMOS turns ON, allowing the charge on $C_S$ to be shared with the Bit line capacitance ($C_L$), which is then sensed by a sense amplifier.
* **Write path:** When WL is asserted HIGH, the Bit line (BL) is driven to the desired voltage level ($V_{DD}$ or GND) to charge or discharge $C_S$.
* **Feedback structure:** None (Dynamic, requires periodic refresh).
* **Cell operation:** 1) 1-Transistor consist of mainly one Capacitor ($C_S$) as a storage element.
    2) Here transistor will be a Mos pass transistor.
    3) Word line (WL) is required to define the operation of memory circuit. For Read/Write operation it must be high.
    4) Data will be supplied by precharge bit line.
    5) The gate of NMOS will be connected with Word line.
    6) Drain of NMOS will be connected with Bit line (BL).

---
Page 2
---

**Read Operation (BL will be acted as Output line)**
1) $C_L = \frac{V_{DD}}{2}$
2) $WL = 1$
   $M = ON$
3) If $C_S$ at 1V (If $C_S$ has the Voltage)
   Then Voltage at Bit line will be greater than $\frac{V_{DD}}{2}$
   $$V > \frac{V_{DD}}{2} = BL$$
   It send to Sense amplifier & $O/P = 1$
   again if $V < \frac{V_{DD}}{2}$
   It send to Sense amplifier & $O/P = 0$
4) Refreshed cell

**Write Operation (BL will be acted as Input line)**
1) $BL = \text{Input}$
2) $WL = 1$
3) If $BL = 1$ then $C_S = V_{DD}$ (Charging)
   If $BL = 0$ then $C_S = 0$ (Discharging)

---
Page 3
---

**3T Dynamic RAM Cell $\rightarrow$**

![Memory Cell: 3T DRAM]

**Description:**
* **Storage node(s):** Parasitic capacitance ($C_S$) at the gate of transistor M2.
* **Access transistor(s):** Transistor M1 acts as the write access transistor. Transistor M3 acts as the read access transistor. Transistor M2 acts as the read evaluation transistor.
* **Word line(s):** WL (Write Line) controls transistor M1.
* **Bit line(s):** RL (Read Line) controls transistor M3. $\overline{BL}$ (Write Bit Line) feeds data into M1. BL (Read Bit Line) outputs data from M3.
* **Read path:** During a read, RL is HIGH, turning ON M3. If $C_S$ holds a logic 1, M2 is ON, providing a discharge path from BL (which is precharged HIGH) to ground. If $C_S$ is 0, M2 is OFF and BL remains HIGH.
* **Write path:** During a write, WL is HIGH, turning ON M1. The value on $\overline{BL}$ is passed to the gate of M2, charging or discharging $C_S$.
* **Feedback structure:** None (Dynamic memory, requires refresh).
* **Cell operation:** $\rightarrow$ In 3T-DRAM, two separate control lines will be used for Read operation (RL: Read line) & Write operation (WL: Write line).
    $\rightarrow$ BL & $\overline{BL}$, Precharged through capacitor $C_1$ & $C_2$ respectively.
    $\rightarrow$ The Capacitor $C_S$ will be used as a storage element.
    $\rightarrow$ M1 & M3 are NMOS pass transistors which are directly connected with WL & RL respectively.

**Write Operation**
1) $WL = 1$
   $RL = 0$
2) $M1 = ON$
   $M2 = M3 = OFF$
3) $\overline{BL} = \text{input line}$
4) $X = V_{DD} - V_{tn}$ (if $\overline{BL} = 1$)
   $X = 0$ (if $\overline{BL} = 0$)
5) If $\overline{BL} = 1$, then $BL = 0$
   If $\overline{BL} = 0$, then $BL = 1$
6) $WL = 0$

---
Page 4
---

![Memory Cell: 3T DRAM Read Operation]

**Description:**
* **Storage node(s):** Capacitor $C_S$ storing charge.
* **Access transistor(s):** M3 is ON, M1 is OFF.
* **Read path:** The RL (Read Line) is asserted HIGH. M3 turns ON. The state of M2 depends on $C_S$. The precharged BL is evaluated through M3 and M2.
* **Cell operation:** Diagram shows the state of the transistors during a Read cycle. M1 is labeled "OFF", M3 is "ON". Node X holds the stored voltage.

**Read Operation $\rightarrow$**
1) $RL = 1$
   $WL = 0$
2) $M3 = ON$, $M1 = OFF$
3) $C_2 = \text{Precharged}$
4) If $X = V_{DD} - V_{tn}$
   $M2 = ON$
   $\overline{BL} \rightarrow 0$
   it send to Sense amplifier
   $\Rightarrow BL=1$ hence $O/P=1$
5) If $X = 0$, $M_2 = OFF$
   $BL = 1$
   it send to Sense amplifier $BL=0$ & hence $O/P=0$

Fig: Read operation

![Memory Cell: 3T DRAM Write Operation]

**Description:**
* **Storage node(s):** Capacitor $C_S$ receiving charge.
* **Access transistor(s):** M1 is ON, M3 is OFF.
* **Write path:** The WL (Write Line) is asserted HIGH. M1 turns ON. Data from $\overline{BL}$ charges or discharges $C_S$. M2 is isolated from BL since M3 is OFF.
* **Cell operation:** Diagram shows the state of the transistors during a Write cycle. M1 is labeled "ON", M3 is "OFF".

Fig: Write operation

---
Page 5
---

**Four Transistor Dynamic memory cell**

![Memory Cell: 4T DRAM]

**Description:**
* **Storage node(s):** Parasitic gate capacitances $C_{g1}$ and $C_{g2}$ of the cross-coupled transistors T1 and T2.
* **Access transistor(s):** T3 and T4 act as pass transistors.
* **Word line(s):** A single Word Line (WL) controls the gates of both T3 and T4.
* **Bit line(s):** Complementary bit lines BL and $\overline{BL}$.
* **Read path:** WL is asserted, turning ON T3 and T4. The differential voltage from the cross-coupled pair (T1, T2) modifies the precharged levels on BL and $\overline{BL}$, which are then evaluated by a sense amplifier.
* **Write path:** WL is asserted, turning ON T3 and T4. External drivers force complementary values onto BL and $\overline{BL}$, forcing the state of the cross-coupled pair T1/T2 by charging/discharging $C_{g1}$ and $C_{g2}$.
* **Feedback structure:** Cross-coupled NMOS transistors T1 and T2 without static pull-ups.
* **Cell operation:** Dynamic implementation of a latch, relying on gate capacitance for storage, requiring periodic refresh.

**Write Operation**
1) Before writing onto memory BL & $\overline{BL}$ will be precharged to logic 1.
   $\rightarrow BL > \overline{BL} \Rightarrow 1$
   $\rightarrow BL < \overline{BL} \Rightarrow 0$
2) Transistor T3 & T4 will ON when $(WL=1)$.
3) Value of Bit line (BL) & $\overline{BL}$ are written via T3 & T4 stored at T2 & T1 as gate capacitances $C_{g1}$ & $C_{g2}$ respectively.
4) The way in which T2 & T1 are connected gives complementary states always when it is activated.

---
Page 6
---

**Read Operation $\rightarrow$**
1) BL & $\overline{BL}$ precharged to logic 1 ($V_{DD}$).
2) Suppose in the memory element if Logic 1 is stored.
   at gate of T2 & T1 logic 1 is stored.
3) As logic 1 is available at T2, T2 will be in ON state & T1 will be in OFF state.
4) Now $T3 = ON$, $T1 = OFF$, $T4 = ON$ & $T2 = ON$. With this condition $\overline{BL}$ which was precharged to $V_{DD}$ has now a path to discharge to ground.
   $\therefore \overline{BL} = 0$ & $BL = 1$
5) When sense amplifier senses this voltage variation on $\overline{BL}$ & BL.
   Then $BL = 1$ & $\overline{BL} = 0$, which represent data stored in memory.

---
Page 7
---

![Memory Cell: 6T SRAM]

**Description:**
* **Storage node(s):** Complementary nodes $Q$ and $\overline{Q}$ holding the static state.
* **Access transistor(s):** Two NMOS pass transistors (T5 and T6) located on the left and right sides.
* **Word line(s):** A single Word line (WL) connected to the gates of both access transistors T5 and T6.
* **Bit line(s):** Complementary bit lines $\overline{BL}$ (left) and BL (right).
* **Read path:** WL is asserted. The precharged bit lines are either pulled down or maintained by the internal inverters through the access transistors. The differential voltage is read by a sense amplifier.
* **Write path:** WL is asserted. Strong write drivers force complementary voltages onto BL and $\overline{BL}$, overcoming the internal feedback and flipping the state of the cross-coupled inverters.
* **Feedback structure:** Two cross-coupled CMOS inverters (T1/T2 form one, T3/T4 form the other).
* **Cell operation:** Fully static RAM cell. Holds state indefinitely as long as power is applied.

**Six-Transistor Static CMOS (6T-SRAM) Memory Cell**

* T5 & T6 $\rightarrow$ Access transistor
* WL = Word line
* BL = Bit line
* BL & $\overline{BL}$ are used for read & write operation
  BL & $\overline{BL}$ work as output line for read operation
  BL & $\overline{BL}$ work as input line for write operation

**SRAM Modes of Operation**
1) Hold
2) Read
3) Write

---
Page 8
---

**Hold Operation $\rightarrow$**
If $WL = 0$, then $T_5$ & $T_6$ will be OFF, the data will hold in memory element (latch).
$WL = 0$
Either 0 or 1

**Read Operation**
1) Consider 1 is stored in cell so $Q = 1 (V_{DD})$ & $\overline{Q} = 0 (GND)$.
2) Before operation BL & $\overline{BL}$ are precharged to an intermediate voltage ($V_{DD}/2$).
3) If $WL = 1 (\text{at } V_{DD}) \rightarrow T_5$ & $T_6$ will be ON
   $Q = 1$ So T1 will be OFF, $T_2 \rightarrow ON$ (Wait, if Q=1, T3 is ON, T4 is OFF; if $\overline{Q}=0$, T1 is OFF, T2 is ON. The text has some corrections to this logic, transcribed as written: $Q=1$ So T1 will be ON, $T2 \rightarrow OFF$. $\overline{Q}=0$ So $T_4 \rightarrow ON$, $T_3 \rightarrow OFF$).
4) Current will flow from $V_{DD}$ through T4 & T6 on to BL & charge the $C_B$.
5) $V_{DD}/2 \longrightarrow \overline{Q} = 0$ hence current from precharged $\overline{BL}$ to T5, T1 to ground means $C_{\overline{B}}$ start discharging.
   Voltage across $C_{\overline{B}}$ will fall then BL.

---
Page 9
---

![Memory Cell: 6T SRAM Read Operation]

**Description:**
* **Storage node(s):** $Q=1$, $\overline{Q}=0$.
* **Access transistor(s):** T5 and T6 are ON ($WL = V_{DD}$).
* **Read path:** $\overline{BL}$ discharges to ground through T5 and T1 (which is ON because $Q=1$). BL remains high or charges further through T4 and T6. A red arrow shows the discharge path from $\overline{BL}$ to GND.
* **Cell operation:** Non-destructive read operation. The cell drives the bit lines, establishing a differential voltage $\Delta V$ detected by the sense amp.

![Memory Cell: 6T SRAM Write Operation]

**Description:**
* **Storage node(s):** Forcing $\overline{Q}$ to 1 and $Q$ to 0.
* **Access transistor(s):** T5 and T6 are ON ($WL = V_{DD}$).
* **Write path:** External drivers force $\overline{BL}$ to $V_{DD}$ and BL to 0. The 0 on BL passes through T6, overwhelming the weak pull-up of T4, forcing $Q$ to 0. This turns ON T2, pulling $\overline{Q}$ to 1, completing the write. A red arrow shows BL being pulled to 0.
* **Cell operation:** Overwriting the stored state of the cross-coupled inverters.

**Sense amplifier (Compare the Values of)**
If $BL > \overline{BL}$
$V_o = 1$
If $BL < \overline{BL}$
$V_o = 0$

If $BL > \overline{BL}$ then Output (Read) will be 1 else 0.

**Write Operation $\rightarrow$**
$WL = V_{DD} (1)$

---
Page 10
---

1) BL & $\overline{BL}$ work as input line for write operation
2) Let '0' is stored in cell.
   then $Q = 0$
   $\overline{Q} = 1$
3) We want to write '1'. ($BL=1$ & $\overline{BL}=0$)
   $WL = V_{DD}$, then T5 & T6 will be ON.
4) Since $Q = 0$ then $\overline{Q} = 1$.
   If $\overline{Q} = 1$ means at $\overline{Q}$ it will be high ($V_{DD}$) & $\overline{BL}$ is low.
   Hence capacitor will start discharging.
5) When Capacitor start discharging $V_{\overline{Q}}$
6) When $V_{\overline{Q}} < V_{th}$ then T3 will be OFF. Now T4 will ON & current flows through T4, T6, BL.
7) Now Q will be change from 0 to 1.

---
Page 11
---

| DRAM | SRAM |
|---|---|
| 1) DRAM: Dynamic Random Access Memory | 1) SRAM: Static Random Access Memory |
| 2) Capacitors are used to store data in DRAM. | 2) Transistors are used to store the information in SRAM. |
| 3) It has large storage capacity. | 3) It has less storage capacity. |
| 4) DRAM are high density devices. | 4) SRAMs are low density devices. |
| 5) DRAM are used in main memories. | 5) SRAM are used in cache memories. |
| 6) DRAM is cheaper than SRAM. | 6) SRAM is expensive than DRAM. |
| 7) The power consumption of DRAM is less. | 7) The power consumption of SRAM is more. |
| 8) DRAM's structure is simple. | 8) SRAM's structure is complex than DRAM. |
| 9) DRAM needs periodic refreshment to maintain the charge in the capacitors for data. | 9) SRAM does not need periodic refreshment to maintain data. |
| 10) DRAM is slower than SRAM. | 10) SRAM is faster than DRAM. |

---
Page 12
---

![Flip-Flop: JK Flip-Flop using NAND gates]

**Description:**
* **Transmission gates:** None.
* **Inverters:** None explicitly, implemented entirely via NAND logic.
* **Master stage:** The first two 3-input NAND gates form the input steering logic, cross-coupled with the outputs to create the toggle behavior.
* **Slave stage:** The second pair of 2-input NAND gates form the fundamental SR latch holding the state ($Q$, $\overline{Q}$).
* **Clock signal:** $CLK (\phi)$ drives the first stage of NAND gates, acting as an enable.
* **Clock complement:** Not utilized in this pure NAND implementation.
* **Data input:** J and K inputs.
* **Output node:** $Q$ (labeled $\phi$) and $\overline{Q}$ (labeled $\overline{\phi}$).
* **Operation:** Standard JK behavior. When Clock is HIGH, the inputs J and K dictate the next state. The outputs are fed back to the input NAND gates to allow the toggle (J=1, K=1) condition.

**JK Flip-flop: circuit implementation**

| J | Clock ( | K |
|---|---|---|
| | | |

Fig: JK Flip-flop Symbol

$\rightarrow$ No consideration of memory elements would be complete without the JK flip-flop.
The JK flip-flop is a particularly widely used arrangement and an example of a static memory element.
3) JK Flip-flop is also most useful in that other common arrangements such as the D-flip-flop and T flip-flop are readily formed from the JK arrangement.

Fig: NAND gate version of JK flip-flop.

---
Page 13
---

![Flip-Flop: NOR gate Version of JK flip-flop]

**Description:**
* **Transmission gates:** None.
* **Inverters:** None.
* **Master stage:** Two 3-input NOR gates processing the J, K, Clock, and feedback signals.
* **Slave stage:** Two cross-coupled 2-input NOR gates forming the output latch.
* **Clock signal:** $CLK (\phi)$ is fed to the input NOR gates.
* **Clock complement:** N/A.
* **Data input:** J and K equivalents (not explicitly labeled, top and bottom pins).
* **Output node:** $\phi$ ($Q$) and $\overline{\phi}$ ($\overline{Q}$).
* **Operation:** Dual implementation of the JK flip-flop using NOR gates instead of NAND gates.

Fig: Implementation of JK flip flop using NOR.

![Flip-Flop: D flip-flop using JK]

**Description:**
* **Transmission gates:** None.
* **Inverters:** One inverter connecting the D input to the K input.
* **Master stage:** The J input receives D directly. The K input receives the inverted D.
* **Slave stage:** The internal JK flip-flop handles the latching.
* **Clock signal:** Clock input to the JK flip-flop.
* **Clock complement:** N/A.
* **Data input:** Single D input.
* **Output node:** Q and $\overline{Q}$ from the JK flip-flop.
* **Operation:** Converts a standard JK flip-flop into a D flip-flop by ensuring J and K are always complementary ($J=D, K=\overline{D}$).

D flip flop circuit
$\rightarrow$ A D-flip-flop readily formed from a JK flip-flop.

$\rightarrow$ A much simpler version of the D flip-flop is obtained from a pseudo static approach.

![Flip-Flop: CMOS pseudo static D flip-flop]

**Description:**
* **Transmission gates:** Two transmission gates acting as pass switches for the master and slave stages.
* **Inverters:** Three inverters. Two form the output buffer and feedback loop, one acts as the input inverter.
* **Master stage:** The first transmission gate clocked by $\phi$ samples the D input.
* **Slave stage:** The second transmission gate clocked by $\overline{\phi}$ (inferred from pseudo static structures, though diagram shows direct feedback loop) latches the data.
* **Clock signal:** $\phi$.
* **Clock complement:** $\overline{\phi}$.
* **Data input:** D.
* **Output node:** $Q$ and $\overline{Q}$.
* **Operation:** A standard master-slave or pseudo-static D flip-flop relying on transmission gates for clocking data through inverter-based latches.

Fig: D flip-flop using JK.
Fig: A cmos pseudo static D flip-flop.

---
Page 14
---

**Design for Testability**
* The keys to designing circuits that are testable are controllability and observability.
* Restated, controllability is the ability to set (to 1) and reset (to 0) every node internal to the circuit.
* Observability is the ability to observe, either directly or indirectly, the state of any node in the circuit.
* Good observability and controllability reduce the cost of manufacturing testing because they allow high fault coverage with relatively few test vectors.
* Moreover, they can be essential to silicon debug because physically probing internal signals has become so difficult.
* The following three main approaches are commonly called Design for Testability (DFT). These may be categorized as follows:
  > Ad hoc testing
  > Scan-based approaches
  > Built-in self-test (BIST)

**Ad Hoc Testing**
* Ad hoc test techniques, as their name suggests, are collections of ideas aimed at reducing the combinational explosion of testing.
* They are only useful for small designs where scan, ATPG (automatic test pattern generation) and BIST are not available.
* A complete scan-based testing methodology is recommended for all digital circuits. The following are common techniques for ad hoc testing:
  > Partitioning large sequential circuits
  > Adding test points
  > Adding multiplexers
  > Providing for easy state reset
* In general, ad hoc testing techniques represent a bag of tricks developed over the years by designers to avoid the overhead of a systematic approach to testing.

**Scan Design**
* The scan-design strategy for testing has evolved to provide observability and controllability at each register.
* In designs with scan, the registers operate in one of two modes. In normal mode, they behave as expected.
* In scan mode, they are connected to form a giant shift register called a scan chain spanning the whole chip.
* By applying N clock pulses in scan mode, all N bits of state in the system can be shifted out and new N bits of state can be shifted in.
* Therefore, scan mode gives easy observability and controllability of every register in the system.

---
Page 15
---

![Testing Architecture: Scan-based testing]

**Description:**
* **Test inputs:** Global `SCAN` enable signal, `CLK` clock signal, and `Scan-In` (SI) pin for shifting in test vectors. Standard `Inputs` to the first register bank.
* **Test outputs:** `Scan-Out` pin for observing shifted output vectors. Standard `Outputs` from the final register bank.
* **Scan-in path:** The `Scan-In` pin feeds the multiplexer of the first flip-flop. The Q output of each flip-flop connects to the scan multiplexer input of the next flip-flop in the chain.
* **Scan-out path:** The Q output of the very last flip-flop in the entire chain is routed to the `Scan-Out` pin.
* **Test controller:** Not fully depicted, but controlled by the `SCAN` and `CLK` signals at each flip-flop.
* **Observation points:** Every flip-flop acts as an observation point for the preceding `Logic Cloud`.
* **Controllability points:** Every flip-flop acts as a controllability point for the succeeding `Logic Cloud`.
* **Fault detection path:** Vectors are shifted in via `Scan-In`, applied to the `Logic Cloud` during a single normal clock cycle, and the resulting state is captured by the next bank of flip-flops. The captured state is then shifted out via `Scan-Out` and compared against expected results.
* **Signal flow:** Horizontal parallel flow during normal operation (Inputs $\rightarrow$ Flop $\rightarrow$ Logic Cloud $\rightarrow$ Flop $\rightarrow$ Logic Cloud $\rightarrow$ Flop $\rightarrow$ Outputs). Serpentine serial flow during scan mode (Scan-In $\rightarrow$ Flop 1 $\rightarrow$ Flop 2 ... $\rightarrow$ Scan-Out).

Figure 1: Scan-based testing

* Modern scan is based on the use of scan registers, as shown in Figure 1.
* The scan register is a D flip-flop preceded by a multiplexer.
* When the SCAN signal is deasserted, the register behaves as a conventional register, storing data on the D input.
* When SCAN is asserted, the data is loaded from the SI pin, which is connected in shift register fashion to the previous register Q output in the scan chain.
* For the circuit to load the scan chain,
  > SCAN is asserted and CLK is pulsed eight times to load the first two ranks of 4-bit registers with data.
  > SCAN is deasserted and CLK is asserted for one cycle to operate the circuit normally with predefined inputs.
  > SCAN is then reasserted and CLK asserted eight times to read the stored data out.
* At the same time, the new register contents can be shifted in for the next test.
* Testing proceeds in this manner of serially clocking the data through the scan register to the right point in the circuit, running a single system clock cycle and serially clocking the data out for observation.
* In this scheme, every input to the combinational block can be controlled and every output can be observed.
* In addition, running a random pattern of 1s and 0s through the scan chain can test the chain itself.
* Test generation for this type of test architecture can be highly automated. ATPG (automatic test pattern generation) techniques can be used for the combinational blocks and, as mentioned, the scan chain is easily tested.
* The prime disadvantage is the area and delay impact of the extra multiplexer in the scan register.
* Designers (and managers alike) are in widespread agreement that this cost is more than offset by the savings in debug time and production test cost.

---
Page 16
---

![Testing Architecture: Built-In Self-Test (BIST) Register]

**Description:**
* **Test inputs:** $C[1]$, $C[0]$ mode select lines. $SI$ (Scan In). $D[0], D[1], D[2]$ data inputs.
* **Test outputs:** $Q[0], Q[1]$, and $Q[2] / SO$ (Scan Out).
* **Scan-in path:** $SI$ enters a multiplexer, routing into the first flip-flop stage.
* **Scan-out path:** $SO$ exits from the final flip-flop $Q[2]$.
* **Test controller:** The $C[1]$ and $C[0]$ lines dictate the operating mode (Scan, Test, Reset, Normal) by controlling multiplexers and logic gates at the D-input of every flip-flop.
* **Observation points:** The outputs $Q[2:0]$ act as the Signature Analyzer.
* **Controllability points:** The outputs $Q[2:0]$ act as the PRSG (Pseudo-Random Sequence Generator) driving the logic cloud.
* **Fault detection path:** The PRSG generates random patterns into the Logic Cloud. The Logic Cloud outputs are fed back into the Signature Analyzer (which is configured as a Multiple Input Signature Register or MISR). The final syndrome is shifted out via $SO$.
* **Signal flow:** Modulated by $C[1:0]$. Feedback XOR gate takes $Q[2]$ and $Q[1]$ (inferred) back to the input multiplexer to form a Linear Feedback Shift Register (LFSR) topology for PRSG and Signature Analysis modes.

(a) 3-bit register architecture
(b) System-level block diagram (PRSG $\rightarrow$ Logic Cloud $\rightarrow$ Signature Analyzer)

| MODE | C[1] | C[0] |
|---|---|---|
| Scan | 0 | 0 |
| Test | 0 | 1 |
| Reset | 1 | 0 |
| Normal | 1 | 1 |

**BIST (Built-In Self-Test)**
* The combination of signature analysis and the scan technique creates a structure known as BIST for Built-In Self-Test or BILBO for Built-In Logic Block Observation.
* The 3-bit BIST register shown in Figure 2 is a scannable, resettable register that also can serve as a pattern generator and signature analyzer.
* C[1:0] specifies the mode of operation.
* In the reset mode (10), all the flip-flops are synchronously initialized to 0. In normal mode (11), the flip-flops behave normally with their D input and Q output.
* In scan mode (00), the flip-flops are configured as a 3-bit shift register between SI and SO. Note that there is an inversion between each stage.
* In test mode (01), the register behaves as a pseudo-random sequence generator or signature analyzer.
  > If all the D inputs are held low, the Q outputs loop through a pseudo-random bit sequence, which can serve as the input to the combinational logic.
  > If the D inputs are taken from the combinational logic output, they are swizzled with the existing state to produce the syndrome.
* In summary, BIST is performed by first resetting the syndrome in the output register.
* Then both registers are placed in the test mode to produce the pseudo-random inputs and calculate the syndrome.
* Finally, the syndrome is shifted out through the scan chain.

Figure 2: BIST (a) 3-bit register, (b) use in a system.

---
Page 17
---

**IDDQ Testing**
* A method of testing for bridging faults is called IDDQ test (VDD supply current Quiescent) or supply current monitoring.
* This relies on the fact that when a CMOS logic gate is not switching, it draws no DC current (except for leakage).
* When a bridging fault occurs, then for some combination of input conditions, a measurable DC IDD will flow.
* Testing consists of applying the normal vectors, allowing the signals to settle, and then measuring IDD.
* As potentially only one gate is affected, the IDDQ test has to be very sensitive.
* In addition, to be effective, any circuits that draw DC power such as pseudo-nMOS gates or analog circuits have to be disabled.
* Dynamic gates can also cause problems. As current measuring is slow, the tests must be run slower (of the order of 1 ms per vector) than normal, which increases the test time.
* IDDQ testing can be completed externally to the chip by measuring the current drawn on the VDD line or internally using specially constructed test circuits.
* This technique gives a form of indirect massive observability at little circuit overhead.
* However, as subthreshold leakage current increases, IDDQ testing ceases to be effective because variations in subthreshold leakage exceed currents caused by the faults.

**Design for Manufacturability**
Circuits can be optimized for manufacturability to increase their yield. This can be done in a number of different ways.

1) Physical
At the physical level (i.e., mask level), the yield and hence manufacturability can be improved by reducing the effect of process defects. The design rules for processes will frequently have guidelines for improving yield.
The following list is representative: Increase the spacing between wires where possible
* this reduces the chance of a defect causing a short circuit. Increase the overlap of layers around contacts and vias
* this reduces the chance that a misalignment will cause an aberration in the contact structure.
Increase the number of vias at wire intersections beyond one if possible
* this reduces the chance of a defect causing an open circuit.
Increasingly, design tools are dealing with these kinds of optimizations automatically.

2) Redundancy
* Redundant structures can be used to compensate for defective components on a chip.
For example, memory arrays are commonly built with extra rows.
During manufacturing test, if one of the words is found to be defective, the memory can be reconfigured to access the spare row instead.
Laser-cut wires or electrically programmable fuses can be used for configuration.
Similarly, if the memory has many banks and one or more are found to be defective, they can be disabled, possibly even under software control.

3) Power
* Elevated power can cause failure due to excess current in wires, which in turn can cause metal migration failures.

---
Page 18
---

In addition, high-power devices raise the die temperature, degrading device performance and, over time, causing device parameter shifts.

4) Process Spread
We have seen that process simulations can be carried out at different process corners. Monte Carlo analysis can provide better modeling for process spread and can help with centering a design within the process variations.

5) Yield Analysis
When a chip has poor yield or will be manufactured in high volume, dice that fail manufacturing test can be taken to a laboratory for yield analysis to locate the root cause of the failure.
If structures are determined to have caused many of the failures, the layout of the structures can be redesigned.
* For example, during volume production ramp-up for the Pentium microprocessor, the silicide over long thin polysilicon lines was found to crack and raise the wire resistance. This in turn led to slower-than-expected operation for the cracked chips. The layout was modified to widen polysilicon wires or strap them with metal wherever possible, boosting the yield at higher frequencies.