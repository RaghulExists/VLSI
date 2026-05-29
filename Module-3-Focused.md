# Module-3 — Focused Exam Answers
### CMOS VLSI Design (ECE23602)

> **Source policy:** Answers are based **primarily on `Module3.md`** (your notes).
> Where something is **not in the notes** but is needed to answer correctly, it is marked **(BEYOND NOTES)** and the source is stated honestly.
> **Important honesty note:** Your notes (`Module3.md`) cover sheet resistance, capacitance, delay, drivers and super buffers — but contain **no scaling theory at all**. Therefore **Q6, Q7, Q13, Q17, Q18, Q19, Q21 (all the scaling questions) are answered from standard MOS scaling theory (Pucknell/Dennard models), not from your notes.** Scaling uses two factors; conventions differ between teachers, so the symbol convention I use is stated explicitly in Q7 — if your class labels the factors the other way (α ↔ β), swap them.
> Transistor schematics, layouts and stick diagrams cannot be drawn meaningfully in Markdown, so they are marked **[INSERT DIAGRAM: …]**. Constants used throughout (5 μm technology, from notes): `1 □Cg = 0.01 pF`, `τ = 0.1 ns`, n-channel `R_S = 10⁴ Ω/□`, p-channel `R_S = 2.5×10⁴ Ω/□`.

---

## Q1. Derive expression for sheet resistance $R_S$ using the sheet resistance model (7 marks)

Consider a uniform slab of conducting material of resistivity $\rho$, width $w$, thickness $t$, and length $L$ between the two end faces $A$ and $B$. Current $I$ flows from face $A$ to face $B$.

**[INSERT DIAGRAM: conducting slab — current $I$ entering the left face, dimensions length $L$, width $w$, thickness $t$.]**

The resistance between $A$ and $B$ is:

$$R_{AB}=\frac{\rho L}{A}=\frac{\rho L}{w\,t}\qquad(\text{cross-sectional area } A=w\,t)$$

Separate the geometry from the material/thickness term:

$$R_{AB}=\frac{\rho}{t}\cdot\frac{L}{w}=R_S\,\frac{L}{w},\qquad \boxed{R_S=\frac{\rho}{t}}$$

For a **square** of the layer, $L=w$, so $L/w = 1$:

$$R_{AB}=\frac{\rho}{t}=R_S$$

Thus $R_S$ is the resistance of **one square** of the material and is **independent of the size of the square** (units: **ohm per square, Ω/□**). The actual resistance of any rectangle is then $R = R_S\,(L/w)$, i.e. simply the number of squares $(L/w)$ in series.

**Key reasoning (step-by-step):**
1. Start from $R = \rho L / A$ with $A = wt$.
2. Group terms: $R_{AB} = (\rho/t)(L/w)$.
3. Define $R_S = \rho/t$ → $R_{AB} = R_S (L/w)$.
4. Set $L = w$ (a square) → $R_{AB} = R_S$.
5. Hence $R_S$ (Ω/□) is size-independent; total $R$ = (number of squares) × $R_S$.

---

## Q2. Derive rise time $T_r$ and fall time $T_f$ for the CMOS inverter (8 marks)

Take a CMOS inverter driving a load capacitance $C_L$. Threshold voltages $V_{tn}=0.2V_{DD}$, $|V_{tp}|=0.2V_{DD}$.

**Rise time $\tau_r$ — output charging ($V_{in}=0$):** p-MOS ON (saturation), n-MOS OFF. The p-device charges $C_L$ with its saturation current:

$$I_{dsp}=\frac{\beta_p\left(V_{gsp}-|V_{tp}|\right)^2}{2}$$

Treating the charging current as constant, $V_{out}=\dfrac{I_{dsp}\,t}{C_L}$, so the time to reach $V_{out}=V_{DD}$ is:

$$\tau_r=\frac{2\,C_L\,V_{DD}}{\beta_p\left(V_{DD}-|V_{tp}|\right)^2}$$

Putting $|V_{tp}|=0.2V_{DD}$ → $\left(V_{DD}-0.2V_{DD}\right)^2 = 0.64\,V_{DD}^2$:

$$\boxed{\tau_r\approx\frac{3\,C_L}{\beta_p\,V_{DD}}}$$

**Fall time $t_f$ — output discharging ($V_{in}=1$):** n-MOS ON (saturation), p-MOS OFF. By the same steps with $I_{dsn}=\dfrac{\beta_n(V_{gs}-V_{tn})^2}{2}$:

$$\boxed{t_f\approx\frac{3\,C_L}{\beta_n\,V_{DD}}}$$

**Relation:** $\dfrac{\tau_r}{t_f}=\dfrac{\beta_n}{\beta_p}$. Since $\mu_n=2.5\,\mu_p$, $\beta_n=2.5\,\beta_p$, hence

$$\tau_r\approx 2.5\,t_f$$

(p-MOS is ~2.5× slower; for equal edges make $W_p=2.5\,W_n$). Also $t_r,\,t_f\propto C_L$ and $\propto 1/V_{DD}$.

**Key reasoning (step-by-step):**
1. Charging: only p-MOS conducts in saturation; constant-current charge of $C_L$.
2. $V_{out}=I_{dsp}t/C_L$; set $V_{out}=V_{DD}$ → $\tau_r = 2C_LV_{DD}/[\beta_p(V_{DD}-|V_{tp}|)^2]$.
3. Sub $|V_{tp}|=0.2V_{DD}$ → $\tau_r\approx 3C_L/(\beta_p V_{DD})$.
4. Discharging is the dual (n-MOS) → $t_f\approx 3C_L/(\beta_n V_{DD})$.
5. Ratio $\tau_r/t_f=\beta_n/\beta_p=2.5$ → $\tau_r\approx 2.5\,t_f$.

---

## Q3. IC must drive a 20 pF off-chip load — find the nMOS inverter arrangement and total delay (7 marks)

A single minimum inverter cannot drive 20 pF quickly, so use a **geometric cascade (tapered chain) of $N$ nMOS inverters**, each wider than the previous by a width factor $b$, with **$b=e$** for minimum delay.

Given $C_L = 20\text{ pF}$, $1\,\square C_g = 0.01\text{ pF}$, $\tau = 0.1\text{ ns}$.

$$y=\frac{C_L}{\square C_g}=\frac{20}{0.01}=2000$$

Number of stages (optimum $b=e$): $\;\ln y = N\ln e \Rightarrow N=\ln(2000)=7.6\approx \boxed{8}$

Recompute $b$ for the integer $N=8$ (keep $y$ fixed):

$$b=y^{1/N}=2000^{1/8}=2.58$$

Total delay (N even, nMOS): $T_d=\dfrac{N}{2}(5b\tau)=\dfrac{8}{2}(5b\tau)=20\,b\tau$

$$T_d=20\times 2.58\times 0.1\text{ ns}=\boxed{5.16\text{ ns}}$$

**[INSERT DIAGRAM: tapered nMOS inverter chain — ratios 4:1, 4:b, 4:b², … 4:b⁷ driving $C_L$.]**

**Key reasoning (step-by-step):**
1. Big load ⇒ use a tapered cascade, optimum width ratio $b=e$.
2. $y=C_L/\square C_g = 2000$.
3. $N=\ln y = 7.6 \to 8$ stages.
4. Re-solve $b=y^{1/N}=2000^{1/8}=2.58$.
5. $T_d=(N/2)(5b\tau)=20b\tau=5.16$ ns.

---

## Q4. Propagation delay in cascaded pass transistors (6 marks)

When a logic signal passes through several pass transistors in series (all gates at $V_{DD}$), each transistor contributes series resistance $R$ and each node a capacitance $C$ to ground (an RC ladder). For node $V_2$:

$$C\frac{dV_2}{dt}=\frac{[(V_1-V_2)-(V_2-V_3)]}{R}$$

As the number of sections becomes large this reduces to the diffusion-type equation

$$RC\frac{dV}{dt}=\frac{d^2V}{dx^2}\quad\Rightarrow\quad \tau_p\propto x^2$$

so propagation time grows as the **square of the distance** (number of stages). Lumping all sections:

$$R_{total}=n\,r\,R_S,\qquad C_{total}=n\,c\,\square C_g$$

$$\boxed{\tau_d=n^2\,r\,c\,(\tau)}$$

where $r$, $c$ are the relative R and C per section and $n$ the number of sections. Because delay $\propto n^2$, **no more than 4 pass transistors** should be put in series; beyond that, insert a **buffer between each group of four** (or accept the longer delay).

**Key reasoning (step-by-step):**
1. Series pass transistors = RC ladder ($R$ each, $C$ each node).
2. Node equation → in the limit $RC\,dV/dt = d^2V/dx^2$.
3. Hence $\tau_p \propto x^2$ (delay grows with the square of length).
4. Lumped: $R_{total}=nrR_S$, $C_{total}=nc\square C_g$ → $\tau_d=n^2 rc\,\tau$.
5. $n^2$ growth ⇒ limit to 4 in series; buffer between groups of four.

---

## Q5. Propagation delay in long-channel polysilicon wires (5 marks)

A long polysilicon wire also has **distributed series resistance $R$ and shunt capacitance $C$**, so it behaves like the same RC ladder as cascaded pass transistors — signal propagation is **slowed down**, with delay growing as the square of the wire length ($\tau_p\propto x^2$).

Remedy: drive long polysilicon runs with **buffers**, which gives two desirable effects:
1. **Signal propagation is speeded up.**
2. **Reduced sensitivity to noise.**

**[INSERT DIAGRAM: long poly line as 4 cascaded RC sections ($R$ series, $C$ shunt) followed by a buffer driving $V_{out}$.]**

**Key reasoning (step-by-step):**
1. Long poly = distributed R and C.
2. Same RC-ladder model → delay $\propto$ length².
3. Long runs are therefore slow and noise-sensitive.
4. Insert buffers along/at the end of the run.
5. Result: faster propagation + better noise immunity.

---

## Q6. Draw the scaled nMOS transistor and explain scaling factors for device parameters (6 marks) — **(BEYOND NOTES)**

> Not present in `Module3.md`. Answered from standard MOS scaling theory.

In scaling, every dimension of the device is reduced. Using the combined model, lateral dimensions ($W$, $L$, channel length) are divided by $\alpha$, while supply voltage $V_{DD}$ and gate-oxide thickness $D$ are divided by $\beta$ (set $\beta=\alpha$ for **constant-field**, $\beta=1$ for **constant-voltage**).

**[INSERT DIAGRAM: nMOS transistor before and after scaling — original $L,W,D$ shown; scaled device with $L/\alpha$, $W/\alpha$, oxide $D/\beta$, with $V_{DD}/\beta$ applied.]**

Scaling factors for the basic device parameters (combined model):

| Device parameter | Expression | Scaling factor |
|---|---|---|
| Channel length $L$, width $W$ | — | $1/\alpha$ |
| Gate-oxide thickness $D$, supply $V_{DD}$ | — | $1/\beta$ |
| Gate area $A_g$ | $W\cdot L$ | $1/\alpha^2$ |
| Gate capacitance/unit area $C_{OX}$ | $\varepsilon_{ox}/D$ | $\beta$ |
| Gate capacitance $C_g$ | $C_{OX}\,A_g$ | $\beta/\alpha^2$ |
| Saturation current $I_{dss}$ | $\tfrac{\mu C_{OX}}{2}\tfrac{W}{L}(V-V_t)^2$ | $1/\beta$ |
| Gate delay $T_d$ | $C_g V/I_{dss}$ | $\beta/\alpha^2$ |

**Key reasoning (step-by-step):**
1. Define dimension factor $1/\alpha$ and voltage/oxide factor $1/\beta$.
2. Areas scale as (length factor)² → $1/\alpha^2$.
3. $C_{OX}=\varepsilon/D$ rises by $\beta$ as $D$ shrinks.
4. Currents/capacitances follow from the device equations.
5. Constant-field is the special case $\beta=\alpha$.

---

## Q7. Determine the scaling factor for: (i) Gate delay $T_D$, (ii) Parasitic capacitance $C_X$, (iii) Gate capacitance/unit area $C_{OX}$, (iv) Current density $J$, (v) Power dissipation per gate $P_g$ (10 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. Answered from the **combined scaling model**.

**Convention used (state this in the exam):** $1/\alpha$ = scaling factor for all linear dimensions ($W,L$); $1/\beta$ = scaling factor for supply voltage $V_{DD}$ and gate-oxide thickness $D$. *(If your class swaps the labels, swap $\alpha\leftrightarrow\beta$.)*

| # | Parameter | Derivation | Scaling factor |
|---|---|---|---|
| (i) | Gate delay $T_D = C_g V_{DD}/I_{dss}$ | $(\beta/\alpha^2)(1/\beta)\div(1/\beta)$ | $\beta/\alpha^2$ |
| (ii) | Parasitic capacitance $C_X$ | scales with linear dimension | $1/\alpha$ |
| (iii) | Gate cap/unit area $C_{OX}=\varepsilon_{ox}/D$ | $D\to D/\beta$ | $\beta$ |
| (iv) | Current density $J=I_{dss}/A_g$ | $(1/\beta)\div(1/\alpha^2)$ | $\alpha^2/\beta$ |
| (v) | Power per gate $P_g=V_{DD}\,I_{dss}$ | $(1/\beta)(1/\beta)$ | $1/\beta^2$ |

**Constant-field check ($\beta=\alpha$):** $T_D=1/\alpha$, $C_X=1/\alpha$, $C_{OX}=\alpha$, $J=\alpha$, $P_g=1/\alpha^2$ — the standard results.

**Key reasoning (step-by-step):**
1. $C_{OX}=\varepsilon/D$ → factor $\beta$.
2. $I_{dss}\propto C_{OX}(W/L)V^2 = \beta\cdot1\cdot(1/\beta)^2 = 1/\beta$.
3. $T_D=C_gV/I_{dss}$ with $C_g=\beta/\alpha^2$ → $\beta/\alpha^2$.
4. $J=I_{dss}/A_g=(1/\beta)/(1/\alpha^2)=\alpha^2/\beta$.
5. $P_g=V\,I=1/\beta^2$; $C_X$ tracks linear size → $1/\alpha$.

---

## Q8. Cascaded inverters as drivers for large capacitive loads (8 marks)

Large off-chip loads are $C_L \approx 10$–$100$ pF ($\approx 1000$–$10{,}000\;\square C_g$). A single inverter is too slow (driving with one stage means either small $R$ but huge gate area/own capacitance, or small device but large RC) — there is a conflict. The solution is a **geometric cascade (tapered chain) of inverters**: each stage is made larger than the previous by a constant **width factor $b$**, so a small first stage is driven easily and successive stages provide the current to drive the next larger stage, the final one driving $C_L$.

- Stage sizes: $1,\,b,\,b^2,\dots,b^{N-1}$; final load $C_L=b^N\,\square C_g$.
- Each nMOS inverter **pair delay** works out to $5b\tau$ (and $7b\tau$ for CMOS).
- Total stages: $N=\dfrac{\ln(C_L/\square C_g)}{\ln b}$; total delay (N even, nMOS) $=\dfrac{N}{2}(5b\tau)$.
- There is a trade-off: small $b$ → many stages; large $b$ → few but each slow. The delay-vs-$b$ curve is U-shaped with a **minimum at $b=e$** (see Q15).

**[INSERT DIAGRAM: tapered inverter chain (ratios 4:1, 4:b, 4:b², …) + U-shaped delay-vs-$b$ curve with minimum at $b=e$.]**

**Key reasoning (step-by-step):**
1. Off-chip $C_L$ is 1000–10000 □Cg → one inverter can't drive it fast.
2. Use a chain where each stage scales up by $b$.
3. Each stage only drives a load $b$× its own size → manageable per-stage delay.
4. Total delay $=(N/2)(5b\tau)$ with $N=\ln(C_L/\square C_g)/\ln b$.
5. Optimum width ratio is $b=e$ (minimises total delay).

---

## Q9. Super buffers as drivers for large capacitive loads (8 marks)

The conventional nMOS inverter is **asymmetric** (slow, weak depletion pull-up) — poor at charging large capacitive loads. A **super buffer** fixes this using a two-transistor-pair arrangement (Fig 4-12 inverting, Fig 4-13 non-inverting).

**Inverting super buffer (Fig 4-12):** transistors $T_1$/$T_2$ form a pre-inverter; $T_3$ (depletion load) and $T_4$ (driver) form the output stage; $V_{in}$ drives both $T_2$ and $T_4$.
- **$V_{in}$ rising:** $T_1$–$T_2$ pull the gate of $T_3$ down to ~0 V (small delay); $T_3$ cuts off while $T_4$ turns on → output pulled **low quickly**.
- **$V_{in}$ falling (→0):** the gate of $T_3$ is allowed to rise quickly to $V_{DD}$; $T_4$ is off, so $T_3$ conducts **with $V_{DD}$ on its gate — about twice the average $V_{gs}$** of a normal inverter whose load gate is tied to its source.

Since $I_{ds}\propto V_{gs}$, **doubling the effective $V_{gs}$ increases the charging current**, reducing the delay in charging the output capacitance → **more symmetrical (faster) transitions**. The non-inverting version (Fig 4-13) uses the same idea with the internal connections cross-coupled. In 5 μm technology a super buffer can drive **2 pF with ~5 ns rise time**.

**[INSERT DIAGRAM: (a) inverting nMOS super buffer Fig 4-12 ($T_1$–$T_4$); (b) non-inverting nMOS super buffer Fig 4-13 with cross-coupled internal node.]**

**Key reasoning (step-by-step):**
1. Plain nMOS inverter pull-up is weak/asymmetric → slow for big loads.
2. Super buffer = pre-inverter stage + output driver stage.
3. On the charging edge, the output depletion device gets ~$V_{DD}$ on its gate.
4. $I_{ds}\propto V_{gs}$, so ~2× $V_{gs}$ → much larger charging current.
5. Result: faster, symmetric edges (drives 2 pF in ~5 ns at 5 μm).

---

## Q10. On-resistance of the n-pass transistors in Fig. 1(a), $R_S=10^4\;\Omega/\square$ (5 marks)

On-resistance of a channel: $R = R_S\,\dfrac{L}{W}$ (number of squares × sheet resistance).

**Transistor (a): $W=2\lambda$, $L=2\lambda$**

$$R=R_S\frac{L}{W}=10^4\times\frac{2\lambda}{2\lambda}=10^4\times1=\boxed{10\text{ k}\Omega}$$

**Transistor (b): $W=2\lambda$, $L=8\lambda$**

$$R=R_S\frac{L}{W}=10^4\times\frac{8\lambda}{2\lambda}=10^4\times4=\boxed{40\text{ k}\Omega}$$

**Key reasoning (step-by-step):**
1. $R_{on}=R_S(L/W)$ = (squares in the channel) × $R_S$.
2. (a) $L/W=2\lambda/2\lambda=1$ → $R=R_S=10$ kΩ.
3. (b) $L/W=8\lambda/2\lambda=4$ → $R=4R_S=40$ kΩ.
4. Longer channel (more squares) ⇒ proportionally higher resistance.

---

## Q11. Total capacitance of the multilayer structure in Fig. 2(b) (8 marks)

Relative area-capacitance values (referred to gate-to-channel = 1): metal = 0.075, polysilicon = 0.1, gate-to-channel = 1.
Work in units of $\square C_g$ using: $C\,[\square C_g] = (\text{relative value})\times\dfrac{\text{area}}{4\lambda^2}$ (since $1\,\square C_g$ = gate cap of a $2\lambda\times2\lambda=4\lambda^2$ area).

**Metal-to-substrate** (metal $100\lambda\times3\lambda = 300\lambda^2$):

$$C_{MS}=0.075\times\frac{300\lambda^2}{4\lambda^2}=0.075\times75=5.625\;\square C_g$$

**Polysilicon-to-substrate** (area $= (2\lambda\times2\lambda)+(2\lambda\times1\lambda)+(4\lambda\times4\lambda)=4+2+16=22\lambda^2$):

$$C_{PS}=0.1\times\frac{22\lambda^2}{4\lambda^2}=0.1\times5.5=0.55\;\square C_g$$

**Gate-to-channel** (area $2\lambda\times2\lambda=4\lambda^2$):

$$C_{gC}=1\times\frac{4\lambda^2}{4\lambda^2}=1\;\square C_g$$

**Total (excluding diffusion):**

$$C_T=C_{MS}+C_{PS}+C_{gC}=5.625+0.55+1=7.175\approx\boxed{7.2\;\square C_g}$$

> Note: the figure text lists the metal width as both 4λ and 3λ; the notes' calculation uses **3λ** (area 300λ²), which gives the standard 7.2 □Cg result. If 4λ is intended, $C_{MS}$ and the total change accordingly.

**[INSERT DIAGRAM: stacked metal / diffusion / polysilicon structure of Fig 2(b) with the labelled dimensions.]**

**Key reasoning (step-by-step):**
1. Convert each layer to □Cg via (relative value)×(area/4λ²).
2. Metal: $0.075\times(300/4)=5.625$ □Cg.
3. Poly: $0.1\times(22/4)=0.55$ □Cg.
4. Gate: $1\times(4/4)=1$ □Cg.
5. Sum = 7.175 ≈ 7.2 □Cg.

---

## Q12. Two cascaded nMOS inverters driving $C_L=16\,\square C_g$ — delay in $\tau$; with $\tau=0.3$ ns; with stray $+4\,\square C_g$ (8 marks)

**Specifications.** Inverter 1: $L_{pu}=16\lambda,W_{pu}=2\lambda$ (ratio 8:1), $L_{pd}=2\lambda,W_{pd}=2\lambda$ (1:1). Inverter 2: $L_{pu}=2\lambda,W_{pu}=2\lambda$ (1:1), $L_{pd}=2\lambda,W_{pd}=8\lambda$ (1:4).
Interstage capacitance $C_1$ = gate capacitance of inverter-2 input device $=\dfrac{\varepsilon(2\lambda\times8\lambda)}{d}=4\,\square C_g$. Load $C_L=16\,\square C_g$.

**Case $V_i=0$ V:** inverter-1 output charges through its pull-up; inverter-2 output discharges through its (wide) pull-down.

$$T_{charg}=R_{pu1}\,C_1=10^4\left(\tfrac{16\lambda}{2\lambda}\right)4\,\square C_g=10^4(8)(4)\;\square C_g=32\tau$$
$$T_{dis}=R_{pd2}\,C_L=10^4\left(\tfrac{2\lambda}{8\lambda}\right)16\,\square C_g=10^4\left(\tfrac14\right)(16)\;\square C_g=4\tau$$
$$\boxed{T_{total}=36\tau}$$

**Case $V_i=5$ V:** inverter-1 output discharges; inverter-2 output charges.

$$T_{dis}=R_{pd1}\,C_1=10^4\left(\tfrac{2\lambda}{2\lambda}\right)4\,\square C_g=4\tau,\qquad T_{charg}=R_{pu2}\,C_L=10^4\left(\tfrac{2\lambda}{2\lambda}\right)16\,\square C_g=16\tau$$
$$\boxed{T_{total}=20\tau}$$

**Delay with $\tau=0.3$ ns:** $36\tau = 36(0.3)=\boxed{10.8\text{ ns}}$ (worst case, $V_i=0$); $20\tau=20(0.3)=6\text{ ns}$ ($V_i=5$).

**Allowing for stray capacitance (+$4\,\square C_g$ each → $C_1=8\,\square C_g$, $C_L=20\,\square C_g$):**

- $V_i=0$: $T_{charg}=10^4(8)(8)=64\tau$, $T_{dis}=10^4(\tfrac14)(20)=5\tau$ → $69\tau=69(0.3)=\boxed{20.7\text{ ns}}$
- $V_i=5$: $T_{dis}=10^4(1)(8)=8\tau$, $T_{charg}=10^4(1)(20)=20\tau$ → $28\tau=28(0.3)=8.4\text{ ns}$

**Key reasoning (step-by-step):**
1. Interstage $C_1$ = gate cap of the $8\lambda\times2\lambda$ input device = 4 □Cg.
2. Each delay $=R_S(L/W)\times C$ in τ units ($R_S=10^4$, $1\,\square C_g\Rightarrow\tau$).
3. $V_i=0$: $32\tau+4\tau=36\tau$; $V_i=5$: $4\tau+16\tau=20\tau$.
4. Multiply by $\tau=0.3$ ns → 10.8 ns / 6 ns.
5. Add 4 □Cg stray to each cap and repeat → 69τ = 20.7 ns / 28τ = 8.4 ns.

---

## Q13. Constant-Field Model scaling factor for: (i) $I_{dss}$, (ii) Switching energy/gate $E_g$, (iii) Power/gate $P_g$, (iv) Max operating frequency $f_o$ (6 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. Constant-field (full) scaling: all dimensions **and** voltages scale by $1/\alpha$ (i.e. $\beta=\alpha$).

| # | Parameter | Derivation | Scaling factor |
|---|---|---|---|
| (i) | $I_{dss}=\tfrac{\mu C_{OX}}{2}\tfrac{W}{L}(V-V_t)^2$ | $\alpha\cdot1\cdot(1/\alpha)^2$ | $1/\alpha$ |
| (ii) | Switching energy $E_g=P_g\,T_d$ | $(1/\alpha^2)(1/\alpha)$ | $1/\alpha^3$ |
| (iii) | Power/gate $P_g=V_{DD}\,I_{dss}$ | $(1/\alpha)(1/\alpha)$ | $1/\alpha^2$ |
| (iv) | Max frequency $f_o=1/T_d$ | $1/(1/\alpha)$ | $\alpha$ |

**Key reasoning (step-by-step):**
1. Constant field → $V_{DD}$ and $D$ scale by $1/\alpha$ (so $C_{OX}\!\to\!\alpha$).
2. $I_{dss}\propto C_{OX}V^2 = \alpha(1/\alpha)^2 = 1/\alpha$.
3. $P_g=V I = (1/\alpha)(1/\alpha)=1/\alpha^2$.
4. $T_d=1/\alpha$ → $E_g=P_gT_d=1/\alpha^3$.
5. $f_o=1/T_d=\alpha$.

---

## Q14. Time-delay calculation for a CMOS inverter pair (5 marks)

Each CMOS inverter drives $2\,\square C_g$. $R_{pu}$ (p-MOS) uses $R_S=2.5\times10^4\,\Omega/\square$; $R_{pd}$ (n-MOS) uses $R_S=10^4\,\Omega/\square$; both 1:1.

**Charging (one stage, $V_{in}=0$):**

$$T_{charg}=R_{pu}\,C=\left(2.5\times10^4\cdot\tfrac11\right)2\,\square C_g=5\tau$$

**Discharging (other stage, $V_{in}=1$):**

$$T_{dis}=R_{pd}\,C=\left(10^4\cdot\tfrac11\right)2\,\square C_g=2\tau$$

**Total pair delay:**

$$T=T_{charg}+T_{dis}=5\tau+2\tau=\boxed{7\tau}$$

**Key reasoning (step-by-step):**
1. CMOS inverter load = 2 □Cg.
2. $R_{pu}=2.5\times10^4$, so charging = $2.5\times10^4\times2\square C_g = 5\tau$.
3. $R_{pd}=10^4$, so discharging = $10^4\times2\square C_g = 2\tau$.
4. Pair delay = 5τ + 2τ = 7τ.

---

## Q15. Condition for minimum pair delay of a geometric cascade of inverters (7 marks)

For a tapered chain, $C_L=b^N\,\square C_g$, so $y=\dfrac{C_L}{\square C_g}=b^N$. Take logs:

$$\ln y=N\ln b\;\Rightarrow\;N=\frac{\ln y}{\ln b}$$

Total nMOS inverter-pair delay ($T_d=\tfrac{N}{2}\times 5b\tau = 2.5Nb\tau$):

$$T_d=2.5\left(\frac{\ln y}{\ln b}\right)b\,\tau$$

Differentiate w.r.t. $b$ and set to zero ($\ln y,\tau$ constant):

$$\frac{\partial T_d}{\partial b}=2.5\,(\ln y)\,\tau\left[\frac{\ln b-1}{(\ln b)^2}\right]=0\;\Rightarrow\;\ln b-1=0$$

$$\ln b=1\;\Rightarrow\;\boxed{b=e\approx 2.718}$$

So **minimum total delay is achieved when the width-scaling factor $b=e$** (each stage ≈ 2.7× the previous).

**Key reasoning (step-by-step):**
1. $C_L=b^N\square C_g$ → $N=\ln y/\ln b$.
2. Total delay $T_d=2.5Nb\tau=2.5(\ln y/\ln b)b\tau$.
3. $dT_d/db=0$ → numerator $\ln b - 1 = 0$.
4. $\ln b = 1$ → $b=e$.
5. Hence optimum taper ratio is $e$.

---

## Q16. Time-delay calculation for an nMOS inverter pair (5 marks)

Each nMOS inverter: pull-up 4:1 ($L/W=8\lambda/2\lambda$), pull-down 1:1, load $1\,\square C_g$; $R_S=10^4\,\Omega/\square$.

**Charging (stage with $V_{in}=0$):**

$$T_{charg}=R_{pu}\,C=\left(10^4\cdot\tfrac41\right)1\,\square C_g=4\tau$$

**Discharging (stage with $V_{in}=1$):**

$$T_{dis}=R_{pd}\,C=\left(10^4\cdot\tfrac11\right)1\,\square C_g=1\tau$$

**Total pair delay:**

$$T=T_{charg}+T_{dis}=4\tau+1\tau=\boxed{5\tau}$$

**Key reasoning (step-by-step):**
1. nMOS inverter load = 1 □Cg, pull-up ratio 4:1, pull-down 1:1.
2. Charging via pull-up: $10^4(4)(1\square C_g)=4\tau$.
3. Discharging via pull-down: $10^4(1)(1\square C_g)=1\tau$.
4. Pair delay = 4τ + 1τ = 5τ.

---

## Q17. Determine scaling factor for: (i) Power–speed product $P_T$, (ii) Carrier density in channel $Q_{on}$, (iii) Gate delay $T_d$, (iv) Current density $J$ (8 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. Combined model (same convention as Q7: dimensions $1/\alpha$, voltage & oxide $1/\beta$).

| # | Parameter | Derivation | Scaling factor |
|---|---|---|---|
| (i) | Power–speed product $P_T=P_g\,T_d$ | $(1/\beta^2)(\beta/\alpha^2)$ | $1/(\alpha^2\beta)$ |
| (ii) | Carrier density $Q_{on}=C_{OX}V_{gs}$ | $\beta\cdot(1/\beta)$ | $1$ (unchanged) |
| (iii) | Gate delay $T_d=C_gV/I_{dss}$ | as in Q7 | $\beta/\alpha^2$ |
| (iv) | Current density $J=I_{dss}/A_g$ | $(1/\beta)/(1/\alpha^2)$ | $\alpha^2/\beta$ |

**Constant-field check ($\beta=\alpha$):** $P_T=1/\alpha^3$, $Q_{on}=1$, $T_d=1/\alpha$, $J=\alpha$.

**Key reasoning (step-by-step):**
1. $Q_{on}=C_{OX}V$: $\beta\times(1/\beta)=1$ → stays constant.
2. $T_d=\beta/\alpha^2$ (from Q7 derivation).
3. $P_g=1/\beta^2$, so $P_T=P_gT_d=1/(\alpha^2\beta)$.
4. $J=I_{dss}/A_g=(1/\beta)/(1/\alpha^2)=\alpha^2/\beta$.

---

## Q18. Constant-Field Model scaling factor for: (i) $I_{dss}$, (ii) $E_g$, (iii) $P_g$, (iv) $f_o$ (8 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. **Same as Q13** (constant-field, $\beta=\alpha$):

| # | Parameter | Scaling factor |
|---|---|---|
| (i) | Saturation current $I_{dss}$ | $1/\alpha$ |
| (ii) | Switching energy/gate $E_g$ | $1/\alpha^3$ |
| (iii) | Power dissipation/gate $P_g$ | $1/\alpha^2$ |
| (iv) | Max operating frequency $f_o$ | $\alpha$ |

**Key reasoning (step-by-step):**
1. $I_{dss}\propto C_{OX}V^2=\alpha(1/\alpha)^2=1/\alpha$.
2. $P_g=VI=1/\alpha^2$.
3. $E_g=P_gT_d=(1/\alpha^2)(1/\alpha)=1/\alpha^3$.
4. $f_o=1/T_d=\alpha$.

---

## Q19. Constant-Voltage Model scaling factor for: (i) Gate delay $T_D$, (ii) Parasitic cap $C_X$, (iii) Gate cap/unit area $C_{OX}$, (iv) Current density $J$, (v) Power/gate $P_g$ (6 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. **Constant-voltage**: lateral dimensions scale by $1/\alpha$ but $V_{DD}$ and oxide thickness $D$ are held constant ($\beta=1$).

| # | Parameter | Derivation ($\beta=1$) | Scaling factor |
|---|---|---|---|
| (i) | Gate delay $T_D$ | $\beta/\alpha^2$ | $1/\alpha^2$ |
| (ii) | Parasitic cap $C_X$ | linear dimension | $1/\alpha$ |
| (iii) | Gate cap/area $C_{OX}$ | $D$ constant | $1$ |
| (iv) | Current density $J$ | $\alpha^2/\beta$ | $\alpha^2$ |
| (v) | Power/gate $P_g$ | $1/\beta^2$ | $1$ |

**Key reasoning (step-by-step):**
1. Constant voltage → $V_{DD}$ and $D$ fixed ($\beta=1$), so $C_{OX}=1$, $P_g=1$.
2. $I_{dss}\propto C_{OX}(W/L)V^2 = 1$ → unchanged.
3. $T_D=C_gV/I_{dss}=(1/\alpha^2)(1)/(1)=1/\alpha^2$ (delay improves by $\alpha^2$).
4. $J=I_{dss}/A_g = 1/(1/\alpha^2)=\alpha^2$ (high current density → reliability issue).
5. $C_X$ tracks linear size → $1/\alpha$.

---

## Q20. IC must drive a 5 pF off-chip load — find the nMOS inverter arrangement and total delay (7 marks)

Same tapered-cascade method as Q3. Given $C_L=5\text{ pF}$, $1\,\square C_g=0.01\text{ pF}$, $\tau=0.1\text{ ns}$, optimum $b=e$.

$$y=\frac{C_L}{\square C_g}=\frac{5}{0.01}=500$$

$$N=\ln y=\ln(500)=6.21\approx \boxed{6}$$

$$b=y^{1/N}=500^{1/6}=2.817$$

Total delay (N even, nMOS): $T_d=\dfrac{N}{2}(5b\tau)=\dfrac{6}{2}(5b\tau)=15\,b\tau$

$$T_d=15\times2.817\times0.1\text{ ns}=\boxed{4.23\text{ ns}}$$

**[INSERT DIAGRAM: 6-stage tapered nMOS inverter chain driving 5 pF.]**

**Key reasoning (step-by-step):**
1. $y=C_L/\square C_g=500$.
2. $N=\ln(500)=6.21\to 6$ stages.
3. $b=500^{1/6}=2.817$.
4. $T_d=(N/2)(5b\tau)=15b\tau$.
5. $=15\times2.817\times0.1=4.23$ ns.

---

## Q21. Determine scaling factor for: (i) Channel resistance $R_{ON}$, (ii) Parasitic cap $C_X$, (iii) Gate area $A_g$, (iv) Gate cap/unit area $C_{OX}$ (6 marks) — **(BEYOND NOTES)**

> Not in `Module3.md`. Combined model (dimensions $1/\alpha$, voltage & oxide $1/\beta$; same convention as Q7).

| # | Parameter | Derivation | Scaling factor |
|---|---|---|---|
| (i) | $R_{ON}=\dfrac{(L/W)}{\mu C_{OX}(V_{gs}-V_t)}$ | $\dfrac{1}{\beta\,(1/\beta)}$ | $1$ (unchanged) |
| (ii) | Parasitic cap $C_X$ | linear dimension | $1/\alpha$ |
| (iii) | Gate area $A_g=W\cdot L$ | $(1/\alpha)(1/\alpha)$ | $1/\alpha^2$ |
| (iv) | Gate cap/unit area $C_{OX}=\varepsilon_{ox}/D$ | $D\to D/\beta$ | $\beta$ |

**Key reasoning (step-by-step):**
1. $R_{ON}$: $L/W$ ratio invariant; $C_{OX}\!\to\!\beta$, $V\!\to\!1/\beta$ → product cancels → factor 1.
2. $C_X$ scales with the linear dimension → $1/\alpha$.
3. $A_g=W L$ → $(1/\alpha)^2=1/\alpha^2$.
4. $C_{OX}=\varepsilon/D$ → $\beta$.

---

### Honest summary of confidence

- **Fully from your notes (high confidence):** Q1, Q2, Q3, Q4, Q5, Q8, Q9, Q10, Q11, Q12, Q14, Q15, Q16, Q20.
- **Beyond your notes — standard scaling theory (verify the α/β labelling against your class notes):** Q6, Q7, Q13, Q17, Q18, Q19, Q21. The numbers are internally consistent and reduce to the textbook constant-field results; the only thing that can differ between teachers is *which* factor is called α and which β. If your notes/teacher define them oppositely, swap α ↔ β in those answers.
