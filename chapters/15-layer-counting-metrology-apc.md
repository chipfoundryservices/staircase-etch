# Chapter 15: Layer Counting, Metrology & Advanced Process Control

## Overview

A staircase can fail in two ways: it can miscount, or it can misplace. A miscount is binary and catastrophic, so it must be caught during the etch or immediately after, before more value is added to the wafer. Misplacement is continuous and accumulates, so it must be measured and fed back so that the next wafer's trims land where the contacts will be printed. This chapter covers both: the optical emission signals that reveal each layer clearing, the limits of counting layers in deep etches, fault detection for missed steps, the metrology of tread width, height, and position, and the APC loops that hold the trim on target.

**Learning Objectives:**
- Identify the OES signatures of oxide clearing, nitride clearing, and resist trim
- Estimate endpoint signal-to-noise from exposed area
- Explain why OES layer counting fails in deep etches and how selective landings restore it
- Design fault detection that catches missed and doubled steps
- Select metrology for tread width, placement, step height, tread oxide, and level count
- Build feedback and feed-forward APC for trim time and estimate its performance

---

## 15.1 OES Signatures

### 15.1.1 Emission Lines

```
Species   Wavelength (nm)     Behavior in staircase steps
──────────────────────────────────────────────────────────────────────────────
CO        483.5, 519.8        Oxide etch: high while oxide etches (O from
                              film + carbon from polymer); falls at clearing
                              Trim: strong (resist oxidation product)
CN        388.3               Nitride exposed in fluorocarbon or HFC plasma:
                              rises when nitride is reached; falls when
                              nitride clears
N₂        337.1               Rises with nitride etching
SiF       440.0               Both films; small change at interfaces
F         703.7               Rises when the etched film clears (less
                              consumption); during trim, marks fluorine
                              release from walls and crust
H         656.3               HFC chemistry; trim (resist H)
OH        309                 Trim (resist oxidation)
O         777.4, 844.6        Trim: rises as resist consumption falls
Ar        750.4, 811.5        Reference (actinometry) lines
```

### 15.1.2 A Cycle's Trace

```
Normalized intensity vs. time through one cycle (schematic):

        oxide step        nitride step              trim
       ├──────────────┤ ├──────────────┤ ├─────────────────────────────┤
CN/Ar   ▁▁▁▁▁▁▁▁▁╱‾‾‾‾  ‾‾‾‾‾‾‾‾‾‾╲▁▁▁▁    (not used)
                 ↑ oxide clears        ↑ nitride clears
CO/Ar   ‾‾‾‾‾‾‾‾‾╲▁▁▁▁  (low)               ╱‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
                                          ↑ crust broken, bulk resist
F/Ar                                     ╱╲▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁
                                          wall/crust F release
```

Each etch should show **one rise and one fall** of CN: the oxide-to-nitride transition in the oxide step and the nitride-to-oxide transition in the nitride step. Each trim should show the CO rise that marks the start of bulk resist removal.

---

## 15.2 Signal Strength and Open Area

### 15.2.1 Scaling

The pair etch acts only on exposed area, typically 3–15% of the wafer. The emission change at clearing scales with it:

```
ΔI / I ≈ f_open · χ

  f_open  = exposed fraction of the wafer
  χ       = relative change in emission for a fully exposed wafer
            (e.g., CN rise when a blanket oxide clears to nitride)

Example: χ = 0.40, f_open = 0.05
  ΔI / I = 2%

Noise: 0.2% per sample at 10 Hz; averaged over 1 s → 0.2/√10 = 0.063%

SNR ≈ 2% / 0.063% ≈ 32
```

### 15.2.2 Implications

```
Case                          f_open    ΔI/I     SNR (1 s)
────────────────────────────────────────────────────────────
Early trim–etch mask          0.04      1.6%     25
Late mask (more exposed)      0.10      4.0%     63
Y-chop (half of one zone)     0.02      0.8%     13
Narrow test-only opening      0.005     0.2%     3 (unusable)
```

Clearing transitions are detectable in most steps but weak in chop masks with small open areas. Multivariate methods (principal components across many wavelengths, ratio of CN to Ar and CO to Ar) gain a factor of 2–4 in effective SNR.

---

## 15.3 Layer Counting in Deep Etches

### 15.3.1 Counting Oscillations

During a non-selective deep etch (multi-layer step or chop), the exposed surface alternates between oxide and nitride. The CN signal oscillates with one period per pair. Counting periods counts pairs.

### 15.3.2 Why Counting Fades

Across the wafer, the etch front is not at the same depth everywhere. Regions at different depths are at different phases of the oscillation, and the wafer-averaged signal loses amplitude:

```
Oscillation amplitude with depth spread σ_D (Gaussian) and pitch p:

  A / A₀ = exp(−2π² σ_D² / p²)

p = 55 nm:
  σ_D (nm)    A/A₀
  ──────────────────
   5          0.85
  10          0.52
  15          0.23
  20          0.07
  30          0.003

Depth spread grows with depth: σ_D ≈ σ_R · D (rate non-uniformity σ_R)

  σ_R = 1.5%:  σ_D = 15 nm at D ≈ 1.0 µm (~18 pairs)
```

By about 15–20 pairs into a non-selective etch with typical uniformity, the oscillation is washed out and counting fails. Deep chops of 36–72 pairs cannot be counted end to end.

### 15.3.3 Resynchronizing With Selective Landings

A selective landing step stops every region at the same interface and erases the depth spread, the same self-correction described in Chapter 11.2:

```
Deep chop of 72 pairs as nine sub-steps of 8 pairs:
  Each sub-step: non-selective main etch (~7.5 pairs), then selective
  oxide and nitride landing steps
  Depth spread at the end of each main etch: 1.5% × 440 nm ≈ 6.6 nm (1σ)
    → oscillation amplitude stays ≥ 0.6 → each sub-step can be counted
  Selective landing resets σ_D to ~1–2 nm before the next sub-step

Total count is the sum of nine verified sub-counts.
```

The cost is time and recipe complexity. The benefit is a chop etch whose layer count is verified at every sub-step.

---

## 15.4 Fault Detection for Missed and Doubled Steps

### 15.4.1 Per-Step Checks

```
Step      Required evidence                     Fault if
──────────────────────────────────────────────────────────────────────────────
Oxide     CN rise within expected time window   No rise (missed: oxide not
          (t_clear,expected ± tolerance)        cleared or plasma fault)
                                                Rise far too early (thin layer
                                                or previous etch overshoot)
Nitride   CN fall within expected window        No fall (nitride not cleared)
Trim      CO rise (bulk resist) after           No rise (plasma fault, resist
          induction; CO integral within         missing); integral off
          limits                                (trim rate anomaly)
All       RF power, reflected power, bias       Excursion during the step
          voltage, pressure within limits
```

### 15.4.2 Sequence-Level Checks

```
1. Count of confirmed etches = number of etches in the recipe level table
2. Each trim preceded by a confirmed etch and followed by a confirmed
   etch (except the last)
3. On abort and resume, the resume point matches the last confirmed step
   (Chapter 11.5.1)
4. Any failed check → wafer held; no further processing until reviewed
```

### 15.4.3 Detection Power

A missed oxide clearing is the most important fault to catch. Its evidence (no CN rise) is present only if SNR is adequate:

```
Detection threshold set at 5σ of noise:
  SNR 25 → threshold at 20% of expected rise → reliable detection
  SNR 13 (Y-chop) → threshold at 40% of expected rise → acceptable
  SNR 3 → cannot distinguish a missed clear from a normal one
```

Chop steps with very small open area may need dedicated, larger exposed regions (e.g., in scribe lines) to make the clearing signal reliable.

---

## 15.5 Metrology

### 15.5.1 What to Measure

```
Quantity                    Method                           Notes
──────────────────────────────────────────────────────────────────────────────────
Tread width and edge        Top-down CD-SEM; optical image   Edge contrast from
  positions                 metrology                        topography and
                                                             material; measure all
                                                             treads of a mask
Placement relative to a     Optical overlay-type metrology   Ties staircase edges
  reference                 on dedicated marks; CD-SEM with  to the frame the
                            reference features               contact mask uses
Step height / level count   AFM or stylus profilometer on a  Each step must be one
                            wide-tread test staircase        pair (or m pairs);
                                                             catches missed or
                                                             doubled steps
Tread oxide remaining       Spectroscopic ellipsometry on    Needs treads wider than
                            wide test treads                 the optical spot
                                                             (~30–50 µm)
Riser profile, foot,        Cross-section SEM / TEM          Destructive; for
  landing interface                                          qualification and
                                                             excursions
Radial trim profile         Many-site tread metrology on     Feeds ESC/gas tuning
                            monitor wafers
Level correctness           Electrical test after contacts   Final confirmation;
  (electrical)              and metal: per-level contact     too late for the
                            resistance and leakage           wafer itself
```

### 15.5.2 Test Structures

A product staircase has treads too narrow for ellipsometry or profilometry. Scribe-line or dedicated test staircases with wide treads (tens of µm) are formed by the same masks:

```
Test staircase uses:
  Step-height and level-count verification (profilometer, AFM)
  Tread-oxide thickness per level (ellipsometry)
  OES clearing signal enhancement (larger open area)

Caution: wide-tread test structures sit in a different loading and
aspect-ratio environment (Chapter 13); their trim rate may differ from
the product staircase by a known offset.
```

### 15.5.3 Sampling

```
Measurement                          Frequency (illustrative)
───────────────────────────────────────────────────────────────────
Tread width / placement, all treads  Every lot, 1–2 wafers, 5–13 sites
  of the last mask
Level count (test staircase)         Every lot, 1 wafer
Tread oxide                          Daily per chamber, and on excursions
Radial profile (many sites)          Weekly per chamber, after PM
Cross-section                        Qualification, excursions
```

Integrated metrology on the etch platform (Chapter 5.6.3) can measure tread positions on every wafer, which shortens the APC loop to one wafer.

---

## 15.6 APC for the Trim

### 15.6.1 The Model

```
Tread width:   w = R̂ · (t_trim − t̂_ind) + λ̂

Controller:    t_trim = (w_target − λ̂) / R̂ + t̂_ind

R̂, t̂_ind, λ̂ are estimates per chamber, product, and level.
```

### 15.6.2 Feedback: EWMA on Trim Rate

```
After each measured wafer:
  R_meas = (w_meas − λ̂) / (t_trim − t̂_ind)
  R̂_new = α · R_meas + (1 − α) · R̂_old

Example: α = 0.3, w_target = 600 nm, λ̂ = 20 nm, t̂_ind = 4 s,
  R̂_old = 6.67 nm/s → t_trim = (600 − 20)/6.67 + 4 = 91.0 s

  Measured w = 609 nm (chamber drifted 1.6% fast)
  R_meas = (609 − 20) / (91.0 − 4) = 6.77 nm/s
  R̂_new = 0.3 × 6.77 + 0.7 × 6.67 = 6.70 nm/s
  Next t_trim = 580 / 6.70 + 4 = 90.6 s
```

EWMA filters measurement noise but lags a step change. With α = 0.3, it removes about 65% of a step change after three measured wafers. Because placement error is linear in the trim-rate error, the lag shows up directly as placement error on the next few wafers.

### 15.6.3 Feed-Forward Terms

```
Input                          Adjustment                      Source
──────────────────────────────────────────────────────────────────────────────
Product (resist coverage)      Loading factor on R̂ (Ch. 13.1)  Design data
Resist lot                     Lot offset on R̂                 Incoming qualification
Track bake temperature         Bake offset on R̂                Track logs
Chamber idle time / first      t̂_ind and R̂ offsets for first   Tool state
  wafer                        trims (Ch. 9.5)
Edge ring hours                Edge-zone offset                Tool counters
Incoming bow                   Edge temperature offset          Bow metrology
Level (first trim of mask,     Per-level trim-time table        Characterization
  thicker layers before trim)
```

### 15.6.4 Controlling Placement, Not Width

The contact mask cares about tread positions. The controller can target the cumulative sum rather than individual widths:

```
Objective: minimize Σ_k (x_k − x_k,target)² over treads of a mask
→ equivalent to targeting the mean trim rate with weight on the later trims

Per-level corrections (e.g., first trim slow) are then chosen so that
the running sum stays on target, not each width individually.
```

### 15.6.5 Radial Control Loop

A slower loop adjusts ESC zone offsets (or gas split) from radial tread-width maps:

```
Zone offset update (per zone j):
  ΔT_j = −g · (w_j − w̄) / (w̄ · S_T)

  S_T = 0.061 per °C, g = 0.5 (damped gain)

Example: outer zone treads 0.8% wide
  ΔT_outer = −0.5 × 0.008 / 0.061 = −0.066 °C
```

---

## 15.7 APC for the Pair Etch

```
Variable                    Control approach
───────────────────────────────────────────────────────────────────────────
Oxide and nitride step      Endpoint-assisted: step time = max(fixed
  times (intermediate)      minimum, detected clearing + generous OE).
                            Overetch stays generous (Chapter 11.4)
Last etch of mask           Fixed or endpoint + limited OE; feedback from
                            tread-oxide measurement
Per-mask exposed area       Feed-forward from design data to set step
                            times (Chapter 13.2)
Selectivity drift           Monitor nitride and oxide loss on test treads;
                            adjust O₂ flow within a narrow window
Deep chop count             OES sub-step counting with selective
                            resynchronization (Section 15.3.3)
```

---

## 15.8 Summary & Key Takeaways

1. **OES shows every clearing.** CN rises and falls once per etch, and CO marks the start of bulk resist removal in each trim.

2. **Signal scales with open area.** Most steps give adequate SNR. Small-area chops may need larger dedicated openings.

3. **Layer counting fades with depth.** Depth spread across the wafer washes out the oscillation after 15–20 pairs. Selective landing steps resynchronize it.

4. **Fault detection must confirm every etch.** Missed clearings, skipped steps, and resume errors are caught by per-step evidence and sequence checks.

5. **Metrology needs test staircases.** Wide treads allow profilometry, level counting, and ellipsometry, with a known offset from the product staircase.

6. **APC targets placement.** EWMA feedback on trim rate plus feed-forward for product, resist, bake, tool state, and level keep the cumulative tread positions on target.

---

## Study Questions

1. A Y-chop exposes 1.5% of the wafer. With χ = 0.35 and noise of 0.25% per sample at 20 Hz, compute the SNR over 1 s. Is a missed clearing detectable at a 5σ threshold?

2. With σ_R = 1.2%, at what depth does the OES oscillation amplitude fall to 0.25 of its initial value for p = 58 nm? How many pairs is that?

3. Design a sub-step plan for a 48-pair chop with σ_R = 1.5%, requiring A/A₀ ≥ 0.5 at the end of each main etch. How many sub-steps are needed?

4. Using the EWMA example, simulate three more wafers if the chamber stays 1.6% fast. What is the tread-8 placement error on each wafer (w = 0.60 µm)?

5. The outer zone treads are 0.5% narrow and the middle zone 0.2% wide. Compute the zone temperature offsets with g = 0.5 and S_T = 0.06/°C. Why is a damped gain used?

6. A wafer's trace shows a normal CN rise in the oxide step of etch 6 but no CN fall in its nitride step. List the possible causes and the decision the tool should make.

---

**Previous Chapter:** [Chapter 14: Advanced Staircases — Chop Masks, Split Cells & Multi-Deck](./14-advanced-staircase.md)  
**Next Chapter:** [Chapter 16: Word-Line Contacts, Integration, Yield & Cost of Ownership](./16-contacts-integration-yield-coo.md)

---

**Chapter 15 Development Status:** Complete  
**Version:** 1.0
