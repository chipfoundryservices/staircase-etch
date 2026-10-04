# Chapter 2: The Alternating Stack — Materials, Deposition & Stress

## Overview

A staircase is carved out of the stack. Every property of the stack that the etch cares about (the thickness of each layer, its density and hydrogen content, the sharpness of each interface, the stress it carries, and the bow it gives the wafer) shows up somewhere in the staircase result. A layer that is 2% thick at the wafer edge needs 2% more etch time there. An interface with a graded oxynitride transition blurs the selective stop. A stack under compressive stress bows the wafer by hundreds of microns, and the chuck has to flatten it before the trim can run at a uniform temperature.

This chapter describes the stacks that staircases are cut into, the deposition processes that make them, and the ways their properties feed into the etch.

**Learning Objectives:**
- Compare ON (SiO₂/Si₃N₄) and OP (SiO₂/poly-Si) stacks and the integration schemes that use them
- Describe how PECVD stack deposition sets layer thickness, density, hydrogen content, and interface quality
- Estimate how layer-thickness variation translates into pair-etch time variation and landing risk
- Apply the Stoney equation to estimate wafer bow from stack stress
- Identify the special layers at the top and bottom of the stack and why they need their own etch steps

---

## 2.1 Stack Types

### 2.1.1 ON Stack (Replacement Gate)

The dominant architecture uses alternating silicon dioxide and silicon nitride. The nitride is a **sacrificial** layer. After the staircase is formed and filled, slits are etched through the stack, the nitride is removed with hot phosphoric acid, and the cavities are filled with a barrier and tungsten (or molybdenum) to form the word lines.

```
ON pair (reference process):

  ┌────────────────────────┐
  │ SiO₂   25 nm           │  ← inter-gate dielectric
  ├────────────────────────┤
  │ Si₃N₄  30 nm           │  ← sacrificial; becomes W word line
  └────────────────────────┘
  Pair pitch p = 55 nm
```

### 2.1.2 OP Stack (Gate-First)

In gate-first designs, the stack alternates silicon dioxide with doped polysilicon, and the polysilicon layers are the word lines directly. The staircase etch is then an oxide/poly-Si pair etch, which uses the familiar halogen chemistry of silicon etch for one half of each cycle.

```
OP pair (illustrative):

  ┌────────────────────────┐
  │ SiO₂     25–30 nm      │
  ├────────────────────────┤
  │ poly-Si  30–40 nm      │  ← doped; is the word line
  └────────────────────────┘
```

### 2.1.3 Comparison

```
Property                    ON stack                     OP stack
──────────────────────────────────────────────────────────────────────────────
Word line material          W or Mo after replacement    Doped poly-Si
Pair-etch chemistry         Fluorocarbon (oxide) +       Fluorocarbon (oxide) +
                            hydrofluorocarbon (nitride)  HBr/Cl₂ (poly)
Natural selectivity         Moderate (5–20:1 each way)   High (poly:oxide > 50:1
                                                         in HBr; oxide:poly 10–30:1)
Tread surface after etch    Oxide over nitride           Oxide over poly
Word-line resistance        Low (metal)                  Higher (poly)
Stack stress                Nitride tensile/oxide        Poly near neutral/oxide
                            compressive; tunable         compressive
Contact landing material    W (after replacement)        Poly (silicided or not)
Dominant use                Most current products        Some early generations
```

Most of this book uses the ON stack because it dominates current production. Where OP behavior differs, the text notes it.

---

## 2.2 Stack Deposition

### 2.2.1 PECVD in Alternation

Stacks are deposited by plasma-enhanced CVD, typically in a single chamber that switches between oxide and nitride chemistries:

```
Film     Precursors (typical)          Temperature     Rate (typical)
─────────────────────────────────────────────────────────────────────────
SiO₂     TEOS/O₂ or SiH₄/N₂O           350–550 °C     5–20 nm/s
Si₃N₄    SiH₄/NH₃/N₂                   350–550 °C     3–10 nm/s
poly-Si  SiH₄ (or a-Si, crystallized)  400–600 °C     2–8 nm/s
```

A 136-pair stack is 272 depositions. Even at several nanometers per second plus transition time, stack deposition takes hours per wafer. It is a major part of 3D NAND capital cost.

### 2.2.2 What Deposition Sets for the Etch

```
Deposition property         Effect on staircase etch
─────────────────────────────────────────────────────────────────────────────
Layer thickness (mean)      Sets pair-etch time per step
Layer thickness (WIW)       Sets overetch needed for complete landing
                            across the wafer
Layer thickness (drift      Lower layers may differ from upper layers;
  through stack)            etch time per level may need adjusting
Density                     Higher density → slower etch; affects
                            selectivity ratio
H content (Si–H, N–H)       H-rich nitride etches faster in
                            hydrofluorocarbon; shifts selectivity
Interface transition        Oxynitride (SiOₓN_y) at each interface blurs
  (1–2 nm)                  the selective stop and the OES endpoint edge
Stress                      Bow, chucking, lithography focus
Defects / particles         Embedded particles mask the etch locally →
                            pillars, stringers
```

### 2.2.3 Interface Quality

At each switch from oxide to nitride or back, gas composition changes over a finite time. The result is a thin graded layer:

```
Composition profile across an interface (schematic):

  SiO₂        │ transition │      Si₃N₄
  O ███████████▓▓▓▒▒░░                       
  N              ░░▒▒▓▓▓█████████████████
              ◄── 1–2 nm ──►

The transition layer is an oxynitride. It etches at a rate between
oxide and nitride in either selective chemistry. It is part of the
reason a "selective" step does not stop abruptly.
```

When the oxide etch reaches the transition layer, the etch rate falls gradually rather than all at once. For landing accuracy (Chapter 11), the interface width adds directly to the uncertainty in where the etch stops.

---

## 2.3 Thickness Variation and Landing

### 2.3.1 Per-Layer Variation

PECVD thickness uniformity is good, but not perfect, and it compounds:

```
Layer          Mean      WIW (1σ)       WIW range (typical)
──────────────────────────────────────────────────────────────
SiO₂           25 nm     ~1.0% (0.25 nm)  ±2–3%
Si₃N₄          30 nm     ~1.2% (0.36 nm)  ±2–4%

Radial signature is common: thicker at center or at edge,
depending on showerhead and temperature profile.
```

### 2.3.2 Etch-Time Requirement

The pair etch must clear each layer at its thickest point on the wafer:

```
Required main-etch time for one layer:

  t_ME = t_max / R_min

where t_max = thickest point, R_min = slowest etch rate on the wafer.

Example (oxide half of the pair, reference process):
  t_mean = 25.0 nm, t_max = 25.6 nm (+2.4%)
  R_mean = 120 nm/min, R_min = 115 nm/min (−4%)
  Nominal time: 25.0/120 = 12.5 s
  Required:     25.6/115 = 13.4 s
  → 7% overetch just to cover thickness and rate non-uniformity,
    before any margin for interfaces or endpoint uncertainty
```

The overetch lands on the nitride below. Its cost in nitride loss is set by selectivity (Chapter 11).

### 2.3.3 Cumulative Depth Variation

The depth of tread *k* is the sum of the thicknesses of all pairs above it. Uncorrelated variation adds in quadrature, while correlated (systematic) variation adds linearly:

```
Depth of tread k: D_k = Σ_{i=1}^{k} p_i

Uncorrelated layer-to-layer variation:
  σ_D,k = √k · σ_p

Systematic radial signature (same sign in every layer):
  ΔD_k = k · Δp

Example: k = 128, σ_p = 0.4 nm, Δp (edge vs. center) = +0.8 nm
  σ_D = √128 × 0.4 = 4.5 nm   (small)
  ΔD  = 128 × 0.8 = 102 nm    (large)
```

**Systematic thickness signatures dominate the depth variation of deep treads.** The staircase etch handles this naturally, because each cycle etches one pair whatever its thickness. The word-line contact etch, which must reach all treads in one step, has to accommodate the full ΔD (Chapter 16).

---

## 2.4 Etch-Relevant Film Properties

### 2.4.1 Density and Hydrogen

```
Property (representative PECVD)    SiO₂ (TEOS)     Si₃N₄ (SiH₄/NH₃)
─────────────────────────────────────────────────────────────────────
Density (g/cm³)                    2.15–2.25       2.6–2.9
H content (at.%)                   1–5             8–20
Refractive index (633 nm)          1.45–1.47       1.95–2.05
Dielectric constant                ~4.0–4.2        ~6.5–7.5
100:1 HF wet etch rate (nm/min)    3–8             0.3–2
Hot H₃PO₄ etch rate (nm/min)       ~0.05–0.2       4–8 (sets replacement)
```

Hydrogen-rich nitride is less dense and etches faster in hydrofluorocarbon plasma. A change in nitride deposition that shifts H content by a few percent can change the nitride step time by several percent and shift the oxide-to-nitride selectivity of the oxide step. **A deposition change is an etch change.** Etch engineers should own a hydrogen and refractive-index monitor on the stack.

### 2.4.2 Nitride for Replacement

The nitride must etch quickly in hot phosphoric acid during replacement gate, and the oxide must not. That favors moderately hydrogen-rich nitride. It is one reason stack nitride is not optimized for dry-etch selectivity alone.

---

## 2.5 Stress, Bow, and Warpage

### 2.5.1 Why the Stack Bows the Wafer

Each film carries an intrinsic stress. A 7–15 µm stack on a 775 µm silicon wafer exerts enough force to bend it noticeably. The Stoney equation relates film force to curvature:

```
κ = 6 · σ_f · t_f · (1 − ν_s) / (E_s · t_s²)

Center-to-edge bow for a wafer of radius R:
  δ ≈ κ · R² / 2

where:
  σ_f · t_f        = film force per unit width (stress × thickness)
  E_s / (1 − ν_s)  = biaxial modulus of Si(100) ≈ 180 GPa
  t_s              = substrate thickness (775 µm for 300 mm)
```

### 2.5.2 Worked Example

```
Stack: 7.5 µm, net average stress −40 MPa (compressive)
  σ_f · t_f = −40 × 10⁶ × 7.5 × 10⁻⁶ = −300 N/m

  κ = 6 × 300 / (180 × 10⁹ × (775 × 10⁻⁶)²)
    = 1800 / (180 × 10⁹ × 6.006 × 10⁻⁷)
    = 1800 / 108,100
    = 0.0167 m⁻¹

  δ = 0.0167 × (0.150)² / 2 = 1.87 × 10⁻⁴ m ≈ 190 µm

At −80 MPa net, the bow doubles to ~375 µm.
```

Stack stress is tuned layer by layer (oxide compressive, nitride tunable from compressive to tensile) and often balanced with backside films. Even so, 3D NAND wafers commonly arrive at staircase etch with bows of tens to a few hundred microns, and the bow may not be spherically symmetric.

### 2.5.3 Consequences for Staircase Etch

```
Effect                              Mechanism                           Chapter
──────────────────────────────────────────────────────────────────────────────────
Chucking difficulty                 ESC must pull wafer flat;           8
                                    high bow → He leak, poor contact
Radial temperature non-uniformity   Poor contact at edge → hotter       8
                                    edge → faster edge trim
Lithography focus / resist          Thick-resist exposure on a bowed    12
  sidewall angle                    wafer → edge sidewall varies
Local stress relaxation at the      Removing stack layers in the        12, 16
  staircase                         staircase changes local stress →
                                    in-plane distortion → contact
                                    overlay error
Bow change during the sequence      Each cycle removes material over    8
                                    the staircase area; bow drifts
                                    slightly through the sequence
```

### 2.5.4 Saddle and Anisotropic Bow

After slit etch, which cuts the stack into long strips, stress relaxes differently along and across the slits, and the wafer can take a saddle shape. Staircase etch usually precedes slit etch, so it sees mainly the more symmetric as-deposited bow. Multi-deck flows, however, can bring a partly patterned lower deck into the upper-deck staircase sequence (Chapter 14).

---

## 2.6 The Top and Bottom of the Stack

The stack is not 136 identical pairs. The ends carry special layers:

```
Position          Layers (illustrative)                  Etch implication
────────────────────────────────────────────────────────────────────────────────
Top               Cap oxide (100–300 nm) and/or hard     First step(s) need
                  mask remnant                           thicker oxide etch
                  Drain select gates (SGD): 2–6 levels,  Different thickness →
                  sometimes thicker oxide between them   different step time
                  Dummy word lines: 1–4 levels           Normal pairs
Middle            Word-line pairs (repeating)            Repeating recipe
Bottom            Dummy word lines                       Normal pairs
                  Source select gates (SGS): 1–4 levels  May be thicker
                  Bottom oxide / source layer            Final landing on a
                                                         different stop layer
```

Because of these special layers, a staircase recipe is rarely one cycle repeated. It is a **sequence of cycles with per-level step times**, and sometimes per-level chemistry. Recipe management (Chapter 15) must keep the right step on the right level.

In multi-deck stacks there is also an **inter-deck layer**, a thicker dielectric (and sometimes a plug landing layer) between decks. The staircase must cross it, and its etch step is different from the repeating pair.

---

## 2.7 OP Stack Specifics

For oxide/poly-Si stacks, the pair etch is:

```
Step                 Chemistry (typical)          Selectivity
───────────────────────────────────────────────────────────────────────
Oxide (stop on poly) C₄F₆ or C₄F₈/O₂/Ar           Oxide:poly 10–30:1
Poly (stop on oxide) HBr/Cl₂/O₂ or HBr/O₂         Poly:oxide 50–200:1
```

The very high poly-to-oxide selectivity of HBr chemistry makes the poly half of the pair easy to land. The oxide half is harder, as in ON stacks. Poly doping level changes the etch rate, so doping uniformity matters as thickness uniformity does.

---

## 2.8 Summary & Key Takeaways

1. **ON stacks dominate.** Nitride is a placeholder that becomes the metal word line after the staircase and slit steps.

2. **Every deposition property is an etch input.** Thickness, density, hydrogen content, and interface width all show up in pair-etch time and landing accuracy.

3. **Systematic thickness variation dominates deep treads.** Uncorrelated variation grows as √k, and systematic variation grows as k.

4. **Interfaces are graded.** A 1–2 nm oxynitride transition at each interface softens every selective stop.

5. **Stacks bow wafers.** Tens to hundreds of microns of bow affect chucking, temperature uniformity, lithography, and contact overlay.

6. **The stack has ends.** Cap layers, select gates, dummy layers, and inter-deck layers require per-level recipe steps.

---

## Study Questions

1. An oxide layer has mean thickness 25 nm with a +3% thick spot at the wafer edge, and the oxide etch rate is 4% slow there. Compute the required main-etch time if the mean rate is 120 nm/min. What overetch fraction does this imply relative to the nominal time?

2. A 160-pair stack has uncorrelated layer-to-layer pitch variation σ_p = 0.3 nm and a systematic edge-to-center pitch offset of +0.5 nm. Compute σ_D and ΔD for the deepest tread. Which matters more for word-line contact etch, and why?

3. Using the Stoney equation, compute the bow of a 300 mm wafer carrying a 12 µm stack with net stress −30 MPa. What net stress would be needed to keep the bow below 100 µm?

4. Nitride deposition is changed so that hydrogen content rises from 12 at.% to 16 at.%, and the nitride etch rate in the pair etch rises 6%. If the nitride step is timed at 15 s with 20% overetch, how much more overetch into the tread oxide results if the time is not adjusted? With nitride:oxide selectivity of 8:1 and a nitride rate of 150 nm/min, how much additional tread oxide is lost?

5. Explain why a staircase recipe for a real stack is a sequence of per-level steps rather than one cycle repeated N times. List three layer types that need special treatment.

---

**Previous Chapter:** [Chapter 1: 3D NAND Architecture & the Word-Line Staircase](./01-staircase-architecture.md)  
**Next Chapter:** [Chapter 3: Trim–Etch Physics — Building Steps From Resist Pullback](./03-trim-etch-physics.md)

---

**Chapter 2 Development Status:** Complete  
**Version:** 1.0
