# Chapter 10: Step Profile Control — Edge Taper, Footing & Corner Rounding

## Overview

A tread is useful only where it is flat and clean. A riser that leans, a foot of unetched material at its base, a rounded or faceted top edge, or a residue left on a freshly exposed strip all take away landing area, and some of them add risk on top of the lost area. In plan view, the tread turns into a curve wherever the resist boundary had a corner. This chapter covers the shape of the staircase in cross-section and in plan: what sets it, how it changes over a mask sequence, and how to keep the usable tread close to the drawn tread.

**Learning Objectives:**
- Define usable tread width and compute it from riser angle, foot, and top-edge loss
- Explain how resist edge profile transfers into the riser
- Describe how riser profiles degrade on treads exposed to many etches
- Identify sources of footing, micromasking, and residue on newly exposed strips
- Design corner exclusion zones from cumulative trim and corner blunting
- Choose recipe adjustments that improve profile without costing landing

---

## 10.1 Profile Metrics

```
Cross-section of one step (word-line direction →):

  tread k (upper)                      
  ─────────────────╮  ← top-edge rounding / facet (e_t)
                    ╲
                     ╲  riser, angle θ from horizontal,
                      ╲ height p (or m·p for multi-layer steps)
                       ╲__
                          ‾‾‾╲___ ← foot (e_f)
                                  ────────────────────  tread k+1 (lower)
  ◄─────────── w (drawn tread) ───────────►

Metric                Symbol    Definition
────────────────────────────────────────────────────────────────────────
Drawn tread width     w         Distance between successive resist edges
Riser angle           θ         Riser slope from horizontal
Riser run             p·cot θ   Horizontal extent of the riser
Foot extent           e_f       Horizontal extent of unetched material at
                                the riser base
Top-edge loss         e_t       Horizontal extent of rounding or facet at
                                the riser top
Usable tread width    w_u       Flat, clean width available for landing
```

### 10.1.1 Usable Tread Width

```
w_u = w − p · cot θ − e_f − e_t

Reference (single-layer step):
  w = 600 nm, p = 55 nm, θ = 80°, e_f = 10 nm, e_t = 5 nm
  p · cot θ = 55 × 0.176 = 9.7 nm
  w_u = 600 − 9.7 − 10 − 5 = 575 nm  (96% of drawn)

Multi-layer step (m = 4, riser 220 nm), θ = 75°:
  p · cot θ = 220 × 0.268 = 59 nm
  w_u = 600 − 59 − 15 − 8 = 518 nm  (86% of drawn)
```

For single-layer steps, profile costs only a few percent of the tread. For multi-layer steps, the taper term grows with riser height and becomes a real design constraint (Chapter 14).

---

## 10.2 Resist Edge Profile and Its Transfer

### 10.2.1 The Resist Edge

```
Thick resist sidewall (KrF or i-line, 5–12 µm):
  Sidewall angle α:   80–88° as developed
  Base:               may show footing (substrate reflection, acid
                      quenching) or undercut
  Top:                rounded after hard bake; rounded further by trim
```

### 10.2.2 How the Trim Reshapes the Edge

An isotropic trim moves every exposed surface inward along its normal by about the same distance. That preserves the resist sidewall angle while moving it back, with three departures:

```
Departure                          Effect on the edge
────────────────────────────────────────────────────────────────────────
Top corner sees more plasma        Top rounds more each cycle; harmless
  (higher O flux)                  to the base until resist is thin
Base corner sees less plasma       Slightly slower lateral trim within
  (floor blocks part of view)      ~0.1–0.3 µm of the floor → small resist
                                   foot at the base
Crust on sidewall (from etch)      Delay that is not uniform in height;
                                   the base, which saw fewest ions, may
                                   clear first or last depending on
                                   polymer distribution
```

### 10.2.3 Transfer Into the Riser

The pair etch copies the resist base, not the resist top, into the riser. A resist foot of extent e_r produces a riser whose top edge is shifted outward by e_r and whose slope depends on how the foot erodes during the etch:

```
Resist foot e_r = 30 nm, height ~50 nm, eroded during the pair etch:
  If the foot erodes fully during the etch: riser is tapered over ~30 nm
  If it survives: riser top edge shifts outward by 30 nm (tread k wider,
    tread k+1 narrower by the same amount)
```

A consistent resist foot is a systematic offset and is calibrated out. A variable foot (from crust variation or temperature) becomes tread-width variation.

---

## 10.3 Riser Degradation Over Many Etches

### 10.3.1 Every Riser Is Etched Repeatedly

Each pair etch lowers both treads on either side of an existing riser, so the riser is translated downward one pair per etch. In an ideal anisotropic etch, the riser shape is preserved exactly. In a real etch, the base corner of the riser etches slightly slower than the open tread:

```
Causes of slower etching at the riser base:
  Polymer accumulation in the concave corner
  Partial shadowing of ions arriving off-normal
  Redeposition of sputtered or low-volatility products from the riser
```

### 10.3.2 Foot Accumulation

If each etch leaves a small additional foot δ_f at every existing riser, the outer risers, which are etched many times, carry more foot than the inner ones:

```
The riser at edge x_i (the resist edge after trim i; x₀ is the original
edge) is formed by etch i + 1 and then etched again by etches i + 2
through n + 1, which is n − i further etches.

  e_f(i) ≈ e_f,0 + (n − i) · δ_f

  e_f,0 = foot left by the etch that formed the riser

Example: n = 8, e_f,0 = 5 nm, δ_f = 2 nm
  Innermost riser (x₈):  e_f = 5 + 0 × 2 = 5 nm
  Outermost riser (x₀):  e_f = 5 + 8 × 2 = 21 nm
```

The trim does not remove these feet. They are made of oxide and nitride, which oxygen does not etch. Only the pair etch can clear them.

### 10.3.3 Controlling Foot Growth

```
Lever                                   Mechanism                     Cost
──────────────────────────────────────────────────────────────────────────────
Slight lateral component in the         O₂-richer nitride step clears  Lateral resist
  nitride step (more O₂, higher T)      corner polymer                 loss rises
                                                                       (Ch. 4.3.3)
Lower polymer in oxide step             Less corner accumulation       Lower Ox:SiN
                                                                       selectivity
Narrow IED, normal-incidence ions       Less shadowing of the corner   Hardware
Short "foot-clean" step at the end      Removes corner polymer each    Time; tread
  of each etch (low-polymer chemistry,  cycle                          oxide loss
  low bias, 2–3 s)
```

The choice is a trade between profile on outer treads and landing selectivity on all treads (Chapter 11).

---

## 10.4 Residue and Micromasking on Newly Exposed Strips

### 10.4.1 What Lies Under the Retreating Resist Edge

When the trim pulls the resist edge back, it exposes a strip of tread that was under the resist base. That surface should be clean oxide. It may not be:

```
Possible residue                      Origin
──────────────────────────────────────────────────────────────────────
Resist foot remnant                   Slow trim at the base corner
Fluorinated crust fragments           Crust on the lower sidewall that
                                      breaks up rather than ashing
                                      smoothly
CFₓ polymer that deposited at the     Etch polymer in the concave corner
  resist base during the etch         between resist and tread
Adhesion promoter / BARC remnants     Lithography (first strip of a mask)
```

### 10.4.2 Consequence

Any residue on the newly exposed strip delays or blocks the next pair etch there:

```
Residue type          Effect on the strip
─────────────────────────────────────────────────────────────────────────
Continuous thin film  Etch starts late on the whole strip → strip under-
                      etched (lands short) unless overetch covers it
Islands (micro-       Unetched pillars and roughness ("grass") on the
  masking)            strip; pillars carried down by later etches
```

Micromasking on a strip is especially damaging because the islands it leaves are carried down by every later etch in the mask, as with particles (Chapter 9.8).

### 10.4.3 Prevention

```
1. Crust breakthrough before the timed trim (Ch. 4.6.4)
2. Trim end with a short over-trim at slightly higher O₂/N₂ to clear
   the base corner
3. Keep base-corner polymer low in the etch (Section 10.3.3)
4. First-strip descum after lithography (O₂ short step) before the
   first etch of each mask
```

---

## 10.5 Corner Rounding and Layout

### 10.5.1 Corner Zones

From Chapter 3.6, isotropic trim makes every inside corner of the opening into an arc whose radius equals the cumulative pullback, and blunts outside corners of the resist block by flux enhancement:

```
Inside-corner arc radius after i trims:   R_i = Σ_{j=1}^{i} w_j + R_litho

Outside-corner blunting after i trims:    b_i ≈ (κ − 1) · Σ_{j=1}^{i} w_j
                                          κ = corner/edge trim rate ratio
```

### 10.5.2 Exclusion Zone

Contacts cannot land on a curved tread at a predictable position, so layout reserves a corner exclusion zone:

```
Exclusion zone (along the edge from the original corner):

  L_excl ≈ R_n + d_c/2 + margin

Reference: R₈ = 8 × 0.60 + 0.3 (litho corner) = 5.1 µm
           d_c = 0.20 µm, margin = 0.5 µm
  L_excl ≈ 5.1 + 0.1 + 0.5 = 5.7 µm from the corner on each side
```

### 10.5.3 Side Treads

Along the edges of the opening parallel to the word lines, the trim forms side treads (Chapter 3.6.1). In a simple staircase, they are dead area:

```
Side-tread width per mask = n · w = 8 × 0.60 = 4.8 µm
With 16 masks of single-layer steps, the opening grows by
  16 × 4.8 = 77 µm in the y-direction at each side

Layout either:
  - places the opening's y-edges outside the active blocks (area cost),
  - or uses the side treads as landing area in split-cell designs
    (Chapter 14).
```

### 10.5.4 Corner Rounding in Plan, Seen From Above

```
Plan view of an inside corner after 4 trims (schematic):

  ████████████████████████████████
  ██████████████████████████  ·  ·      ← side treads (y-direction)
  ████████████████████████  ·  ·  ·
  ███████████████████████ ╭─╮─╮─╮─╮     ← concentric arcs at the
  ██████████████████████ │ │ │ │ │        corner, radius growing
  ██████████████████████ │ │ │ │ │        outward
  ██████████████████████ │ │ │ │ │      ← main treads (x-direction)
```

---

## 10.6 Tread Surface Quality

```
Property              Target                  Threats
──────────────────────────────────────────────────────────────────────────
Flatness              No pillars or pits      Micromasking, particles
Roughness             < 1–2 nm RMS            Polymer islands, ion texturing
Tread oxide           Uniform, ≥ spec         Overetch variation, wall F
  remaining                                   (Ch. 9, 11)
Contamination         No C, F residue         Polymer, crust, incomplete
                                              strip (Ch. 4.7)
```

Rough or contaminated treads cause poor adhesion and voids in the fill dielectric and can change the local word-line contact etch (Chapter 16).

---

## 10.7 Profile Recipes in Practice

```
Problem                          First knob                  Watch for
──────────────────────────────────────────────────────────────────────────────────
Riser too tapered (all steps)    Reduce polymer in nitride   Lower SiN:ox selectivity
                                 step (O₂ up slightly); bias   → more tread-oxide loss
                                 voltage up 10–20 eV
Feet grow on outer treads        Foot-clean step; narrow     Tread-oxide budget
                                 IED
Micromasking on new strips       Crust breakthrough; over-   Resist budget
                                 trim at end of trim
Top-edge facet                   Lower ion energy; less Ar   Rate
Corner arcs too large            Narrow w or fewer trims     Masks (Ch. 12, 14)
                                 per mask
```

---

## 10.8 Summary & Key Takeaways

1. **Usable tread is less than drawn tread.** Riser run, foot, and top-edge loss subtract from it. The losses are small for single-layer steps and significant for multi-layer steps.

2. **The resist base sets the riser.** A resist foot shifts or tapers the riser. A consistent foot is calibrated out; a variable one is tread-width noise.

3. **Outer risers degrade.** Every riser is etched again in every later etch, and feet accumulate on the outermost treads. The trim cannot remove them.

4. **New strips must be clean.** Residue under the retreating resist edge delays or micromasks the next etch, and the damage is carried down.

5. **Corners become arcs.** Inside corners round with radius equal to the cumulative trim, and layout needs exclusion zones several microns long.

6. **Side treads are dead area unless the scheme uses them.**

---

## Study Questions

1. Compute w_u for w = 500 nm, p = 58 nm, θ = 78°, e_f = 12 nm, and e_t = 6 nm. Repeat for a two-pair step (riser 116 nm) at θ = 76°.

2. A mask has n = 10 trims. Feet start at 4 nm and grow 1.5 nm per etch. What is the foot on the outermost riser? If contacts require 25 nm clearance from any foot, how much usable tread is lost on the outermost tread compared with the innermost?

3. A resist foot of 25 nm survives the pair etch on half the wafer and erodes completely on the other half. Describe the resulting tread-width pattern and explain how you would detect it.

4. Compute the inside-corner arc radius and the exclusion zone length after 9 trims of 0.55 µm, with a 0.25 µm lithographic corner radius, d_c = 0.18 µm, and 0.4 µm margin.

5. A process change halves base-corner polymer in the oxide step but lowers Ox:SiN selectivity from 12 to 9. Discuss the trade-off for outer-tread foot growth and tread-oxide loss. What measurements would you use to decide?

6. Explain why micromasking on a newly exposed strip is more damaging in a staircase than micromasking in a single blanket etch.

---

**Previous Chapter:** [Chapter 9: Chamber Conditioning & Wall Memory](./09-chamber-conditioning.md)  
**Next Chapter:** [Chapter 11: Layer Landing, Selectivity & Step-Height Accuracy](./11-layer-landing-selectivity.md)

---

**Chapter 10 Development Status:** Complete  
**Version:** 1.0
