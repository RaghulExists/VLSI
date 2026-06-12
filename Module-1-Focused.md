# Module 1 — Focused Exam Answers

> Source: your chat notes are the primary source. Anywhere I use knowledge **not in your notes**, it is marked **[outside notes]**.
> Each ASCII diagram is **kept**, with a ready-to-compile **TikZ block beneath it**. All blocks assume the one-time **LaTeX setup** section is pasted once into your preamble. There is **no LaTeX on this machine**, so the TikZ was written but **not compiled/verified here** — build on Overleaf.

---

## The 7 Master Diagrams (learn these, reuse everywhere)

| Master | What it is | Used in |
|---|---|---|
| **M1** | nMOS cross-section, 4 channel states (none / uniform / tapered / pinched) | Q1, Q5, Q7 |
| **M2** | MOS-capacitor: accumulation / depletion / inversion | Q10 |
| **M3** | CMOS inverter schematic (pMOS over nMOS) | Q4, Q8, Q11, Q12 |
| **M4** | Inverter transfer curve (S-curve) + noise-margin bands | Q4, Q8 |
| **M5** | Transmission gate + 2:1 mux | Q2, Q9, Q12 |
| **M6** | Ids–Vds output-characteristic family | Q1, Q6 |
| **M7** | Subthreshold log-plot (leakage/DIBL/GIDL) | Q6 |
| **M8** | Tristate inverter 4-transistor stack | Q12, Q18 |
| **M9** | Compound (AOI) gate skeleton — series/parallel build | Q21 |

Newly added questions reuse the same masters:
**Q18** → M8 · **Q19** → M6 + M7 (mobility/velocity) · **Q20** → M3 + M4 (skewed S-curve) · **Q21** → M9 · **Q23** → M3 (NOR = inverter with parallel-nMOS / series-pMOS).

If you memorize **M1, M2, M3, M4, M5** by hand, you can draw 14 of 17 questions.

---

## LaTeX setup — paste this **ONCE** (preamble)

Every TikZ block below uses these styles + one glyph macro (`\mosbox` = a labelled MOSFET box, drain stub on top, source stub on bottom, gate = the label).

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,calc}
\tikzset{
  wire/.style ={line width=0.8pt},
  dot/.style  ={circle,fill=black,inner sep=1.3pt},
  lbl/.style  ={font=\footnotesize},
  axis/.style ={-{Stealth[length=2mm]},line width=0.8pt},
  curve/.style={line width=1pt},
}
% vertical MOSFET as a labelled box: (x,y,label).  Drain stub top, source stub bottom.
\newcommand{\mosbox}[3]{\draw[wire](#1-0.5,#2-0.4) rectangle (#1+0.5,#2+0.4);%
  \node[font=\tiny] at (#1,#2){#3};%
  \draw[wire](#1,#2+0.4)--(#1,#2+0.6); \draw[wire](#1,#2-0.4)--(#1,#2-0.6);}
```

---

# Q1. Derive an expression for the drain current of a MOS transistor (10M)

**Setup.** nMOS, source grounded, body grounded. Gate is a parallel-plate capacitor over the channel.

**Step 1 — Charge in the channel.**
Charge on a capacitor is Q = CV. The mobile charge in the channel is the part of the gate voltage above threshold:
$$Q_{channel} = C_g (V_{gc} - V_t)$$

**Step 2 — Gate-to-channel voltage.** The channel is not grounded; its average potential is V_c = (V_s+V_d)/2 = V_ds/2. So
$$V_{gc} = V_{gs} - \tfrac{V_{ds}}{2}$$

**Step 3 — Gate capacitance.** Modelling the gate as a parallel-plate cap of width W, length L, oxide thickness t_ox:
$$C_g = \varepsilon_{ox}\frac{WL}{t_{ox}} = C_{ox}WL$$

**Step 4 — Carrier velocity.** Carriers drift at v = μE, where the lateral field is E = V_ds/L. So v = μV_ds/L.

**Step 5 — Time to cross channel** = L/v.

**Step 6 — Current = charge / transit time:**
$$I_{ds}=\frac{Q_{channel}}{L/v}=\mu C_{ox}\frac{W}{L}\Big(V_{gs}-V_t-\frac{V_{ds}}{2}\Big)V_{ds}=\beta\Big(V_{GT}-\frac{V_{ds}}{2}\Big)V_{ds}$$
where β = μC_ox(W/L) and V_GT = V_gs − V_t. **This is the linear region.**

**Step 7 — Saturation.** When V_ds = V_dsat = V_GT the channel pinches off. Substitute V_ds = V_GT:
$$\boxed{I_{ds}=\frac{\beta}{2}V_{GT}^{2}}$$

**Full model (three regions):**
$$I_{ds}=\begin{cases}0 & V_{gs}<V_t & \text{(cutoff)}\\[2pt]\beta\big(V_{GT}-V_{ds}/2\big)V_{ds} & V_{ds}<V_{dsat} & \text{(linear)}\\[2pt]\dfrac{\beta}{2}V_{GT}^{2} & V_{ds}>V_{dsat} & \text{(saturation)}\end{cases}$$

**Diagram — M6 (Ids–Vds family):**
```
 Ids |                         ___________  Vgs5  (saturation: flat)
     |                    ____/
     |               ____/______________   Vgs4
     |          ____/____/
     |      ___/___/___________________     Vgs3
     |   __/__/__/
     | _/_/_/________________________        Vgs2
     |//___________________________          Vgs1
     +--------------------------------- Vds
       linear |  Vdsat   saturation
```

```latex
% Q1 -- Ids vs Vds family of curves (rise then saturate)
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[axis](0,0)--(5,0) node[right]{$V_{ds}$};
  \draw[axis](0,0)--(0,3.4) node[above]{$I_{ds}$};
  \foreach \h/\name in {0.6/$V_{gs1}$,1.1/$V_{gs2}$,1.7/$V_{gs3}$,2.3/$V_{gs4}$,3.0/$V_{gs5}$}{
    \draw[curve] (0,0) .. controls (\h*0.7,\h) and (\h*0.9,\h) .. (\h,\h) -- (4.7,\h);
    \node[font=\tiny,right] at (4.7,\h){\name};
  }
  \draw[dashed] (0,0) .. controls (1.2,1.4) and (2.0,2.6) .. (2.6,3.2);
  \node[font=\tiny] at (2.0,2.95){$V_{dsat}$ locus};
  \node[lbl] at (0.9,-0.35){linear}; \node[lbl] at (3.6,-0.35){saturation};
\end{tikzpicture}
```

**Reasoning recap:** charge (Q=CV) × how fast it moves (v=μE) ÷ how long to cross (L/v) → current. Linear when channel is continuous; pinch-off freezes the current → square-law saturation.

---

# Q2. Explain transmission gates in detail (6M)

**Definition.** A transmission gate (TG) is a CMOS switch = **one nMOS and one pMOS in parallel**, driven by **complementary** controls C and C̄. It passes both logic 0 and 1 **without degradation**.

**Why both transistors?** (from notes, pages 16–24)
- nMOS alone passes a **strong 0** but a **weak 1** (rises only to V_DD − V_tn).
- pMOS alone passes a **strong 1** but a **weak 0** (falls only to |V_tp|).
- Put in parallel: nMOS handles the 0s, pMOS handles the 1s → output always strongly driven, **levels never degraded**. This is a **fully restored** switch.

**Structure / operation.**
- nMOS gate = C, pMOS gate = C̄. Sources/drains tied together (input side `a`, output side `b`).
- C = 1, C̄ = 0 → **both ON** → b = a (closed switch).
- C = 0, C̄ = 1 → **both OFF** → b floats (Z, open switch).
- Needs both control and its complement → called **double-rail logic**.

**Truth (g = control):**

| g | g̅ | State | Output |
|---|---|---|---|
| 1 | 0 | ON | passes strong 0 and strong 1 |
| 0 | 1 | OFF | high-impedance (Z) |

**Diagram — M5:**
```
            g (to nMOS)
            |
   a ---+--[nMOS]--+--- b
        |          |
        +--[pMOS]--+
            |
           g̅ (to pMOS)
```

```latex
% Q2 -- Transmission gate: nMOS || pMOS, complementary control
\begin{tikzpicture}[x=1cm,y=1cm]
  \node[lbl,left] at (0,1){a}; \draw[wire](0,1)--(0.7,1); \node[dot]at(0.7,1){};
  \draw[wire](1,1.45) rectangle (2,2.05); \node[font=\tiny]at(1.5,1.75){nMOS};
  \draw[wire](1,-0.05) rectangle (2,0.55); \node[font=\tiny]at(1.5,0.25){pMOS};
  % parallel wiring (a -> both lefts, both rights -> b)
  \draw[wire](0.7,1)--(0.7,1.75)--(1,1.75);
  \draw[wire](0.7,1)--(0.7,0.25)--(1,0.25);
  \draw[wire](2,1.75)--(2.4,1.75)--(2.4,1);
  \draw[wire](2,0.25)--(2.4,0.25)--(2.4,1);
  \node[dot]at(2.4,1){}; \draw[wire](2.4,1)--(3.1,1) node[right]{b};
  % gates
  \draw[wire](1.5,2.05)--(1.5,2.45) node[above]{$C$};
  \draw[wire](1.5,-0.05)--(1.5,-0.45) node[below]{$\overline{C}$};
\end{tikzpicture}
```

**Reasoning recap:** nMOS weak-1 + pMOS weak-0 are each other's blind spots; wiring them in parallel covers both → undegraded pass switch.

---

# Q3. Distinguish enhancement vs depletion mode MOSFETs (5M)

**[Outside notes]** — your notes only describe enhancement-type devices and mention the symbols; they do **not** define depletion mode. The following is standard knowledge, flagged honestly.

| Feature | Enhancement mode | Depletion mode |
|---|---|---|
| Channel at V_gs = 0 | **No** channel (OFF) | **Pre-formed** channel (ON) |
| To turn nMOS ON | apply V_gs > V_t (V_t > 0) | already ON; needs V_gs < V_t to turn OFF |
| Threshold V_t (nMOS) | positive | negative |
| Normal logic use | yes — standard CMOS | rarely (mostly analog/special) |
| "Default" state | normally-OFF | normally-ON |

What your notes **do** cover (page 50): symbols for n-channel and p-channel **enhancement** MOSFETs, and that for pMOS holes conduct, body tied to V_DD.

**Reasoning recap:** the difference is whether a channel exists with zero gate voltage — enhancement must be "enhanced" into existence, depletion must be "depleted" away.

---

# Q4. Explain the DC transfer characteristics of a CMOS inverter (10M)

**What it is.** The DC transfer curve plots V_out vs V_in (input changed slowly so capacitors fully settle). Found by enforcing I_dsn = |I_dsp| at every V_in.

**Bias relations** (notes p59): nMOS source = GND → V_gsn = V_in, V_dsn = V_out. pMOS source = V_DD → V_gsp = V_in − V_DD, V_dsp = V_out − V_DD. Assume V_tp = −V_tn and size pMOS ~2–3× wider so β_n = β_p.

**Five regions (A–E):**

| Region | V_in range | pMOS | nMOS | V_out |
|---|---|---|---|---|
| A | 0 ≤ V_in < V_tn | linear | cutoff | V_DD |
| B | V_tn ≤ V_in < V_DD/2 | linear | saturated | > V_DD/2 |
| C | V_in = V_DD/2 | saturated | saturated | drops sharply |
| D | V_DD/2 < V_in ≤ V_DD−\|V_tp\| | saturated | linear | < V_DD/2 |
| E | V_in > V_DD−\|V_tp\| | cutoff | linear | 0 |

- In **C** both transistors saturate; ideal slope is −∞ → **infinite gain** at V_in = V_DD/2 (real devices have finite output resistance → finite, broader region C).
- Supply current I_DD = I_dsn = |I_dsp| is ~0 near the rails and forms a **current pulse** peaking near V_in = V_DD/2 (both transistors momentarily ON).

**Diagram — M4 (transfer curve):**
```
 Vout
 VDD |‾‾‾‾A‾‾B\
     |         \  C (steep, gain = -inf)
VDD/2|          \
     |           \D
   0 |____________\__E__ Vout=0
     +-------------------------- Vin
       Vtn   VDD/2   VDD-|Vtp|
```

```latex
% Q4 -- CMOS inverter DC transfer curve with regions A..E
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[axis](0,0)--(5,0) node[right]{$V_{in}$};
  \draw[axis](0,0)--(0,3.4) node[above]{$V_{out}$};
  \node[lbl,left]at(0,3){$V_{DD}$};
  \draw[curve] (0,3)--(1.2,2.9)
    .. controls (1.8,2.7) and (2.1,2.2) .. (2.4,1.5)
    .. controls (2.7,0.8) and (3.0,0.3) .. (3.6,0.15) -- (4.8,0.1);
  \node[lbl]at(0.6,3.15){A}; \node[lbl]at(1.9,2.78){B}; \node[lbl]at(2.6,1.5){C};
  \node[lbl]at(3.15,0.6){D}; \node[lbl]at(4.2,0.33){E};
  \foreach \x/\t in {1.2/$V_{tn}$,2.4/$V_{DD}/2$,3.6/$V_{DD}{-}|V_{tp}|$}{
    \draw[dashed](\x,0)--(\x,0.12); \node[font=\tiny,below]at(\x,0){\t};}
\end{tikzpicture}
```

**Reasoning recap:** sweep V_in; at each value the two transistors trade who's ON; the operating point is where their currents match; collecting those points draws the S-curve, steepest (highest gain) when both are saturated at mid-rail.

---

# Q5. Effect of channel length modulation on an nMOS transistor (5M)

**Ideal:** in saturation I_ds is independent of V_ds → perfect current source.

**Reality:** the drain–body p–n junction forms a depletion region of width L_d that grows with V_db. This eats into the channel:
$$L_{eff}=L-L_d$$
Shorter effective channel → **higher current**, so I_ds **rises slightly with V_ds** in saturation (the "flat" curves tilt up). Modelled by an Early-voltage factor:
$$I_{ds}=\frac{\beta}{2}V_{GT}^{2}\Big(1+\frac{V_{ds}}{V_A}\Big)$$

**Impact:** important to **analog** designers (it reduces amplifier gain). Generally **unimportant for digital** behaviour.

**Diagram — M1(d) pinch-off detail:**
```
 S====n+       channel pinches here
   |‾‾‾‾‾‾‾‾‾‾‾‾\ <- Leff
   |  electrons  \  | Ld (depletion grows with Vds)
 p-body          n+ = D
```

```latex
% Q5 -- nMOS cross-section: pinch-off, Leff and Ld near drain
\begin{tikzpicture}[x=1cm,y=0.7cm]
  \fill[gray!15](0,0) rectangle (6,1.8); \node at (3,0.5){p-body};
  \fill[green!55!black](0.3,1.4) rectangle (1.5,1.8); \node[white,font=\tiny]at(0.9,1.6){n+};
  \fill[green!55!black](4.5,1.4) rectangle (5.7,1.8); \node[white,font=\tiny]at(5.1,1.6){n+};
  \node[lbl]at(0.9,2.15){S}; \node[lbl]at(5.1,2.15){D};
  \fill[red!70](1.7,1.85) rectangle (4.3,2.2); \node[lbl]at(3,2.5){gate ($V_{gs}$)};
  \draw[blue,line width=1.4pt](1.5,1.55)--(3.7,1.4); \node[font=\tiny]at(2.5,1.15){channel (pinches)};
  \draw[<->](1.5,0.95)--(3.7,0.95); \node[font=\tiny,below]at(2.6,0.95){$L_{eff}$};
  \draw[<->](3.7,0.95)--(4.5,0.95); \node[font=\tiny,below]at(4.1,0.78){$L_d$};
  \node[font=\tiny]at(4.1,1.55){depletion};
\end{tikzpicture}
```

**Reasoning recap:** rising V_ds widens the drain depletion region → effective channel shortens → current creeps up instead of staying flat.

---

# Q6. Explain all the non-ideal I-V effects in detail (10M)

**1. Mobility degradation.** High vertical field (V_gs/t_ox) pulls carriers into the oxide interface; they scatter off surface roughness → μ falls. Model:
$$\mu_{eff}=\frac{\mu_0}{1+\theta(V_{gs}-V_t)}$$ → less current than the square law predicts at high V_gs.

**2. Velocity saturation.** Lateral field E = V_ds/L. Because L < 1 µm, even moderate V_ds gives a huge field; carrier velocity caps at v_sat instead of μE:
$$v=\begin{cases}\dfrac{\mu_{eff}E}{1+E/E_c}& E<E_c\\ v_{sat}& E\ge E_c\end{cases},\quad E_c=\frac{2v_{sat}}{\mu_{eff}}$$
Result: saturation current grows **less than quadratically** with V_gs.

**3. Channel-length modulation.** L_eff = L − L_d; I_ds rises with V_ds in saturation: I_ds = (β/2)V_GT²(1 + V_ds/V_A). (See Q5.)

**4. Threshold-voltage effects.**
 - **Body effect:** source–body voltage V_sb raises V_t: V_t = V_t0 + γ(√(φ_s+V_sb) − √φ_s).
 - **DIBL:** drain field lowers the barrier: V_t = V_t0 − ηV_ds (η ≈ 0.1 = 100 mV/V); increases I_ds in saturation and worsens subthreshold leakage.
 - **Short-channel effect (V_t roll-off):** for small L, source/drain depletion regions take over part of the channel → V_t shifts with L. Also a narrow-channel effect (V_t varies with W).

**5. Leakage** (transistor "OFF" still conducts):
 - **Subthreshold:** for V_gs < V_t, current falls exponentially (weak inversion): I_ds ∝ e^{(V_gs−V_t)/nV_T}; worsens with V_ds via DIBL.
 - **Gate leakage:** quantum tunneling through thin oxide; I_gate = WA(V_DD/t_ox)² e^{−B t_ox/V_DD} — grows exponentially as t_ox shrinks.
 - **Junction leakage:** reverse-biased source/drain-to-body diodes; plus BTBT and GIDL.

**6. Temperature dependence.** Mobility ↓ with T (μ(T)=μ(T_r)(T/T_r)^{−1.5}); v_sat ↓; I_on at high V_DD ↓; subthreshold leakage ↑ exponentially with T.

**Diagram — M7 (subthreshold log-plot):**
```
 log Ids
   |          Vds=1.0 ___/ saturation
   |   DIBL→  Vds=0.1 _/
   |        __/  __/
   |      _/   _/   <- slope S ≈ 100 mV/decade
   |  GIDL\__ /
   +----------------------- Vgs
        0      Vt
```

```latex
% Q6 -- Subthreshold log-plot (leakage / DIBL / GIDL)
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[axis](-1,0)--(4,0) node[right]{$V_{gs}$};
  \draw[axis](0,-0.2)--(0,3.4) node[above]{$\log I_{ds}$};
  \draw[dashed](1.5,-0.2)--(1.5,3.0); \node[font=\tiny,above]at(1.5,3.0){$V_t$};
  \draw[curve](-1,0.4)--(1.5,1.9)--(3.6,2.4); \node[font=\tiny,right]at(3.6,2.4){$V_{ds}{=}1.0$};
  \draw[curve](-1,0.1)--(1.5,1.5)--(3.6,1.7); \node[font=\tiny,right]at(3.6,1.7){$V_{ds}{=}0.1$};
  \draw[<->](0.12,0.55)--(0.12,0.95); \node[font=\tiny,right]at(0.18,0.75){DIBL};
  \node[font=\tiny]at(-0.7,0.65){GIDL};
  \node[font=\tiny]at(0.7,2.5){subthreshold}; \node[font=\tiny]at(2.7,2.7){saturation};
\end{tikzpicture}
```

**Reasoning recap:** every effect is the long-channel model breaking down when fields get large or dimensions get tiny — vertical field kills μ, lateral field caps v, depletion regions shorten/weaken the channel, and "OFF" stops meaning zero current.

---

# Q7. With neat sketch, working of nMOS transistor with regions of operation (10M)

p-type body, two n+ regions (source, drain), poly gate over thin oxide, body grounded.

**(a) Cutoff — V_gs < V_t.** No inversion layer; source and drain isolated by reverse-biased junctions. **I_ds = 0** (open switch).

**(b) Linear/Triode — V_gs > V_t and 0 < V_ds < V_gs−V_t** (so V_gd > V_t).
Channel is inverted **everywhere**. A lateral field drives electrons source→drain. Channel is **tapered** (thick at source, thin at drain). Current rises with V_ds:
$$I_{ds}=\beta\big(V_{GT}-V_{ds}/2\big)V_{ds}$$
(At V_ds = 0 the channel is uniform and no current flows — "filled pipe, no pressure.")

**(c) Saturation — V_gs > V_t and V_ds ≥ V_gs−V_t** (so V_gd < V_t).
Channel **pinches off** before the drain. Current becomes nearly independent of V_ds:
$$I_{ds}=\frac{\beta}{2}V_{GT}^{2}$$

**Diagram — M1 (the 4 states):**
```
(a) Cutoff           (b) Vds=0 uniform     (c) Linear tapered     (d) Saturation pinched
 S=n+    n+=D         S=n+~~~~~~~n+=D        S=n+\____ n+=D          S=n+\___   n+=D
   p-body (holes)        full channel          thick->thin            channel ends | gap
   no channel            no current            Ids increases          Ids flat (sat)
```

```latex
% Q7 -- nMOS in 4 states: cutoff / uniform (Vds=0) / linear / saturation
\begin{tikzpicture}[scale=0.6]
  \foreach \xs/\cap in {0/(a) cutoff, 3.6/(b) Vds=0, 7.2/(c) linear, 10.8/(d) saturation}{
    \fill[gray!15](\xs,0) rectangle (\xs+3,1.6);
    \fill[green!55!black](\xs+0.2,1.2) rectangle (\xs+1.0,1.6);
    \fill[green!55!black](\xs+2.0,1.2) rectangle (\xs+2.8,1.6);
    \fill[red!70](\xs+1.1,1.65) rectangle (\xs+1.9,2.0);
    \node[font=\tiny] at (\xs+1.5,-0.55){\cap};
  }
  % channel overlays
  \draw[blue,line width=1pt](4.6,1.18)--(5.6,1.18);     % (b) uniform
  \draw[blue,line width=1pt](8.2,1.18)--(9.2,1.05);     % (c) tapered
  \draw[blue,line width=1pt](11.8,1.18)--(12.7,1.05);   % (d) pinched (stops before drain)
\end{tikzpicture}
```

**Reasoning recap:** raise V_gs to make a channel (cutoff→on); raise V_ds to push current (linear); raise V_ds too far and the drain end starves of inversion (pinch-off→saturation).

---

# Q8. Short notes (6M)

## (a) Noise Margin
How much noise a signal tolerates before being misread. Defined from four levels:
- V_OH = min HIGH **output**, V_OL = max LOW **output**
- V_IH = min HIGH **input**, V_IL = max LOW **input**

$$NM_H = V_{OH}-V_{IH}\qquad NM_L=V_{IL}-V_{OL}$$

V_IL and V_IH are the **unity-gain points** (slope = −1) on the transfer curve. Bigger margins → better immunity. Between V_IL and V_IH is the **indeterminate region** (both transistors conduct, unstable).

```
 VDD ──────── VOH ──┐
                    │ NM_H
            VIH ────┘
   (indeterminate)
            VIL ────┐
                    │ NM_L
 GND ──────── VOL ──┘
```

```latex
% Q8(a) -- Noise-margin bands
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[wire](0,0) rectangle (1.2,5);
  \fill[blue!20](0,4.2) rectangle (1.2,5);  \node[font=\tiny]at(0.6,4.6){HIGH out};
  \fill[blue!20](0,0) rectangle (1.2,0.8);  \node[font=\tiny]at(0.6,0.4){LOW out};
  \foreach \y/\t in {5/$V_{DD}$,4.2/$V_{OH}$,3.2/$V_{IH}$,1.8/$V_{IL}$,0.8/$V_{OL}$,0/GND}{
    \draw[dashed](1.2,\y)--(1.9,\y); \node[font=\tiny,right]at(1.95,\y){\t};}
  \draw[<->](3.0,4.2)--(3.0,3.2); \node[font=\tiny,right]at(3.05,3.7){$NM_H$};
  \draw[<->](3.0,1.8)--(3.0,0.8); \node[font=\tiny,right]at(3.05,1.3){$NM_L$};
  \node[font=\tiny]at(0.6,2.5){indet.};
\end{tikzpicture}
```

## (b) Body effect on threshold voltage
The body is an implicit 4th terminal. A source-to-body voltage V_sb increases the charge needed to invert the channel, so **V_t rises**:
$$V_t=V_{t0}+\gamma\Big(\sqrt{\phi_s+V_{sb}}-\sqrt{\phi_s}\Big)$$
- V_t0 = threshold at V_sb = 0, φ_s = surface potential, γ = body-effect coefficient (0.4–1 V^½), γ = √(2qε_si N_A)/C_ox.
- Linearized for small V_sb: V_t = V_t0 + k_γ V_sb.

**Reasoning recap:** noise margin = gap between what a gate guarantees to output and what the next gate accepts; body effect = a back-bias makes the channel harder to invert, pushing V_t up.

---

# Q9. Design a 2:1 MUX using transmission gates and explain (6M)

**Function:** Y = S̄·D0 + S·D1. Use **two TGs** sharing output Y; S and S̄ enable exactly one.

```
        S           S̄
        |           |
 D0 --[ TG0 ]--+--[ TG1 ]-- D1
        |      |      |
        S̄      Y      S
                |
               Y out
```
- **S = 0** (S̄ = 1): TG0 ON → Y = D0; TG1 OFF.
- **S = 1** (S̄ = 0): TG1 ON → Y = D1; TG0 OFF.

| S | S̄ | ON gate | Y |
|---|---|---|---|
| 0 | 1 | TG0 | D0 |
| 1 | 0 | TG1 | D1 |

Because TGs are used, both outputs pass undegraded but the mux is **non-restoring** (passes input noise through).

```latex
% Q9 -- 2:1 mux from two transmission gates
\begin{tikzpicture}[x=1cm,y=1cm]
  \node[lbl,left]at(0,2){D0}; \draw[wire](0,2)--(1,2);
  \node[lbl,left]at(0,0){D1}; \draw[wire](0,0)--(1,0);
  \draw[wire](1,1.7) rectangle (2,2.3); \node[font=\tiny]at(1.5,2){TG0};
  \draw[wire](1,-0.3) rectangle (2,0.3); \node[font=\tiny]at(1.5,0){TG1};
  \draw[wire](2,2)--(2.8,2)--(2.8,1); \draw[wire](2,0)--(2.8,0)--(2.8,1);
  \node[dot]at(2.8,1){}; \draw[wire](2.8,1)--(3.6,1) node[right]{Y};
  \draw[wire](1.5,2.3)--(1.5,2.7) node[above]{$S$};
  \draw[wire](1.5,1.7)--(1.5,1.4) node[below,font=\tiny]{$\overline{S}$};
  \draw[wire](1.5,0.3)--(1.5,0.6) node[above,font=\tiny]{$\overline{S}$};
  \draw[wire](1.5,-0.3)--(1.5,-0.7) node[below]{$S$};
\end{tikzpicture}
```

**Reasoning recap:** S and S̄ are mutually exclusive, so wiring one TG per data input with opposite control lets exactly one input reach Y at a time = selection.

---

# Q10. Formation of accumulation, depletion, inversion in nMOS (7M)

MOS capacitor: gate / oxide / p-type body.

**(a) Accumulation — V_g < 0.** Negative gate attracts the body's holes to the surface → holes **accumulate** at the oxide interface. Surface stays p-type. No channel.

**(b) Depletion — 0 < V_g < V_t.** Positive gate **repels holes**, leaving fixed negative acceptor ions → a carrier-free **depletion region**. Still no electron channel; device OFF.

**(c) Inversion — V_g > V_t.** Stronger field now **attracts electrons** to the surface, forming a thin n-type **inversion layer (channel)** over the depletion region. Channel connects source↔drain → device ON.

**Diagram — M2:**
```
(a) Vg<0          (b) 0<Vg<Vt        (c) Vg>Vt
[ gate - ]        [ gate + ]         [ gate + ]
[ oxide  ]        [ oxide  ]         [ oxide  ]
 +++++++++  holes  ---------- deplete  ‾‾‾‾‾‾  e- inversion
 p-body +++         depletion          ---- depletion
                    p-body +++          p-body +++
```

```latex
% Q10 -- MOS capacitor: accumulation / depletion / inversion
\begin{tikzpicture}[scale=0.7]
  \foreach \xs/\cap in {0/(a) Vg<0, 4/(b) 0<Vg<Vt, 8/(c) Vg>Vt}{
    \fill[gray!40](\xs,2.0) rectangle (\xs+3,2.4); \node[font=\tiny]at(\xs+1.5,2.2){gate};
    \fill[cyan!20](\xs,1.85) rectangle (\xs+3,2.0);
    \fill[gray!12](\xs,0) rectangle (\xs+3,1.85);
    \node[font=\tiny]at(\xs+1.5,-0.4){\cap};
  }
  \node[font=\tiny]at(1.5,1.55){$+\,+\,+\,+$ holes};   \node[font=\tiny]at(1.5,0.8){p-body $+++$};
  \node[font=\tiny]at(5.5,1.55){$-\,-\,-$ depletion};  \node[font=\tiny]at(5.5,0.8){p-body $+++$};
  \node[font=\tiny]at(9.5,1.6){$-\,-\,-$ e$^-$ (inv)}; \node[font=\tiny]at(9.5,1.2){depletion};
  \node[font=\tiny]at(9.5,0.7){p-body $+++$};
\end{tikzpicture}
```

**Reasoning recap:** gate voltage sets what gathers at the surface — negative→holes (accumulate), small positive→bare ions (deplete), large positive→electrons (invert into a channel).

---

# Q11. Design a CMOS NAND gate and analyse all input combinations (7M)

**Structure:** two nMOS in **series** (pull-down) between Y and GND; two pMOS in **parallel** (pull-up) between V_DD and Y.

```
        VDD
     ┌───┴───┐
   [pA]    [pB]      (pMOS parallel)
     └───┬───┘
         Y ───── output
       [nA]            (nMOS series)
         │
       [nB]
         │
        GND
```
Function: **Y = (A·B)′**

| A | B | Pull-down (series nMOS) | Pull-up (parallel pMOS) | Y |
|---|---|---|---|---|
| 0 | 0 | OFF | ON | 1 |
| 0 | 1 | OFF | ON | 1 |
| 1 | 0 | OFF | ON | 1 |
| 1 | 1 | **ON** | OFF | **0** |

Only when A = B = 1 are **both** series nMOS ON (path to GND) and both pMOS OFF → Y = 0. Any 0 input breaks the series path but turns on a pMOS → Y = 1.

```latex
% Q11 -- CMOS NAND: pMOS parallel (pull-up), nMOS series (pull-down)
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[wire](0.5,5)--(3.5,5) node[right]{$V_{DD}$};
  \mosbox{1.3}{4.2}{pMOS A} \mosbox{2.7}{4.2}{pMOS B}
  \draw[wire](1.3,4.6)--(1.3,5); \draw[wire](2.7,4.6)--(2.7,5);     % sources to VDD
  \draw[wire](1.3,3.8)--(1.3,3.4)--(2.7,3.4)--(2.7,3.8);            % drains join at Y
  \node[dot]at(2,3.4){}; \draw[wire](2,3.4)--(3.4,3.4) node[right]{$Y$};
  \mosbox{2}{2.6}{nMOS A} \mosbox{2}{1.4}{nMOS B}
  \draw[wire](2,3.4)--(2,3.0);   % Y -> nA drain
  \draw[wire](2,2.2)--(2,1.8);   % nA source -> nB drain
  \draw[wire](2,1.0)--(2,0.6); \draw[wire](1.6,0.6)--(2.4,0.6); \node[font=\tiny,below]at(2,0.6){GND};
\end{tikzpicture}
```

**Reasoning recap:** series nMOS needs *all* inputs high to pull low; parallel pMOS needs *any* input low to pull high → that's exactly NAND.

---

# Q12. Tristate buffer from a tristate inverter + normal inverter (6M)

**Idea (notes p28):** "A tristate buffer can be built as an ordinary inverter followed by a tristate inverter."

```
 A --[ inverter ]--Ā--[ tristate inverter ]-- Y
                          ↑ EN, EN̄
```
- Stage 1 (normal inverter): A → Ā.
- Stage 2 (tristate inverter): inverts again → Y = A, **but only when enabled**.
- **EN = 1:** both enable transistors ON → stage 2 acts as a plain inverter → Y = (Ā)′ = A. Non-inverting buffer.
- **EN = 0:** both enable transistors OFF → output **floats (Z)**.

| EN | A | Y |
|---|---|---|
| 0 | X | Z |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Tristate inverter internals (4-transistor stack): pMOS(A) – pMOS(EN̄) – [Y] – nMOS(EN) – nMOS(A). EN/EN̄ transistors gate the output; the A transistors do the inversion. It is **restoring** (output actively driven from V_DD/GND).

```latex
% Q12 -- tristate buffer = inverter followed by tristate inverter
\begin{tikzpicture}[x=1cm,y=1cm]
  \node[lbl,left]at(0,0){A}; \draw[wire](0,0)--(0.6,0);
  \draw[wire](0.6,-0.4) rectangle (1.8,0.4); \node[font=\tiny]at(1.2,0){inverter};
  \draw[wire](1.8,0)--(2.4,0); \node[font=\tiny,above]at(2.1,0){$\overline{A}$};
  \draw[wire](2.4,-0.4) rectangle (4.0,0.4); \node[font=\tiny,align=center]at(3.2,0){tristate\\inverter};
  \draw[wire](4.0,0)--(4.7,0) node[right]{Y};
  \draw[wire](3.2,0.4)--(3.2,0.9) node[above,font=\tiny]{EN,\,$\overline{EN}$};
\end{tikzpicture}
```

**Reasoning recap:** two inversions cancel → buffer; the middle enable transistors let you disconnect the output entirely → the third (Z) state.

---

# Q13. Find k_n in saturation. I_DS = 4 mA, V_GS = 3 V, V_t = 1 V, W/L = 1 (5M)

Saturation: $I_{DS}=\dfrac{k_n}{2}\dfrac{W}{L}(V_{GS}-V_t)^2$

$$4\text{ mA}=\frac{k_n}{2}(1)(3-1)^2=\frac{k_n}{2}(4)=2k_n$$
$$\boxed{k_n = 2\ \text{mA/V}^2}$$

**Steps:** confirm saturation form → plug numbers → (3−1)² = 4 → 4mA = 2k_n → k_n = 2 mA/V².

---

# Q14. Region of operation and I_DS. V_GS = 3 V, V_t = 1 V, W/L = 1 (5M)

⚠️ **This question is missing data as written: no V_DS and no k_n are given.** I cannot fully solve it as stated. I'll be honest and give the method + the most likely intended answer.

- **Region:** V_GS − V_t = 2 V > 0, so the device is ON. It is in **saturation if V_DS ≥ 2 V**, otherwise linear. (Without V_DS this cannot be decided for certain.)
- **Drain current:** needs k_n. If this question **continues from Q13** (k_n = 2 mA/V²) and V_DS ≥ 2 V (saturation), then
$$I_{DS}=\frac{k_n}{2}(V_{GS}-V_t)^2=\frac{2}{2}(2)^2=4\ \text{mA}$$

If a V_DS value is printed on your paper, tell me and I'll redo it exactly.

**Steps:** check V_GS−V_t>0 (ON) → compare V_DS to V_GS−V_t for region → apply matching formula with k_n.

---

# Q15. Region and I_DS. V_GS = 1.5 V, V_t = 2 V, V_DS = 0.5 V, W/L = 1 (5M)

V_GS = 1.5 V < V_t = 2 V → **cutoff** (no channel).
$$\boxed{I_{DS}=0}$$

**Steps:** compare V_GS to V_t → 1.5 < 2 → below threshold → cutoff → I_DS = 0 (V_DS irrelevant).

---

# Q16 & Q17. Output voltage of the given pass-transistor circuits (6M each)

⚠️ **The actual circuit diagrams ("given below") were not included in what you gave me, so I cannot read off the specific node voltages.** I won't guess them. Instead, here are the **exact rules from your notes (p16–21)** — apply them to whatever your paper shows:

**nMOS pass transistor (gate = V_DD, passing logic):**
- Passes **strong 0** → output = 0 V.
- Passes **weak/degraded 1** → output = **V_DD − V_tn** (threshold drop).

**pMOS pass transistor (gate = GND, passing logic):**
- Passes **strong 1** → output = V_DD.
- Passes **weak/degraded 0** → output = **|V_tp|**.

**Series of nMOS passing a 1:** no *extra* degradation — output stays at **V_DD − V_tn** (one threshold drop total), *provided each gate is at V_DD*.

**If a degraded output drives the next transistor's gate:** that stage drops **another** V_tn → e.g. **V_DD − 2V_tn** (cascaded threshold drops).

**Transmission gate (nMOS∥pMOS):** passes both **strong 0 and strong 1** — no degradation.

**How to answer your specific Q16/Q17:** identify each pass device (n or p), what logic value the input is, and whether its gate is fed a full rail or a degraded signal; then apply the matching bullet above.

If you paste the two circuits (or describe them: which transistor type, gate voltage, input value, series/cascade), I'll compute the exact output voltages.

**Reasoning recap:** an nMOS can't pull its source above V_DD−V_tn and a pMOS can't pull above-GND below |V_tp|; track those drops node by node, adding a fresh drop only when a degraded signal drives a gate.

---

# Q18. Implement a tristate inverter and explain its working in detail (7M)

**What it is.** An inverter whose output has **three states**: logic 1, logic 0, and **high-impedance (Z)**. It inverts when enabled and electrically disconnects when disabled. It is **restoring** (output actively driven from V_DD/GND).

**Structure — 4-transistor series stack** (V_DD at top, GND at bottom):
```
        VDD
         │
       [pMOS]  gate = A          ← inverting pMOS
         │
       [pMOS]  gate = EN̄         ← enable pMOS
         │
         Y ───── output
         │
       [nMOS]  gate = EN          ← enable nMOS
         │
       [nMOS]  gate = A          ← inverting nMOS
         │
        GND
```
The two **A**-gated transistors form the actual inverter; the **EN / EN̄** transistors gate the output.

**Operation:**

| EN | EN̄ | Enable transistors | Output Y |
|---|---|---|---|
| 0 | 1 | both OFF | **Z** (floating) |
| 1 | 0 | both ON (act as wires) | **Ā** (normal inverter) |

- **EN = 0:** both enable transistors OFF → the path from V_DD and from GND to Y is broken → **Y floats (Z)**, regardless of A.
- **EN = 1:** both enable transistors ON → conceptually removed (just wires) → remaining top-pMOS(A) + bottom-nMOS(A) = ordinary inverter → **Y = Ā**.

**Note (from notes p28):** it does **not** obey the conduction-complement rule, because it deliberately lets the output float for some input combinations. The complementary enable EN̄ may be generated inside the cell or routed in.

```latex
% Q18 -- tristate inverter: 4-transistor series stack
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[wire](0.5,5.4)--(2.7,5.4) node[right]{$V_{DD}$};
  \mosbox{1.5}{4.6}{pMOS A}                 \draw[wire](1.5,5.0)--(1.5,5.4);
  \mosbox{1.5}{3.4}{pMOS $\overline{EN}$}   \draw[wire](1.5,4.2)--(1.5,3.8);
  \draw[wire](1.5,3.0)--(1.5,2.6); \node[dot]at(1.5,2.6){}; \draw[wire](1.5,2.6)--(2.9,2.6) node[right]{Y};
  \mosbox{1.5}{1.8}{nMOS EN}                \draw[wire](1.5,2.6)--(1.5,2.2);
  \mosbox{1.5}{0.6}{nMOS A}                 \draw[wire](1.5,1.4)--(1.5,1.0);
  \draw[wire](1.5,0.2)--(1.5,-0.1); \draw[wire](1.1,-0.1)--(1.9,-0.1); \node[font=\tiny,below]at(1.5,-0.1){GND};
\end{tikzpicture}
```

**Reasoning recap:** stack two enable transistors in series with a normal inverter; turning them OFF cuts the output off from both rails (Z); turning them ON leaves a plain inverter (Ā).

---

# Q19. Explain (a) mobility degradation (b) velocity saturation (8M)

**(a) Mobility degradation.** Caused by the **high vertical field** (V_gs/t_ox). A high gate voltage pulls carriers hard against the oxide interface, where they collide with surface roughness and scatter more → effective mobility μ falls. Because μ drops, the drive current is **less than the square law predicts at high V_gs**.

Model:
$$\mu_{eff}=\frac{\mu_0}{1+\theta(V_{gs}-V_t)}\qquad(\text{as }V_{gs}\uparrow,\ \mu_{eff}\downarrow)$$
From notes (more detailed form):
$$\mu_{eff\text{-}n}=\frac{540}{1+\left(\frac{V_{gs}+V_t}{0.54\,t_{ox}}\right)^{1.85}}\ \tfrac{\text{cm}^2}{\text{V·s}}$$

**(b) Velocity saturation.** Carriers normally drift at v = μE, where E = V_ds/L is the **lateral field**. Since L < 1 µm, even a moderate V_ds gives a very large E. At high fields the velocity stops rising linearly and **caps at a maximum v_sat**:
$$v=\begin{cases}\dfrac{\mu_{eff}E}{1+E/E_c}& E<E_c\\[4pt] v_{sat}& E\ge E_c\end{cases}\qquad E_c=\frac{2v_{sat}}{\mu_{eff}}$$
The critical voltage is V_c = E_c·L. Result: saturation current grows **less than quadratically** with V_gs (lower I_ds than the ideal model at high V_ds).

**Difference in one line:** mobility degradation is a **vertical-field** effect (gate pushes carriers into the surface); velocity saturation is a **lateral-field** effect (source–drain field caps carrier speed). Both reduce current below the ideal square law.

**Reasoning recap:** big gate field → carriers scrape the oxide → slower (μ↓); big drain field → carriers hit their top speed v_sat → current flattens early.

---

# Q20. How inverter DC characteristics vary with β_p/β_n (5M)

Let **r = β_p/β_n**. With β_n = β_p (r = 1) the switching threshold V_inv = V_DD/2 (maximizes noise margins; equal charge/discharge currents).

Changing r **shifts the switching threshold horizontally**, but the transition stays **sharp**:

| Ratio | Name | Stronger device | Switching threshold V_inv |
|---|---|---|---|
| r > 1 | **HI-skew** | stronger pMOS | **higher** than V_DD/2 (shifts right) |
| r = 1 | normal/unskewed | balanced | = V_DD/2 |
| r < 1 | **LO-skew** | weaker pMOS | **lower** than V_DD/2 (shifts left) |

- **HI-skew (stronger pMOS):** at V_in = V_DD/2 the pMOS wins → output stays high longer → input threshold must be **raised** to switch.
- **LO-skew (weaker pMOS):** nMOS wins earlier → **lower** switching threshold.

From notes: V_inv is set by I_dsn = |I_dsp|, i.e. by where (β_n/2)(V_in−V_tn)² = (β_p/2)(V_in−V_DD−V_tp)². As the ratio changes, this balance point (and hence V_inv) moves; the curve only **slides sideways**, it does not lose steepness.

**Diagram (M4, shifted curves):**
```
 Vout
 VDD|‾‾‾\   \   \
    |    \   \   \      ratios shift the curve sideways
    |     \   \   \
  0 |______\___\___\___ Vin
       LO   r=1  HI    (β_p/β_n: 0.1 ... 1 ... 10)
```

```latex
% Q20 -- skewed inverter transfer curves (shift sideways, stay sharp)
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[axis](0,0)--(5,0) node[right]{$V_{in}$};
  \draw[axis](0,0)--(0,3.4) node[above]{$V_{out}$};
  \node[lbl,left]at(0,3){$V_{DD}$};
  \foreach \dx in {-0.8,0,0.8}{
    \draw[curve] (0,3)--({1.4+\dx},2.8)
      .. controls ({1.9+\dx},2.4) and ({2.1+\dx},1.0) .. ({2.3+\dx},0.6)
      -- (4.8,0.25);
  }
  \node[font=\tiny]at(1.0,3.15){LO}; \node[font=\tiny]at(2.0,3.15){r=1}; \node[font=\tiny]at(3.0,3.15){HI};
  \node[font=\tiny]at(2.0,-0.35){$\beta_p/\beta_n$: 0.1 \dots 1 \dots 10};
\end{tikzpicture}
```

**Reasoning recap:** the switching point is where the two transistor currents balance; making pMOS relatively stronger (r↑) pushes that balance to a higher V_in, sliding the whole S-curve right without flattening it.

---

# Q21. Transistor-level schematic for Y = (AB + C(A+B))′ (5M)

**Step 1 — pull-down network (nMOS).** Build the expression inside the bar directly:
`f = A·B + C·(A+B)`
- `A·B` → A **in series** with B.
- `A+B` → A **in parallel** with B; then `C·(A+B)` → C **in series** with that parallel pair.
- The two product terms are OR'd → the two branches go **in parallel**.

**Step 2 — pull-up network (pMOS) = dual** (swap every series↔parallel):
`Y_pullup = (A+B)·(C + A·B)`
- `(A+B)` → A **parallel** B.
- `A·B` → A **series** B; then `C + A·B` → C **parallel** with that series pair.
- The two factors are AND'd → the two groups go **in series**.

**Schematic (ASCII):**
```
                      VDD
                       │
            ┌──────────┴──────────┐
          [pA]                  [pB]        (A ∥ B)
            └──────────┬──────────┘
                       │   ← these two groups in SERIES
            ┌──────────┴──────────┐
          [pC]            ┌──[pA]──┐
            │             │        │        (C ∥ (A series B))
            │           [pB]       │
            └──────┬──────┘        │
                   └───────────────┘
                       │
                       Y ───── output
                       │
        ┌──────────────┴──────────────┐
        │ (A series B)                 │ (C series (A∥B))
      [nA]                           [nC]
        │                              │
      [nB]                    ┌────────┴────────┐
        │                   [nA]              [nB]      (A ∥ B)
        │                     └────────┬────────┘
        └──────────────┬───────────────┘
                       │
                      GND
```

```latex
% Q21 -- compound (AOI) gate  Y = (AB + C(A+B))'
% Pull-up (pMOS): (A||B) series (C || (A-B))   Pull-down (nMOS): (A-B) || (C-(A||B))
\begin{tikzpicture}[x=1cm,y=0.95cm]
  \draw[wire](0.5,8.4)--(4.5,8.4) node[right]{$V_{DD}$};
  % ---- PULL-UP ----
  % G1 = A || B
  \mosbox{1.5}{7.4}{pA} \mosbox{3.3}{7.4}{pB}
  \draw[wire](1.5,7.8)--(1.5,8.4); \draw[wire](3.3,7.8)--(3.3,8.4);          % sources -> VDD
  \draw[wire](1.5,7.0)--(1.5,6.6)--(3.3,6.6)--(3.3,7.0); \node[dot]at(2.4,6.6){}; % G1 drains join
  % G2 = C || (A-B), in series below G1
  \mosbox{1.5}{5.6}{pC} \mosbox{3.3}{6.0}{pA} \mosbox{3.3}{4.8}{pB}
  \draw[wire](1.5,6.0)--(1.5,6.6); \draw[wire](3.3,6.4)--(3.3,6.6);          % G2 tops -> 6.6 rail
  \draw[wire](3.3,5.6)--(3.3,5.2);                                          % A-B internal series
  \draw[wire](1.5,5.2)--(1.5,4.4)--(3.3,4.4)--(3.3,4.4); \node[dot]at(2.4,4.4){}; % G2 bottoms join = Y
  \draw[wire](2.4,4.4)--(4.6,4.4) node[right]{$Y$};
  % ---- PULL-DOWN ----
  % Branch1 = A - B (series)
  \mosbox{1.5}{3.4}{nA} \mosbox{1.5}{2.2}{nB}
  \draw[wire](1.5,4.0)--(1.5,4.4); \draw[wire](1.5,3.0)--(1.5,2.6); \draw[wire](1.5,1.8)--(1.5,0.6);
  % Branch2 = C - (A||B)
  \mosbox{3.3}{3.4}{nC} \mosbox{2.8}{1.9}{nA} \mosbox{3.8}{1.9}{nB}
  \draw[wire](3.3,4.0)--(3.3,4.4);                       % C top -> Y
  \draw[wire](3.3,3.0)--(3.3,2.5)--(2.8,2.5)--(2.8,2.3); % C bottom -> pair top
  \draw[wire](3.3,2.5)--(3.8,2.5)--(3.8,2.3);
  \draw[wire](2.8,1.5)--(2.8,0.6); \draw[wire](3.8,1.5)--(3.8,0.6);         % pair bottoms -> GND
  % GND rail
  \draw[wire](0.8,0.6)--(4.2,0.6); \node[font=\tiny,below]at(2.5,0.6){GND};
\end{tikzpicture}
```

- **Pull-down** = `(A·B)` ∥ `(C·(A+B))` → pulls Y to 0 exactly when AB+C(A+B) = 1.
- **Pull-up** = `(A+B)` series `(C + A·B)` → the conduction complement → pulls Y to 1 otherwise.

**Transistor count:** the function uses A, B (twice each) and C once → 5 nMOS + 5 pMOS = **10 transistors**.

**Reasoning recap:** AND→series, OR→parallel for the nMOS pull-down written straight from the equation; the pMOS pull-up is the exact dual (series↔parallel swapped). Y is the complement, as the outer bar requires.

---

# Q23. Design a CMOS NOR gate and analyse all input combinations (6M)

**Structure:** two nMOS in **parallel** (pull-down) between Y and GND; two pMOS in **series** (pull-up) between V_DD and Y.

```
        VDD
         │
       [pA]          (pMOS series)
         │
       [pB]
         │
         Y ───── output
     ┌───┴───┐
   [nA]    [nB]      (nMOS parallel)
     └───┬───┘
        GND
```
Function: **Y = (A + B)′**

| A | B | Pull-down (parallel nMOS) | Pull-up (series pMOS) | Y |
|---|---|---|---|---|
| 0 | 0 | OFF | **ON** | **1** |
| 0 | 1 | ON | OFF | 0 |
| 1 | 0 | ON | OFF | 0 |
| 1 | 1 | ON | OFF | 0 |

- **A = B = 0:** both parallel nMOS OFF; both series pMOS ON → path to V_DD → **Y = 1**.
- **Any input = 1:** that nMOS turns ON (pulls Y to 0) and breaks the series pMOS path → **Y = 0**.

```latex
% Q23 -- CMOS NOR: pMOS series (pull-up), nMOS parallel (pull-down)
\begin{tikzpicture}[x=1cm,y=1cm]
  \draw[wire](0.5,5.4)--(3.5,5.4) node[right]{$V_{DD}$};
  \mosbox{2}{4.6}{pMOS A} \draw[wire](2,5.0)--(2,5.4);
  \mosbox{2}{3.4}{pMOS B} \draw[wire](2,4.2)--(2,3.8);
  \draw[wire](2,3.0)--(2,2.6); \node[dot]at(2,2.6){}; \draw[wire](2,2.6)--(3.4,2.6) node[right]{$Y$};
  \mosbox{1.2}{1.7}{nMOS A} \mosbox{2.8}{1.7}{nMOS B}
  \draw[wire](1.2,2.1)--(1.2,2.6)--(2.8,2.6)--(2.8,2.1);   % drains -> Y
  \draw[wire](1.2,1.3)--(1.2,0.8)--(2.8,0.8)--(2.8,1.3);   % sources -> GND
  \node[font=\tiny,below]at(2,0.8){GND};
\end{tikzpicture}
```

**Reasoning recap:** parallel nMOS pulls low if *any* input is high; series pMOS pulls high only when *all* inputs are low → that is exactly NOR.

---

## Honesty checklist
- **Q3** depletion-mode content is **[outside notes]** (your notes cover only enhancement).
- **Q14** is missing V_DS / k_n — answered conditionally.
- **Q16, Q17** circuits were not provided — gave the governing rules only.
- **TikZ blocks are NOT compiled here** (no LaTeX on this machine). They encode the correct *topology*, but exact coordinates may need small nudges on Overleaf. Paste the **LaTeX setup block once** into your preamble first — every diagram needs its `\mosbox` macro and the colour/line styles.
