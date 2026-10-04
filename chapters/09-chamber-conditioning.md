# Chapter 9: Chamber Conditioning & Wall Memory

## Overview

In most etch processes, the chamber wall settles into one condition and stays there. In staircase etch, the wall is pushed back and forth. Every pair etch coats it with fluorocarbon and hydrofluorocarbon polymer. Every trim attacks that polymer with oxygen atoms and releases fluorine back into the plasma. The wall becomes a reservoir that fills during the etch and empties during the trim. Each step inherits whatever the previous step left behind.

This chapter treats the wall as a process variable. It describes what the etch deposits and what the trim removes, shows how fluorine released from the wall speeds up the trim and etches exposed treads, builds a simple inventory model that predicts whether a sequence settles into a repeatable cycle or drifts, and covers waferless autoclean, seasoning, preventive-maintenance recovery, wall materials, and particles.

**Learning Objectives:**
- Describe the alternating wall states produced by pair etch and trim
- Estimate fluorine release from wall polymer during trim and its effect on trim rate and tread oxide
- Use an inventory model to predict first-cycle effects and within-sequence drift
- Explain why silicon-containing wall deposits accumulate and how waferless autoclean removes them
- Design seasoning and PM-recovery procedures for a staircase chamber
- Identify particle mechanisms specific to trim–etch sequences

---

## 9.1 Two Wall States

```
After a pair etch                           After a trim
─────────────────────────────────────────────────────────────────────────
CₓF_y / CₓH_yF_z polymer, ~0.5–3 nm         Polymer largely burned off
  per step on walls, ceiling, and           Surfaces oxidized; adsorbed O
  edge ring                                 SiOₓF_y remains (O cannot remove it)
Fluorinated surface                         Some F remains bound in the
Recombination probability for O: low        oxidized layer
  on polymer, higher on clean oxide         O recombination probability: set
                                            by the clean wall surface
```

The two states affect the plasma differently:

```
Wall state           Effect on the next step
──────────────────────────────────────────────────────────────────────────
Polymer-coated       Trim: O consumed by wall polymer; F released →
  (entering trim)    faster trim at first; tread oxide etched slightly
Oxidized/clean       Etch: O released from the wall; less polymer at the
  (entering etch)    start of the oxide step; F not scavenged by wall
                     polymer → slightly faster, less selective start
```

---

## 9.2 Fluorine Release During the Trim

### 9.2.1 Order-of-Magnitude Estimate

```
Wall polymer deposited per etch step: 2 nm over 0.5 m² of wall area
  Volume:  2 × 10⁻⁹ m × 0.5 m² = 1.0 × 10⁻⁹ m³ = 1.0 × 10⁻³ cm³
  Mass:    ~2 g/cm³ × 10⁻³ cm³ = 2 × 10⁻³ g
  F atoms (taking ~CF units, 31 g/mol):
           2 × 10⁻³ / 31 × 6.0 × 10²³ ≈ 3.9 × 10¹⁹ F

Released over ~10 s at the start of the trim:
  Release rate ≈ 3.9 × 10¹⁸ F/s

Steady-state F density (effective loss time τ_F ≈ 0.2 s, V = 40 L):
  n_F ≈ 3.9 × 10¹⁸ × 0.2 / 4 × 10⁴ cm³ ≈ 2 × 10¹³ cm⁻³

Compare with n_O ≈ 5 × 10¹⁴ cm⁻³ (Chapter 5.4.2):
  F/O ≈ 4% during the release period
```

### 9.2.2 Effect on the Trim

A few percent of fluorine is exactly the range that boosts ashing rate in conventional resist strip (Chapter 4.5.3). The trim therefore runs fast while the wall is releasing fluorine:

```
Illustrative: trim rate enhanced ~20% during the first 10 s

  Extra lateral trim ≈ 0.40 µm/min × 0.20 × 10/60 min ≈ 13 nm per tread
```

If the release is the same in every cycle, this is a constant offset that the trim time absorbs. If it changes from cycle to cycle, tread width changes with it.

### 9.2.3 Effect on the Treads

Fluorine atoms also etch exposed oxide treads and nitride risers. Spontaneous F etching of SiO₂ is slow, but low-energy ions in the trim assist it:

```
F flux at n_F = 2 × 10¹³ cm⁻³ (v̄_F ≈ 580 m/s):
  Γ_F = n_F · v̄ / 4 ≈ 2 × 10¹³ × 5.8 × 10⁴ / 4 ≈ 2.9 × 10¹⁷ cm⁻²s⁻¹

Ion-assisted reaction probability for SiO₂ ~10⁻³ (illustrative),
4 F per Si removed, SiO₂ Si density 2.3 × 10²² cm⁻³:
  Etch rate ≈ 2.9 × 10¹⁷ × 10⁻³ / 4 / 2.3 × 10²² ≈ 3 × 10⁻⁹ cm/s
            ≈ 0.03 nm/s → ~0.3 nm per 10 s release period

The field and outermost treads see every trim of the mask:
  8 trims × 0.3 nm ≈ 2.4 nm of tread-oxide loss from wall fluorine
```

The tread-oxide budget (Chapter 11) must include this term. Nitride risers etch faster than oxide in F, so riser nitride recesses slightly at each exposed riser.

---

## 9.3 The Wall Inventory Model

### 9.3.1 Balance Equations

Let m_d be the polymer deposited on the wall per etch step and f the fraction of wall polymer removed by one trim. Write M_k for the wall inventory just after etch k:

```
M_k = (1 − f) · M_(k−1) + m_d         (after etch k)

Polymer burned (and F released) in trim k:
  B_k = f · M_k

Starting from a clean wall (M₀ = 0):
  M_k = (m_d / f) · [1 − (1 − f)^k]
  B_k = m_d · [1 − (1 − f)^k]

Steady state (k → ∞):
  M* = m_d / f,   B* = m_d
```

### 9.3.2 Fast vs. Slow Removal

```
Release in trim k relative to steady state, B_k / B*:

Trim k    f = 0.9     f = 0.6     f = 0.3
──────────────────────────────────────────
1         0.90        0.60        0.30
2         0.99        0.84        0.51
3         1.00        0.94        0.66
4         1.00        0.97        0.76
6         1.00        1.00        0.88
8         1.00        1.00        0.94
```

With hot, well-cleaned walls (f near 1), the wall reaches steady state after one or two cycles. With cool or shadowed wall regions (f ≈ 0.3), fluorine release keeps rising for most of a mask, and the trim gets slightly faster every cycle:

```
Example: steady-state enhancement 13 nm per tread (Section 9.2.2)
  f = 0.3: trim 1 gets 0.30 × 13 = 3.9 nm extra; trim 8 gets 12.2 nm extra
  Tread width drifts by ~8 nm across the mask
  Cumulative placement at tread 8 is off by Σ(B_k/B* − 1) × 13 nm
    ≈ (−0.70 − 0.49 − 0.34 − 0.24 − 0.17 − 0.12 − 0.08 − 0.06) × 13
    ≈ −2.2 × 13 ≈ −29 nm relative to a steady-state calibration
```

### 9.3.3 What Raises f

```
Lever                              Mechanism
──────────────────────────────────────────────────────────────────────
Heated liner and ceiling           Polymer burns faster on hot surfaces;
  (60–120 °C)                      less polymer deposits in the first place
Fewer cold, shadowed surfaces      Pump port, gate valve, and viewport
                                   areas hold polymer longest
Higher O flux to walls during      Trim conditions that also clean walls
  trim
Lower polymer deposition in etch   Less to remove (but selectivity depends
                                   on polymer; balance carefully)
```

### 9.3.4 Wafer-to-Wafer Memory

Between wafers, the chamber is idle or runs a waferless autoclean (WAC). If wall polymer remaining after the last trim of a wafer is not fully removed, the next wafer starts with a different wall state. The inventory model applies across wafers as well as across cycles. Without a WAC, the first wafer of a lot and the wafers that follow one after another see different first-cycle behavior.

---

## 9.4 Silicon-Containing Deposits

### 9.4.1 Origin

The pair etch produces SiF₄ (and SiFₓ fragments) from both oxide and nitride. When these meet oxygen during transitions and trims, they form SiOₓF_y deposits on the walls:

```
SiF₄ + O (plasma) → SiOₓF_y (wall deposit) + F

Properties:
  Glass-like, not removed by O₂ plasma
  Grows wafer after wafer
  Flakes under thermal cycling → particles
  Changes O recombination probability on the wall → trim drift
```

### 9.4.2 Removal

Silicon-containing deposits require fluorine chemistry:

```
WAC step                  Chemistry (illustrative)      Removes
──────────────────────────────────────────────────────────────────────
Fluorine WAC              NF₃/O₂ or SF₆/O₂              SiOₓF_y
Oxygen WAC                O₂ (± N₂)                     Organic polymer
Final conditioning        O₂ or short etch-gas season   Sets the starting
                                                         wall state
```

A WAC that leaves the wall fluorinated changes the first trim of the next wafer. The final conditioning step decides what the next wafer sees.

---

## 9.5 First-Cycle and First-Wafer Effects

```
Effect                          Cause                           Typical size
───────────────────────────────────────────────────────────────────────────────
First trim of a mask slower     Clean wall after WAC → less F   Few nm narrower
  or faster than the rest       released (if f small) or        first tread; can be
                                different O recombination       either sign
First etch of a wafer less      O-rich wall from WAC → less     Slightly more stop-
  selective                     polymer at start                layer loss on the
                                                                field
First wafer after idle          Cooler liner and chuck surface  Trim and etch rates
                                (Ch. 8.5.4), wall desorption    shift 1–3%
First lots after PM             Fresh wall surfaces, new parts  Trim-rate drift over
                                                                tens of wafers
```

**Mitigation**:

```
1. WAC after every wafer, ending with a reproducible conditioning step
2. Per-level trim-time offsets for the first trim of each mask
   (Chapter 15 APC)
3. Seasoning wafers after idle beyond a set time
4. Heated walls (raising f) to shorten the transient
```

---

## 9.6 Seasoning and PM Recovery

### 9.6.1 Recovery Sequence

```
After wet clean / part replacement:

1. Pump-down, leak check, bake-out (liner at temperature)
2. Long WAC (fluorine + oxygen steps)
3. Seasoning: run the full trim–etch sequence on resist-coated
   dummy wafers (typically 10–25 wafers)
4. Monitor wafers:
   ☐ Blanket resist trim rate (vertical) and uniformity
   ☐ Patterned lateral trim rate (tread width)
   ☐ Oxide and nitride etch rates and selectivity
   ☐ Particles
5. Release when trim rate is within ±0.5% of the pre-PM baseline
   and stable over 3 consecutive monitors
```

### 9.6.2 Why Seasoning Takes So Long

Fresh walls have high O recombination probability and adsorb polymer and fluorine readily. The trim runs slower until wall surfaces reach their working state. Seasoning curves typically show:

```
Seasoning wafer #     Lateral trim rate relative to baseline
───────────────────────────────────────────────────────────
1                     −4.0%
3                     −2.2%
6                     −1.0%
10                    −0.4%
15                    −0.1%
```

The tread positions on the first post-PM product wafers depend on the last few percent of this curve, which is why release criteria are tight.

---

## 9.7 Wall Materials

```
Material                Fluorocarbon      O plasma         Notes
                        resistance        resistance
──────────────────────────────────────────────────────────────────────────────
Anodized Al             Moderate          Good             AlF₃ formation →
                                                           particles; avoided
                                                           near the wafer
Y₂O₃ (sprayed or        Good              Good             Standard; forms YOF
  dense coatings)                                          surface in F plasma
YOF / YF₃ coatings      Very good         Good             Fewer first-wafer
                                                           effects (already
                                                           fluorinated)
Quartz / ceramic        Erodes in F       Good             Window material;
  windows                                                  erosion changes ICP
                                                           coupling over time
SiC, Si (edge ring)     Erodes            Good             Wear changes edge
                                                           sheath (Ch. 13)
```

Wall coatings that are already fluorinated (YOF) change less when alternating between fluorocarbon and oxygen plasmas, which shortens first-cycle transients.

---

## 9.8 Particles in Trim–Etch Sequences

### 9.8.1 Sources

```
Source                               When
─────────────────────────────────────────────────────────────────────
SiOₓF_y flakes                       Thermal cycling, idle, WAC gaps
Polymer flakes                       Thick deposits on cold surfaces
Resist-derived particles             Resist edge bead, flakes from
                                     wafer edge during long trims
Plasma-extinction drop               Plasma-off transitions (Ch. 7.3)
Edge-ring and window erosion         Long-term
```

### 9.8.2 Why a Particle Matters More in a Staircase

A particle that lands on a tread during cycle k masks the etch below it:

```
Organic particle (burned off by the next trim):
  Leaves a one-pair-high island on that tread. The island is carried
  down by later etches as a bump.

Inorganic particle (not removed by O plasma):
  Masks every remaining etch of the mask. Leaves a pillar several
  pairs tall standing on the tread.
```

A pillar under a word-line contact makes the contact land on a higher layer than intended, a short or misconnection that behaves like a local miscount. A pillar elsewhere on the tread may be harmless. Because a particle can arrive at any cycle, and every cycle exposes the outer treads, **staircase defect density scales with the number of cycles**. Particle control is a yield requirement (Chapter 16).

---

## 9.9 Summary & Key Takeaways

1. **The wall is a reservoir.** It fills with polymer during each etch and empties during each trim.

2. **Wall fluorine speeds up the trim and etches treads.** A few percent F during the release period changes tread width by nanometers and takes oxide from exposed treads.

3. **The inventory model predicts drift.** If the trim removes most wall polymer each cycle, the sequence repeats. If not, fluorine release grows through the mask, and tread width drifts.

4. **Hot walls shorten wall memory.** Heated liners and fewer cold surfaces raise the removal fraction.

5. **Silicon-containing deposits need fluorine WAC.** Oxygen alone cannot remove SiOₓF_y.

6. **Seasoning and PM recovery are long and tight.** Tread positions depend on the last fraction of a percent of trim rate recovery.

7. **Particles leave pillars.** Defect density scales with cycle count.

---

## Study Questions

1. Wall polymer deposition is 1.5 nm per etch over 0.6 m². Estimate the number of F atoms released per trim and the steady-state F density, using the assumptions of Section 9.2.1.

2. With f = 0.45, compute B_k/B* for k = 1 to 8. If the steady-state wall enhancement adds 10 nm per tread, compute the cumulative placement error at tread 8 relative to a steady-state calibration.

3. A process engineer raises the liner temperature from 60 °C to 100 °C and f rises from 0.5 to 0.85. By how much does the tread-8 placement error from Question 2's method change?

4. Explain why an oxygen-only WAC between wafers can make the chamber worse over time. What deposit accumulates, and what symptoms would you expect after several hundred wafers?

5. A seasoning curve follows R/R₀ = 1 − 0.045 · exp(−n/4), where n is the seasoning wafer number. How many wafers are needed to reach −0.5%? What tread-placement error at tread 8 would the first product wafer have if released at −1.0%?

---

**Previous Chapter:** [Chapter 8: Wafer Temperature, Bow & Electrostatic Chuck Design](./08-wafer-temperature-esc.md)  
**Next Chapter:** [Chapter 10: Step Profile Control — Edge Taper, Footing & Corner Rounding](./10-step-profile-control.md)

---

**Chapter 9 Development Status:** Complete  
**Version:** 1.0
