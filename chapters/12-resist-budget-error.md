# Chapter 12: Resist Budget & Cumulative Placement Error

## Overview

Two limits decide how many steps one staircase mask can make. The first is material: the resist gets thinner with every trim, and at some point it can no longer mask an etch. The second is geometric: every trim adds its error to the position of every tread that follows, and at some point the last tread drifts too far from where the word-line contact will be printed. A good staircase process pushes both limits outward together. More resist budget is wasted if placement error runs out first, and better placement control is wasted if the resist runs out first.

This chapter refines the resist budget of Chapter 3 with real-world terms, separates tread-width error into its systematic, drifting, and random parts, shows how those parts accumulate into placement error, and treats the stitch between masks, where the errors of two lithography steps meet in one tread.

**Learning Objectives:**
- Build a complete resist budget including coat variation, crust breakthrough, and worst-case trim ratio
- Compare resist types and approaches for lowering the trim ratio
- Decompose tread-width error into systematic, drift, and random components
- Compute cumulative placement error at any tread and identify the dominant terms
- Analyze the stitch tread between masks and size it
- Determine whether a mask is resist-limited or placement-limited

---

## 12.1 The Full Resist Budget

### 12.1.1 Terms

Chapter 3 used a simple budget: T₀ − n·r·w − (n+1)·δ_E ≥ T_min. Production budgets include more terms, and they must hold at the **worst location on the wafer**:

```
Term                                   Reference value      Note
──────────────────────────────────────────────────────────────────────────────
Nominal coat T₀                        8.00 µm
Coat non-uniformity (thinnest point)   −0.15 µm             ~2% range
Topography (coat over earlier          −0.10 µm             Thinner over raised
  staircase masks)                                          regions near the edge
Pre-etch descum                        −0.05 µm
Pair-etch loss, 9 × δ_E                −0.13 µm             δ_E = 14 nm
Crust breakthrough, 8 × 20 nm          −0.16 µm
Trim loss, 8 × r_max · w               −6.53 µm             r_max = 1.36 (center
                                                            r = 1.30, +4.6% at
                                                            worst location)
Lateral loss in nitride steps          (counted in w)
──────────────────────────────────────────────────────────────────────────────
Remaining at worst location            0.88 µm
Required T_min                         1.00 µm              ✗ short by 0.12 µm
```

### 12.1.2 What the Detailed Budget Shows

The simple budget gave 1.63 µm remaining. The detailed one gives 0.88 µm at the worst location. Eight trims no longer fit. The options are:

```
Option                                   Effect
───────────────────────────────────────────────────────────────────────
Drop to 7 trims per mask                 Remaining: 1.74 µm ✓
                                         Costs a mask every ~8 masks
Raise T₀ to 8.3 µm                       Remaining 1.18 µm ✓; litho impact
Improve r uniformity (r_max 1.36 → 1.31) Saves 0.24 µm → 1.12 µm ✓
Reduce crust breakthrough to 10 nm       Saves 0.08 µm
```

Improving trim-ratio uniformity across the wafer is often the cheapest fix, because the budget is set by the worst point, not the mean.

### 12.1.3 What Sets T_min

```
Concern at low remaining thickness        Typical threshold
──────────────────────────────────────────────────────────────────
Pinholes / thin spots in resist           < 0.5 µm
Resist top rounding reaching the base     Depends on edge profile;
  (edge recedes faster)                   ~0.5–1.0 µm for thick resist
Faceting from etch reaching the base      Multi-layer and chop etches
Endpoint / metrology margin               Process policy
```

---

## 12.2 Lowering the Trim Ratio: Resist and Process Choices

```
Approach                              Effect on r              Trade-off
──────────────────────────────────────────────────────────────────────────────────
Lower trim ion flux (pulsing,         1.3 → 1.1–1.2            Slower trim
  higher pressure, remote source;
  Ch. 6.5)
Top-surface hardening (UV cure,       Top induction delay      Adds a step; lateral
  ion-hardened crust kept in place)   → effective r lower      crust variability
Resist chemistry (higher carbon,      Lower vertical and       Litho performance;
  more aromatic content)              lateral rates alike;     slower trim
                                      r nearly unchanged
Capping layer on resist top (thin     r → near 0 while cap     Cap overhang must
  low-temperature film; research)     lasts                    break off cleanly;
                                                               particles
Thicker resist                        r unchanged; more        Litho limits
                                      budget
```

```
Example: r from 1.30 to 1.15 by pulsed trim, reference budget (Ch. 3):
  r · w + δ_E = 0.69 + 0.014 = 0.704 µm per cycle
  n_max = ⌊6.986 / 0.704⌋ = 9 trims → 10 levels per mask (was 9)
  For 136 levels: ⌈136 / 10⌉ = 14 masks (was 16)
```

---

## 12.3 Tread-Width Error

### 12.3.1 The Tread-Width Equation

```
w_i = R_L,i · (t_trim − t_ind,i) + λ_etch,i + Δ_edge,i

  R_L,i       lateral trim rate in cycle i (temperature, O flux,
              wall F, loading)
  t_ind,i     induction time (crust)
  λ_etch,i    lateral resist loss during the pair etch (O₂ in the
              nitride step, Ch. 4.3.3)
  Δ_edge,i    resist edge-profile change (foot formation, Ch. 10.2)
```

### 12.3.2 Error Components

```
Component              Behavior over cycles       Examples (reference)
──────────────────────────────────────────────────────────────────────────────
Systematic bias β      Same every cycle, every    Trim-time calibration error;
                       wafer (until recalibrated) ESC offset; MFC offset
Within-mask drift      Changes smoothly through   Wall inventory build-up
                       the mask                   (Ch. 9.3); thermal
                                                  transients; first-trim
                                                  effects
Wafer-to-wafer drift   Changes between wafers     Wall condition, chamber
                                                  aging, idle effects
Random σ_w             Independent each cycle     Induction-time jitter, RF
                                                  and flow noise, local
                                                  temperature noise
Spatial (within wafer) Fixed map, same sign       Radial trim profile, edge
                       every cycle                temperature, loading
```

### 12.3.3 Representative Magnitudes

```
Source                                     Per-tread error (nm)
──────────────────────────────────────────────────────────────────────
Induction-time jitter (±0.3 s, 1σ)         2.0 (random)
Wafer temperature noise (0.03 °C, 1σ)      1.1 (random)
Nitride-step lateral loss (1σ)             0.8 (random)
──────────────────────────────────────────────────────────────────────
Random total σ_w (RSS)                     2.4

Residual calibration bias after APC        0.3% → 1.8 (systematic)
Radial within-wafer residual               ±0.5% → ±3.0 (spatial)
Chamber-to-chamber residual                ±0.5% → ±3.0 (systematic per
                                           chamber)
```

Every tread individually is within a few nanometers of target. The trouble is accumulation.

---

## 12.4 Cumulative Placement Error

### 12.4.1 Accumulation Rules

The edge of tread k (the resist edge after trim k) is displaced by the sum of all width errors so far:

```
ε_k = Σ_{i=1}^{k} e_i

Random part:       σ_rand,k = √k · σ_w
Systematic part:   ε_sys,k  = k · β         (linear in k)
Drift part:        ε_drift,k = Σ d_i          (depends on drift shape)
Spatial part:      ε_sp,k(r) = k · s(r)       (linear in k at each point)
```

### 12.4.2 Reference Example at Tread 8

```
Term                          Per trim      At k = 8          3σ or max
─────────────────────────────────────────────────────────────────────────
Random (σ_w = 2.4 nm)         2.4 (1σ)      6.8 nm (1σ)       20 nm (3σ)
Residual bias (0.3%)          1.8           14 nm             14 nm
Radial residual (±0.5%)       3.0           24 nm             24 nm
Chamber matching (±0.5%)      3.0           24 nm             24 nm
Wall-inventory drift          —             ~10 nm            10 nm
  (f ≈ 0.7, Ch. 9.3)
─────────────────────────────────────────────────────────────────────────
Trim-related total (RSS)                                      ~43 nm
Overlay: staircase mask                                       30 nm
  to contact mask (3σ)
─────────────────────────────────────────────────────────────────────────
Total placement error at tread 8 (RSS)                        ~52 nm
Allowance (Chapter 1.4)                                       ±60 nm
```

The random part is the smallest term. **Systematic and spatial terms that are linear in k dominate.** Placement control is therefore mostly a calibration and uniformity problem, not a noise problem. That is why APC on trim time, ESC zone tuning, and chamber matching get so much attention (Chapters 8, 13, 15).

### 12.4.3 Why the Contact Mask Cannot Fix It

The scanner printing word-line contacts can correct translations, rotations, and field magnification. A trim-rate error, though, scales only the staircase:

```
Tread edge positions with a fractional trim error ε:
  x_k = x₀ − k · w · (1 + ε)

This is a scaling of the staircase about x₀ (the mask's starting edge),
not of the whole exposure field. The array next to the staircase has
no such error. A field magnification correction would fix one and
break the other.
```

Some fabs use per-region contact placement (design-level offsets by tread index, fed back from metrology), but the cleanest solution is to keep ε small at the source.

---

## 12.5 The Stitch Between Masks

### 12.5.1 The Stitch Tread

When mask m + 1 continues the staircase, its innermost tread lies against the outermost edge of mask m. The width of that tread is set by two lithography steps and a full mask of trims:

```
Stitch tread width:

  w_stitch = (x₀^(m+1) − Σ_{i=1}^{n} w_i^(m+1)) − x₀^(m)

  x₀^(m)       starting edge of mask m (lithography m)
  x₀^(m+1)     starting edge of mask m+1 (lithography m+1), drawn at
               x₀^(m) + (n + 1) · w
  Σ w^(m+1)    cumulative trim of mask m+1
```

### 12.5.2 Stitch Error

```
σ²_stitch = σ²_OL(m) + σ²_OL(m+1) + n · σ_w² + (trim systematic terms)²

Reference (3σ values):
  Overlay of each staircase mask:      30 nm each
  Cumulative trim, mask m+1 (n = 8):   ~43 nm (from Section 12.4.2,
                                       excluding the contact overlay term)

  Stitch error (3σ) ≈ √(30² + 30² + 43²) ≈ 60 nm
```

The stitch tread carries the largest width error in the staircase. The usual design responses:

```
1. Draw stitch treads wider than regular treads (e.g., w + 0.1 µm)
2. Align each staircase mask to the same reference (not to the previous
   staircase mask) so overlay errors do not chain
3. Place alignment marks that survive staircase processing (marks etched
   into the stack in an early mask)
```

### 12.5.3 Placement Errors Reset at Each Mask

The good news is that each new mask resets cumulative trim error. Tread positions within mask m + 1 are measured from x₀^(m+1), which lithography places fresh. **Fewer trims per mask means smaller worst-case placement error**, the reverse of the resist-budget incentive.

---

## 12.6 Resist-Limited or Placement-Limited?

### 12.6.1 Placement Limit

```
Placement error at the last tread of a mask with n trims:

  E(n) = √[(a · n)² + 9 · n · σ_w² + OL²]   (3σ-equivalent)

  a  = RSS of linear-in-k terms per trim (bias, radial, matching, drift)
  OL = staircase-to-contact overlay (3σ)

Reference: a = √(1.8² + 3.0² + 3.0² + 1.2²) ≈ 4.8 nm per trim
           σ_w = 2.4 nm, OL = 30 nm

  n = 8:   E = √(38.4² + 415 + 900) = √(1475 + 415 + 900) ≈ 53 nm
  n = 10:  E = √(48² + 518 + 900) = √(2304 + 518 + 900) ≈ 61 nm
  n = 12:  E = √(57.6² + 622 + 900) = √(3318 + 622 + 900) ≈ 70 nm

Placement allowance ±60 nm → n_max,placement ≈ 9–10
```

### 12.6.2 Comparison

```
Limit                       n_max (reference)
────────────────────────────────────────────────
Resist budget (detailed)    7
Placement (±60 nm)          9–10
```

The reference process is **resist-limited**. Improving trim-ratio uniformity or resist thickness gains steps until about n = 9, after which placement control must improve as well. A process that lowers r dramatically (for example with a remote-source trim) becomes placement-limited, and its next gain comes from uniformity and APC.

---

## 12.7 Summary & Key Takeaways

1. **Resist budgets are set at the worst point.** Coat variation, topography, crust breakthrough, and trim-ratio non-uniformity together can cost a whole cycle.

2. **Tread-width error has many parts.** Random noise is small. Systematic, drifting, and spatial errors are linear in tread index and dominate placement.

3. **Placement error accumulates; width error does not.** The last tread of a mask carries the sum of every trim's error.

4. **The contact mask cannot correct staircase scaling.** Trim errors must be fixed at the source.

5. **The stitch tread is the worst tread.** It carries two overlay errors and a full mask of trim error, so it is usually drawn wider.

6. **Know which limit binds.** More resist budget helps only until placement runs out, and vice versa.

---

## Study Questions

1. Recompute the detailed budget of Section 12.1.1 for 7 trims and 8 etches. What is the remaining resist at the worst location?

2. A process has σ_w = 3.0 nm, a residual bias of 0.4% per trim, radial residual of ±0.7%, and chamber matching of ±0.3% at w = 0.55 µm. Compute the placement error at tread 10 (3σ, RSS), including 25 nm of overlay.

3. For the stitch tread, compute the 3σ width error with staircase overlay of 25 nm per mask (aligned to a common reference), n = 9, and the trim terms of Question 2. How much wider should the stitch tread be drawn to keep the same contact margin as a regular tread with ±45 nm error?

4. A wall-inventory drift makes trims 1–3 progressively 2.0, 1.2, and 0.6 nm narrower than steady state, with later trims at steady state. What is the drift contribution to placement at tread 8? Is it better corrected by per-level trim times or by improving walls?

5. Show that for n trims with only linear terms, E(n) = a·n, and with only random terms, E(n) = 3·√n·σ_w. For a = 4 nm and σ_w = 2.5 nm, at what n do the two contributions become equal?

6. A remote-source trim lowers r to 1.05, making the resist budget allow 12 trims per mask. Using the reference placement model, what limits the mask now? What improvements in a would be needed to use all 12 trims?

---

**Previous Chapter:** [Chapter 11: Layer Landing, Selectivity & Step-Height Accuracy](./11-layer-landing-selectivity.md)  
**Next Chapter:** [Chapter 13: Loading, Pattern Dependence & Uniformity](./13-loading-uniformity.md)

---

**Chapter 12 Development Status:** Complete  
**Version:** 1.0
