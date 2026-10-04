# Chapter 13: Loading, Pattern Dependence & Uniformity

## Overview

A trim recipe is calibrated on one product, one wafer, one chamber. It then runs on wafers with different layouts, at different radii, after different idle times, on resist from different lots. Each of those differences can change the trim rate by a percent or more, and every percent becomes tread placement error that grows with tread index. This chapter catalogs the ways trim and pair-etch rates depend on where and when they run, and the methods used to compensate.

**Learning Objectives:**
- Apply a macroloading model to estimate how resist coverage changes trim rate
- Explain why pair-etch rates depend on exposed area and how that varies between masks
- Identify the sources of radial and extreme-edge trim non-uniformity
- Estimate resist-aspect-ratio effects on lateral trim in narrow openings
- Recognize wafer-to-wafer and lot-to-lot drivers, including resist lot and bake history
- Select compensation strategies for each type of variation

---

## 13.1 Macroloading of the Trim

### 13.1.1 The Model

Resist covers most of the wafer during a staircase mask, typically 80–95% of the area. Every square centimeter of it consumes O atoms. The O-atom density therefore depends on how much resist is present:

```
Balance on O atoms (well-mixed approximation):

  Generation G = losses (pumping + wall recombination + resist reaction)

  n_O = G / (k_pump + k_wall + k_resist · A_r)

  n_O = n_O,0 / (1 + κ · A_r)

  n_O,0  = O density with no resist load
  A_r    = resist area on the wafer
  κ      = k_resist / (k_pump + k_wall)

Trim rate R ∝ n_O, so:

  ΔR / R ≈ − [κ A_r / (1 + κ A_r)] · (ΔA_r / A_r)
```

### 13.1.2 Example

```
Measured: trim rate on a fully resist-coated wafer is 0.80 of the rate on
a wafer with 10% resist coverage (small load).

  1 / (1 + κ · 1.0) ÷ 1 / (1 + κ · 0.1) = 0.80
  (1 + 0.1κ) / (1 + κ) = 0.80 → κ ≈ 0.28 (per unit coverage fraction)

At A_r = 0.90:
  κ A_r / (1 + κ A_r) = 0.252 / 1.252 = 0.20

Product A: 90% resist coverage. Product B: 85% (more staircase and
  periphery openings).
  ΔA_r / A_r = −5.6%
  ΔR / R ≈ −0.20 × (−5.6%) = +1.1%

Product B trims 1.1% faster:
  6.6 nm per 0.60 µm tread; 53 nm at tread 8
```

A recipe tuned on one product is off by about one percent on another. **Each product needs its own trim calibration**, or a loading model in APC (Chapter 15).

### 13.1.3 Does Loading Change During a Sequence?

```
Resist area change per trim:
  Each staircase opening edge moves w = 0.6 µm. For a die with two
  staircase edges of ~5 mm each per plane and 4 planes:
    ΔA ≈ 2 × 4 × 5 mm × 0.6 µm = 0.024 mm² per die per trim
  Die area ~ 70 mm² → ΔA/A ≈ 0.03% per trim

Over 8 trims: ~0.3% change in resist area → ~0.06% trim-rate change
```

Within one mask, resist area hardly changes. Loading is a product-to-product and mask-to-mask effect, not a within-sequence effect.

### 13.1.4 Wafer-Edge Loading

Resist is removed from the outer 1–2 mm by edge-bead removal, and partial dies near the edge may be fully exposed or fully covered depending on the reticle layout. The edge region therefore has a different local resist density from the interior:

```
Effect: O atoms not consumed at the bare edge ring diffuse inward over
a distance of roughly a mean free path to a few cm (80 mTorr: λ ≈ 1 mm;
diffusion length over the O lifetime is several cm)
→ slightly higher O density near the wafer edge
→ edge trim faster by ~0.5–2% (illustrative)
```

---

## 13.2 Exposed-Area Loading in the Pair Etch

### 13.2.1 Exposed Area Varies by Mask

The pair etch acts on the area outside the resist. That area changes from mask to mask:

```
Mask       Exposed regions (illustrative)                  Exposed fraction
──────────────────────────────────────────────────────────────────────────
Mask 1     Staircase openings (all levels below start)     ~5%
Mask 5     Staircase openings + periphery stack removal    ~12%
             region (if opened in this mask)
Chop mask  Half the staircase area, by design              ~3%
```

### 13.2.2 Effect on Rate and Landing

```
Etch rate vs. exposed fraction (fluorocarbon, mildly loading):
  R(f) = R₀ / (1 + κ_E · f),  κ_E ≈ 1.5 (illustrative)

  f = 0.05: R = R₀ / 1.075
  f = 0.12: R = R₀ / 1.18   → ~9% slower than at f = 0.05
```

A 9% slower etch on one mask uses most of a 24% minimum overetch. Generous overetch (Chapter 11.4) absorbs this. But the per-mask step times must be checked whenever exposed area changes by more than a few percent. OES signal strength also scales with exposed area (Chapter 15).

---

## 13.3 Within-Wafer Trim Uniformity

### 13.3.1 Radial Sources

```
Source                       Typical signature           Main knob
──────────────────────────────────────────────────────────────────────────
O-atom generation profile    Mid-radius peak or          Coil current ratio /
  (ICP)                      center peak                 source zoning
Gas injection                Center- or edge-rich        Center/edge split
                                                         (Ch. 7.5)
Wafer temperature            Edge warm on bowed          ESC zones; He zones
                             wafers                      (Ch. 8)
Loading at the edge          Edge fast (bare ring)       ESC edge zone; gas
Wall/liner temperature       Edge-weighted               Liner temperature
  gradients
```

### 13.3.2 Target

```
Placement allowance for trim-related errors (Chapter 12): ~±43 nm at
tread 8, of which spatial (radial) ~±24 nm
  → radial trim uniformity ±0.5% at the end of a mask (w = 0.60 µm,
    8 trims: 4.8 µm × 0.5% = 24 nm)

That is ~±0.08 °C equivalent at 6.1%/°C.
```

### 13.3.3 Measuring the Profile

Radial trim profiles are measured on patterned monitor wafers (tread width by CD-SEM or optical metrology at many sites) or on blanket resist (vertical rate by ellipsometry, as a proxy). The proxy fails when the trim ratio itself varies radially. Lateral measurement is the reference (Chapter 15).

### 13.3.4 Extreme Edge (Outer 5–10 mm)

```
Effect at the extreme edge           Cause                        Size
─────────────────────────────────────────────────────────────────────────────
Trim faster                          Hot edge (poor He contact),  +1–4%
                                     bare-ring loading
Riser tilt                           Edge sheath bending in the   0.5–2° tilt
                                     pair etch (edge ring wear)
Resist thinner or thicker            Coat edge effects; edge bead Budget at edge
                                     removal profile
Etch rate faster or slower           Edge-ring height, gas        ±3–5%
                                     depletion
```

Edge die are a disproportionate share of staircase yield loss. The edge ring (height, material, temperature) and the ESC edge zone are the principal edge knobs. Edge ring wear changes the pair etch over the ring's life, so ring hours belong in the APC model.

---

## 13.4 Resist-Aspect-Ratio Effects in Narrow Openings

### 13.4.1 Where Narrow Openings Occur

Most staircase openings are wide (tens of µm). Some designs have narrow openings in thick resist: gaps between neighboring staircases, slots in split-cell designs (Chapter 14), and small openings for test structures.

### 13.4.2 Effect on Lateral Trim

In a slot of width W in resist of thickness T, the lower sidewall sees less of the plasma. Because O atoms react with only ~1% probability per collision, they bounce many times and fill the slot fairly evenly, so the depletion is modest:

```
Lateral trim at the base of a slot relative to an open edge
(illustrative, reaction probability ~0.01):

Resist aspect ratio T/W     Base trim rate (relative)
──────────────────────────────────────────────────
0.25 (8 µm resist, 32 µm slot)   ~1.00
0.5                              ~0.99
1.0                              ~0.97–0.98
2.0                              ~0.93–0.96
```

### 13.4.3 Why This Changes Through a Sequence

The resist thins and the slot widens with every trim, so the aspect ratio falls through the mask:

```
Slot initially W = 4 µm, T = 8 µm (AR = 2.0)
After 4 trims: W = 4 + 2 × 4 × 0.6 = 8.8 µm, T ≈ 4.8 µm (AR ≈ 0.55)

Early trims in the slot run ~5% slow, later ones ~1% slow.
Tread widths inside the slot grow through the mask, unlike treads on
open edges.
```

Layout rules that set a minimum opening width in staircase masks (for example W ≥ 2T) avoid this effect.

---

## 13.5 Wafer-to-Wafer and Lot-to-Lot Variation

```
Driver                              Typical effect on trim rate     Detection
─────────────────────────────────────────────────────────────────────────────────
Wall inventory and idle time        ±0.5–2% (first wafers)          FDC on OES;
  (Ch. 9)                                                           first-wafer
                                                                    metrology
Chamber aging (window, edge ring,   Slow drift, ~0.1%/100 RF h      Monitor trend
  liner)
Resist lot (formulation             ±1–3% between lots              Incoming lot
  tolerance)                                                        qualification
Resist bake history (hotplate       ±0.5–2% per ±2 °C bake          Track hotplate
  temperature, post-bake delay)     temperature                     logs
Resist thickness (coat)             Small for lateral rate;         Coat metrology
                                    budget effect only
Incoming bow class                  Edge temperature → edge trim    Bow measurement
                                                                    feed-forward
```

**Resist bake temperature deserves special attention.** Hotter bakes cross-link and densify the resist, which lowers its trim rate. A track hotplate that drifts 2 °C can shift tread positions as much as a 0.2 °C error in the etch chamber. Staircase control therefore extends to the lithography track.

---

## 13.6 Compensation Strategies

```
Variation type                Compensation
─────────────────────────────────────────────────────────────────────────────
Product-to-product loading    Per-product trim time, or loading term in the
                              APC model using resist coverage from design data
Mask-to-mask etch loading     Per-mask etch step times; generous overetch
Radial profile                Gas split, ESC zones, coil ratio; edge ring
                              tuning
Extreme edge                  ESC edge zone, edge ring height/temperature,
                              He zoning, bow-class recipes
Narrow openings               Layout rule (minimum width); per-region design
                              bias
Chamber-to-chamber            Per-chamber trim offsets from monitors
Wafer-to-wafer (wall)         WAC + conditioning; first-wafer offsets
Lot-to-lot (resist, bake)     Resist lot qualification; track hotplate control;
                              feed-forward from track data
Within-mask drift             Per-level trim time table
```

The strategies operate at different time scales. Hardware tuning fixes the radial profile once per chamber state. APC handles slow drifts and product differences. Per-level tables handle predictable within-sequence effects. Chapter 15 brings these together.

---

## 13.7 Summary & Key Takeaways

1. **Resist loads the trim.** Different resist coverage between products changes trim rate by about a percent.

2. **Loading is a between-product and between-mask effect.** Resist area barely changes within a mask sequence.

3. **Pair-etch rate depends on exposed area.** Masks with more open area etch slower. Overetch absorbs it, and step times should track it.

4. **Radial trim uniformity must reach about ±0.5%.** That corresponds to less than a tenth of a degree of wafer temperature.

5. **The extreme edge needs its own control.** Hot edges, bare-ring loading, and edge-ring wear all act there.

6. **Narrow resist openings trim slowly at first.** The effect fades as the resist thins and the opening widens.

7. **The track is part of the trim.** Resist lot and bake temperature change trim rate as much as chamber variables do.

---

## Study Questions

1. A chamber's trim rate on a 95% resist-coverage wafer is 0.75 of that on a 10% coverage wafer. Compute κ. What rate change results from moving a product from 92% to 88% coverage?

2. Mask 3 exposes 6% of the wafer and mask 4 exposes 14%. Using R(f) = R₀/(1 + 1.5f), compute the etch-rate change. If the oxide step has 30% overetch at mask 3, how much margin remains at mask 4?

3. Radial tread width at the edge is 1.2% wider than at the center. What cumulative placement offset results at tread 9 with w = 0.55 µm? What ESC edge-zone temperature change would correct it at 6%/°C?

4. A split-cell layout has 6 µm slots in 9 µm resist. Estimate the aspect ratio at the start and after 3 trims of 0.6 µm (with r = 1.3). Describe how tread widths in the slot will differ from those on open edges.

5. A new resist lot trims 2% slower. If it is not detected, what is the placement error at tread 8 for w = 0.60 µm? Propose an incoming-qualification test that would catch it in under an hour.

---

**Previous Chapter:** [Chapter 12: Resist Budget & Cumulative Placement Error](./12-resist-budget-error.md)  
**Next Chapter:** [Chapter 14: Advanced Staircases — Chop Masks, Split Cells & Multi-Deck](./14-advanced-staircase.md)

---

**Chapter 13 Development Status:** Complete  
**Version:** 1.0
