# Chapter 3: Trim–Etch Physics — Building Steps From Resist Pullback

## Overview

Staircase etch makes many steps from one lithography exposure. It does so by alternating two very different plasma processes on the same resist block. The **pair etch** is anisotropic: it removes one oxide/nitride pair wherever the resist is absent and copies the resist edge straight down. The **trim** is isotropic: it pulls the resist edge back by one tread width and also thins the resist from the top. Each etch–trim cycle adds one level to the staircase. The cycle repeats until the resist is too thin to mask another etch.

This chapter builds the geometry of trim–etch from first principles. It derives how many steps one resist coat can make, shows how tread edges inherit the sum of every earlier trim, and works out how the trim moves the resist boundary in two dimensions. These results are the foundation for the error budgets in Chapter 12 and the advanced schemes in Chapter 14.

**Learning Objectives:**
- Trace the geometry of a trim–etch sequence cycle by cycle
- Explain why the trim has a vertical-to-lateral ratio greater than one, and estimate it
- Compute resist loss per cycle from trim and from pair etch
- Derive the maximum number of steps per mask from the resist budget
- Express tread edge position as a cumulative sum of trims
- Predict how the trim moves convex and concave resist corners in plan view
- Estimate the time for a complete trim–etch sequence

---

## 3.1 One Cycle, Step by Step

### 3.1.1 Starting Point

A thick resist block covers the memory array and ends at an edge at x = x₀. Beyond x₀ lies the staircase region. Everything to the right of the edge is exposed.

### 3.1.2 The Sequence

```
E1: pair etch                  T1: trim                   E2: pair etch
                                                     
 resist                         resist                     resist
┌────────┐                     ┌──────┐                   ┌──────┐
│        │                     │      │ ← edge moved      │      │
│        │                     │      │   back by w;      │      │
│        │                     │      │   top lowered     │      │
├────────┤                     ├──────┼──┐                ├──────┼──┐
│ level 0│                     │      │L0│                │      │L1│
│        └────── level 1       │      │  └──── level 1    │      │  └──── level 2
                               ◄──────►◄w►                ◄──────►◄w►
                                                          (strip of width w
                                                           now at level 1;
                                                           field at level 2)
```

After the first etch, the field outside the resist is one pair down. The trim pulls the resist edge back by w and exposes a strip of top-level surface. The second etch takes that strip down one pair and the field down to a second pair. **Each etch lowers every exposed surface by exactly one pair. Each trim exposes one new strip.**

### 3.1.3 After n Trims

A sequence of n trims and n + 1 etches leaves:

```
Position                       Level (pairs below top)   Width
────────────────────────────────────────────────────────────────
Under remaining resist          0                         —
Strip exposed by trim n         1                         w_n
Strip exposed by trim n − 1     2                         w_(n−1)
  ...                           ...                       ...
Strip exposed by trim 1         n                         w_1
Field beyond original edge      n + 1                     —

One mask sequence adds n + 1 levels: n treads plus the field,
which becomes the starting surface for the next mask.
```

Note the order. **The tread closest to the array was exposed by the last trim. The tread farthest from the array was exposed by the first trim and has been etched n times.** Treads in the same sequence carry different histories (Chapter 11).

### 3.1.4 Continuing With the Next Mask

When the resist is used up, it is stripped. A new resist block is patterned, covering all the treads already formed plus a margin, and the sequence repeats on the field. The new block's starting edge must be placed exactly one tread width beyond the last tread of the previous mask. That placement is a lithography overlay problem, the **stitch** between masks (Chapter 12).

```
After mask 1 (n = 8):    levels 0..9 formed
After mask 2 (n = 8):    levels 9..18 formed, beginning one tread
                          beyond the end of mask 1
  ...
Single-layer staircase of 136 levels with 9 levels per mask:
  ⌈136 / 9⌉ = 16 masks
```

Sixteen thick-resist masks for one staircase is expensive. Reducing that number is the motivation for multi-layer steps and chop masks (Chapter 14).

---

## 3.2 The Isotropic Trim

### 3.2.1 What the Trim Does

The trim is a resist-ashing plasma, usually O₂-based (Chapter 4), run with little or no ion energy. Oxygen atoms reach every exposed resist surface and oxidize it to CO, CO₂, and H₂O. The resist recedes along every exposed surface normal:

```
Lateral trim:   the vertical sidewall moves back by w = R_L · t_trim
Vertical trim:  the top surface moves down by  Δ_V = R_V · t_trim

Trim ratio:     r = R_V / R_L = Δ_V / w
```

### 3.2.2 Why the Top Etches Faster

```
Contributor                         Effect on r (= R_V / R_L)
────────────────────────────────────────────────────────────────────────────
View factor: the top sees the       Raises r. The sidewall sees plasma
  whole plasma hemisphere           above it and the wafer surface below.
                                    Some O reaching it has first struck
                                    the floor and lost reactivity
Residual ion flux (even at zero     Raises r. Ions arrive vertically, so
  bias, ions arrive at ~10–20 eV)   they assist the top and barely touch
                                    the sidewall
Top crust (fluorinated, from        Lowers r for the first cycle(s). The
  the pair etch)                    top surface carries a fluorinated or
                                    cross-linked skin that trims slowly
                                    until it is broken through (Ch. 4.5)
Low O reaction probability          Pushes r toward 1. With many wall
                                    and floor collisions per reaction, the
                                    O flux near the feature becomes nearly
                                    isotropic
Heat: top is not hotter than the    Neutral (resist is thin enough to be
  sidewall                          isothermal)

Typical range: r ≈ 1.1–2.0; reference process r = 1.3
```

### 3.2.3 Why the Trim Ratio Matters

The vertical loss is the price of the lateral pullback. Every micron of tread costs r microns of resist thickness. With r = 1.3 and w = 0.60 µm, each trim consumes 0.78 µm of resist. Over eight trims that is 6.2 µm, most of an 8 µm resist budget.

**Lowering r is the most direct way to get more steps per mask.** The levers (lower ion energy in trim, chemistry that favors lateral attack, top-surface protection) are discussed in Chapters 4, 6, and 12.

---

## 3.3 Resist Loss During the Pair Etch

The pair etch also erodes the resist, but much less:

```
δ_E = p_etched / S_R

where p_etched = stack thickness removed in the etch (one pair, or
                 m pairs for multi-layer steps)
      S_R      = effective stack-to-resist selectivity

Reference: p = 55 nm, S_R ≈ 4 (fluorocarbon oxide and HFC nitride steps)
  δ_E ≈ 55 / 4 ≈ 14 nm per etch

Multi-layer step (m = 4 pairs, 220 nm):
  δ_E ≈ 220 / 4 = 55 nm per etch
```

For single-layer steps, δ_E is small compared with the trim loss (14 nm vs. 780 nm). For multi-layer steps it grows but usually stays below 10% of the trim loss. **The trim, not the etch, consumes the resist budget.**

The pair etch also deposits a fluorocarbon film on the resist and can harden its surface. That raises the trim's induction time in the next cycle (Chapter 4.5).

---

## 3.4 The Resist Budget and Steps per Mask

### 3.4.1 The Budget Equation

After n trims and n + 1 etches, the remaining resist is:

```
T_n = T₀ − n · r · w − (n + 1) · δ_E

The sequence must stop while T_n ≥ T_min, where T_min is the
minimum resist thickness that still masks an etch reliably
(covering pinholes, thinning at the edge, and margin).

Maximum number of trims:

  n_max = ⌊ (T₀ − T_min − δ_E) / (r · w + δ_E) ⌋

Levels per mask = n_max + 1
```

### 3.4.2 Reference Calculation

```
T₀    = 8.0 µm
T_min = 1.0 µm
r     = 1.3
w     = 0.60 µm
δ_E   = 0.014 µm

r · w + δ_E = 0.780 + 0.014 = 0.794 µm per cycle

n_max = ⌊ (8.0 − 1.0 − 0.014) / 0.794 ⌋ = ⌊ 8.80 ⌋ = 8 trims

Levels per mask = 9
Resist left:  T₈ = 8.0 − 8 × 0.780 − 9 × 0.014 = 1.63 µm
```

### 3.4.3 Sensitivities

```
Change from reference              n_max    Levels/mask
──────────────────────────────────────────────────────────
Reference                           8        9
r = 1.1                             10       11
r = 1.6                             7        8
w = 0.45 µm                         11       12
T₀ = 12 µm                          13       14
T_min = 1.5 µm                      8        9
m = 4 pairs/step (δ_E = 55 nm)      8        9 steps → 36 levels

(n_max from the budget equation, rounded down)
```

The table points to three conclusions:

1. **Narrower treads give more steps per mask.** The resist pays per micron of trim, not per step.
2. **The trim ratio is a strong lever.** Going from r = 1.3 to 1.1 adds two levels per mask, which is about 20%.
3. **Multi-layer steps multiply levels at almost no resist cost.** Etching four pairs per cycle quadruples the levels per mask, because etch loss is small next to trim loss. This is the basis of the schemes in Chapter 14.

### 3.4.4 Limits on Thicker Resist

Thicker resist gives more steps per mask, but it has costs:

```
Limit                              Typical concern
──────────────────────────────────────────────────────────────────────
Lithography                        Thick-resist exposure and development;
                                   sidewall angle and edge placement degrade
Coat uniformity                    Thickness variation scales with T₀; edge
                                   bead; topography over earlier steps
Resist stress / cracking           Thick films crack or lift after hard bake
Outgassing and chamber load        More carbon to remove per cycle (Ch. 9)
Trim time                          Unchanged per cycle; total time grows with
                                   steps per mask
```

Representative practice uses 5–15 µm KrF or i-line resist for staircase masks.

---

## 3.5 Edge Position as a Cumulative Sum

### 3.5.1 Tread Edges

Let the initial resist edge be at x₀. After trim i, the edge is at:

```
x_i = x₀ − Σ_{j=1}^{i} w_j

where w_j = lateral trim in cycle j (ideally all equal to w).
```

The tread exposed by trim i lies between x_i and x_(i−1). Its **width** is w_i, set by one trim. The **position** of its edge toward the array, x_i, is set by every trim up to and including trim i.

### 3.5.2 What This Means for Errors

```
Quantity                     Set by                  Error grows as
──────────────────────────────────────────────────────────────────────
Tread width (tread i)        Trim i alone            Constant with i
Edge position (x_i)          Trims 1 through i       Linearly (systematic)
                                                     √i (random)
```

Word-line contacts are printed in one lithography step for all treads at once, at fixed design positions. **Contacts do not care about tread width as much as about tread position.** A 1% systematic trim error (6 nm per trim) leaves every tread the right width to within 6 nm, but it puts the edge of tread 8 off by 48 nm. Chapter 12 develops the full error budget.

---

## 3.6 Two-Dimensional Trim Geometry

### 3.6.1 The Staircase in Plan View

The resist does not have a single straight edge. In plan view it is a polygon, and the trim moves every edge inward along its normal. A staircase opening on the right side of an array block therefore grows on three sides:

```
Plan view: resist (█) and growing staircase opening

  Before trims                    After n trims
  ██████████████████████          ██████████████████████
  ██████████████████████          ███████████████  ·····   ← side treads
  ████████████│                   ███████████│ · · · · ·      also form
  ████████████│  opening          ███████████│ · · · · ·      along the
  ████████████│                   ███████████│ · · · · ·      y-edges
  ████████████│                   ███████████│ · · · · ·
  ██████████████████████          ███████████████  ·····
  ██████████████████████          ██████████████████████
              ▲                              ▲
        initial edge x₀                 x₀ − Σw
```

The treads along the y-directed edges (the **side treads**) are wasted area in a simple staircase. Layout puts them outside the active blocks or uses them on purpose in split-cell schemes (Chapter 14).

### 3.6.2 Convex and Concave Corners

Pure isotropic trim moves each point of the resist boundary inward along its normal by the trim distance. The result depends on the type of corner:

```
Corner type (seen from the resist)     Under uniform normal recession
────────────────────────────────────────────────────────────────────────
Concave resist corner (an inside       Becomes an arc. Radius grows with
  corner of the opening, where two     cumulative trim: R = Σ w_j
  resist edges meet at 270° on the     (for an initially sharp corner)
  resist side)
Convex resist corner (a corner of      Stays nominally sharp under ideal
  the resist block, 90° on the         uniform recession. In practice it
  resist side)                         blunts, because the corner sees
                                       more plasma (higher O flux) and
                                       the lithographic corner is already
                                       rounded
```

The concave case is the important one for staircase openings. At each inside corner of the opening, the tread edge becomes an arc whose radius after i trims equals the total pullback:

```
Reference: 8 trims of 0.60 µm
  R₈ = 4.8 µm

The treads near the corner follow concentric arcs of radius
0.6, 1.2, 1.8, ... 4.8 µm. A contact placed inside the corner zone
lands on a curved tread. Layout must keep contacts outside this zone
or account for the arc in the design.
```

### 3.6.3 Flux-Driven Corner Blunting

Real trims are not perfectly uniform in normal speed. Convex resist corners and top edges see a larger solid angle of plasma and recede faster:

```
Local trim rate enhancement at a convex 90° corner (illustrative):
  R_corner / R_edge ≈ 1.1–1.4

Effect after n trims:
  extra corner recession ≈ (R_corner/R_edge − 1) · Σ w_j
  Reference (ratio 1.2): 0.2 × 4.8 = ~1 µm of corner blunting
```

Corner blunting and arc formation together set the **corner exclusion zone** in staircase layout (Chapter 10).

---

## 3.7 Edge Transfer Through the Pair Etch

The pair etch copies the resist edge downward. Two effects shift the copied edge from the resist base:

```
1. Resist edge recession during the etch:
   If the resist sidewall makes angle α with the horizontal, vertical
   erosion δ_E moves the edge back by δ_E / tan(α).

   Reference: δ_E = 14 nm, α = 85°
     Δx = 14 / 11.4 = 1.2 nm (negligible)

   With α = 75° (sloped thick resist):
     Δx = 14 / 3.73 = 3.8 nm per etch

2. Riser taper:
   The etched riser is not vertical. With riser angle θ, the tread
   loses p · cot(θ) of flat width at its upper edge (Chapter 10).
```

Both are small per cycle for single-layer steps, but the first one accumulates. In a sequence of n etches, the edge of the outermost tread has been recessed by n · Δx. A sloped resist sidewall therefore adds a **systematic** term to tread placement (Chapter 12).

---

## 3.8 Sequence Timing

```
Reference cycle (single-layer step):

Step                       Time
──────────────────────────────────────────────
Oxide etch (25 nm + OE)    ~14 s
Transition                 ~3 s
Nitride etch (30 nm + OE)  ~16 s
Transition to trim         ~8 s
Trim (0.60 µm at           ~90 s
  0.40 µm/min)
Transition to etch         ~8 s
──────────────────────────────────────────────
Cycle total                ~139 s

Per mask (8 trims, 9 etches):
  Etch steps:   9 × (14 + 3 + 16) = 297 s
  Trims:        8 × (90 + 8 + 8) = 848 s
  Crust break / first-trim extra:  ~20 s
  Total:        ~1165 s ≈ 19.4 min of process time per mask
```

**The trim dominates sequence time,** about 70% in this example. A faster trim saves time, but only if it can be controlled to the same nanometer accuracy. Chapter 5 turns these times into a throughput model.

---

## 3.9 Summary & Key Takeaways

1. **Each etch lowers every exposed surface by one pair. Each trim exposes one new strip.** n trims and n + 1 etches add n + 1 levels.

2. **The trim is isotropic but not symmetric.** The top recedes faster than the sidewall, with a trim ratio r ≈ 1.1–2.0.

3. **The trim consumes the resist budget.** Etch loss per cycle is tens of nanometers. Trim loss is r·w, close to a micron.

4. **Steps per mask follow from the budget equation.** Narrower treads, lower r, and thicker resist all give more steps. Multi-layer steps multiply levels per mask almost for free.

5. **Tread edges are cumulative.** Width comes from one trim, and position comes from all of them.

6. **The trim works in two dimensions.** Side treads form along every resist edge, and inside corners become arcs whose radius equals the total pullback.

7. **The trim dominates sequence time.**

---

## Study Questions

1. Sketch the cross-section after three trims and four etches for a single-layer staircase. Label each surface with its level and give the number of times each tread has been exposed to a pair etch.

2. Using the budget equation, compute n_max for T₀ = 10 µm, T_min = 1.2 µm, r = 1.5, w = 0.50 µm, and δ_E = 0.015 µm. How many levels does one mask produce? How many masks are needed for 180 levels?

3. A process change lowers the trim ratio from 1.5 to 1.2 with no other change. Using the parameters of Question 2, how many masks are saved for the 180-level staircase?

4. An inside corner of a staircase opening is initially sharp. After seven trims of 0.55 µm each, what is the radius of the outermost tread arc? If contacts must stay at least 0.5 µm outside the arc zone, how far from the initial corner (along the edge) must the first contact sit?

5. Resist sidewall angle is 78° and resist erosion per etch is 18 nm. How far does the edge of the outermost tread recede over nine etches? Is this term systematic or random, and why?

6. In the reference cycle, the trim takes 90 s at 0.40 µm/min. If the trim rate were doubled with no loss of control, what fraction of total per-mask time would be saved? Why might a faster trim be harder to control?

---

**Previous Chapter:** [Chapter 2: The Alternating Stack — Materials, Deposition & Stress](./02-stack-materials.md)  
**Next Chapter:** [Chapter 4: Pair-Etch and Trim Chemistries](./04-etch-trim-chemistries.md)

---

**Chapter 3 Development Status:** Complete  
**Version:** 1.0
