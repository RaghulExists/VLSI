# Module 3 — Basic Circuit Concepts: Sheet Resistance, Capacitance, Delay

---
Page 1
---

> Sheet resistance concept used in semiconductor physics — how resistant a thin layer of material is to electric current.

## Sheet Resistance

**Sheet Resistance** is the resistance of a square piece of material, independent of its size.

![Layout Diagram: Conducting Slab for Sheet Resistance Derivation]

Description of diagram: A parallelogram/rectangular 3D slab is drawn in perspective showing:
- Current $I$ entering from the left face (arrow pointing right, labeled $I \rightarrow$)
- Dimension $B$ labeled at the top-left corner (top edge)
- Dimension $L$ labeled on the right side (vertical double arrow, top to bottom)
- Dimension $t$ labeled on the right side (thickness, small double arrow)
- Dimension $w$ labeled on the bottom face (width, horizontal double arrow, labeled $\leftarrow w \rightarrow$)
- Point $A$ labeled at the bottom center

$$
R_{AB} = \frac{\rho L}{A} = \frac{\rho L}{wt}
$$

$$
A = wt \quad \rightarrow \quad \text{cross sectional area.}
$$

$$
R_{AB} = R_S \frac{L}{w} \qquad R_S = \frac{\rho}{t}
$$

Consider a uniform slab of conducting material of resistivity $\rho$, of width $w$, thickness $t$ & length b/w faces $L$. The arrangement is shown in fig.

Consider the case in which $L = w$:

$$
R_{AB} = \frac{\rho}{t} = R_S
$$

$$
\boxed{R_S = \text{ohm per square or sheet resistance.}}
$$

---
Page 2
---

> Inversely proportional to *possible_word*

**Typical Sheet Resistance Values of MOS layers — $R_S$ (ohms/square)**

*(Table is rotated 90°; transcribed in correct orientation)*

| Layer | Technology | $R_S$ (ohms/square) | | |
|---|---|---|---|---|
| | | **5 μm** | **2 μm** | **1.2 μm** |
| noted | | 0.03 | 0.04 | 0.04 |
| Diffusion | | 10 → 50 | 20 → 45 | 20 → 45 |
| polysilicon | | 15 → 100 | 15 → 30 | 15 → 30 |
| n-transistor channel | | $10^4$ | $2 \times 10^4$ | $2 \times 10^4$ |
| p-transistor channel | | $2.5 \times 10^4$ | $4.5 \times 10^4$ | $4.5 \times 10^4$ |

> $R_S$ for MOS layers @ 5 μm

---
Page 3
---

## Sheet Resistance Concepts Applied for MOS Transistor

**1) nMOS – Enhancement transistor with channel length $2\lambda$ and $w = 2\lambda$ for 5μm technology**

![Layout Diagram: nMOS Enhancement Transistor L=2λ, W=2λ]

Description of diagram: A rectangular transistor layout is drawn showing:
- **Polysilicon** layer (upper-right, cross-hatched region, labeled "polysilicon" with arrow)
- **Channel** region (center, diagonal-hatched, labeled "channel" with arrow)
- **n-diffusion** region (lower, labeled "n-diffu-" with arrow, "$\leftarrow 2\lambda^3$" and "–n'on")
- Width $w = 2\lambda$ labeled on the left with a double-headed arrow
- Length $L = 2\lambda$ labeled at the bottom

$$
R_{Nchannel} = R_{AB} = R_S \frac{L}{w}
$$

$$
= R_S \frac{2\lambda}{2\lambda}
$$

$$
R_{AB} = R_S
$$

$$
R_{Nchannel} = 10^4 \;\Omega/\text{square} \left[\frac{2\lambda}{2\lambda}\right] \text{square}
$$

$$
\boxed{R_{Nchannel} = 10\,k\Omega}
$$

---

**2) Design A nMOS – Enhancement transistor $L = 8\lambda$, $4 \;\omega = 2\lambda$. and find the channel resistance**

![Layout Diagram: nMOS Enhancement Transistor L=8λ, W=2λ]

Description of diagram: A tall rectangular transistor layout with:
- Width $w$ labeled at the top with arrow $\rightarrow w \leftarrow$
- Length $L$ labeled on the right side (tall double arrow, top to bottom)
- Diagonal cross-hatching throughout indicating channel/active region
- $2\lambda = w$ labeled at the bottom

$$
R_{Nchannel} = R_S \frac{L}{w}
$$

$$
= 10^4 \times \frac{8\lambda}{2\lambda}
$$

$$
= 4 \times 10^4
$$

$$
\boxed{R_{Nchannel} = 40\,k\Omega}
$$

---

**3) nMOS – Inverter** *(strikethrough: "inverter")*

---
Page 4
---

## NMOS Inverter

$$
Z_{pu} : Z_{pd} = 4:1
$$

![Circuit Diagram: NMOS Inverter with Depletion Load]

Description of circuit: Standard nMOS inverter with depletion load:
- $V_{DD}$ at top (T-bar symbol)
- Pull-up transistor (depletion mode): gate connected to its own drain (diode-connected), labeled $\theta_1$ with ratio **4:1** ($L_{pu} = 8\lambda$, $W_{pu} = 2\lambda$). Gate-drain shorted (enhancement depletion symbol).
- Output $o/p$ taken from the junction between pull-up and pull-down transistors.
- Pull-down transistor (enhancement mode): labeled $\theta_2$ with ratio **1:1** ($L_{pd} = 2\lambda$, $W_{pd} = 2\lambda$). Input $V_{in}$ on gate.
- GND (arrow pointing down) at the bottom of pull-down.

> → it acts as a buffer

$$
\boxed{4:1 = 8\lambda : 2\lambda}
$$
$$
\boxed{1:1 = 2\lambda : 2\lambda}
$$

- for pull up transistor: $L = 8\lambda$, $4\; w = 2\lambda$
- for pull down: $-\!\!-$ $L = 2\lambda$, $4\; w = 2\lambda$

$$
R_{pu} = R_S \frac{L}{w} = 10^4 \times \frac{8\lambda}{2\lambda} = 40\,k\Omega
$$

$$
R_{pd} = R_S \frac{L}{w} = 10^4 \times \frac{2\lambda}{2\lambda} = 10\,k\Omega
$$

$$
R_{Nchannel} = R_{pu} + R_{pd} = 40k + 10k
$$

$$
\boxed{R_{Nchannel} = 50\,k\Omega}
$$

---

## CMOS Inverter

![Circuit Diagram: CMOS Inverter Minimum Size]

Description of circuit: Standard CMOS inverter:
- $V_{DD}$ at top (T-bar)
- PMOS pull-up transistor (pMOS symbol, gate to $V_{in}$): labeled **1:1** ($L_{pu} = 2\lambda$, $W_{pu} = 2\lambda$). Source to $V_{DD}$.
- $V_{in}$ connects to both PMOS gate and NMOS gate.
- Output $o/p$ taken from the common drain node.
- NMOS pull-down transistor (nMOS symbol): labeled **1:1** ($L_{pd} = 2\lambda$, $W_{pd} = 2\lambda$). Source to GND.
- GND (arrow) at bottom.
- $3\lambda = L$ annotation on left.

$$
pu = L = w = 2\lambda \quad 1:1
$$
$$
pd = L = w = 2\lambda \quad 1:1
$$

$$
R_{pu} = R_S \frac{L}{w} = 2.5 \times 10^4 \times \frac{2\lambda}{2\lambda}
$$

$$
\boxed{R_{pu} = 25\,k\Omega}
$$

$$
R_{pd} = R_S \frac{L}{w} = 10^4 \times \frac{2\lambda}{2\lambda}
$$

$$
\boxed{R_{pd} = 10\,k\Omega}
$$

$$
R_{CMOS} = 25k + 10k
$$

$$
\boxed{R_{CMOS} = 35\,k\Omega} \qquad \boxed{R_{ON} = 35\,k\Omega}
$$

---
Page 5
---

## Area Capacitance of Layers

**Area capacitance** of a layer is defined as capacitance per unit area.

$$
C_A = \frac{C}{A} \qquad \because C = \varepsilon_0 \varepsilon_r \frac{A}{d}
$$

**Area capacitance of GATE terminal:**

$$
C_A = \frac{\varepsilon_0 \varepsilon_r A}{d A} = \frac{\varepsilon_0 \varepsilon_r}{d}
$$

$$
= \frac{8.854 \times 10^{-12} \times 4}{850 \times 10^{-10}}
$$

$$
\boxed{C_A = 4.166 \times 10^{-4} \;\text{F/m}^2}
$$

$$
C_A = 4.166 \times 10^{-4} \;\text{pF}/\mu\text{m}^2
$$

$$
\boxed{C_A = 4 \times 10^{-4} \;\text{pF}/\mu\text{m}^2}
$$

> Notes on the right side:
> - $\varepsilon_r \Rightarrow$ relative permittivity of $\text{SiO}_2$
> - $\boxed{\varepsilon_r = 4}$ for $\text{SiO}_2$
> - $\varepsilon_0 \Rightarrow$ permittivity of free space
> - $\varepsilon_0 = 8.854 \times 10^{-12}$ F/m$^2$
> - $d \Rightarrow$ thickness of the material
> - $d = 850\,\text{Å}$ for $\text{SiO}_2$; $t_{ox} = 850\,\text{Å}$

---

### Typical Area Capacitance Values of MOS Circuit — $\text{pF}/\mu\text{m}^2 \times 10^{-4}$

| MOS Layer | $C_A$ Sum | $2\,\mu\text{m}$ $C_A$ | $1.2\,\mu\text{m}$ $C_A$ |
|---|---|---|---|
| Gate to channel | $4 \times 10^{-4}$ | $8 \times 10^{-4}$ | $16 \times 10^{-4}$ |
| Diffusion to Substrate | $1 \times 10^{-4}$ | $1.75 \times 10^{-4}$ | $3.75 \times 10^{-4}$ |
| polysilicon to Sub | $0.4 \times 10^{-4}$ | $0.6$ — | $0.6$ — |
| M1 to Sub | $0.3$ — | $0.33$ — | $0.33$ — |
| M2 to Sub | $0.2$ — | $0.17$ — | $0.17$ — |
| M2 to M1 | $0.4$ — | $0.51$ — | $0.5$ — |
| M2 to polysilicon | $0.3$ — | $0.3$ — | $0.3$ — |

---
Page 6
---

> *possible_word* standard

**Relative area capacitance** = $\dfrac{\text{Area capacitance of layer}}{\text{Gate to channel capacitance}}$

### Standard Unit of Capacitance $[\square C_g]$ (express in pF/μm²)

It is defined as gate to channel capacitance of a minimum size MOS transistor for length $2\lambda$ and width $2\lambda$, which is called a standard size @ unit of capacitance.

**To evaluate the value of $\square C_g$ for MOS process:**

**For 5μm:**

$$
1\,\square C_g = C_A \times A
$$

$$
\text{Area} = 2\lambda \times 2\lambda = 5\,\mu\text{m} \times 5\,\mu\text{m} = 25\,\mu\text{m}^2 \quad (\text{for 5μm Tech., } 2\lambda = 5\,\mu\text{m})
$$

$$
= 4 \times 10^{-4} \times 2\lambda \times 2\lambda
$$

$$
= 4 \times 10^{-4} \times 5\,\mu\text{m} \times 5\,\mu\text{m}
$$

$$
= 4 \times 10^{-4} \times 25 \times 10^{-12}
$$

$$
= 100 \times 10^{-16}
$$

$$
\boxed{1\,\square C_g = 0.01\,\text{pF}}
$$

---

**For 2μm:**

$$
1\,\square C_g = C_A \times A
$$

$$
= 8 \times 10^{-4} \times 2\,\mu\text{m} \times 2\,\mu\text{m}
$$

$$
= 32 \times 10^{-16}
$$

$$
\boxed{1\,\square C_g = 0.0032\,\text{pF}}
$$

---

**For 1.2μm:**

$$
1\,\square C_g = C_A \times A
$$

$$
= 16 \times 10^{-4} \times 1.2\,\mu\text{m} \times 1.2\,\mu\text{m}
$$

$$
= 23.04 \times 10^{-16}
$$

$$
\boxed{1\,\square C_g = 0.002304\,\text{pF}}
$$

$\therefore$ Value of $\square C_g$ is technology dependent.

---
Page 7
---

## Capacitance of a Multi-Layer Structure

![Layout Diagram: Multi-Layer Capacitance Structure]

Description of diagram: A multi-layer stack cross-section drawn in perspective, showing:
- **Metal** layer (top-left, with long horizontal extent labeled $100\lambda \rightarrow$, wide hatching)
- **Diffusion** layer (middle, cross-hatched, labeled "Diffusion", width $\leftarrow 2\lambda \rightarrow$ and $\leftarrow 2\lambda \rightarrow$)
- **Polysilicon** layer (bottom-most named layer, labeled "polysilicon", width $\leftarrow 2\lambda \rightarrow$)
- Vertical dimension labels from top to bottom: $4\lambda$, $1\lambda$, $2\lambda$, $2\lambda$ (individual layer thicknesses or spacings)
- A filled black square in the metal layer indicating a contact/via

**Problem:** Show that total capacitance excluding diffusion of a metal layer structure is $7.2\,\square C_g$.

Layer of a metal layer:

$$
\frac{\lambda}{c_n} \quad c_p \quad g \quad g
$$

*(circuit model of stacked capacitances)*

$$
2\lambda = 5\,\mu\text{m} \qquad \boxed{\lambda = 2.5\,\mu\text{m}}
$$

$$
\frac{100\lambda \times 3\lambda}{2\lambda \times 2\lambda} = 75 \qquad 2\lambda = 5\,\mu\text{m}, \quad \lambda = (2.5 \times 10^{-6})^2
$$

$$
C_{MS} = C_A \times A
$$

$$
= 0.3 \times 10^{-4} \left[100\lambda \times 3\lambda\right]
$$

$$
= 0.3 \times 10^{-4} \left[300 \times \lambda^2\right]
$$

$$
= 90 \times 10^{-4} \times (2.5 \times 10^{-6})^2
$$

$$
= 90 \times 6.25 \times 10^{-16}
$$

$$
= 562.5 \times 10^{-16}
$$

$$
C_{MS} = 562.5 \times 10^{-14} \;\text{*crossed out*}
$$

$$
\left\langle C_{MS} = 0.05625 \;\text{*crossed out*} \right\rangle \text{pF} \qquad 1\,\square C_g = 0.01\,\text{pF} \quad x = 0.056
$$

$$
\boxed{C_{MS} = 5.625\,\square C_g}
$$

---

$$
C_{PS} = C_A \times A
$$

$$
= 0.4 \times 10^{-4} \left[(2\lambda \times 2\lambda) + (2\lambda \times 1\lambda) + (4\lambda \times 4\lambda)\right]
$$

$$
= 0.4 \times 10^{-4} \left[4\lambda^2 + 2\lambda^2 + 16\lambda^2\right]
$$

$$
= 0.4 \times 10^{-4} \left[\lambda^2 [22]\right]
$$

$$
= 55 \times 10^{-4} \times 10^{-12}
$$

---
Page 8
---

$$
= 0.055
$$

$$
= 0.0055 \times 10^{-12}
$$

$$
C_{PS} = 0.0055\,\text{pF}
$$

$$
\boxed{C_{PS} = 0.55\,\square C_g}
$$

---

$$
C_{gC} = C_A \times C
$$

$$
= 4 \times 10^{-4} \times \left[2\lambda \times 2\lambda\right]
$$

$$
= 4 \times 4\lambda^2 \times 10^{-4}
$$

$$
= 100 \times 10^{-12} \times 10^{-4}
$$

$$
C_{gC} = 0.01\,\text{pF}
$$

$$
\boxed{C_{gC} = 1\,\square C_g}
$$

---

$$
C_T = C_{PS} + C_{MS} + C_{gC}
$$

$$
= 5.625 + 0.55 + 1
$$

$$
\boxed{C_T = 7.175\,\square C_g}
$$

$$
\boxed{C_T \approx 7.2\,\square C_g}
$$

---

**Problem:** Repeat above problem if diffusion extends by $2\lambda$ from polysilicon and **Relative area capacitance** of diffusion layer is **0.25**.

$$
(\text{Area}) \rightarrow C_A = \text{relative area capacitance} \times C_{\text{gate to channel}}
$$

$$
= \text{relative area capacitance} \times 4 \times 10^{-4}
$$

$$
= 0.25 \times 4 \times 10^{-4}
$$

$$
C_A = 1 \times 10^{-4}
$$

$$
C_d = C_A \times A
$$

$$
= 1 \times 10^{-4} \left[(2\lambda \times 2\lambda) + (2\lambda + 2\lambda)\right]
$$

$$
= 1 \times 10^{-4} \left[\lambda^2 [8]\right]
$$

$$
= 8 \times 10^{-4} \lambda^2
$$

$$
= 8 \times 10^{-4} \times 10^{-12}
$$

$$
C_d = 0.005\,\text{pF} \qquad \boxed{C_d = 0.5\,\square C_g}
$$

$$
C_T = 7.7\,\square C_g
$$

---
Page 9
---

## Delay Unit ($\tau$)

It is defined as product of sheet resistance of n-channel transistor and gate to channel capacitance of minimum size transistor.

$$
\tau\,(T) = R_S \times C = (R_S\,(\text{Nchannel})) \times 1\,\square C_g \;\text{seconds}
$$

| Technology | 5μm | 2μm | 1.2μm |
|---|---|---|---|
| $T = R_S \times C$ | $T = R_S \times C$ | $T = R_S \times C$ |  |
| $= 10^4 \times 1\,\square C_g$ | $= 2 \times 10^4 \times 10\,\square C_g$ | $= 2\lambda \times 10^4 \times 100$ |  |
| $= 10 \times 10^3$ | | $= 2\lambda 10^4 \times 0.04$ |  |
| $= 10 \times 10^3 \times 0.01\,\text{pF}$ | $\boxed{T = 0.2\,\text{nsec}}$ | $\boxed{T \approx 0.2\,\text{nsec}}$ |  |
| $T = 0.1 \times 10^{-9}\,\text{sec}$ | $= 20 \times 10^3 \times 0.0032\,\text{pF}$ | $= 20 \times 10^3 \times 0.0$ |  |
| $\boxed{T = 0.1\,\text{nsec}}$ | $\boxed{T = 0.064\,\text{nsec}}$ | $\boxed{T = 0.046\,\text{nsec}}$ |  |

---

## Inverter Delay

### Time Delay Calculations for NMOS Inverter

![Circuit Diagram: NMOS Inverter with Depletion and Enhancement Load]

Description of circuit — nMOS Inverter (right-side detail):
- $V_{DD}$ at top.
- Depletion load (pull-up): gate-drain shorted, labeled "depletion", ratio **PU 4:1** ($L = 8\lambda$, $W = 2\lambda$ implied by 4:1 ratio). Source to $V_{DD}$.
- Output node $o/p$ (labeled $\theta_p$) from drain.
- Enhancement driver (pull-down): gate input $V_{in}$, ratio **Pd 1:1** ($L = 2\lambda$, $W = 2\lambda$). Source to GND.
- Load capacitance: $1\,\square C_g$ at output, from $o/p$ to GND.
- **gate-to-channel cap** annotation with upward arrow at the $1\,\square C_g$ capacitor.

**Left-side (symbol level):**
- $V_{in}$ drives a triangle (inverter symbol) with ratio **4:1** at PU and **1:1** at PD.
- $V_{out}$ at output.
- Load capacitance $1\,\square C_g$ to GND.

---

**Case(i): $V_{in} = 0$ $\left(\frac{1}{\cancel{0}}\right)$**

$$
PU \rightarrow ON \qquad PD \rightarrow OFF
$$

Equivalent circuit:
- $V_{DD}$ → $R_{pu}$ → $o/p$ → $1\,\square C_g$ → GND

$$
T_{charging} = R_C = \left(R_S \times \frac{L}{w}\right) 1\,\square C_g
$$

---
Page 10
---

$$
T_{charging} = \left[10^4 \times \frac{4}{1}\right] 1\,\square C_g
$$

$$
= 4 \times 10^4 \times 0.01 \times 10^{-12}
$$

$$
= 0.04 \times 10^{-8}
$$

$$
T = 0.4\,\text{nsec}
$$

$$
\boxed{T = 4\tau} \qquad T = 0.1\,\text{nsec}
$$

---

**Case(ii): $V_{in} = 1$ $\left(\frac{0}{1}\right)$**

$$
PU \rightarrow ON \qquad PD \rightarrow ON
$$

Equivalent circuit:
- $V_{DD}$ → $R_{pu}$ → node $dp$ (output node) → $R_{pd}$ → GND
- Load capacitor $1\,\square C_g$ connected from $dp$ to GND

$$
T_{discharging} = RC = \left(R_S \times \frac{L}{w}\right) 1\,\square C_g
$$

$$
= R_{pd} \times 1\,\square C_g = \left(10^4 \times \frac{1}{1}\right) 1\,\square C_g
$$

$$
= 10^4 \times 1\,\square C_g
$$

$$
T = 0.1\,\text{nsec}
$$

$$
\boxed{T = 1\tau}
$$

$$
\text{Total delay} = T_c + T_{dis} = 4\tau + 1\tau = 5\tau
$$

> Timing waveform (right side):
> - Signal transitions shown as step waveforms
> - Vin: LOW → HIGH shown (0 → 1)
> - Output charging: step up then down
> - Timing markers: $t_{40\%}$, $t_{H}$ (half-transition points)

---
Page 11
---

## Time Delay Calculations for NMOS Inverter Pair

![Circuit Diagram: NMOS Inverter Pair — Two-Stage Cascade]

Description of circuit (top, symbolic level):
- $V_{in}$ feeds inverter 1 (triangle symbol, **4:1** PU, **1:1** PD), output to first $1\,\square C_g$
- First inverter output feeds inverter 2 (triangle symbol, **4:1** PU, **1:1** PD), output to second $1\,\square C_g$
- Final output is $V_{out}$

Description of circuit (middle, transistor level):
- Stage 1: $V_{DD}$ → depletion load (4:1) → internal node → nMOS driver (1:1, $V_{in}$ on gate) → GND. Load cap $1\,\square C_g$ (labeled $1\,\text{Dcg}$ in diagram) between output and GND.
- Stage 2: $V_{DD}$ → depletion load (4:1) → output node $V_{out}$ → nMOS driver (1:1, interstage node on gate) → GND. Load cap $1\,\square C_g$ at output.

---

**Case(i): $V_{in} = 0$ $\left(\frac{1}{0}\right)$**

Stage 1: $PU1 = ON$, $PD1 = OFF$
Stage 2: $PU2 = ON$, $PD2 = ON$

Equivalent circuits shown with $R_{pu}$ and $R_{pu}$ charging respective $1\,\square C_g$ capacitors.

$$
T_{charging} = RC = \left(R_S \frac{L}{w}\right) 1\,\square C_g
$$

$$
= \left(10^4 \times \frac{4}{1}\right) 1\,\square C_g = 4\tau
$$

$$
T_{dis} = \left(R_S \times \frac{L}{w}\right) 1\,\square C_g = 10^4 \times 1\,\square C_g = \tau
$$

$$
T_{total} = 4\tau + \tau = 5\tau
$$

---

**Case(ii): $V_{in} = 1$ $\left(\frac{0}{1}\right)$**

$$
T_{dis} = \tau \qquad T_{charg} = 4\tau \qquad T_{total} = 5\tau
$$

Equivalent circuits:
- Stage 1: $V_{DD}$ → $R_{pu1}$ → $R_{pd1}$ → GND; $1\,\square C_g$ at output.
- Stage 2: $V_{DD}$ → $R_{pu2}$ → node $b$ → GND; $1\,\square C_g$ at output.

---
Page 12
---

## Time Delay Calculations for CMOS Inverter — Minimum Size

![Circuit Diagram: CMOS Inverter Minimum Size — Symbolic and Transistor Level]

**Left — Symbolic:**
- $V_{in}$ → inverter (triangle, **1:1** PMOS, **1:1** NMOS) → $V_{out}$
- Load $2\,\square C_g$ to GND.

**Right — Transistor level:**
- $V_{DD}$ at top.
- PMOS transistor (labeled **1:1**, $L = w = 2\lambda$): gate ← $V_{in}$, source to $V_{DD}$, drain to $V_{out}$.
- NMOS transistor (labeled **1:1**, $L = w = 2\lambda$): gate ← $V_{in}$, source to GND, drain to $V_{out}$.
- Load capacitor $2\,\square C_g$ from $V_{out}$ to GND.

---

**Case(i): $V_{in} = 0$**

$$
PU = ON \qquad Pd = OFF
$$

Equivalent circuit: $V_{DD}$ → $R_{pu}$ → $V_{out}$ → $2\,\square C_g$ → GND

$$
T_{charg} = RC
$$

$$
= \left(R_S \frac{L}{w}\right) 2\,\square C_g
$$

$$
= \left(2.5 \times 10^4 \times \frac{1}{1}\right) 2\,\square C_g
$$

$$
= 5 \times 10^4 \times 10^{-12} \times 0.01
$$

$$
T_{charg} = 5\tau
$$

---

**Case(ii): $V_{in} = 1$**

$$
PU = OFF \qquad Pd = ON
$$

$$
T_{discharg} = RC = \left(R_S \frac{L}{w}\right) 2\,\square C_g
$$

$$
= 10^4 \times 2\,\square C_g
$$

$$
T_{dis} = 2\tau
$$

$$
\text{Total delay} = T_{charg} + T_{dis} = 5\tau + 2\tau
$$

$$
\boxed{= 7\tau}
$$

---
Page 13
---

## Delay Calculation for CMOS Inverter Pair

![Circuit Diagram: CMOS Inverter Pair — Symbolic and Transistor Level]

**Symbolic (top):**
- $V_{in}$ → inverter 1 (triangle) → inverter 2 (triangle) → $V_{out}$
- Load $2\,\square C_g$ after each stage.

**Transistor-level circuit (middle):**

Stage 1:
- $V_{DD}$ at top.
- PMOS $PU_1$ (1:1): gate ← $V_{in}$, drain to output node.
- NMOS $Pd_1$ (1:1): gate ← $V_{in}$, source to GND.
- Capacitor $2\,\square C_g$ from Stage 1 output to GND.

Stage 2:
- $V_{DD}$ at top.
- PMOS $PU_2$ (1:1): gate ← Stage 1 output.
- NMOS $Pd_2$ (1:1): gate ← Stage 1 output.
- Output $V_{out}$.
- Capacitor $2\,\square C_g$ from $V_{out}$ to GND.

---

**Case① $V_{in} = 0$ $\left(\frac{1}{\cancel{0}}\right)$**

Stage 1: $PV_1 \rightarrow ON$, $Pd_1 \rightarrow OFF$

Stage 2: $PV_2 = OFF$, $Pd_2 = ON$

Equivalent circuit (right side):
- $V_{DD}$ → series: $R_{pu1}$ (charging cap) → and → discharging via $R_{pd2}$ on Stage 2
- Stage 1 output charges $2\,\square C_g$; Stage 2 discharges $2\,\square C_g$ via $R_{pd2}$

$$
T_{charging} = R \times C = \left(R_S \frac{L}{w}\right) \times C
$$

$$
= 2.5 \times 10^4 \times 2\,\square C_g
$$

$$
T_{charg} \approx 5\tau
$$

$$
T_{dischar} = R_S \times C = 10^4 \times 2\,\square C_g = 2\tau
$$

$$
\text{Total delay} = T = 5\tau + 2\tau = 7\tau
$$

---

**Case② $V_{in} = 1$ $\left(\frac{1}{0}\right)$**

Stage 1: $PV_1 = OFF$, $Pd_1 = ON$

Stage 2: $PV_2 = ON$, $Pd_2 = OFF$

$$
T_{dischar} = R_S \times C = 10^4 \times 2\,\square C_g = 2\tau
$$

$$
T_{charg} = R_S \times C = 2.5 \times 10^4 \times 2\,\square C_g
$$

$$
T_{charg} = 5\tau
$$

$$
\text{Total delay} = T = 2\tau + 5\tau = 7\tau
$$

---
Page 14
---

> ✱✱ **(Problem)**

Two nmos inverters are cascaded as shown in fig. Calculate the delay in terms of $\tau$. The load cap is $16\,\square C_g$.

What is the delay if $\tau = 0.3\,\text{ns}$.

If due to connecting wires, the stray cap increases by $4\,\square C_g$, what is delay time.

Specifications of inverter are as follows:

![Circuit Diagram: Cascaded NMOS Inverter Pair with Specifications]

**Symbolic circuit:**
- $V_{in}$ → Inverter 1 (triangle, bubble) → Inverter 2 (triangle, bubble) → $V_{out}$

Inverter 1 specifications:
- $L_{pu} = 16\lambda$, $W_{pu} = 2\lambda$
- $L_{pd} = 2\lambda$, $W_{pd} = 2\lambda$

Inverter 2 specifications:
- $L_{pu} = 2\lambda$, $W_{pu} = 2\lambda$
- $L_{pd} = 2\lambda$, $W_{pd} = 8\lambda$

---
Page 15
---

![Circuit Diagram: Cascaded NMOS Inverter Pair — Transistor Level]

**Transistor-level circuit (from page 15):**

Two-stage nMOS inverter pair:
- Stage 1 ($Q_2$, **8:1** ratio): $V_{DD}$ → depletion load ($Q_2$, $8:1$) → output node → nMOS driver ($Q_1$, **1:1**) → GND. Load $C_1$ at interstage node.
- Stage 2 ($Q_4$, **1:1** ratio): $V_{DD}$ → depletion load ($Q_4$, $1:1$) → output $V_o$ → nMOS driver ($Q_3$, **1:4** — wide pull-down) → GND. Load $C_L = 16\,\square C_g$ at $V_o$.

$$
C_L = \frac{\varepsilon (2\lambda \times 8\lambda)}{d} = 4 \left[\frac{\varepsilon\, 2\lambda \times 2\lambda}{d}\right] = 4\,\square C_g
$$

> $1\,\square C_g = 0.01\,\text{pF}$, $\tau = 0.1\,\text{nsec}$, $10\,\square C_g = 0.01 \times 10^{-12}$

---

**Case(i): $V_i = 0\,\text{V}$**

$$
T_{charg} = R_{pu} \cdot C_1 \qquad T_{dis} = R_{pd} \cdot C_L
$$

$$
= R_S \left(\frac{L}{w}\right)(4\,\square C_g) \qquad = R_S\left(\frac{L}{w}\right) 16\,\square C_g
$$

$$
= 10^4 \left(\frac{8}{1}\right) 4\,\square C_g \qquad = 10^4\left(\frac{1}{4}\right) 16\,\square C_g
$$

$$
= 32\tau \qquad = 4\tau
$$

$$
\text{Total delay} = 36\tau = 36(0.3\,\text{n}) = 10.8\,\text{ns}
$$

---

**$V_i = 5\,\text{V}$:**

$$
T_{dis} = R_{pd} \cdot C_1 \qquad T_{ch} = R_{po} \cdot C_L
$$

$$
= R_S\left(\frac{L}{w}\right) C_1 \qquad = R_S\left(\frac{L}{w}\right) C_L
$$

$$
= 10^4(1) \cdot 4\,\square C_g \qquad = 10^4(1)\, 16\,\square C_g
$$

$$
= 4\tau \qquad = 16\tau
$$

---
Page 16
---

$$
\text{Total delay} = 20\tau = 20(0.3) = 6\,\text{ns.}
$$

---

**(ii) Stray cap $C_1$ to $8\,\square C_g$, $C_L = 20\,\square C_g$**

$$
C_1 = 8\,\square C_g
$$

Follow the same steps done in case(i).

**$V_i = 0\,\text{V}$:** Total delay $= 69\tau = 69(0.3 \times 10^{-9}) = 20.7\,\text{ns}$

**$V_i = 5\,\text{V}$:** Total delay $= 28\tau = 28(0.3 \times 10^{-9}) = 8.4\,\text{ns}$

---

## Delay of CMOS Inverter in terms of Rise Time ($T_r$) and Fall Time ($T_f$)

![Circuit Diagram: CMOS Inverter — Rise/Fall Time Analysis]

**Left — symbolic:** $V_{in}$ → inverter (triangle with bubble) → $V_{out}$. Load $C_L$ to GND.

**Right — transistor level:**
- $Q_2$ (PMOS): gate ← $V_{in}$, source to $V_{DD}$, drain to $V_{out}$.
- $Q_1$ (NMOS): gate ← $V_{in}$, source to GND, drain to $V_{out}$.
- Load $C_L = 8$ attached to $V_{out}$.

---

**Timing diagram:**

![Timing Diagram: CMOS Inverter Rise and Fall Times]

**Top waveform — $V_{in}$:**
- Horizontal axis: time
- Vertical axis: voltage (0 to 5V)
- Signal: LOW (0) → HIGH (5V) step → remains HIGH (labeled $V_{DD}$)
- Then HIGH → LOW → HIGH: square pulse

**Bottom waveform — $V_{out}$:**
- Vertical axis: 5V at top ($V_{DD}$), with 90% level ($0.9\,V_{DD}$) and 10% level ($0.1\,V_{DD}$) marked
- $V_{out}$ shows: HIGH ($V_{DD}$) → 0 (fall transition) → rise transition
- **Rise time** ($T_r$): measured from $0.1\,V_{DD}$ to $0.9\,V_{DD}$ on the rising edge
- **Fall time** ($T_f$): measured from $0.9\,V_{DD}$ to $0.1\,V_{DD}$ on the falling edge
- Timing markers: $t_1$ (start of fall), $t_H$ (midpoint), $t_K$ (end of fall or start of rise)

---
Page 17
---

## Rise Time Estimation

$V_{in} = 0$: $Q_2 = ON$ (PMOS), $Q_1 = OFF$ (NMOS)

![Circuit Diagram: CMOS Inverter — Rise Time PMOS Analysis]

**Left — functional diagram:**
- $V_{DD}$ at top.
- PMOS transistor: source $S$, drain $D$, gate $g$ connected to $V_{in} = 0\,\text{V}$.
- $I_{dsp}$ current arrow pointing upward from Source $S$.
- Capacitor $2\,\square C_g$ from output node to GND.

**Right — transistor diagram with labels:**
- $+V_{gsp}$ labeled between gate and source of PMOS.
- $V_{in}$ on gate (g).
- $V_{dsp}$ between drain (d) and source, labeled with + on drain side and arrow to $V_{out}$.

$$
V_{gsp} = V_{in} - V_{DD}
$$

$$
V_{dsp} = V_{out} - V_{DD}
$$

$$
V_{tp} = 0.2\,V_{DD}
$$

- pmos → saturation mode
- nmos → OFF state

The saturation current for the p-transistor is given by:

$$
I_{dsp} = \frac{\beta_p \left(V_{gsp} - |V_{tp}|\right)^2}{2}
$$

$$
V_{out} = \frac{I_{dsp}\,t}{C_L}
$$

$$
t = \frac{2\,C_L\,V_{out}}{\beta_p (V_{gs} - |V_{tp}|)^2}
$$

$$
t = \tau_r \quad \text{when} \quad V_{out} = +V_{DD}
$$

$$
\tau_r = \frac{2\,C_L\,V_{DD}}{\beta_p \left(V_{DD} - |V_{tp}|\right)^2}
$$

$$
V_{tp} = 0.2\,V_{DD}
$$

$$
\tau_r = \frac{2\,C_L\,V_{DD}}{\beta_p (V_{DD} - 0.2\,V_{DD})^2} \approx \frac{3\,C_L}{\beta_p\,V_{DD}}
$$

---
Page 18
---

## Fall Time

**(i) $V_{in} = 1$: nmos → saturation, pmos → off**

$$
I_{dsn} = \frac{\beta_n \left(V_{gs} - V_{tn}\right)^2}{2}
$$

$$
V_{out} = \frac{I_{dsn}\,t}{C_L}
$$

$$
t = \frac{2\,C_L\,V_{out}}{\beta_p (V_{gs} - V_{tn})^2}
$$

$$
t = t_f
$$

$$
\boxed{t_f = \frac{3\,C_L}{\beta_n\,V_{DD}}}
$$

> Right side: small circuit diagram showing:
> - nMOS transistor with drain connected to output, source to GND
> - $C_L$ at output

---
Page 19
---

If $V_{tp}$ is too large:

pmos turns off too early.

$$
\begin{cases} V_{tp} = -0.2\,V_{DD} \\ V_{tn} = 0.2\,V_{DD} \end{cases}
$$

$$
\frac{t_r}{t_p} = \frac{\beta_n}{\beta_p}
$$

But $\mu_n = 2.5\,\mu_p$ hence $\beta_n = 2.5\,\beta_p$

pmos is 2.5 times slower than nmos.

If $t_r = t_f$: $\quad W_p = 2.5\,W_n$ ✱

$$
t_r \propto t_f \approx \frac{1}{V_{DD}}
$$

$$
t_r \propto C_L
$$

$$
\boxed{t_r \approx 2.5\,\tau_f}
$$

---
Page 20
---

## 4.9 Propagation Delays

### 4.9.1 Cascaded Pass Transistors

A degree of freedom offered by MOS technology is the use of pass transistors as series or parallel switches in logic arrays. Quite frequently, therefore, logic signals must pass through a number of pass transistors in series. A chain of four such transistors is shown in Figure 4–17(a) in which all gates have been shown connected to $V_{DD}$ (logic 1), which would be the case for a signal to be propagated to the output. The circuit thus formed may be modeled as in Figure 4–17(b) and it is then possible to evaluate the delay through the network.

The response at node $V_2$ with respect to time is given by

$$
C\frac{dV_2}{dt} = (I_1 - I_2) = \frac{[(V_1 - V_2) - (V_2 - V_3)]}{R}
$$

![Circuit Diagram: Cascaded Pass Transistor Chain]

**Figure 4–17 Propagation delays in pass transistor chain**

**(a) Physical circuit:**
- $V_{in}$ steps from $0\,\text{V}$ to $V_{DD}$ at time $t_0$.
- Four nMOS pass transistors connected in series (source-to-drain chain).
- All gates tied to $V_{DD}$ (logic 1).
- $V_{out}$ at the far right, which reaches $V_{DD} - V_{tp}$ at time $t_0$ (threshold loss shown).
- Each transistor drawn as a box with gate on top and source/drain on sides.
- Dots at each intermediate node and at input/output.
- $V_{DD}$ rail connected to each gate.

**(b) RC lumped model:**
- $V_{in}$ feeds a chain of 4 identical RC sections.
- Each section: resistance $R$ in series, and capacitance $C$ to ground.
- Intermediate voltages labeled $V_1$, $V_2$, $V_3$ at internal nodes.
- $V_{out}$ at the far right.
- Current $I_1$ flows between the $V_1$ and $V_2$ nodes, $I_2$ flows between $V_2$ and $V_3$.

---
Page 21
---

*— Basic circuit concepts — 115*

In the limit as the number of sections in such a network becomes large, this expression reduces to

$$
RC\frac{dV}{dt} = \frac{d^2V}{dx^2}
$$

where

- $R$ = resistance per unit length
- $C$ = capacitance per unit length
- $x$ = distance along network from input.

The propagation time $\tau_p$ for a signal to propagate a distance $x$ is such that

$$
\tau_p \propto x^2
$$

The analysis can be simplified if all $R$s and $C$s are lumped together, then

$$
R_{total} = nr R_s
$$

$$
C_{total} = n\,\square\,C_g
$$

where $r$ gives the relative resistance per section in terms of $R_s$ and $c$ gives the relative capacitance per section in terms of $\square C_g$.

Then, it may be shown that overall delay $\tau_d$ for $n$ sections is given by

$$
\tau_d = n^2\,rc(\tau)
$$

Thus, the overall delay increases rapidly as $n$ increases and in practice no more than *four* pass transistors should be normally connected in series. However, this number can be exceeded if a buffer is inserted between each group of four pass transistors *or* if relatively long time delays are acceptable.

---
Page 22
---

## Design of Long Polysilicon Wires

Long polysilicon wire also contributed series $R$ & $C$
→ signal propagation is slowed down.

If we use buffers to drive long polysilicon runs have 2 desirable effects:
1. The signal propagation is speeded up.
2. There is a reduction in sensitivity to noise.

![Circuit Diagram: Buffered Long Polysilicon Wire with RC Sections]

Description of diagram — RC distributed line with buffers:

- Input $V_{in}$ (labeled with a timing arrow $t_0$ showing step transition) enters from the left.
- A chain of 4 RC sections followed by a buffer (inverter symbol with bubble at output):
  - Section 1: resistance $R$ in series (box), capacitance $C$ shunt to GND (vertical capacitor symbol).
  - Section 2: resistance $R$, capacitance $C$ to GND.
  - Section 3: resistance $R$, capacitance $C$ to GND.
  - Section 4: resistance $R$, capacitance $C$ to GND.
  - Buffer/inverter at the end driving $V_{out}$.
- Timing arrows at intermediate nodes: $t_0$, $t_a$, $t_a$, $t_2$, showing propagation times getting later toward the right.
- At input: $t_0$ (step up), $t_2$ (further delayed arrival shown at right).

$$
\longleftarrow \quad \text{long polysilicon wire} \quad \longrightarrow
$$

---
Page 23
---

## Driving Large Capacitive Load

> $1\,\square C_g = 0.01\,\text{pF}$, $\lambda = 10\,\text{pF}$

$$
V_{in} \rightarrow \boxed{IC} \rightarrow V_{out}
$$

$$
\downarrow\, C_L
$$

- nmos Inverter: $4\tau + 1\tau = 5\tau$
- cmos: $5\tau + 2\tau = 7\tau$

$$
C_L = 10\,\text{pf to } 100\,\text{pf}
$$

$$
C_L = 1000\,\square C_g \text{ to } 10{,}000\,\square C_g
$$

$$
n\tau = RCn
$$

$$
\tau \downarrow \quad R\downarrow \quad \because R = R_S\frac{L}{w}\downarrow
$$

$$
\uparrow A = L \times wn
$$

$$
\tau C_g \propto An \left(\frac{\varepsilon_0 A}{d}\right) \qquad \text{delay} \uparrow
$$

---

**Tapered buffer chain / Geometric cascade of inverters:**

![Datapath Diagram: Tapered Inverter Chain for Large Capacitive Load]

Description of diagram:
- A chain of $n$ inverters, each scaled by a factor $b$ in width (width ratio factor):
  - Inverter 1: ratio **4:1**, size $1:1$ (minimum)
  - Inverter 2: ratio **4:b**, size $1:b$, load cap $1\,\square C_g$ (at output)
  - Inverter 3: ratio **4:b²**, size $1:b^2$, load cap $b^2\,\square C_g$
  - …
  - Inverter $n$: ratio **4:b^{n-1}**, size $1:b^{m-1}$, load $b^n\,\square C_g$
  - Final output: $V_{out}$, load $C_L = b^n\,\square C_g$, load $b^n_{big}$ (large transistor)

**Graph (below):**

![Graph: Delay vs. Width Scale Factor b]

- **X-axis:** $b$ (width scale factor / width rate factor)
- **Y-axis:** delay (↑ increasing upward)
- **Curve:** U-shaped (parabolic-like), with minimum at point $e$ on the x-axis
- **Minimum delay** marked as "min delay" with a dashed horizontal line pointing to the optimum.
- **Optimum delay value** labeled at $b = e$ (Euler's number $e \approx 2.718$)

$$
b \rightarrow \text{width rate factor.}
$$

---
Page 24
---

![Circuit Diagram: Two-Stage CMOS Tapered Inverter — Detail]

**Circuit (top):**

Stage 1 (left):
- $V_{DD}$ at top.
- PMOS $\theta_1$ (ratio **4:b**): source to $V_{DD}$, gate ← $V_{in}$, drain to output node.
- NMOS $\theta_2$ (ratio **1:b**): gate ← $V_{in}$, drain from stage output, source to GND.
- Load cap $2\,\square C_g$ (labeled $b^2\,\text{Dcg}$) at stage 1 output to GND.

Stage 2 (right):
- $V_{DD}$ at top.
- PMOS $\theta_3$ (ratio **4:b²**): source to $V_{DD}$, gate ← Stage 1 output, drain to $V_{out}$.
- NMOS $\theta_4$ (ratio **1:b²**): gate ← Stage 1 output, source to GND. *(labeled $\frac{1}{4}b^2$)*
- Load $C_L = b^3\,\square C_g$ at $V_{out}$ to GND.

$$
C = \frac{\varepsilon_0 A}{d} \qquad A = \omega L = 2\lambda \times 2\lambda \times b^2 = 2\lambda \times 2\lambda b^2
$$

$$
= \frac{\varepsilon_0 \times 2\lambda \times 2\lambda b^2}{d}
$$

$$
= \square C_g \cdot b^2
$$

---

**Case(i): $V_{in} = 0$:**

$$
T_C = R \times C = \left(R_S \frac{L}{w}\right) C
$$

$$
= \left(R_S \frac{1}{b}\right) \cdot b^2\,\square C_g
$$

$$
= \underbrace{\approx}_{\approx} \times 10^4 \times \frac{1}{b} \times b^2\,\square C_g
$$

$$
= b\,\underbrace{\approx} \times 10^4 \times 4
$$

$$
= b\,40 \times 10^4
$$

$$
T_C = 40\cdot b\,\tau
$$

$$
T_{dis} = R \times C = \left(R_S \times \frac{L}{w}\right) C
$$

$$
= 10^4 \times \frac{1}{b^2} \times b^3\,\square C_g
$$

$$
= b\,10^4\,\square C_g
$$

$$
T_{dis} = b\tau
$$

$$
T_{total} = T_C + T_d = 4b\tau + b\tau
$$

$$
= 5b\tau
$$

---
Page 25
---

**Case(ii): $V_{in} = 1$**

$$
T_d = R \times C = R_S \times \frac{L}{w} \left(b^2\,\square C_g\right)
$$

$$
= 10^4 \times \frac{1}{b} \cdot b^2\,\square C_g
$$

$$
T_d = b\tau
$$

$$
T_C = R \times C = R_S \times \frac{L}{w} \left(b^3\,\square C_g\right)
$$

$$
= 10^4 \times \frac{4}{b^2} \times b^3\,\square C_g
$$

$$
= b\cdot 4\tau
$$

$$
T_C = 4b\tau
$$

$$
\text{Total} = T_C + T_d = b\tau + 4b\tau = 5b\tau
$$

**The total delay of a geometric cascade of inverters if $n$ is even:**

$$
T_{d\,total} = \frac{N}{2}\,(5b\tau)
$$

**if $n$ is odd:** $V_{in} = \underbrace{0}_{=1}$:

$$
T_{d\,total} = \left(\frac{N-1}{2}\right) 5b\tau + b\tau
$$

$$
V_{in} = \cancel{1}_0 = 0
$$

$$
T_{d\,total} = \left(\frac{N-1}{2}\right) 5b\tau + 4b\tau
$$

---

**To find the condition for minimum pair delay of geometric cascade of inverters:**

$$
C_L = b^n\,\square C_g
$$

$$
b^n = \frac{C_L}{\square C_g}
$$

$$
y = b^n \rightarrow \circled{1} \quad y = \frac{C_L}{\square C_g}
$$

Take natural logarithm on both sides:

---
Page 26
---

$$
y = b^n
$$

$$
\ln y = n\ln(b) \Rightarrow n = \frac{\ln y}{\ln b} \rightarrow \circled{2}
$$

**Total nMOS inverter pair delay:**

$$
T_d = \frac{N}{2}\,(\text{pair delay})
$$

$$
= \cancel{\otimes}\frac{N}{2}\,(5b\tau)
$$

$$
T_d = 2.5\,N\,b\,\tau
$$

$$
T_d = 2.5\left(\frac{\ln y}{\ln b}\right) b\,\tau \rightarrow \circled{3}
$$

**Diff eq③ wrt '$b$' and equate to zero:**

$$
\text{i.e.} \quad \frac{\partial T_d}{\partial b} = 0
$$

$$
\frac{\partial T_d}{\partial b} = 2.5(\ln y)\tau \left[\frac{\partial b}{\partial(\ln b)}\right]
$$

$$
= 2.5(\ln y)\tau \left[\frac{(\ln b - b\cdot\frac{1}{b})}{(\ln b)^2}\right]
$$

$$
0 = 2.5(\ln y)\tau \left[\frac{\ln b - 1}{2(\ln b)^2}\right]
$$

$$
\ln(b) - 1 = 0
$$

$$
\ln(b) = 1 \qquad b \rightarrow \text{width range factor}
$$

$$
\boxed{b = e}
$$

$\therefore$ **minimum delay is achieved when $b = e$.**

---
Page 27
---

## CMOS Tapered Buffer — Solved Example

![Circuit Diagram: CMOS Two-Stage Tapered Buffer]

**Circuit:**

Stage 1 (left): $V_{DD}$ → PMOS (ratio **4:b**) → output; NMOS (ratio **1:b**) → GND. Load cap $2\,\square C_g$ (labeled $2b\,\square C_g$).

Stage 2 (right): $V_{DD}$ → PMOS (ratio **4:b²**) → $V_{out}$; NMOS (ratio **1:b²**) → GND. Load cap $2\,\square C_g$ (labeled $2b^3\,\square C_g$).

$$
T_{total} = 7b\tau
$$

$$
T_C = 5b\tau \qquad T_d = 2b\tau
$$

---

**Problem ①:** It is required that an IC must derive *on* load a off chip load. Find capacitance of $20\,\text{pF}$ as a off chip load. Find suitable arrangement of nmos inverter and calculate the total delay.

$$
V_{in} \rightarrow \boxed{IC} \xrightarrow{y} \frac{C_L}{20\,\text{pF}}
$$

$$
y = \frac{C_L}{\square C_g} = \frac{20\,\text{pF}}{0.01\,\text{pF}} = 2000
$$

$$
y = 2000
$$

$$
y = b^N = 2000
$$

$$
\ln y = N \ln b
$$

$$
\ln(2000) = N\ln(e)
$$

$$
7.6 = N
$$

$$
\boxed{N = 8}
$$

$$
y_{\cdot} = b^N \qquad b = y^{1/N} = (2000)^{1/8} = 2.58
$$

$$
\text{Since } N \approx 8, \quad b \neq e
$$

So, recalculate $b$ to maintain $y$ constant:

$$
y = b^N \quad \Rightarrow \quad b = y^{1/N} \Rightarrow b = (2000)^{1/8} = 2.58
$$

$$
T_{d\,total} = \frac{N}{2}(5\tau b) = \frac{8}{2}(5\tau b) = 4(5\tau b) = 20b\tau
$$

$$
T_{even} = 20b\tau
$$

---

**Parallel solution (right column, $C_L = 5\,\text{pF}$):**

$$
y = \frac{5\,\text{pF}}{0.01\,\text{pF}} = 500
$$

$$
y \approx 500
$$

$$
y = b^N
$$

$$
\ln y = N\ln b
$$

$$
\ln(500) = N\ln(e)
$$

$$
6.21 = N
$$

$$
\boxed{N = 6}
$$

$$
T_{deven} = \frac{6}{2}(5\tau b)
$$

$$
\boxed{T_{deven} = 15b\tau}
$$

$$
y = b^N, \quad b = (y)^{1/N} = (500)^{1/6}
$$

$$
b = 2.817
$$

$$
\text{Total delay} = 15 \times 2.82 \times 0.1\,\text{ns} = 4.23\,\text{ns}
$$

---
Page 28
---

$$
\text{Total delay} = 20b\tau = 20 \times 2.58\tau
$$

$$
= 51.6 \cancel{8} \times 0.1\,\text{ns}
$$

$$
= 5.16\,\text{nsec.}
$$

---

**Geometric cascade of inverters — summary diagram:**

![Datapath Diagram: Geometric Cascade of Inverters Showing Scaling]

Chain of inverters with scaling ratios:
$$
\text{4:1} \xrightarrow{} \text{4:2.58} \xrightarrow{} \text{4:(2.58)^2} \quad \cdots \quad \text{4:(2.58)^7}
$$

Sizes below each:
$$
\text{1:1} \quad \text{1:2.58} \quad \text{1:(2.58)^2} \quad \cdots \quad \text{1:(2.58)^7} \quad \bot\, C_L
$$

---

**Delay formulae — NMOS and CMOS summary:**

| Case | nMOS | CMOS |
|---|---|---|
| **N is odd, $V_{in}=0$** | $T_d = \frac{(N-1)}{2}(5b\tau) + 4b\tau$ | $T_d = \frac{(N-1)}{2}(7b\tau) + 2b\tau$ |
| **$V_{in}=1$** | $T_d = \frac{(N-1)}{2}(5b\tau) + b\tau$ | $T_d = \frac{(N-1)}{2}(7b\tau) + 5b\tau$ |
| **N is even** | $T_d = \frac{N}{2}(5b\tau)$ | $T_d = \frac{N}{2}(7b\tau)$ |

---
Page 29
---

## Problems

**a) Calculate the capacitance to substrate if $\forall$ area represents (i) metal (ii) polysilicon (iii) n-type diffusion**

![Layout Diagram: Rectangular Wire Segments for Capacitance Calculation]

Description of diagram: Two rectangular wire segments drawn side by side:
- Left segment: width $w = 3\lambda$, length $L = 20\lambda$
- Right segment: width $w = 3\lambda$, length $L = 50\lambda$
- Both separated by the $\leftarrow k \rightarrow$ arrow

$$
C_M = C_A \times \text{Area} = 0.3 \times 10^{-4} \times (3\lambda \times 20\lambda)
$$

---

**(i)** $C_M$ = relative area × relative C value:

$$
C_M = \frac{20\lambda \times 3\lambda}{2\lambda \times 2\lambda} \times \frac{0.3 \times 10^{-4}}{4 \times 10^{-4}}
$$

$$
= 15 \times 0.075\,\square C_g
$$

$$
= 1.125\,\square C_g
$$

---

**(ii)** Consider the same area in polysilicon $\mu\text{m}^2$:

Capacitance to substrate $= 15 \times \dfrac{0.4 \times 10^{-4}}{4 \times 10^{-4}}$

$$
= 15 \times 0.1\,\square C_g
$$

$$
= 1.5\,\square C_g
$$

---

**(iii)** Consider the same area in n-type diffusion:

Capacitance to substrate $= 15 \times \dfrac{1 \times 10^{-4}}{4 \times 10^{-4}}$

$$
= 15 \times 0.25\,\square C_g
$$

$$
= 3.75\,\square C_g
$$

---
Page 30
---

## 4.8.2 Super Buffers

*(Chapter 4, page 110)*

The asymmetry of the conventional inverter is clearly undesirable, and gives rise to significant delay problems when an inverter is used to drive more significant capacitive loads.

A common approach used in nMOS technology to alleviate this effect is to make use of super buffers as in Figures 4–12 and 4–13.

An inverting type is shown in Figure 4–12; considering a positive going logic transition $V_{in}$ at the input, it will be seen that the inverter formed by $T_1$ and $T_2$ is turned on and, thus, the gate of $T_3$ is pulled down toward 0 volt with a small delay. Thus, $T_3$ is cut off while $T_4$ (the gate of which is also connected to $V_{in}$) is turned on and the output is pulled down quickly.

![Circuit Diagram: nMOS Inverting Super Buffer]

**Figure 4–12 Inverting type nMOS super buffer**

Circuit description:
- $V_{DD}$ at top connected to the drain of $T_1$ and $T_3$.
- **$T_1$** (depletion load/nMOS): gate connected to its own source (diode-connected depletion), drain to $V_{DD}$, source to internal node (gate of $T_3$). Forms pull-up for the pre-inverter.
- **$T_2$** (enhancement nMOS, driver): gate ← $V_{in}$, drain to the internal node (junction of $T_1$ source and $T_3$ gate), source to GND.
- **$T_3$** (depletion load/nMOS): gate connected to its own source (diode-connected), drain to $V_{DD}$, source to output node $V_{out}$.
- **$T_4$** (enhancement nMOS, driver): gate ← $V_{in}$ (same as $T_2$), drain to $V_{out}$, source to GND.
- $V_{out}$ is taken from the node between $T_3$ source and $T_4$ drain.

---

![Circuit Diagram: nMOS Non-Inverting Super Buffer]

**Figure 4–13 Non-inverting type nMOS super buffer**

Circuit description:
- $V_{DD}$ at top connected to drains of $T_1$ and $T_3$.
- **$T_1$** (depletion load): gate tied to source, drain to $V_{DD}$, source to internal node.
- **$T_2$** (enhancement driver): gate ← $V_{in}$, drain to internal node (source of $T_1$), source to GND.
- **$T_3$** (depletion load): gate connects to the internal node (source of $T_1$ = drain of $T_2$) — **cross-coupled** compared to Figure 4–12.
- **$T_4$** (enhancement driver): gate tied to $V_{in}$.
- The outputs of $T_1$/$T_2$ stage and $T_3$/$T_4$ stage are cross-coupled: an X-shaped connection between the intermediate node and the gate of $T_3$, and similarly the gate of $T_1$ connects to the drain of $T_4$ / source of $T_3$.
- $V_{out}$ at the junction of $T_3$ source and $T_4$ drain.
- GND at bottom.

> **Key difference from Figure 4–12:** In Figure 4–13, the internal wiring is crossed (non-inverting configuration), creating the X-cross connection visible in the diagram.

---
Page 31
---

*(Basic circuit concepts — 111)*

Now consider the opposite transition: when $V_{in}$ drops to 0 volt, then the gate of $T_3$ is allowed to rise quickly to $V_{DD}$. Thus, as $T_4$ is also turned off by $V_{in}$, $T_3$ is made to conduct with $V_{DD}$ on its gate, that is, with twice the average voltage that would apply if the gate was tied to the source as in the conventional nMOS inverter. Now, since $I_{ds} \propto V_{gs}$ then doubling the effective $V_{gs}$ will increase the current and thus reduce the delay in charging any capacitance on the output, so that more symmetrical transitions are achieved.

The corresponding non-inverting nMOS super buffer circuit is given at Figure 4–13 and, to put matters in perspective, the structures shown when realized in 5 μm technology are capable of driving loads of 2 pF with 5 nsec rise-time.

Other nMOS arrangements such as those based on the native transistor, and known as native super buffers, may be used, but such processes are not readily available to the designer and are mentioned here only briefly.
