# Module 5
## Testing, Debugging and Verification

---
Page 1
---

# Module 5
# Testing, Debugging and Verification

---
Page 2
---

## Introduction to Testing

- While in real estate the refrain is "Location! Location! Location!" the comparable advice in IC design should be "Testing! Testing! Testing!"
- For many chips, testing accounts for more effort than does design.
- Tests fall into three main categories.
  - The first set of tests verifies that the chip performs its intended function. These tests, called **functionality tests** or **logic verification**, are run before tapeout to verify the functionality of the circuit.
  - The second set of tests are run on the first batch of chips that return from fabrication. These tests confirm that the chip operates as it was intended and help debug any discrepancies.
  - The third set of tests verify that every transistor, gate, and

---
Page 3
---

- Testing a die (chip) can occur at the following levels:
  - Wafer level
  - Packaged chip level
  - Board level
  - System level
  - Field level

- The **yield** of a particular IC was the number of good die divided by the total number of die per wafer. Because of the complexity of the Manufacturing process, not all die on a wafer function correctly.

- Dust particles and small imperfections in starting material or photomasking can result in bridged connections or missing features. These

---
Page 4
---

![Figure: PCB Board with ICs]

> Image: A photograph of a printed circuit board (PCB) populated with multiple integrated circuit packages (red/dark-colored ICs), passive components (capacitors, resistors), and various SMD components mounted on a green PCB substrate with a blue grid cutting mat visible underneath. A barcode label reading "50612020" is visible on one of the larger IC packages. Multiple ICs of varying sizes are visible, along with fine-pitch QFP packages.

---
Page 5
---

![Figure: IC Die Photograph]

> Image: A high-magnification die photograph of a multi-core processor or SoC (System-on-Chip). The die shows distinct functional blocks including: large cache memory arrays (orange/yellow rectangular regions in the lower half), processor core logic (teal/cyan colored complex logic regions in the upper half), I/O pads along the periphery (orange/yellow bordered regions on left and right edges), and a structured bus/interconnect region in the center. The die uses a multi-layer metal interconnect process visible as colored layering across different functional blocks.

---
Page 6
---

## Logic Verification

- **Verification tests** are usually the first ones a designer might construct as part of the design process.
- **RTL (Register Transfer Logic)** is equivalent to the design specification at a higher behavioral or specification level of abstraction. The behavioral specification might be:
  - A verbal description
  - A plain language textual specification
  - A description in some high level computer language such as C
  - A program in a system-modeling language such as SystemC
  - A hardware description language such as VHDL or Verilog
  - Simply a table of inputs and required outputs
- **Functional equivalence** involves running a simulator on the two descriptions of the chip (e.g., one at the gate level and one at a functional level) and ensuring that the outputs are equivalent at some convenient check points in time for all inputs applied. This is most conveniently done in an HDL by employing a **test bench**; i.e., a wrapper that surrounds a module and provides for stimulus and automated checking.
- To check functional equivalence through simulation at various levels of the design hierarchy. If the description is at the RTL level, the behavior at a system level may be able to be fully verified.
- At each level you can write small tests to verify the equivalence.

---
Page 7
---

![Testing Architecture: Functional Equivalence at Various Levels of Abstraction]

**FIGURE 1. Functional equivalence at various levels of abstraction**

Description of Figure 1 — Hierarchical Verification Flow Diagram:

The diagram is a top-to-bottom flowchart showing four specification levels connected by downward arrows, with comparison operations (shown as circles containing "=") between adjacent levels, each comparison pointing to associated analysis types on the right.

```
┌──────────────────────────────┐
│   Behavioral Specification   │
└──────────────┬───────────────┘
               │
               ▼
           ┌───────┐  ──────────────► Formal Verification
           │   =   │                  Test Vector Equivalence
           └───────┘
               │
               ▼
┌──────────────────────────────┐
│      RTL Specification       │
└──────────────┬───────────────┘
               │
               ▼
           ┌───────┐  ──────────────► Timing Analysis
           │   =   │                  **Noise Analysis**
           └───────┘
               │
               ▼
┌──────────────────────────────┐
│   Structural Specification   │
└──────────────┬───────────────┘
               │
               ▼
           ┌───────┐  ──────────────► Layout vs. Schematic
           │   =   │                  Power Analysis
           └───────┘                  DRC
               │                      ERC
               ▼                      Parasitic Extraction
┌──────────────────────────────┐
│    Physical Specification    │
└──────────────────────────────┘
```

- **Level 1 (Top):** Behavioral Specification — compared via Formal Verification and Test Vector Equivalence with the level below (RTL).
- **Level 2:** RTL Specification — compared via Timing Analysis and **Noise Analysis** with the level below (Structural).
- **Level 3:** Structural Specification — compared via Layout vs. Schematic, Power Analysis, DRC, ERC, and Parasitic Extraction with the level below (Physical).
- **Level 4 (Bottom):** Physical Specification.

Each comparison circle (=) has one input arrow from the level above and one from the level below it, with an output arrow pointing right to the associated analysis/check types.

---
Page 8
---

## Manufacturing Test Principle

The purpose of manufacturing test is to screen out most of the defective parts before they are shipped to customers. Typical commercial products target a defect rate of **350–1000 defects per million (DPM)** chips shipped.

### 1. Fault Models

#### **(i) Stuck-At Faults**

- In the **Stuck-At model**, a faulty gate input is modeled as a **stuck at zero (Stuck-At-0, S-A-0)** or **stuck at one (Stuck-At-1, S-A-1)**.
- This model dates from board-level designs, where it was determined to be adequate for modeling faults.
- Figure 2 illustrates how an S-A-0 or S-A-1 fault might occur. These faults most frequently occur due to gate oxide shorts (the nMOS gate to GND or the pMOS gate to $V_{DD}$) or metal-to-metal shorts.

![Testing Architecture: CMOS Stuck-At Faults]

**FIGURE 2: CMOS stuck-at faults**

Description of Figure 2 — CMOS Stuck-At Fault Diagram:

The figure shows two parts: a schematic-level representation (left column) and a corresponding layout-level cross-sectional/top-view representation (right column), each illustrating a different stuck-at fault.

**Top half — Stuck-At-1 (SA1 Fault):**
- Left: A standard CMOS NAND or NOR gate symbol with two inputs, one output. The output is labeled with a bubble/inversion marker. The gate is shown producing a Stuck-At-1 fault condition. Label: "Stuck-At-1 SA1 Fault".
- Right: A CMOS transistor cross-section/layout diagram with hatched (blue diagonal-striped) diffusion/active regions on the left and right, a vertical polysilicon gate running vertically through the center, and contact squares (black filled squares) at regular intervals along the diffusion and metal interconnect. At the top, a label "SA1" indicates the node stuck at logic 1. The short to $V_{DD}$ is shown at the top of the structure.

**Bottom half — Stuck-At-0 (SA0 Fault):**
- Left: A similar CMOS gate symbol, this time illustrating a Stuck-At-0 fault. The output has a downward arrow indicating GND connection. Label: "Stuck-At-0 SA0 Fault".
- Right: The same style of cross-section layout, with a grey shaded region in the center of the polysilicon/diffusion intersection area, representing a gate oxide short to GND. A label "SA0" is shown at the bottom of the structure. Contact squares (black filled squares) are present throughout.

The layout uses the following visual conventions:
- Cyan/blue hatched (diagonal stripes): diffusion or active areas (p-diffusion and n-diffusion regions)
- Vertical lines: polysilicon gate
- Black filled squares at intersections: contacts/vias
- Grey fill: short-circuit defect region

---
Page 9
---

#### **(ii) Short-Circuit and Open-Circuit Faults**

- Other models include **stuck-open** or **shorted** models. Two bridging or shorted faults are shown in Figure 3.
- The short **S1** results in an **S-A-0** fault at input A, while short **S2** modifies the function of the gate.
- It is evident that to ensure the most accurate modeling, faults should be modeled at the **transistor level** because it is only at this level that the complete circuit structure

![Testing Architecture: CMOS Bridging Faults]

**FIGURE 3 CMOS bridging faults**

Description of Figure 3 — CMOS Bridging Fault Diagram:

The figure has two sections: a transistor-schematic view (left/center) and a layout cross-section view (right).

**Schematic view (center-left):**

A four-input CMOS gate is drawn with the following topology:
- **PMOS pull-up network (top):** Two PMOS transistors in parallel/series configuration connected to $V_{DD}$ (indicated by T-bar at top). Input lines C and D feed the PMOS gates. Output node is labeled Z (to the right, with a rightward arrow).
- **nMOS pull-down network (bottom):** Two nMOS transistors in a series/parallel arrangement. Input A connects to one gate (upper nMOS), input B connects to another gate. Inputs A and B also feed the nMOS network (labeled A→, B→ on the left). The output is $Z = \sim(A|B)$ or a composite function depending on the bridging fault state.
- **Bridging short S2:** A diagonal arrow line connects across two gate nodes in the nMOS pull-down network (between transistors), creating a cross-coupling. Labeled "S2".
- **Bridging short S1:** A diagonal arrow line in the lower portion connects two source/drain nodes of adjacent nMOS transistors. Labeled "S1". Inputs on the left are labeled A (upper) and B (lower).
- Output side labels are C (upper) and D (lower), consistent with a two-stage or complex gate configuration.

**Layout cross-section view (right):**

- Four vertical columns at top labeled D, C, A, B (left to right), each with contact squares at the top.
- Blue/cyan hatched diffusion regions form two main horizontal bands.
- Vertical polysilicon gate lines run through the diffusion areas, with contact squares at each intersection.
- A short "S2" is indicated between two adjacent columns (C and A columns) — shown as a connection bridging between these nodes, roughly in the middle vertical zone.
- A short "S1" is shown at the bottom of the layout between the A and B columns, at the source/drain level.
- Grey rectangular region in the center represents the channel region or an intermediate metal layer.
- Contact squares (black filled) are present at each diffusion/metal intersection throughout.

---
Page 10
---

![Testing Architecture: CMOS Open Fault Causing Sequential Faults and Static IDD Current]

**FIGURE 4: A CMOS open fault that causes sequential faults and a defect that causes static $I_{DD}$ current**

Description of Figure 4 — CMOS Open Fault and Static IDD Defect Diagram:

The figure has three main sections: two schematic diagrams on the left and two corresponding layout cross-section diagrams in the center and right.

**Left column — Schematic diagrams:**

*Top schematic (Fault-free NOR gate):*
- Two PMOS transistors in series forming the pull-up network, connected to $V_{DD}$ (T-bar symbol at top).
- Inputs A and B feed the PMOS gates (A at top, B below).
- Output Z is connected at the drain junction of the two PMOS transistors.
- Two nMOS transistors in parallel forming the pull-down network.
- Input A feeds one nMOS gate (upper), input B feeds the other.
- Output connected to Z and then to GND (downward arrow).
- Boolean function: $Z = \sim(A|B)$

*Bottom schematic (NOR gate with open fault):*
- Identical structure to the top, but one nMOS transistor has an **open** at its source connection to GND (labeled "Open" with a downward arrow and break in the wire between nMOS source and GND).
- The modified function becomes: $Z = \sim(A|B) \mid (\sim B \& Z')$
  - where Z' denotes the previous state (making this a sequential/state-dependent fault).

**Center column — Layout cross-section (Open fault):**
- Two input contacts at top labeled A and B.
- Cyan/blue hatched diffusion regions (upper and lower), with contact squares.
- Polysilicon gate (vertical stripe, hatched differently) running through center.
- Node Z labeled to the right of the polysilicon.
- "Open" fault indicated by an arrow pointing to a break in the metal/diffusion connection in the lower nMOS section, between the nMOS source and GND contact.
- GND contacts at the bottom.

**Right column — Layout cross-section (Static IDD defect):**
- Similar layout structure to the center column.
- A "Short to $V_{DD}$" is indicated by an annotation at the upper right, showing a bridging connection between the pull-down network and $V_{DD}$ rail.
- This short causes a DC path from $V_{DD}$ to GND through the pull-down network, resulting in static (quiescent) $I_{DD}$ current flow — the basis of IDDQ testing.
- Same contact and diffusion conventions as in Figure 2 and Figure 3.

---
Page 11
---

### 2. Observability

- The **observability** of a particular circuit node is the degree to which you can observe that node at the outputs of an integrated circuit (i.e., the pins).
- This metric is relevant when you want to measure the output of a gate within a larger circuit to check that it operates correctly.
- Given the limited number of nodes that can be directly observed, it is the aim of good chip designers to have easily observed gate outputs.

### 3. Controllability

- The **controllability** of an internal circuit node within a chip is a measure of the ease of setting the node to a 1 or 0 state.
- This metric is of importance when assessing the degree of difficulty of testing a particular signal within a circuit.
- An easily controllable node would be directly settable via an input pad.
- A node with little controllability, such as the most significant bit of a counter, might require many hundreds or thousands of cycles to get it to the right state.

---
Page 12
---

### 4. Repeatability

- The **repeatability** of a system is the ability to produce the same outputs given the same inputs. Combinational logic and synchronous sequential logic is always repeatable when it is functioning correctly.
- However, certain asynchronous sequential circuits are nondeterministic. For example, an arbiter may select either input when both arrive at nearly the same time. Testing is much easier when the system is repeatable.
- Some systems with asynchronous interfaces have a lock-step mode to facilitate repeatable testing.

### 5. Survivability

- The **survivability** of a system is the ability to continue function after a fault.
- For example, error-correcting codes provide survivability in the event of soft errors. Redundant rows and columns in memories and spare cores provide survivability in the event of manufacturing defects.
- Adaptive techniques provide survivability in the event of process variation. Some survivability features are invoked automatically by the hardware, while others are activated by blowing fuses after manufacturing test.

---
Page 13
---

### 6. Fault Coverage

- A measure of goodness of a set of test vectors is the amount of **fault coverage** it achieves.
- That is, for the vectors applied, what percentage of the chip's internal nodes were checked? Conceptually, the way in which the fault coverage is calculated is as follows.
- Each circuit node is taken in sequence and held to 0 (S-A-0), and the circuit is simulated with the test vectors comparing the chip outputs with a **known good machine** — a circuit with no nodes artificially set to 0 (or 1).
- When a discrepancy is detected between the faulty machine and the good machine, the fault is marked as detected and the simulation is stopped. This is repeated for setting the node to 1 (S-A-1).
- In turn, every node is stuck (artificially) at 1 and 0 sequentially. The fault coverage of a set of test vectors is the percentage of the total nodes that can be detected as faulty when the vectors are applied.
- To achieve world-class quality levels, circuits are required to have in excess of **98.5% fault coverage**.

---
Page 14
---

### 7. Automatic Test Pattern Generation (ATPG)

- Historically, in the IC industry, logic and circuit designers implemented the functions at the RTL or schematic level, mask designers completed the layout, and test engineers wrote the tests.
- In many ways, the test engineers were the Sherlock Holmes of the industry, reverse engineering circuits and devising tests that would test the circuits in an adequate manner.
- For the longest time, test engineers implored circuit designers to include extra circuitry to ease the burden of test generation.
- Happily, as processes have increased in density and chips have increased in complexity, the inclusion of test circuitry has become less of an overhead for both the designer and the manager worried about the cost of the die.
- In addition, as tools have improved, more of the burden for generating tests has fallen on the designer. To deal with this burden, **Automatic Test Pattern Generation (ATPG)** methods have been invented.
- The use of some form of ATPG is standard for most digital

---
Page 15
---

### 8. Delay Fault Testing

- The fault models dealt with until this point have neglected timing. Failures that occur in CMOS could leave the functionality of the circuit untouched but affect the timing.
- For instance, consider the layout shown in Figure 5 for an inverter gate composed of paralleled nMOS and pMOS transistors.
- If an open circuit occurs in one of the nMOS transistor source connections to GND, then the gate would still function but with increased $t_{pdf}$.
- In addition, the fault now becomes sequential as the detection of the fault depends on the previous state of the gate.

![Testing Architecture: Delay Fault Example]

**FIGURE 5 An example of a delay fault**

Description of Figure 5 — Delay Fault in a Multi-Finger Inverter:

The figure shows a parallel (multi-finger) CMOS inverter schematic on the left and its corresponding layout cross-section on the right.

**Left — Schematic diagram:**

The circuit shows a paralleled CMOS inverter with three parallel transistor pairs stacked vertically:

*Top pair:*
- PMOS transistor: Gate input A (left), Drain/Source connected to output A (right), source connected to $V_{DD}$ (T-bar at top).
- Below: Output line labeled Z.

*Middle pair:*
- nMOS transistor: Gate input A (left), output side labeled A (right).
- Below: Output labeled Z, connecting to GND (downward arrow ∇).

*Bottom pair (faulty):*
- nMOS transistor: Gate input A (left), output side labeled A (right).
- Below: Output labeled Z.
- The source of this nMOS has an **Open** fault: the wire to GND (∇) is broken, labeled "Open ∇".

All three pairs share the same input A and output Z nodes — the transistors are in parallel.

**Right — Layout cross-section:**

- Top rail: cyan/blue hatched diffusion region (pMOS active area), with contact squares.
- Polysilicon gate: shown as a vertical stripe in the center with cross-hatching (distinct from diffusion hatching).
- Column A labeled on the left side of the polysilicon, column Z labeled on the right side.
- Middle section: Standard nMOS drain/source contacts and diffusion regions (light blue solid fill with contacts).
- Bottom section: Another nMOS region.
- **Open fault:** Indicated near the bottom nMOS source/GND connection, labeled "Open" with an arrow. The grey-filled area at the lower nMOS source represents the broken contact or missing diffusion connection to GND.
- GND rail: bottom cyan/blue hatched region with contacts.

The open eliminates one parallel nMOS path to GND, increasing effective resistance in the pull-down network, and therefore increasing the pull-down propagation delay $t_{pdf}$. The gate still functions (Z = $\sim$A) but with degraded timing.

---
Page 16
---

## Design for Testability

- The keys to designing circuits that are testable are **controllability** and **observability**.
- **Controllability** is the ability to set (to 1) and reset (to 0) every node internal to the circuit.
- **Observability** is the ability to observe, either directly or indirectly, the state of any node in the circuit.
- Good observability and controllability reduce the cost of manufacturing testing because they allow high fault coverage with relatively few test vectors.
- Moreover, they can be essential to silicon debug because physically probing internal signals has become so difficult.
- The following three main approaches are commonly called **Design for Testability (DFT)**. These may be categorized as follows:
  - Ad hoc testing
  - Scan-based approaches

---
Page 17
---

### Ad Hoc Testing

- **Ad hoc test techniques**, as their name suggests, are collections of ideas aimed at reducing the combinational explosion of testing.
- They are only useful for small designs where scan, ATPG (automatic test pattern generation) and BIST are not available.
- A complete scan-based testing methodology is recommended for all digital circuits.
- The following are common techniques for ad hoc testing:
  - ➢ Partitioning large sequential circuits
  - ➢ Adding test points
  - ➢ Adding multiplexers
  - ➢ Providing for easy state reset
- In general, ad hoc testing techniques represent a bag of tricks developed over the years by designers to avoid the hard of testing

---
Page 18
---

### Scan Design

- The **scan-design** strategy for testing has evolved to provide observability and controllability at each register.
- In designs with scan, the registers operate in one of two modes. In **normal mode**, they behave as expected. In **scan mode**, they are connected to form a giant shift register called a **scan chain** spanning the whole chip.
- By applying N clock pulses in scan mode, all N bits of state in the system can be shifted out and new N bits of state can be shifted in.
- Therefore, scan mode gives easy observability and controllability of every register in the system.

---
Page 19
---

### Scan Design

![Testing Architecture: Scan-Based Testing]

**Figure 1: Scan-based testing**

Description of Figure 1 — Scan Design Architecture:

The diagram shows a complete scan-based digital circuit architecture with two logic stages separated by flip-flop (Flop) registers.

**Overall structure (left to right):**

- **Primary Inputs** enter from the left side.
- **Scan-In** enters from the upper left corner.
- **Primary Outputs** exit from the right side.
- **Scan-Out** exits from the lower right corner.

**Register columns (three vertical banks of 4 Flops each):**

1. **First column (leftmost):** Four flip-flops (Flop) arranged vertically. Each has a D input from primary inputs (left) and a clock input (shown as an angled arrow at the top-left corner of each Flop). Scan-In feeds the top Flop; each subsequent Flop receives its scan input from the Flop above it (chained vertically). Outputs (Q) feed into the first Logic Cloud.

2. **Second column (middle):** Four flip-flops receiving their D inputs from the outputs of the first Logic Cloud. The scan chain continues from the first column's last Flop into the top Flop of this column and chains downward. Outputs feed into the second Logic Cloud.

3. **Third column (rightmost):** Four flip-flops receiving D inputs from the second Logic Cloud outputs. Scan chain continues from second column. Outputs form the primary outputs. The last Flop's Q output becomes Scan-Out.

**Logic Clouds:**
- Two "Logic Cloud" blocks are shown between the register columns, each drawn as a large oval/lens shape (representing combinational logic). Each Logic Cloud receives outputs from the register column on its left and feeds results into the register column on its right.

**Control signals (bottom center):**
- **CLK** (Clock): Feeds all flip-flops.
- **SCAN** signal: Mode select input to a special scan flip-flop shown at the bottom center.
- **SI** (Scan Input): Serial data input for the scan flip-flop.
- **D** (Normal data input): Normal mode data input to the scan flip-flop.
- The scan flip-flop at the bottom center has both SCAN, SI, and D inputs and a Q output, illustrating the multiplexed structure of scan flip-flops.

**Scan chain path:** Scan-In → top Flop of column 1 → chains through all 4 Flops of column 1 → chains through all 4 Flops of column 2 → chains through all 4 Flops of column 3 → Scan-Out.

---
Page 20
---

### Scan Design

- Modern scan is based on the use of **scan registers**, as shown in Figure 1.
- The scan register is a D flip-flop preceded by a multiplexer. When the **SCAN** signal is deasserted, the register behaves as a conventional register, storing data on the D input.
- When **SCAN** is asserted, the data is loaded from the **SI** pin, which is connected in shift register fashion to the previous register Q output in the scan chain.
- For the circuit to load the scan chain:
  - **SCAN** is asserted and **CLK** is pulsed eight times to load the first two ranks of 4-bit registers with data. SCAN is deasserted and CLK is asserted for one cycle to operate the circuit normally with predefined inputs.
  - SCAN is then reasserted and CLK asserted eight times to read the stored data out. At the same time, the new register contents can be shifted in for the next test.

---
Page 21
---

### Scan Design

- Testing proceeds in this manner of serially clocking the data through the scan register to the right point in the circuit, running a single system clock cycle and serially clocking the data out for observation.
- In this scheme, every input to the combinational block can be **controlled** and every output can be **observed**. In addition, running a random pattern of 1s and 0s through the scan chain can test the chain itself.
- Test generation for this type of test architecture can be highly automated. ATPG (automatic test pattern generation) techniques can be used for the combinational blocks and, as mentioned, the scan chain is easily tested.
- The **prime disadvantage** is the area and delay impact of the extra multiplexer in the scan register. Designers (and managers alike) are in widespread agreement that this cost is more than offset by the savings in debug time and production test cost.

---
Page 22
---

## Built-In Self Test

- The combination of **signature analysis** and the scan technique creates a structure known as **BIST** for **Built-In Self-Test** or **BILBO** for **Built-In Logic Block Observation**.
- The 3-bit BIST register shown in Figure 2 is a scannable, resettable register that also can serve as a **pattern generator** and **signature analyzer**.
- **C[1:0]** specifies the mode of operation.
- In the **reset mode (10)**, all the flip-flops are synchronously initialized to 0.
- In **normal mode (11)**, the flip-flops behave normally with their D input and Q output.
- In **scan mode (00)**, the flip-flops are configured as a 3-bit shift register between SI and SO. Note that there is an inversion between each stage.
- In **test mode (01)**, the register behaves as a pseudo-random sequence generator or signature analyzer.
  - ➢ If all the D inputs are held low, the Q outputs loop through a pseudo-random bit sequence, which can serve as

---
Page 23
---

- In summary, BIST is performed by first **resetting the syndrome** in the output register. Then both registers are placed in the **test mode** to produce the pseudo-random inputs and calculate the syndrome. Finally, the syndrome is shifted out through the scan chain.

![Testing Architecture: BIST 3-Bit Register and System Use]

**Figure 2: BIST (a) 3-bit register, (b) use in a system.**

Description of Figure 2 — BIST Architecture Diagram:

**(a) 3-bit BIST register circuit (top diagram):**

The circuit shows three identical BIST stages connected in series, with a feedback path. From left to right:

**Control bus:**
- Two control lines C[0] and C[1] run horizontally across the top, feeding into each stage's multiplexer/logic.

**Stage 0 (leftmost):**
- Two inputs at top: D[0] (data input) and C[0]/C[1] control lines.
- An AND gate (or complex gate) processes SI and a logic constant "1" (connected to one input) and "0" (connected to another input) along with C[0]/C[1] to select between scan, test, reset, and normal modes.
- A D flip-flop (labeled "Flop") stores the state.
- Output labeled Q[0].
- Another AND/NAND gate at the output processes Q[0] with C[0] for the feedback path.

**Stage 1 (middle):**
- Data input D[1] at top.
- Same AND/mux gate structure as stage 0.
- D flip-flop labeled "Flop".
- Output labeled Q[1].
- Gate at output for feedback.

**Stage 2 (rightmost):**
- Data input D[2] at top.
- Same gate structure.
- D flip-flop labeled "Flop".
- Output labeled **Q[2] / SO** (i.e., this is also the Scan-Out).

**Feedback path:**
- A large XNOR/XOR gate (shown at the bottom center with multiple inputs) takes Q[0] and Q[1] as inputs and feeds back to the SI (scan-in) input of Stage 0. This forms the Linear Feedback Shift Register (LFSR) polynomial for pseudo-random sequence generation.

**SI (Scan-In):** Enters from the left via a 2:1 mux selecting between SI and the LFSR feedback depending on mode.

**(b) System-level BIST block diagram (bottom diagram):**

Three blocks connected in series with arrows:
- **PRSG** (Pseudo-Random Sequence Generator) → feeds into →
- **Logic Cloud** (combinational circuit under test) → feeds into →
- **Signature Analyzer** (output response compactor)

**Mode table (shown to the right of the system diagram):**

| MODE   | C[1] | C[0] |
|--------|------|------|
| Scan   | 0    | 0    |
| Test   | 0    | 1    |
| Reset  | 1    | 0    |
| Normal | 1    | 1    |

---
Page 24
---

## IDDQ Testing

- A method of testing for bridging faults is called **IDDQ test** ($V_{DD}$ supply current Quiescent) or **supply current monitoring**.
- This relies on the fact that when a CMOS logic gate is not switching, it draws **no DC current** (except for leakage).
- When a bridging fault occurs, then for some combination of input conditions, a measurable DC $I_{DD}$ will flow.
- Testing consists of applying the normal vectors, allowing the signals to settle, and then measuring $I_{DD}$. As potentially only one gate is affected, the IDDQ test has to be very sensitive.
- In addition, to be effective, any circuits that draw DC power such as pseudo-nMOS gates or analog circuits have to be disabled.
- Dynamic gates can also cause problems. As current measuring is slow, the tests must be run slower (of the order of 1 ms per vector) than normal, which increases the test time.
- IDDQ testing can be completed **externally** to the chip by measuring the current drawn on the $V_{DD}$ line or **internally** using specially constructed test circuits.
- This technique gives a form of indirect massive observability at little

---
Page 25
---

## Design for Manufacturability

Circuits can be optimized for manufacturability to increase their yield. This can be done in several ways.

### 1. Physical

At the physical level (i.e., mask level), the yield and hence manufacturability can be improved by reducing the effect of process defects. The design rules for processes will frequently have guidelines for improving yield. The following list is representative:

- **Increase the spacing between wires where possible** — this reduces the chance of a defect causing a short circuit.
- **Increase the overlap of layers around contacts and vias** — this reduces the chance that a misalignment will cause an aberration in the contact structure.
- **Increase the number of vias at wire intersections beyond one if possible** — this reduces the chance of a defect causing an open circuit.

Increasingly, design tools are dealing with these kinds of optimizations automatically.

---
Page 26
---

### 2. Redundancy

- **Redundant structures** can be used to compensate for defective components on a chip. For example, memory arrays are commonly built with extra rows.
- During manufacturing test, if one of the words is found to be defective, the memory can be reconfigured to access the spare row instead.
- **Laser-cut wires** or **electrically programmable fuses** can be used for configuration. Similarly, if the memory has many banks and one or more are found to be defective, they can be disabled, possibly even under software control.

---
Page 27
---

### 3. Power

- Elevated power can cause failure due to excess current in wires, which in turn can cause **metal migration failures**.
- In addition, high-power devices raise the die temperature, degrading device performance and, over time, causing device parameter shifts.

### 4. Process Spread

- We have seen that process simulations can be carried out at different **process corners**.
- **Monte Carlo analysis** can provide better modeling for process spread and can help with centering a design within the process variations.

---
Page 28
---

### 5. Yield Analysis

- When a chip has poor yield or will be manufactured in high volume, dice that fail manufacturing test can be taken to a laboratory for **yield analysis** to locate the root cause of the failure.
- If structures are determined to have caused many of the failures, the layout of the structures can be redesigned.
- For example, during volume production ramp-up for the **Pentium microprocessor**, the silicide over long thin polysilicon lines was found to crack and raise the wire resistance. This in turn led to slower-than-expected operation for the cracked chips. The layout was modified to widen polysilicon wires or strap them with metal wherever possible, boosting the yield at higher frequencies.

---
Page 29
---

## Module-4

**Subsystem Design and Layout:** Some Architectural Issues, Switch Logic, Gate (restoring) Logic, A parity generator, Multiplexers (Data selectors), Design of 4-bit shifter.

**Illustration of the Design Processes:** Some observations on the design process, Design of an ALU Subsystem, (Text 1: 6.1 to 6.3, 6.4.1, 6.4.3, 7.2.2, 8.1, 8.3)

**8 Hours** &nbsp;&nbsp; L2, L3

## Module-5

**Memory, Registers and Aspects of system Timing:** System Timing Considerations, commonly used Storage/Memory elements (Text 1: 9.1, 9.2).

**Testing, Debugging and Verification:** Introduction: Logic Verification, Manufacturing Test Principles, Design for testability, Boundary Scan (Text 2: 15.1.1, 15.5, 15.6, 15.7).

**8 Hours** &nbsp;&nbsp; L2, L3

---
Page 30
---

## SRAM vs DRAM Comparison

| No. | SRAM (Static RAM) | DRAM (Dynamic RAM) |
|-----|-------------------|---------------------|
| 1 | SRAM stands for Static Random Access Memory | DRAM stands for Dynamic Random Access Memory |
| 2 | Stores data using flip-flops | Stores data using capacitors |
| 3 | Does not require refreshing | Requires periodic refreshing |
| 4 | Faster access speed | Slower compared to SRAM |
| 5 | More expensive | Less expensive |
| 6 | Lower memory density | Higher memory density |
| 7 | Consumes more power (generally) | Consumes less power per bit |
| 8 | Circuit is complex (typically 6 transistors/bit) | Circuit is simpler (1 transistor + capacitor/bit) |
| 9 | Used for cache memory | Used for main memory (RAM) |
| 10 | Data remains as long as power is supplied | Data fades quickly without refresh |

---
Page 31
---

## SYSTEM TIMING CONSIDERATIONS

1. A two-phase non-overlapping clock signal is assumed to be available, and this clock alone will be used throughout the system.

2. Clock phases are to be identified as $\phi_1$ and $\phi_2$ where $\phi_1$ is assumed to lead $\phi_2$.

3. Bits (or data) to be stored are *written to* registers, storage elements, and subsystems on $\phi_1$ of the clock; that is, write signals *WR* are *Anded* with $\phi_1$.

4. Bits or data written into storage elements may be assumed to have settled before the immediately following $\phi_2$ signal, and $\phi_2$ signals may be used to *refresh* stored data where appropriate.

5. In general, delays through data paths, combinational logic, etc. are assumed to be less than the interval between the leading edge of $\phi_1$ of the clock and the leading edge of the following $\phi_2$ signal.

6. Bits or data may be *read* from storage elements on the next $\phi_1$ of the clock; that is, read signals *RD* are *Anded* with $\phi_1$. Obviously, *RD* and *WR* are generally mutually exclusive to any one storage element.

7. A general requirement for system stability is that there must be at least one clocked storage element in series with every closed loop signal path.

---
Page 32
---

*(Page 32 — blank page)*
