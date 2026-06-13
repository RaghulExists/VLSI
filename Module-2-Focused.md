# Module 2 — Focused Exam Answers

> Primary source = your chat notes. Anything from my own knowledge is marked **[outside notes]**.
> No LaTeX on this machine → the TikZ below is written but **not compiled/verified here**; build on Overleaf.
> Every ASCII diagram is **kept**, with a ready-to-compile **TikZ code block beneath it**. All blocks assume the one-time **LaTeX setup** section is pasted once into your preamble.

---

## LaTeX setup — paste this **ONCE** (preamble)

Every diagram block below uses these styles + three tiny glyph macros (`\nT` = nMOS, `\pT` = pMOS, `\dload` = depletion load).

```latex
\usepackage{tikz}
\usetikzlibrary{arrows.meta,calc,positioning}
\tikzset{
  ndiff/.style={green!60!black,line width=3pt},   % n-diffusion (green)
  pdiff/.style={yellow!85!orange,line width=3pt}, % p-diffusion (yellow)
  poly/.style ={red,line width=2pt},              % polysilicon (red)
  metal/.style={blue,line width=2pt},             % metal 1 (blue)
  cont/.style ={circle,fill=black,inner sep=1.5pt},
  demarc/.style={brown,dashed,line width=1pt},
  implant/.style={yellow!70!black,dashed,line width=1pt},
  lbl/.style  ={font=\footnotesize},
  Xtie/.style ={font=\bfseries},
}
% nMOS transistor glyph: (x,y,label) -- vertical green conduction path, red gate
\newcommand{\nT}[3]{\draw[ndiff](#1,{#2-0.45})--(#1,{#2+0.45});%
  \draw[poly]({#1-0.4},#2)--({#1+0.4},#2);\node[lbl] at ({#1+0.6},#2){#3};}
% pMOS transistor glyph: (x,y,label)
\newcommand{\pT}[3]{\draw[pdiff](#1,{#2-0.45})--(#1,{#2+0.45});%
  \draw[poly]({#1-0.4},#2)--({#1+0.4},#2);\node[lbl] at ({#1+0.6},#2){#3};}
% nMOS depletion load: (x,y) -- glyph + implant box + gate-tied-to-source
\newcommand{\dload}[2]{\draw[ndiff](#1,{#2-0.45})--(#1,{#2+0.45});%
  \draw[poly]({#1-0.4},#2)--({#1+0.4},#2);%
  \draw[implant]({#1-0.55},{#2-0.6}) rectangle ({#1+0.7},{#2+0.6});%
  \draw[metal]({#1+0.4},#2)--({#1+0.55},#2)--({#1+0.55},{#2-0.45})--(#1,{#2-0.45});}
```

---

## Read this first — the whole module is 4 drawings + 1 rule

Every Boolean-expression question (Q7–13, 16, 19, 20) and the gate questions (Q14, 15, 17) are the **same frame**. Only the small pull-down network changes.

**The frame (memorize once):**

- **CMOS stick (S1):** `Vdd` metal rail on top (inside n-well, with a ✕ well-tie) → **p-diffusion pull-up** (yellow) → **demarcation line** → **n-diffusion pull-down** (green) → `Vss` metal rail at bottom (with ✕ well-tie). Red polysilicon gates run **vertically** through both halves (each input drives one pMOS + one nMOS). Output = blue metal joining the pMOS drain to the nMOS drain.
- **nMOS stick (S2):** `Vdd` on top → **depletion load** (one transistor, gate tied to its own source, with a yellow implant box) → **output node** → **enhancement pull-down network** → `GND`. Inputs = red poly gates crossing green n-diffusion.

**The rule (this is the only thing you compute per question):**

| In the expression | nMOS pull-down (PDN) | CMOS pull-up (PUN) |
|---|---|---|
| **AND** ( · ) | transistors in **series** (stack) | transistors in **parallel** |
| **OR** ( + ) | transistors in **parallel** | transistors in **series** |

The CMOS pull-up is the **dual** of the pull-down (swap series↔parallel). The nMOS pull-up is **not** a network — just one depletion load.

**Layer colour code (your notes, p3–6):** n-diffusion = **green**, p-diffusion = **yellow**, polysilicon = **red**, Metal-1 = **blue**, Metal-2 = **purple**, contact = **black dot**, implant = **yellow box**, p-well edge / demarcation = **brown dashed**, Vdd/Vss contact = **black ✕**. A transistor is formed **wherever red poly crosses diffusion**.

> Note on notation: I read every "Y′ = E" as a single **inverting** gate whose pull-down network implements E, so the **output = E′** (the only thing a single static gate can do). I state the PDN/PUN for each. If your sheet means something else, the frame is identical — only the network swaps.

---

# Q1. List VLSI fabrication processes; compare P-well vs N-well (8M)

**Fabrication processes (notes p10):**
1. **nMOS** process (single transistor type).
2. **CMOS – p-well** process.
3. **CMOS – n-well** process.
4. **CMOS – twin-tub** process.
5. **Silicon-on-insulator (SOI)** process.

**P-well vs N-well (the comparison the question asks for):**

| Feature | **N-well** | **P-well** |
|---|---|---|
| Starting substrate | p-type | n-type |
| Well formed | n-well (holds **pMOS**) | p-well (holds **nMOS**) |
| Other device sits in | nMOS in p-substrate | pMOS in n-substrate |
| Threshold/body effect | **lower** substrate-bias effect on Vt | higher (well doping affects nMOS Vt & breakdown) |
| Parasitic capacitance | **lower** (better source/drain caps) | higher |
| Performance bias | better-controlled **pMOS**, good for digital | better-controlled **nMOS** |
| Substrate connections | n-well→VDD, p-sub→VSS | p-well→VSS, n-sub→VDD |

From notes: "N-well CMOS circuits are superior to p-well because of the lower substrate bias effects on threshold voltage and lower parasitic capacitances."

**Reasoning recap:** pick a substrate, diffuse the *opposite*-type well to host the minority transistor; n-well wins on Vt control and parasitics because pMOS lives in the lighter, better-controlled well.

---

# Q2. Mask layers in **P-well** CMOS fabrication and their functions (8M)

From notes (p16), the p-well process uses **8 masks**:

| Mask | Defines | Function |
|---|---|---|
| **1** | deep **p-well** regions | create the well that will hold nMOS |
| **2** | **thinox** (active) regions | where thick oxide is stripped and thin oxide grown for transistors/wires |
| **3** | **polysilicon** pattern | gate electrodes & poly interconnect (deposited after thin oxide) |
| **4** | **p-plus** (p⁺) mask | defines all p-diffusion areas (PMOS S/D); "AND"ed with Mask 2 |
| **5** | **negative of p-plus** | defines n-diffusion areas (NMOS S/D) |
| **6** | **contact cuts** | open windows in oxide for metal-to-layer contacts |
| **7** | **metal** pattern | aluminium interconnect wiring |
| **8** | **overglass / passivation** | openings for bonding pads; protect chip |

Self-aligning note: source/drain diffusions align to the poly gate automatically.

**Reasoning recap:** sequence = make the well (1) → define active & gates (2,3) → put in both diffusions using a mask and its complement (4,5) → connect up (6,7) → seal & open pads (8).

---

# Q3. Mask layers in **N-well** CMOS fabrication and their functions (8M)

Your notes give the n-well **step sequence** (p17 flowchart) but not a numbered mask list, so I map the steps to masks **[partly outside notes, standard ordering]**:

| Mask | Defines | Function |
|---|---|---|
| **1** | **n-well** regions | low-dose phosphorus implant + drive-in to form wells that hold pMOS |
| **2** | **thinox / active** areas | nMOS & pMOS active regions, field vs gate oxide |
| **3** | **polysilicon** | gates and poly interconnect |
| **4** | **p⁺ diffusion** | PMOS source/drain (also includes VDD contacts) |
| **5** | **n⁺ diffusion** (complement of mask 4) | NMOS source/drain (also includes VSS contacts) |
| **6** | **contact cuts** | oxide windows for contacts |
| **7** | **metal** | interconnect |
| **8** | **overglass** | bond-pad openings / passivation |

From notes: "an n⁺ mask and its complement may be used to define the n- and p-diffusion regions respectively; these same masks also include the VDD and VSS contacts."

**Reasoning recap:** identical flow to p-well but **mask 1 makes the n-well** (not p-well); the n⁺ mask + its complement place both diffusions and supply contacts.

---

# Q4. Basic steps in nMOS fabrication with sketches (10M)

From notes (p4–9):

1. **Substrate:** p-type Si wafer (boron-doped, ~10¹⁵–10¹⁶/cm³, 25–2 Ω·cm), 75–150 mm dia, 0.4 mm thick.
2. **Thick oxide:** grow ~1 µm SiO₂ over the whole surface (protection + dopant barrier + insulator).
3. **Photoresist:** spin on an even layer.
4. **Expose** through **Mask 1** to UV (areas for diffusion/channels are shielded).
5. **Etch** the exposed oxide → opens a **window** to bare silicon.
6. **Thin oxide + poly:** strip resist, grow thin SiO₂ (~0.1 µm), deposit polysilicon (CVD) for gates.
7. **Pattern poly** (Mask 3), remove thin oxide off S/D areas, **diffuse n⁺** (phosphorus) for source & drain — poly acts as mask → **self-aligning**.
8. **Thick oxide** again; mask & etch **contact cuts** (Mask 4).
9. **Metal (Al ~1 µm)** deposited, masked & etched into interconnect (Mask 5). (Mask 6 = overglass for pads.)

**Sketch (cross-section build-up):**
```
1-3:  [resist///]            5: [oxide]   [oxide]   <- window opened
      [oxide-----]              \________/   bare Si
      [ p-substrate ]            [ p-substrate ]

7:    [poly]                 9:  metal──┐  poly  ┌──metal
      n+──┘  └──n+               n+ contact    n+ contact
      [ p-substrate ]            [ p-substrate ]
```

```latex
% Q4 -- finished nMOS cross-section (the exam-useful end state)
\begin{tikzpicture}[x=1cm,y=0.7cm]
  \fill[gray!15] (0,0) rectangle (6,1.6);   \node at (3,0.55){p-substrate};
  % n+ source / drain
  \fill[green!55!black] (0.8,1.2) rectangle (2.0,1.6);
  \fill[green!55!black] (4.0,1.2) rectangle (5.2,1.6);
  \node[white,font=\tiny] at (1.4,1.4){n+}; \node[white,font=\tiny] at (4.6,1.4){n+};
  % gate oxide + poly
  \fill[cyan!20] (2.4,1.6) rectangle (3.6,1.75);
  \fill[red!70]  (2.4,1.75) rectangle (3.6,2.15); \node[lbl] at (3.0,2.4){poly gate};
  % thick field oxide
  \draw[line width=1pt] (0,1.6)--(0.8,1.6); \draw[line width=1pt] (5.2,1.6)--(6,1.6);
  % metal contacts to S and D
  \draw[metal] (1.4,1.6)--(1.4,2.6); \fill[blue!60] (1.0,2.6) rectangle (1.8,2.8);
  \draw[metal] (4.6,1.6)--(4.6,2.6); \fill[blue!60] (4.2,2.6) rectangle (5.0,2.8);
  \node[lbl] at (1.4,3.05){metal S}; \node[lbl] at (4.6,3.05){metal D};
\end{tikzpicture}
```

**Reasoning recap:** grow oxide → open windows with masks → lay poly gate → self-align n⁺ S/D to the gate → cut contacts → metallise. Six masks total.

---

# Q5. Complete CMOS **P-well** fabrication sequence (10M)

From notes (p12–16). Substrate = **n-type**.

1. **Diffuse the deep p-well** (4–5 µm) into the n-substrate (Mask 1). Doping/depth set nMOS Vt & breakdown.
2. **Grow thin oxide + deposit & pattern polysilicon** over both regions (Mask 2 thinox, Mask 3 poly) — PMOS gates (in n-substrate) and NMOS gates (in p-well) formed together.
3. **p⁺ diffusion** using the p-plus mask (Mask 4): forms PMOS source/drain in the n-substrate; the well region is blocked.
4. **n⁺ diffusion** using the **negative** p-plus mask (Mask 5): forms NMOS source/drain inside the p-well.
5. **Thick oxide**, then **contact cuts** (Mask 6).
6. **Metallisation** (Mask 7) — interconnect; **two substrate ties** needed: n-substrate→VDD, p-well→VSS.
7. **Overglass** with pad openings (Mask 8).

**Cross-section (Fig 1.10 idea):**
```
 Vdd                              Vss
  │ p+   poly   p+    | thick |   n+  poly  n+ │
  └─PMOS in n-substrate─┘ SiO2 └─NMOS in p-well─┘
  ───────────── n-substrate ──────────[ p-well ]
```

```latex
% Q5 -- CMOS p-well inverter cross-section (PMOS in n-sub, NMOS in p-well)
\begin{tikzpicture}[x=1cm,y=0.7cm]
  \fill[gray!12] (0,0) rectangle (9,1.8); \node at (2.2,0.45){n-substrate};
  \fill[orange!18] (5,0) rectangle (9,1.8); \node at (7.0,0.45){p-well};
  % PMOS (left): p+ poly p+
  \fill[yellow!85!orange] (0.6,1.4) rectangle (1.6,1.8);
  \fill[yellow!85!orange] (2.8,1.4) rectangle (3.8,1.8);
  \fill[red!70] (1.8,1.8) rectangle (2.6,2.2); \node[lbl] at (2.2,2.5){PMOS};
  \node[font=\tiny] at (1.1,1.6){p+}; \node[font=\tiny] at (3.3,1.6){p+};
  % isolation
  \fill[cyan!20] (3.9,1.8) rectangle (5.1,2.1); \node[font=\tiny] at (4.5,2.3){SiO$_2$};
  % NMOS (right, in p-well): n+ poly n+
  \fill[green!55!black] (5.4,1.4) rectangle (6.4,1.8);
  \fill[green!55!black] (7.6,1.4) rectangle (8.6,1.8);
  \fill[red!70] (6.6,1.8) rectangle (7.4,2.2); \node[lbl] at (7.0,2.5){NMOS};
  \node[font=\tiny,white] at (5.9,1.6){n+}; \node[font=\tiny,white] at (8.1,1.6){n+};
  % supply ties
  \draw[metal] (0.6,1.8)--(0.6,3.0); \node[lbl,above] at (0.6,3.0){$V_{DD}$};
  \draw[metal] (8.6,1.8)--(8.6,3.0); \node[lbl,above] at (8.6,3.0){$V_{SS}$};
\end{tikzpicture}
```

**Reasoning recap:** make the p-well first to host nMOS; build both gates together; place p⁺ then n⁺ with a mask and its complement; the two bodies need separate VDD/VSS ties because they're isolated.

---

# Q6. **N-well** CMOS fabrication, step by step (10M)

From notes (p17–18). Substrate = **p-type**.

1. **Form n-well:** Mask 1 → low-dose phosphorus implant, high-temp drive-in (well depth set to avoid p-sub↔p⁺ breakdown while keeping n-well↔n⁺ spacing).
2. **Define nMOS & pMOS active areas** (thinox).
3. **Field + gate oxidation** (thick field oxide, thin gate oxide).
4. **Deposit & pattern polysilicon** gates.
5. **p⁺ diffusion** → PMOS S/D (inside n-well) + VDD contacts.
6. **n⁺ diffusion** (complement mask) → NMOS S/D (in p-substrate) + VSS contacts.
7. **Contact cuts.**
8. **Deposit & pattern metal** interconnect.
9. **Overglass** with bonding-pad cuts.

**Cross-section (N-well inverter):**
```
 Vdd                               Vss
  │ p+  poly  p+   | thick |   n+  poly  n+ │
  └──PMOS in n-well─┘ SiO2 └─NMOS in p-substrate┘
  [───── n-well ─────]──────── p-substrate ───────
```

```latex
% Q6 -- CMOS n-well inverter cross-section (PMOS in n-well, NMOS in p-substrate)
\begin{tikzpicture}[x=1cm,y=0.7cm]
  \fill[gray!12] (0,0) rectangle (9,1.8); \node at (6.8,0.45){p-substrate};
  \fill[cyan!14] (0,0) rectangle (4,1.8); \node at (1.8,0.45){n-well};
  % PMOS in n-well
  \fill[yellow!85!orange] (0.6,1.4) rectangle (1.6,1.8);
  \fill[yellow!85!orange] (2.4,1.4) rectangle (3.4,1.8);
  \fill[red!70] (1.7,1.8) rectangle (2.3,2.2); \node[lbl] at (2.0,2.5){PMOS};
  \node[font=\tiny] at (1.1,1.6){p+}; \node[font=\tiny] at (2.9,1.6){p+};
  % isolation
  \fill[cyan!20] (4.0,1.8) rectangle (5.2,2.1); \node[font=\tiny] at (4.6,2.3){SiO$_2$};
  % NMOS in p-substrate
  \fill[green!55!black] (5.4,1.4) rectangle (6.4,1.8);
  \fill[green!55!black] (7.4,1.4) rectangle (8.4,1.8);
  \fill[red!70] (6.6,1.8) rectangle (7.2,2.2); \node[lbl] at (6.9,2.5){NMOS};
  \node[font=\tiny,white] at (5.9,1.6){n+}; \node[font=\tiny,white] at (7.9,1.6){n+};
  \draw[metal] (0.6,1.8)--(0.6,3.0); \node[lbl,above] at (0.6,3.0){$V_{DD}$};
  \draw[metal] (8.4,1.8)--(8.4,3.0); \node[lbl,above] at (8.4,3.0){$V_{SS}$};
\end{tikzpicture}
```

**Reasoning recap:** opposite of p-well — make the **n-well** first to host pMOS, leave nMOS in the native p-substrate; everything else (poly, p⁺/n⁺, contacts, metal, overglass) is the standard flow.

---

# How to draw the expression questions (applies to Q7–13, 16, 19, 20)

For each: (1) write PDN from the expression (AND=series, OR=parallel); (2) PUN = dual (CMOS only); (3) drop into the frame. I give the network + an ASCII stick **and** a TikZ block for each below.

---

# Q7. CMOS — Y′ = A + BC + D (10M)

- **PDN (n-diff):** `A ∥ (B–C) ∥ D`  → A parallel, (B series C), D — all three in parallel.
- **PUN (p-diff, dual):** `A – (B∥C) – D` → A series (B∥C) series D.
- Output = (A + BC + D)′.

**Stick (S1 frame):**
```
Vdd ══■═══════════════  (nwell tie ✕)
  yellow p-diff:  A ─ (B∥C) ─ D   (series chain, B,C parallel)
  poly gates ↓ A  B  C  D ↓ (vertical, shared)
─ ─ demarcation ─ ─ ─ ─ ─ ─
  green n-diff:  A ∥ (B─C) ∥ D    (all parallel)
Vss ══■═══════════════  (pwell tie ✕)
out ── blue metal joins p-drain & n-drain
```

```latex
% Q7 CMOS  Y'=(A+BC+D)'   PUN: A-(B||C)-D   PDN: A || (B-C) || D
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(6,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){}; \node[Xtie]at(5.5,6){$\times$};
  \draw[demarc](-.3,3)--(6.3,3); \node[lbl,right]at(6.3,3){demarc};
  \draw[metal](0,0)--(6,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){}; \node[Xtie]at(5.5,0){$\times$};
  % ---- PUN (yellow): series  A - (B||C) - D ----
  \pT{2.5}{5.3}{A} \draw[pdiff](2.5,5.75)--(2.5,6);     % A top -> Vdd  (A bottom = 4.85)
  \pT{2}{4.4}{B} \pT{3}{4.4}{C}                          % B || C
  \draw[pdiff](2,4.85)--(3,4.85);                        % parallel TOP rail (A bottom lands here)
  \draw[pdiff](2,3.95)--(3,3.95);                        % parallel BOTTOM rail
  \pT{2.5}{3.6}{D}                                       % D (top = 4.05, bottom = 3.15)
  \draw[pdiff](2.5,3.95)--(2.5,4.05);                    % bottom rail -> D top
  \node[cont]at(2.5,3.15){};                             % out node (= D bottom)
  % ---- PDN (green): A || (B-C) || D ----
  \draw[ndiff](1,2.85)--(4,2.85); \node[cont]at(2.5,2.85){};   % out rail
  \nT{1}{1.9}{A}   \draw[ndiff](1,2.35)--(1,2.85);  \draw[ndiff](1,1.45)--(1,0.3);
  \nT{2.5}{2.0}{B} \nT{2.5}{1.1}{C} \draw[ndiff](2.5,2.45)--(2.5,2.85); \draw[ndiff](2.5,0.65)--(2.5,0.3);
  \nT{4}{1.9}{D}   \draw[ndiff](4,2.35)--(4,2.85);  \draw[ndiff](4,1.45)--(4,0.3);
  % output metal : PUN out (3.15) -> PDN out (2.85)
  \draw[metal](4.6,3.15)--(4.6,2.85)--(4,2.85); \draw[metal](2.5,3.15)--(4.6,3.15); \node[lbl,right]at(4.6,3.0){$V_{out}$};
\end{tikzpicture}
```

**Reasoning recap:** OR'd terms → parallel nMOS; the AND'd `BC` → series pair; pMOS is the mirror (series where nMOS is parallel).

---

# Q8. CMOS — Y′ = AB + CD (10M)  *(classic AOI22)*

- **PDN:** `(A–B) ∥ (C–D)` → two series pairs, in parallel.
- **PUN (dual):** `(A∥B) – (C∥D)` → two parallel pairs, in series.
- Output = (AB + CD)′.

**Stick (S1):**
```
Vdd ══■════════════
  yellow:  (A∥B) ─ (C∥D)      (series of two parallel pairs)
  poly ↓ A B C D ↓
─ ─ demarcation ─ ─ ─
  green:   (A─B) ∥ (C─D)      (parallel of two series pairs)
Vss ══■════════════
```

```latex
% Q8 CMOS  Y'=(AB+CD)'   PUN: (A||B)-(C||D)   PDN: (A-B)||(C-D)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(6,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){}; \node[Xtie]at(5.5,6){$\times$};
  \draw[demarc](-.3,3)--(6.3,3); \node[lbl,right]at(6.3,3){demarc};
  \draw[metal](0,0)--(6,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){}; \node[Xtie]at(5.5,0){$\times$};
  % ---- PUN: (A||B) in series with (C||D) ----
  \pT{1.5}{5.2}{A} \pT{2.5}{5.2}{B}
  \draw[pdiff](1.5,5.65)--(2.5,5.65); \draw[pdiff](2,5.65)--(2,6);      % pair1 top -> Vdd
  \draw[pdiff](1.5,4.75)--(2.5,4.75);                                   % pair1 bottom
  \pT{1.5}{4.2}{C} \pT{2.5}{4.2}{D}
  \draw[pdiff](1.5,4.65)--(2.5,4.65); \draw[pdiff](2,4.75)--(2,4.65);   % join pair1->pair2
  \draw[pdiff](1.5,3.75)--(2.5,3.75); \draw[pdiff](2,3.75)--(2,3.15);   % pair2 bottom -> out
  \node[cont]at(2,3.15){};
  % ---- PDN: (A-B) || (C-D) ----
  \draw[ndiff](1,2.85)--(3,2.85); \node[cont]at(2,2.85){};              % out rail
  \nT{1}{2.2}{A}\nT{1}{1.3}{B} \draw[ndiff](1,2.65)--(1,2.85); \draw[ndiff](1,0.85)--(1,0.3);
  \nT{3}{2.2}{C}\nT{3}{1.3}{D} \draw[ndiff](3,2.65)--(3,2.85); \draw[ndiff](3,0.85)--(3,0.3);
  \draw[metal](4,3.15)--(4,2.85)--(3,2.85); \draw[metal](2,3.15)--(4,3.15); \node[lbl,right]at(4,3.0){$V_{out}$};
\end{tikzpicture}
```

**Reasoning recap:** each product → series stack; the OR between them → put the stacks in parallel; pull-up is the swap.

---

# Q9. CMOS — Y′ = AB + CD + E (10M)

- **PDN:** `(A–B) ∥ (C–D) ∥ E`.
- **PUN (dual):** `(A∥B) – (C∥D) – E`.
- Output = (AB + CD + E)′.

**Stick (S1):** same as Q8 with a **third parallel branch E** added to the green PDN, and **E in series** added to the yellow PUN.

```latex
% Q9 CMOS  Y'=(AB+CD+E)'  = Q8 plus an E branch.
% PDN: (A-B)||(C-D)||E    PUN: (A||B)-(C||D)-E
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6.6)--(6.5,6.6); \node[lbl,left]at(0,6.6){$V_{DD}$}; \node[cont]at(.5,6.6){};
  \draw[demarc](-.3,3)--(6.8,3); \node[lbl,right]at(6.8,3){demarc};
  \draw[metal](0,0)--(6.5,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){};
  % PUN: (A||B)-(C||D)-E
  \pT{1.5}{5.8}{A} \pT{2.5}{5.8}{B} \draw[pdiff](1.5,6.25)--(2.5,6.25); \draw[pdiff](2,6.25)--(2,6.6);
  \draw[pdiff](1.5,5.35)--(2.5,5.35);
  \pT{1.5}{4.8}{C} \pT{2.5}{4.8}{D} \draw[pdiff](1.5,5.25)--(2.5,5.25); \draw[pdiff](2,5.35)--(2,5.25);
  \draw[pdiff](1.5,4.35)--(2.5,4.35); \draw[pdiff](2,4.35)--(2,3.6+0.45);
  \pT{2}{3.6}{E} \draw[pdiff](2,3.15)--(2,3.15); \draw[pdiff](2,3.6-0.45)--(2,3.15); \node[cont]at(2,3.15){};
  % PDN: (A-B)||(C-D)||E
  \draw[ndiff](1,2.85)--(5,2.85); \node[cont]at(3,2.85){};
  \nT{1}{2.2}{A}\nT{1}{1.3}{B} \draw[ndiff](1,2.65)--(1,2.85);\draw[ndiff](1,0.85)--(1,0.3);
  \nT{3}{2.2}{C}\nT{3}{1.3}{D} \draw[ndiff](3,2.65)--(3,2.85);\draw[ndiff](3,0.85)--(3,0.3);
  \nT{5}{1.75}{E} \draw[ndiff](5,2.2)--(5,2.85);\draw[ndiff](5,1.3)--(5,0.3);
  \draw[metal](5.7,3.15)--(5.7,2.85)--(5,2.85); \draw[metal](2,3.15)--(5.7,3.15); \node[lbl,right]at(5.7,3.0){$V_{out}$};
\end{tikzpicture}
```

**Reasoning recap:** extra OR term E = one more parallel nMOS branch (and one more series pMOS in the dual).

---

# Q10. nMOS — XOR and NOR gates (10M)

### NOR (easy)
- **PDN:** `A ∥ B` (two enhancement nMOS in parallel) + one depletion load.
- Output = (A + B)′ = NOR.

```
Vdd ══════
  [depletion load, implant box]
   ──O── out
  A∥B  (two nMOS in parallel)
GND ══════
```

```latex
% Q10 nMOS NOR : depletion load + (A || B) pull-down
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,5.2)--(4,5.2); \node[lbl,left]at(0,5.2){$V_{DD}$}; \node[cont]at(.5,5.2){};
  \dload{2}{4.3} \draw[ndiff](2,5.2)--(2,4.75);
  \draw[ndiff](2,3.85)--(2,3.5); \draw[ndiff](0.8,3.5)--(3.2,3.5); \node[cont]at(2,3.5){}; \node[lbl,right]at(3.4,3.5){out};
  \nT{0.8}{2.6}{A} \nT{3.2}{2.6}{B}
  \draw[ndiff](0.8,3.05)--(0.8,3.5); \draw[ndiff](3.2,3.05)--(3.2,3.5);
  \draw[ndiff](0.8,2.15)--(0.8,0.7); \draw[ndiff](3.2,2.15)--(3.2,0.7); \draw[ndiff](0.8,0.7)--(3.2,0.7);
  \draw[metal](0,0.4)--(4,0.4); \node[lbl,left]at(0,0.4){GND}; \node[cont]at(.5,0.4){};
\end{tikzpicture}
```

### XOR  **[outside notes — needs complemented inputs]**
XOR can't be a single simple pull-down: it needs A′, B′. Build two inverters for A′,B′, then a pull-down that conducts when **A = B**:
- **PDN:** `(A–B) ∥ (A′–B′)` (series A·B in parallel with series A′·B′) + depletion load.
- That pulls output **low when A=B**, i.e. output = A ⊕ B (HIGH only when A≠B).

```
A ─[inv]─ A'      B ─[inv]─ B'
Vdd ══════
  [depletion load]
   ──O── out = A⊕B
  (A─B) ∥ (A'─B')
GND ══════
```

```latex
% Q10 nMOS XOR : load + (A-B) || (A'-B')  ; needs A',B' from inverters
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,5.4)--(4.5,5.4); \node[lbl,left]at(0,5.4){$V_{DD}$}; \node[cont]at(.5,5.4){};
  \dload{2.2}{4.5} \draw[ndiff](2.2,5.4)--(2.2,4.95);
  \draw[ndiff](2.2,4.05)--(2.2,3.6); \draw[ndiff](1,3.6)--(3.4,3.6); \node[cont]at(2.2,3.6){};
  \node[lbl,right]at(3.5,3.6){out $=A\oplus B$};
  % branch 1 : A - B (series)
  \nT{1}{2.7}{A}\nT{1}{1.8}{B} \draw[ndiff](1,3.15)--(1,3.6); \draw[ndiff](1,1.35)--(1,0.8);
  % branch 2 : A' - B' (series)
  \nT{3.4}{2.7}{$\overline{A}$}\nT{3.4}{1.8}{$\overline{B}$} \draw[ndiff](3.4,3.15)--(3.4,3.6); \draw[ndiff](3.4,1.35)--(3.4,0.8);
  \draw[metal](0,0.5)--(4.5,0.5); \node[lbl,left]at(0,0.5){GND}; \draw[ndiff](1,0.8)--(3.4,0.8);
  \node[lbl,align=left] at (2.2,-0.3){(supply $\overline{A},\overline{B}$ from two inverters)};
\end{tikzpicture}
```

**Reasoning recap:** NOR = OR'd inputs → parallel nMOS. XOR is not monotonic, so it needs inverters for A′,B′; the pull-down `AB + A′B′` conducts when inputs match → its inverted output is XOR.

---

# Q11. nMOS — Y′ = AB + C(D + E) (10M)

- **PDN:** `(A–B) ∥ (C–(D∥E))` → series A·B, in parallel with [C in series with (D parallel E)].
- + depletion load.
- Output = (AB + C(D+E))′.

```
Vdd ══════
  [depletion load]
   ──O── out
  (A─B)  ∥  (C─(D∥E))
GND ══════
```

```latex
% Q11 nMOS  Y'=(AB + C(D+E))'   PDN: (A-B) || ( C - (D||E) )
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5.5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){};
  \dload{2.7}{5.1} \draw[ndiff](2.7,6)--(2.7,5.55);
  \draw[ndiff](2.7,4.65)--(2.7,4.2); \draw[ndiff](1,4.2)--(4.4,4.2); \node[cont]at(2.7,4.2){}; \node[lbl,right]at(4.5,4.2){out};
  % branch 1 : A - B
  \nT{1}{3.3}{A}\nT{1}{2.4}{B} \draw[ndiff](1,3.75)--(1,4.2); \draw[ndiff](1,1.95)--(1,0.6);
  % branch 2 : C - (D||E)
  \nT{3.6}{3.3}{C} \draw[ndiff](3.6,3.75)--(3.6,4.2);                 % C top to out
  \nT{3.1}{2.3}{D}\nT{4.1}{2.3}{E}                                    % D||E
  \draw[ndiff](3.1,2.75)--(4.1,2.75); \draw[ndiff](3.6,2.85)--(3.6,2.75); % join C bottom -> pair top
  \draw[ndiff](3.1,1.85)--(4.1,1.85); \draw[ndiff](3.6,1.85)--(3.6,0.6);  % pair bottom -> GND
  \draw[metal](0,0.3)--(5.5,0.3); \node[lbl,left]at(0,0.3){GND}; \draw[ndiff](1,0.6)--(3.6,0.6);
\end{tikzpicture}
```

**Reasoning recap:** AB → series; D+E → parallel pair; C·(that) → C in series with the pair; the two big terms are OR'd → parallel.

---

# Q12. nMOS — Y′ = AB(C + D) + E (10M)

- **PDN:** `(A–B–(C∥D)) ∥ E` → A series B series (C parallel D), all in parallel with E.
- + depletion load.
- Output = (AB(C+D) + E)′.

```
Vdd ══════
  [depletion load]
   ──O── out
  ( A─B─(C∥D) )  ∥  E
GND ══════
```

```latex
% Q12 nMOS  Y'=(AB(C+D)+E)'   PDN: ( A - B - (C||D) ) || E
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6.6)--(5.5,6.6); \node[lbl,left]at(0,6.6){$V_{DD}$}; \node[cont]at(.5,6.6){};
  \dload{2.2}{5.7} \draw[ndiff](2.2,6.6)--(2.2,6.15);
  \draw[ndiff](2.2,5.25)--(2.2,4.8); \draw[ndiff](1.2,4.8)--(4.4,4.8); \node[cont]at(2.2,4.8){}; \node[lbl,right]at(4.5,4.8){out};
  % long branch : A - B - (C||D)
  \nT{1.7}{3.9}{A}\nT{1.7}{3.0}{B} \draw[ndiff](1.7,4.35)--(1.7,4.8);
  \nT{1.2}{2.0}{C}\nT{2.2}{2.0}{D} \draw[ndiff](1.2,2.45)--(2.2,2.45); \draw[ndiff](1.7,2.55)--(1.7,2.45);
  \draw[ndiff](1.2,1.55)--(2.2,1.55); \draw[ndiff](1.7,1.55)--(1.7,0.6);
  % parallel branch : E
  \nT{4.4}{3.2}{E} \draw[ndiff](4.4,3.65)--(4.4,4.8); \draw[ndiff](4.4,2.75)--(4.4,0.6);
  \draw[metal](0,0.3)--(5.5,0.3); \node[lbl,left]at(0,0.3){GND}; \draw[ndiff](1.7,0.6)--(4.4,0.6);
\end{tikzpicture}
```

**Reasoning recap:** A·B·(C+D) is one long series branch (with C,D parallel inside); E OR'd on → its own parallel branch.

---

# Q13. nMOS — Y′ = (A + B)(C + D) + E (10M)

- **PDN:** `((A∥B)–(C∥D)) ∥ E` → (A parallel B) in series with (C parallel D), all in parallel with E.
- + depletion load.
- Output = ((A+B)(C+D) + E)′.

```
Vdd ══════
  [depletion load]
   ──O── out
  ( (A∥B)─(C∥D) )  ∥  E
GND ══════
```

```latex
% Q13 nMOS  Y'=((A+B)(C+D)+E)'   PDN: ( (A||B) - (C||D) ) || E
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6.6)--(5.5,6.6); \node[lbl,left]at(0,6.6){$V_{DD}$}; \node[cont]at(.5,6.6){};
  \dload{2.2}{5.7} \draw[ndiff](2.2,6.6)--(2.2,6.15);
  \draw[ndiff](2.2,5.25)--(2.2,4.8); \draw[ndiff](1.2,4.8)--(4.4,4.8); \node[cont]at(2.2,4.8){}; \node[lbl,right]at(4.5,4.8){out};
  % series of two parallel pairs : (A||B) then (C||D)
  \nT{1.2}{3.9}{A}\nT{2.2}{3.9}{B} \draw[ndiff](1.2,4.35)--(2.2,4.35); \draw[ndiff](1.7,4.35)--(1.7,4.8);
  \draw[ndiff](1.2,3.45)--(2.2,3.45); \draw[ndiff](1.7,3.45)--(1.7,3.0+0.45);
  \nT{1.2}{3.0}{C}\nT{2.2}{3.0}{D} \draw[ndiff](1.2,2.55)--(2.2,2.55); \draw[ndiff](1.7,2.55)--(1.7,0.6);
  % parallel branch E
  \nT{4.4}{3.2}{E} \draw[ndiff](4.4,3.65)--(4.4,4.8); \draw[ndiff](4.4,2.75)--(4.4,0.6);
  \draw[metal](0,0.3)--(5.5,0.3); \node[lbl,left]at(0,0.3){GND}; \draw[ndiff](1.7,0.6)--(4.4,0.6);
\end{tikzpicture}
```

**Reasoning recap:** each OR-group → parallel pair; the AND between groups → put the two pairs in series; E OR'd → parallel branch.

---

# Q14. CMOS 2-input (a) NOR (b) NAND — stick + layout (10M)

From your notes (p11–14).

### (a) NOR — Y = (A+B)′
- **PDN:** `A ∥ B` (parallel nMOS). **PUN:** `A – B` (series pMOS).
```
Vdd ══■═══
  yellow:  A ─ B     (series pMOS)
  poly ↓ A B
─ ─demarc─ ─
  green:   A ∥ B     (parallel nMOS)
Vss ══■═══
```

```latex
% Q14a CMOS NOR : PUN A-B (series pMOS), PDN A||B (parallel nMOS)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){}; \node[Xtie]at(4.5,6){$\times$};
  \draw[demarc](-.3,3)--(5.3,3);
  \draw[metal](0,0)--(5,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){}; \node[Xtie]at(4.5,0){$\times$};
  % PUN : A - B series
  \pT{2}{5.2}{A}\pT{2}{4.3}{B} \draw[pdiff](2,5.65)--(2,6); \draw[pdiff](2,3.85)--(2,3.15); \node[cont]at(2,3.15){};
  % PDN : A || B parallel
  \draw[ndiff](1,2.85)--(3,2.85); \node[cont]at(2,2.85){};
  \nT{1}{1.9}{A}\nT{3}{1.9}{B} \draw[ndiff](1,2.35)--(1,2.85); \draw[ndiff](3,2.35)--(3,2.85);
  \draw[ndiff](1,1.45)--(1,0.3); \draw[ndiff](3,1.45)--(3,0.3); \draw[ndiff](1,0.3)--(3,0.3);
  \draw[metal](3.7,3.15)--(3.7,2.85)--(3,2.85); \draw[metal](2,3.15)--(3.7,3.15); \node[lbl,right]at(3.7,3){$Y$};
\end{tikzpicture}
```

### (b) NAND — Y = (AB)′
- **PDN:** `A – B` (series nMOS). **PUN:** `A ∥ B` (parallel pMOS).
```
Vdd ══■═══
  yellow:  A ∥ B     (parallel pMOS)
  poly ↓ A B
─ ─demarc─ ─
  green:   A ─ B     (series nMOS)
Vss ══■═══
```

```latex
% Q14b CMOS NAND : PUN A||B (parallel pMOS), PDN A-B (series nMOS)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){}; \node[Xtie]at(4.5,6){$\times$};
  \draw[demarc](-.3,3)--(5.3,3);
  \draw[metal](0,0)--(5,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){}; \node[Xtie]at(4.5,0){$\times$};
  % PUN : A || B parallel
  \pT{1}{5.0}{A}\pT{3}{5.0}{B} \draw[pdiff](1,5.45)--(3,5.45); \draw[pdiff](2,5.45)--(2,6);
  \draw[pdiff](1,4.55)--(1,3.15); \draw[pdiff](3,4.55)--(3,3.15); \draw[pdiff](1,3.15)--(3,3.15); \node[cont]at(2,3.15){};
  % PDN : A - B series
  \nT{2}{2.2}{A}\nT{2}{1.3}{B} \draw[ndiff](2,2.65)--(2,2.85); \node[cont]at(2,2.85){}; \draw[ndiff](2,0.85)--(2,0.3);
  \draw[metal](3.7,3.15)--(3.7,2.85)--(2,2.85); \draw[metal](2,3.15)--(3.7,3.15); \node[lbl,right]at(3.7,3){$Y$};
\end{tikzpicture}
```

**Layout (L1):** n-well box on top holds the yellow p-diffusion bar crossed by the two red poly gates (PMOS); below the demarcation, a green n-diffusion bar crossed by the **same** two poly gates (NMOS); Vdd metal rail (top) + Vss metal rail (bottom); output metal links the two drains. NOR vs NAND differ only by **which network is series vs parallel**.

```latex
% Q14 CMOS NAND mask-style LAYOUT (n-well box, diffusion bars, poly gates, metal rails)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[brown,thick] (0.3,3.2) rectangle (5.7,6.2); \node[brown,lbl] at (5.2,6.0){nwell};
  % Vdd rail + p-diffusion bar with two poly gates (parallel pMOS)
  \draw[metal](0.5,5.8)--(5.5,5.8); \node[lbl,left]at(0.5,5.8){$V_{DD}$};
  \draw[pdiff](1,4.6)--(5,4.6);                                 % p-diff bar
  \draw[metal](1.3,5.8)--(1.3,4.6); \draw[metal](4.7,5.8)--(4.7,4.6); \node[cont]at(1.3,4.6){}; \node[cont]at(4.7,4.6){};
  \node[cont]at(3,4.6){};                                       % shared drain contact
  \draw[poly](2,6.2)--(2,0.6); \node[lbl,left]at(2,0.9){a};      % gate A
  \draw[poly](4,6.2)--(4,0.6); \node[lbl,left]at(4,0.9){b};      % gate B
  % n-diffusion bar (series nMOS) below well
  \draw[ndiff](1,1.6)--(5,1.6); \node[cont]at(1,1.6){}; \node[cont]at(5,1.6){};
  % output metal Z : PMOS drain (3,4.6) down to NMOS drain (5,1.6)
  \draw[metal](3,4.6)--(3,2.4)--(5,2.4)--(5,1.6); \node[lbl,right]at(5.05,2.4){Z};
  % Vss rail
  \draw[metal](0.5,0.4)--(5.5,0.4); \node[lbl,left]at(0.5,0.4){$V_{SS}$}; \draw[metal](1,1.6)--(1,0.4); \node[cont]at(1,0.4){};
\end{tikzpicture}
```

**Reasoning recap:** NOR = OR inputs → parallel nMOS / series pMOS; NAND = AND inputs → series nMOS / parallel pMOS. Same frame, networks swapped.

---

# Q15. nMOS 2-input (a) NOR (b) NAND — stick + layout (10M)

- **(a) NOR:** PDN = `A ∥ B` + depletion load → out = (A+B)′.
- **(b) NAND:** PDN = `A – B` + depletion load → out = (AB)′.

```
(a) NOR              (b) NAND
Vdd ════             Vdd ════
 [dep load]           [dep load]
  ──O──out             ──O──out
  A ∥ B                A ─ B
GND ════             GND ════
```

```latex
% Q15 nMOS NOR (left) and NAND (right) side by side
\begin{tikzpicture}[x=1cm,y=0.8cm]
  % ---- (a) NOR : A || B ----
  \begin{scope}
    \draw[metal](0,5.2)--(4,5.2); \node[lbl,left]at(0,5.2){$V_{DD}$};
    \dload{2}{4.3} \draw[ndiff](2,5.2)--(2,4.75);
    \draw[ndiff](2,3.85)--(2,3.5); \draw[ndiff](0.8,3.5)--(3.2,3.5); \node[cont]at(2,3.5){}; \node[lbl,right]at(3.3,3.5){out};
    \nT{0.8}{2.6}{A}\nT{3.2}{2.6}{B}
    \draw[ndiff](0.8,3.05)--(0.8,3.5);\draw[ndiff](3.2,3.05)--(3.2,3.5);
    \draw[ndiff](0.8,2.15)--(0.8,0.7);\draw[ndiff](3.2,2.15)--(3.2,0.7);\draw[ndiff](0.8,0.7)--(3.2,0.7);
    \draw[metal](0,0.4)--(4,0.4); \node[lbl,left]at(0,0.4){GND}; \node[lbl] at (2,-0.1){(a) NOR};
  \end{scope}
  % ---- (b) NAND : A - B ----
  \begin{scope}[xshift=5.5cm]
    \draw[metal](0,5.2)--(4,5.2); \node[lbl,left]at(0,5.2){$V_{DD}$};
    \dload{2}{4.3} \draw[ndiff](2,5.2)--(2,4.75);
    \draw[ndiff](2,3.85)--(2,3.5); \draw[ndiff](1,3.5)--(3,3.5); \node[cont]at(2,3.5){}; \node[lbl,right]at(3.1,3.5){out};
    \nT{2}{2.7}{A}\nT{2}{1.8}{B} \draw[ndiff](2,3.15)--(2,3.5); \draw[ndiff](2,1.35)--(2,0.4);
    \draw[metal](0,0.4)--(4,0.4); \node[lbl,left]at(0,0.4){GND}; \node[lbl] at (2,-0.1){(b) NAND};
  \end{scope}
\end{tikzpicture}
```

**Layout (L2):** Vdd metal at top, one depletion transistor (poly gate tied to its source, **yellow implant box** over it), output node, then the enhancement pull-down (parallel for NOR, series for NAND), GND at bottom. Inputs = red poly crossing green n-diffusion.

**Reasoning recap:** nMOS uses one depletion **load** instead of a pMOS network; only the pull-down arrangement (parallel vs series) distinguishes NOR from NAND.

---

# Q16. nMOS — Y′ = (A+B)(C+D)(E+F) (10M)

- **PDN:** `(A∥B) – (C∥D) – (E∥F)` → three parallel pairs, all in series.
- + depletion load.
- Output = ((A+B)(C+D)(E+F))′.

```
Vdd ══════
  [depletion load]
   ──O── out
  (A∥B) ─ (C∥D) ─ (E∥F)    (three parallel pairs in series)
GND ══════
```

```latex
% Q16 nMOS  Y'=((A+B)(C+D)(E+F))'   PDN: (A||B)-(C||D)-(E||F)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,7.4)--(4,7.4); \node[lbl,left]at(0,7.4){$V_{DD}$}; \node[cont]at(.5,7.4){};
  \dload{1.7}{6.5} \draw[ndiff](1.7,7.4)--(1.7,6.95);
  \draw[ndiff](1.7,6.05)--(1.7,5.6); \draw[ndiff](1.2,5.6)--(2.2,5.6); \node[cont]at(1.7,5.6){}; \node[lbl,right]at(2.3,5.6){out};
  % pair (A||B)
  \nT{1.2}{4.7}{A}\nT{2.2}{4.7}{B} \draw[ndiff](1.2,5.15)--(2.2,5.15);\draw[ndiff](1.7,5.15)--(1.7,5.6);
  \draw[ndiff](1.2,4.25)--(2.2,4.25);\draw[ndiff](1.7,4.25)--(1.7,3.8+0.45);
  % pair (C||D)
  \nT{1.2}{3.8}{C}\nT{2.2}{3.8}{D} \draw[ndiff](1.2,3.35)--(2.2,3.35);\draw[ndiff](1.7,3.35)--(1.7,2.9+0.45);
  % pair (E||F)
  \nT{1.2}{2.9}{E}\nT{2.2}{2.9}{F} \draw[ndiff](1.2,2.45)--(2.2,2.45);\draw[ndiff](1.7,2.45)--(1.7,1.5);
  \draw[metal](0,1.2)--(4,1.2); \node[lbl,left]at(0,1.2){GND};
\end{tikzpicture}
```

**Reasoning recap:** each OR-group → parallel pair; the three groups are AND'd → stack the pairs in series.

---

# Q17. nMOS 3-input (a) NOR (b) NAND — stick + layout (10M)

- **(a) NOR:** PDN = `A ∥ B ∥ C` + load → (A+B+C)′.
- **(b) NAND:** PDN = `A – B – C` + load → (ABC)′.

```
(a) 3-NOR            (b) 3-NAND
Vdd ════             Vdd ════
 [dep load]           [dep load]
  ──O──out             ──O──out
  A ∥ B ∥ C           A ─ B ─ C
GND ════             GND ════
```

```latex
% Q17 nMOS 3-input NOR (left) and NAND (right)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  % (a) 3-NOR : A||B||C
  \begin{scope}
    \draw[metal](0,5.4)--(4.4,5.4); \node[lbl,left]at(0,5.4){$V_{DD}$};
    \dload{2.2}{4.5} \draw[ndiff](2.2,5.4)--(2.2,4.95);
    \draw[ndiff](2.2,4.05)--(2.2,3.7); \draw[ndiff](0.8,3.7)--(3.6,3.7); \node[cont]at(2.2,3.7){}; \node[lbl,right]at(3.7,3.7){out};
    \nT{0.8}{2.8}{A}\nT{2.2}{2.8}{B}\nT{3.6}{2.8}{C}
    \foreach \x in {0.8,2.2,3.6}{\draw[ndiff](\x,3.25)--(\x,3.7);\draw[ndiff](\x,2.35)--(\x,0.7);}
    \draw[ndiff](0.8,0.7)--(3.6,0.7); \draw[metal](0,0.4)--(4.4,0.4); \node[lbl,left]at(0,0.4){GND};
    \node[lbl] at (2.2,-0.1){(a) 3-NOR};
  \end{scope}
  % (b) 3-NAND : A-B-C series
  \begin{scope}[xshift=6cm]
    \draw[metal](0,5.4)--(4,5.4); \node[lbl,left]at(0,5.4){$V_{DD}$};
    \dload{2}{4.5} \draw[ndiff](2,5.4)--(2,4.95);
    \draw[ndiff](2,4.05)--(2,3.7); \draw[ndiff](1,3.7)--(3,3.7); \node[cont]at(2,3.7){}; \node[lbl,right]at(3.1,3.7){out};
    \nT{2}{2.9}{A}\nT{2}{2.0}{B}\nT{2}{1.1}{C} \draw[ndiff](2,3.35)--(2,3.7); \draw[ndiff](2,0.65)--(2,0.4);
    \draw[metal](0,0.4)--(4,0.4); \node[lbl,left]at(0,0.4){GND}; \node[lbl] at (2,-0.1){(b) 3-NAND};
  \end{scope}
\end{tikzpicture}
```

**Layout (L2):** identical frame to Q15, just **three** poly gates crossing the green n-diffusion — three parallel transistors (NOR) or a series chain of three (NAND).

**Reasoning recap:** extend the 2-input case by one input → one more parallel branch (NOR) or one more series device (NAND).

---

# Q18. nMOS vs CMOS design style; inverter in both (10M)

**Differences (from notes p2, p9, and inverter sections):**

| Aspect | nMOS style | CMOS style |
|---|---|---|
| Pull-up | **depletion-mode load** (always-on transistor) | **pMOS network** (complement of pull-down) |
| Logic type | **ratioed** (output low ≠ 0, set by load:driver ratio) | **ratioless / complementary** (full rail-to-rail) |
| Static power | draws current when output LOW | ~zero static (one side always OFF) |
| Layers in stick | green n-diff, red poly, blue metal, yellow **implant** | adds **yellow p-diff** + **n-well / demarcation** |
| Body ties | substrate→GND | n-well→VDD and substrate→VSS |

**Inverter — nMOS:**
```
Vdd ════
 [depletion load: gate tied to source, implant box]
  ──O── Vout
 [enhancement driver, gate = Vin]
GND ════
Vout = Vin'   (ratioed)
```

```latex
% Q18 nMOS inverter : depletion load + enhancement driver
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,5)--(3.5,5); \node[lbl,left]at(0,5){$V_{DD}$}; \node[cont]at(.5,5){};
  \dload{1.7}{4.1} \draw[ndiff](1.7,5)--(1.7,4.55);
  \draw[ndiff](1.7,3.65)--(1.7,3.2); \node[cont]at(1.7,3.2){}; \draw[metal](1.7,3.2)--(3.2,3.2); \node[lbl,right]at(3.3,3.2){$V_{out}$};
  \nT{1.7}{2.3}{$V_{in}$} \draw[ndiff](1.7,2.75)--(1.7,3.2); \draw[ndiff](1.7,1.85)--(1.7,0.4);
  \draw[metal](0,0.4)--(3.5,0.4); \node[lbl,left]at(0,0.4){GND}; \node[cont]at(.5,0.4){};
\end{tikzpicture}
```

**Inverter — CMOS:**
```
Vdd ════
 [pMOS, gate = Vin]   (in n-well)
  ──O── Vout
 [nMOS, gate = Vin]
GND ════
Vout = Vin'   (rail-to-rail, no static current)
```

```latex
% Q18 CMOS inverter : pMOS over nMOS, common gate Vin
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,5)--(3.5,5); \node[lbl,left]at(0,5){$V_{DD}$}; \node[cont]at(.5,5){};
  \pT{1.7}{4.1}{} \draw[pdiff](1.7,4.55)--(1.7,5);
  \draw[demarc](-.3,3.2)--(3.5,3.2);
  \draw[pdiff](1.7,3.65)--(1.7,3.35); \node[cont]at(1.7,3.35){};
  \nT{1.7}{2.5}{} \draw[ndiff](1.7,2.95)--(1.7,3.05); \node[cont]at(1.7,3.05){}; \draw[ndiff](1.7,2.05)--(1.7,0.4);
  % common gate Vin
  \draw[poly](1.3,4.1)--(1.3,2.5); \node[lbl,left]at(1.3,3.3){$V_{in}$};
  \draw[poly](1.3,4.1)--(1.7,4.1); \draw[poly](1.3,2.5)--(1.7,2.5);
  % output
  \draw[metal](1.7,3.35)--(3.2,3.35); \draw[metal](1.7,3.05)--(1.7,3.35); \node[lbl,right]at(3.3,3.35){$V_{out}$};
  \draw[metal](0,0.4)--(3.5,0.4); \node[lbl,left]at(0,0.4){GND}; \node[cont]at(.5,0.4){};
\end{tikzpicture}
```

**Reasoning recap:** nMOS = one switching transistor + a passive depletion load (simple, but ratioed and leaky); CMOS = two complementary networks so exactly one conducts (full swing, ~no static power) at the cost of the extra p-diffusion/n-well layers.

---

# Q19. CMOS — (i) Y′=(A+B)C  (ii) Y′=A+BC (12M)

### (i) (A+B)C
- **PDN:** `(A∥B) – C` → (A parallel B) in series with C.
- **PUN (dual):** `(A–B) ∥ C` → (A series B) in parallel with C.
- Output = ((A+B)C)′.
```
Vdd ══■═══ ;  yellow: (A─B) ∥ C
─ demarc ─ ;  green:  (A∥B) ─ C ;  Vss ══■═══
```

```latex
% Q19(i) CMOS  Y'=((A+B)C)'   PUN:(A-B)||C   PDN:(A||B)-C
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){};
  \draw[demarc](-.3,3)--(5.3,3);
  \draw[metal](0,0)--(5,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){};
  % PUN : (A-B) || C
  \pT{1.5}{5.2}{A}\pT{1.5}{4.3}{B} \draw[pdiff](1.5,5.65)--(1.5,6); \draw[pdiff](1.5,3.85)--(1.5,3.15);
  \pT{3}{4.75}{C} \draw[pdiff](3,5.2)--(3,6); \draw[pdiff](3,4.3)--(3,3.15);
  \draw[pdiff](1.5,3.15)--(3,3.15); \node[cont]at(2.2,3.15){};
  % PDN : (A||B) - C
  \draw[ndiff](1,2.85)--(3,2.85); \node[cont]at(2,2.85){};
  \nT{1}{2.2}{A}\nT{3}{2.2}{B} \draw[ndiff](1,2.65)--(1,2.85);\draw[ndiff](3,2.65)--(3,2.85);
  \draw[ndiff](1,1.75)--(2,1.55);\draw[ndiff](3,1.75)--(2,1.55);                 % join to C top
  \nT{2}{1.1}{C} \draw[ndiff](2,1.55)--(2,1.55); \draw[ndiff](2,0.65)--(2,0.3);
  \draw[metal](3.7,3.15)--(3.7,2.85)--(3,2.85); \draw[metal](2.2,3.15)--(3.7,3.15); \node[lbl,right]at(3.7,3){$Y$};
\end{tikzpicture}
```

### (ii) A+BC
- **PDN:** `A ∥ (B–C)` → A in parallel with (B series C).
- **PUN (dual):** `A – (B∥C)` → A in series with (B parallel C).
- Output = (A+BC)′.
```
Vdd ══■═══ ;  yellow: A ─ (B∥C)
─ demarc ─ ;  green:  A ∥ (B─C) ;  Vss ══■═══
```

```latex
% Q19(ii) CMOS  Y'=(A+BC)'   PUN:A-(B||C)   PDN:A||(B-C)
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){};
  \draw[demarc](-.3,3)--(5.3,3);
  \draw[metal](0,0)--(5,0); \node[lbl,left]at(0,0){$V_{SS}$}; \node[cont]at(.5,0){};
  % PUN : A - (B||C)
  \pT{2.2}{5.2}{A} \draw[pdiff](2.2,5.65)--(2.2,6);                 % A top -> Vdd (A bottom = 4.75)
  \pT{1.7}{4.3}{B}\pT{2.7}{4.3}{C} \draw[pdiff](1.7,4.75)--(2.7,4.75); % parallel top rail (A lands here)
  \draw[pdiff](1.7,3.85)--(2.7,3.85); \draw[pdiff](2.2,3.85)--(2.2,3.15); \node[cont]at(2.2,3.15){}; % bottom rail -> out
  % PDN : A || (B-C)
  \draw[ndiff](1,2.85)--(3.4,2.85); \node[cont]at(2.2,2.85){};
  \nT{1}{1.9}{A} \draw[ndiff](1,2.35)--(1,2.85); \draw[ndiff](1,1.45)--(1,0.3);
  \nT{3.4}{2.2}{B}\nT{3.4}{1.3}{C} \draw[ndiff](3.4,2.65)--(3.4,2.85); \draw[ndiff](3.4,0.85)--(3.4,0.3);
  \draw[metal](4,3.15)--(4,2.85)--(3.4,2.85); \draw[metal](2.2,3.15)--(4,3.15); \node[lbl,right]at(4,3){$Y$};
\end{tikzpicture}
```
> In (ii) delete the `\nT{0}{0}{}` spacer line — it's a no-op placeholder.

**Reasoning recap:** read the brackets — OR inside → parallel nMOS, AND outside → series; pMOS is the dual each time.

---

# Q20. nMOS — (i) Y′=(A+B)CD  (ii) Y′=A+BC+D (12M)

### (i) (A+B)CD
- **PDN:** `(A∥B) – C – D` → (A parallel B) in series with C and D.
- + depletion load. Output = ((A+B)CD)′.
```
Vdd ════ ; [dep load] ; ──O──out
  (A∥B) ─ C ─ D ;  GND ════
```

```latex
% Q20(i) nMOS  Y'=((A+B)CD)'   PDN: (A||B) - C - D
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6.6)--(4,6.6); \node[lbl,left]at(0,6.6){$V_{DD}$}; \node[cont]at(.5,6.6){};
  \dload{1.7}{5.7} \draw[ndiff](1.7,6.6)--(1.7,6.15);
  \draw[ndiff](1.7,5.25)--(1.7,4.8); \draw[ndiff](1.2,4.8)--(2.2,4.8); \node[cont]at(1.7,4.8){}; \node[lbl,right]at(2.3,4.8){out};
  % (A||B)
  \nT{1.2}{3.9}{A}\nT{2.2}{3.9}{B} \draw[ndiff](1.2,4.35)--(2.2,4.35);\draw[ndiff](1.7,4.35)--(1.7,4.8);
  \draw[ndiff](1.2,3.45)--(2.2,3.45);\draw[ndiff](1.7,3.45)--(1.7,3.0+0.45);
  % - C - D series
  \nT{1.7}{3.0}{C}\nT{1.7}{2.1}{D} \draw[ndiff](1.7,2.55)--(1.7,2.55); \draw[ndiff](1.7,1.65)--(1.7,1.2);
  \draw[metal](0,0.9)--(4,0.9); \node[lbl,left]at(0,0.9){GND};
\end{tikzpicture}
```

### (ii) A+BC+D
- **PDN:** `A ∥ (B–C) ∥ D`.
- + depletion load. Output = (A+BC+D)′.  *(Same PDN as Q7, just nMOS load instead of pMOS pull-up.)*
```
Vdd ════ ; [dep load] ; ──O──out
  A ∥ (B─C) ∥ D ;  GND ════
```

```latex
% Q20(ii) nMOS  Y'=(A+BC+D)'   PDN: A || (B-C) || D
\begin{tikzpicture}[x=1cm,y=0.8cm]
  \draw[metal](0,6)--(5,6); \node[lbl,left]at(0,6){$V_{DD}$}; \node[cont]at(.5,6){};
  \dload{2.5}{5.1} \draw[ndiff](2.5,6)--(2.5,5.55);
  \draw[ndiff](2.5,4.65)--(2.5,4.2); \draw[ndiff](1,4.2)--(4,4.2); \node[cont]at(2.5,4.2){}; \node[lbl,right]at(4.1,4.2){out};
  \nT{1}{3.3}{A}  \draw[ndiff](1,3.75)--(1,4.2); \draw[ndiff](1,2.85)--(1,0.6);
  \nT{2.5}{3.4}{B}\nT{2.5}{2.5}{C} \draw[ndiff](2.5,3.85)--(2.5,4.2); \draw[ndiff](2.5,2.05)--(2.5,0.6);
  \nT{4}{3.3}{D}  \draw[ndiff](4,3.75)--(4,4.2); \draw[ndiff](4,2.85)--(4,0.6);
  \draw[metal](0,0.3)--(5,0.3); \node[lbl,left]at(0,0.3){GND}; \draw[ndiff](1,0.6)--(4,0.6);
\end{tikzpicture}
```

**Reasoning recap:** AND-chain (i) → one series stack with the (A+B) parallel pair at the bottom; sum-of-products (ii) → three parallel branches. Same nMOS frame, one depletion load.

---

## Honesty checklist
- **Q3** n-well numbered mask list is **[partly outside notes]** — your notes give the step flow, not a numbered mask table; I mapped them in the standard order.
- **Q10 XOR** is **[outside notes]** and needs two extra inverters (A′, B′) — flagged.
- Notation: every "Y′ = E" treated as a single **inverting** gate (output = E′, PDN implements E). If your sheet intends otherwise, the frame is unchanged.
- **TikZ blocks are NOT compiled here** (no LaTeX on this machine). They encode the correct *topology* (which transistors are series/parallel) but exact coordinates may need small nudges on Overleaf. Paste the **LaTeX setup block once** into your preamble first — every diagram depends on its `\nT`, `\pT`, `\dload` macros and the colour styles.
