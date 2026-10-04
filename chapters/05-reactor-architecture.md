# Chapter 5: Reactor Architecture for Trim–Etch Sequences

## Overview

Most etch reactors are designed around one regime. A dielectric etcher is tuned for directional ions and fluorocarbon polymer, and an asher is tuned for high oxygen-atom flux with no ion damage. A staircase reactor has to be both, alternating between them every minute or two, for twenty minutes or more per wafer, while holding the lateral trim to a few nanometers and landing every etch on the right layer.

This chapter describes how reactors are built for this job: the choice between in-situ and split-chamber trim–etch, the plasma sources that serve both regimes, the flux and residence-time numbers that set the trim and etch, the throughput of long sequences, and the platform and matching requirements of a high-volume staircase module.

**Learning Objectives:**
- Compare in-situ and split-chamber trim–etch architectures
- Explain why decoupled ICP sources dominate staircase etch and where CCP and remote sources fit
- Estimate ion flux, O-atom flux, and residence time for representative conditions
- Build a throughput model for a trim–etch sequence and size a platform fleet
- List the hardware features that make fast, reproducible regime switching possible

---

## 5.1 What the Reactor Must Do

```
Requirement                          Etch regime          Trim regime
──────────────────────────────────────────────────────────────────────────────
Ion energy at wafer                  100–400 eV           As low as possible
                                     (controlled)         (< 15–20 eV)
Dominant reactive species            Ions + CFₓ, CHₓFᵧ    O atoms
Pressure                             10–40 mTorr          50–300 mTorr
Total flow                           200–500 sccm         500–2000 sccm
Uniformity target                    Etch rate ±3%        Lateral trim ±1–2%
Wafer temperature                    Stable to ±1 °C      Stable to ±0.2 °C
                                                          (Ch. 8)
Switching                            Etch ↔ trim in seconds, dozens of times
                                     per wafer, reproducibly
Wall state                           Tolerant of alternating fluorocarbon and
                                     oxygen exposure (Ch. 9)
Wafer handling                       Bowed wafers (Ch. 2, 8)
```

The trim requirements are the harder ones. An asher that varies ±3% in rate is a good asher. A trim that varies ±3% puts every tread 18 nm off its target width, and the error accumulates.

---

## 5.2 In-Situ vs. Split-Chamber Trim–Etch

### 5.2.1 The Two Architectures

```
In-situ:        One chamber does etch and trim. The wafer stays on the
                chuck for the whole mask sequence.

Split-chamber:  Etch in an etch chamber, trim in a dedicated trim
                (strip-type) chamber. The wafer moves between them
                every cycle through the vacuum transfer module.
```

### 5.2.2 Comparison

```
Aspect                      In-situ                       Split-chamber
──────────────────────────────────────────────────────────────────────────────
Transfer overhead           None within sequence          ~40–60 s per transfer
                                                          × 2 per cycle
Wall memory                 Strong (fluorocarbon ↔ O)     Weak; each chamber
                                                          stays in one regime
Trim source                 Must serve both regimes       Optimized for trim
                                                          (downstream, zero
                                                          ion energy)
Wafer temperature           Same chuck; thermal history   Different chucks;
                            couples steps                 matching two chucks
Chamber count per platform  All chambers identical        Etch/trim ratio to
                                                          balance
Typical use                 Dominant in production        Early generations;
                                                          special cases
```

### 5.2.3 Why In-Situ Won

```
Example: 9 etches + 8 trims per mask, 50 s per transfer

Split-chamber transfer overhead:
  16 transfers × 50 s = 800 s ≈ 13 min per mask

In-situ transition overhead:
  16 transitions × ~8 s = 128 s ≈ 2 min per mask
```

The transfer penalty alone nearly doubles sequence time. In-situ trim–etch became standard once reactors could switch regimes quickly and reproducibly and once wall memory was understood well enough to manage (Chapter 9).

---

## 5.3 Plasma Sources

### 5.3.1 ICP With Independent Bias

Inductively coupled plasma (ICP) with a separate RF bias on the chuck is the dominant staircase source:

```
Feature                         Why it matters for staircase
──────────────────────────────────────────────────────────────────────────
Source power sets density       High O-atom production for trim
Bias power sets ion energy      Moderate, controlled energy for etch;
                                zero bias for trim
Low plasma potential at zero    Ion energy in trim ~10–20 eV, below
  bias                          most sputter and crust thresholds
Wide pressure range             5–300 mTorr covers both regimes
Multi-zone coil or gas          Radial tuning of O flux for trim
  injection                     uniformity (Ch. 7)
```

### 5.3.2 Capacitively Coupled Plasma

A dual-frequency CCP can run the etch well, but it is less suited to the trim. With capacitive coupling, even the "source" frequency produces a self-bias at the wafer. The trim then sees ion energies of tens of eV, which raises the trim ratio and consumes more resist per tread:

```
Trim in CCP vs. ICP (illustrative, same lateral rate):

                         ICP (zero bias)     CCP (HF source only)
  Ion energy at wafer    ~15 eV              ~40–80 eV
  Trim ratio r           ~1.2–1.4            ~1.6–2.2
  Steps per mask         8 (reference)       5–6
```

### 5.3.3 Remote (Downstream) Source for Trim

Adding a remote plasma source that feeds O atoms into the chamber gives the trim nearly zero ion energy and a trim ratio close to 1:

```
Remote O₂/N₂ source (illustrative):
  Lateral trim rate        ~0.15–0.30 µm/min (lower than ICP)
  Trim ratio r             ~1.0–1.15
  Tread oxide loss         Negligible
```

The lower rate costs time, but the lower trim ratio adds steps per mask. Some reactors combine an ICP for etch with a remote source or a low-power ICP mode for trim.

---

## 5.4 Flux and Residence-Time Numbers

### 5.4.1 Ion Flux in the Etch

```
Bohm flux:  Γ_i ≈ h · n₀ · u_B,    u_B = √(k_B T_e / M_i)

Example: n₀ = 1 × 10¹¹ cm⁻³, T_e = 3 eV, Ar⁺ (M = 40 amu), h ≈ 0.5
  u_B = √(3 × 1.6 × 10⁻¹⁹ / 6.63 × 10⁻²⁶) = 2.69 × 10³ m/s
  Γ_i = 0.5 × 10¹⁷ m⁻³ × 2.69 × 10³ m/s
      = 1.35 × 10²⁰ m⁻² s⁻¹ = 1.35 × 10¹⁶ cm⁻² s⁻¹
```

### 5.4.2 O-Atom Flux in the Trim

```
Thermal flux:  Γ_O = n_O · v̄ / 4,    v̄ = √(8 k_B T / π m_O)

Example: 80 mTorr O₂-rich gas at 300 K, 10% dissociation
  Total density n = p / k_B T = 10.7 Pa / (1.38 × 10⁻²³ × 300)
                  = 2.6 × 10²¹ m⁻³
  n_O ≈ 2 × 0.10 × 2.6 × 10²¹ ≈ 5 × 10²⁰ m⁻³
  v̄ = √(8 × 1.38 × 10⁻²³ × 300 / (π × 2.66 × 10⁻²⁶)) ≈ 630 m/s
  Γ_O = 5 × 10²⁰ × 630 / 4 ≈ 7.9 × 10²² m⁻² s⁻¹
      ≈ 7.9 × 10¹⁸ cm⁻² s⁻¹
```

### 5.4.3 What That Flux Does

```
Resist (PHS-type, ρ ≈ 1.1 g/cm³) carbon density ≈ 4.4 × 10²² C/cm³

Lateral trim at 0.40 µm/min = 6.7 × 10⁻⁷ cm/s
  Carbon removal flux = 6.7 × 10⁻⁷ × 4.4 × 10²² = 2.9 × 10¹⁶ C/cm²·s
  O consumed ≈ 2.5 O per C (CO, CO₂, H₂O) → ~7 × 10¹⁶ O/cm²·s

Effective reaction probability ≈ 7 × 10¹⁶ / 7.9 × 10¹⁸ ≈ 0.01
```

A reaction probability of ~1% means an O atom strikes a resist surface many times before reacting. That has two consequences. Near the feature, the O flux is close to isotropic, which keeps the trim ratio near 1 when ions are absent. Across the wafer, O atoms consumed by resist are a small fraction of those arriving, but the total resist area is large, so **resist loading of O atoms is real** (Chapter 13).

### 5.4.4 Residence Time

```
τ = p · V / Q

Chamber volume V = 40 L

Trim:  p = 80 mTorr, Q = 880 sccm (= 11.1 Torr·L/s)
       τ = 0.08 × 40 / 11.1 = 0.29 s

Etch:  p = 20 mTorr, Q = 327 sccm (= 4.1 Torr·L/s)
       τ = 0.02 × 40 / 4.1 = 0.20 s

(1 sccm = 0.01267 Torr·L/s)
```

Residence times are short, so gas composition in the chamber can change in about a second once the new gases arrive. In practice, switching time is set by gas-line volumes, valve timing, and pressure-control settling (Chapter 7), not by residence time.

---

## 5.5 Throughput

### 5.5.1 Per-Mask Chamber Time

```
From the reference sequence (Chapter 3.8):
  Process time per mask:                1165 s
  Wafer overhead (transfer in/out,
    chuck, dechuck, pump, stabilize):   90 s
  In-situ final strip (if used):        120 s
  ─────────────────────────────────────────────
  Chamber time per mask pass:           1375 s ≈ 22.9 min

  Chamber throughput: 60 / 22.9 ≈ 2.6 wafer-passes per hour
```

### 5.5.2 Fleet Sizing

```
Fab: 50,000 wafer starts per month (WSPM)
Staircase scheme: 4 trim–etch mask passes per wafer (multi-layer steps,
  split cell; Chapter 14)
Platform: 6 chambers
Available hours: 720 h/month × 85% availability = 612 h

Demand:  50,000 × 4 = 200,000 passes/month
         200,000 / 612 = 327 passes/hour

Platform capacity: 6 × 2.6 = 15.6 passes/hour

Platforms needed:  327 / 15.6 ≈ 21
```

Staircase etch is one of the larger tool-count items in a 3D NAND fab. Every second removed from the cycle and every mask removed from the scheme saves platforms. With 16 single-layer masks instead of 4, the same fab would need ~84 platforms, which is why nobody builds single-layer staircases at high layer counts.

### 5.5.3 Where the Time Goes

```
Component             Share of chamber time (reference)
───────────────────────────────────────────────────────
Trims (incl. transitions)            ~62%
Pair etches (incl. transitions)      ~22%
Strip                                ~9%
Wafer overhead                       ~7%
Crust breakthrough                   ~1%
```

The trim dominates. Raising trim rate shortens the sequence, but every percent of trim-rate non-uniformity at a higher rate costs the same tread-width error in less time. Trim rate is usually set by the control budget, not by the source's capability.

---

## 5.6 Platform Configuration and Chamber Matching

### 5.6.1 Why Matching Is Hard

Lithography prints all contacts at fixed positions, regardless of which chamber made the staircase. Tread positions must therefore match chamber to chamber as tightly as they must match within one chamber:

```
Matching target (illustrative):
  Lateral trim rate:   ±0.5% chamber to chamber
  → at 8 trims × 0.60 µm, ±24 nm cumulative position at the last tread

Contributors to chamber mismatch:
  ESC temperature calibration (±0.1 °C → ±0.6% trim rate)
  Source power delivered (match losses, coil condition)
  Wall condition and liner temperature
  Gas flow calibration (O₂ MFC ±0.5% of setpoint)
  Window or ceiling condition (affects ICP coupling)
```

### 5.6.2 Matching Strategy

```
1. Hardware matching:   ESC temperature calibration with instrumented
                        wafers; RF power calibration; MFC verification
2. Rate matching:       Blanket resist trim-rate monitors on each chamber
                        (lateral rate measured on patterned monitors)
3. Offset correction:   Per-chamber trim time offsets in APC (Chapter 15)
4. Fingerprint control: Radial trim profile matched with gas and
                        temperature zone tuning
```

### 5.6.3 Platform Layout

```
Typical staircase platform:
  4–6 identical in-situ trim–etch chambers
  Optional dedicated strip chamber (remove remaining resist after
    the last etch, freeing etch chambers for the next wafer)
  Optional integrated metrology station (tread width and step height,
    Chapter 15)
```

---

## 5.7 Hardware Features for Fast, Clean Switching

```
Feature                                Purpose
──────────────────────────────────────────────────────────────────────────────
Gas valves close to the chamber        Short dead volume → fast gas exchange
High-conductance pumping, fast         Quick pressure change between 20 and
  throttle valve                       80+ mTorr
RF match with presets or frequency     Plasma impedance changes greatly between
  tuning                               Ar-rich etch and O₂-rich trim; preset
                                       positions avoid slow tuning and
                                       reflected-power spikes
Plasma-on transition capability        Avoid relighting the plasma every step
                                       (Ch. 7)
Multi-zone ESC (radial zones)          Trim-rate profile control (Ch. 8)
Heated liner and ceiling (60–120 °C)   Less polymer on walls → weaker wall
                                       memory (Ch. 9)
Y₂O₃ or YOF wall coatings              Resist both fluorocarbon and O
                                       plasma; low particles
Fast OES (≥ 10 Hz)                     Layer counting, crust breakthrough
                                       detection (Ch. 15)
Edge ring with height or temperature   Extreme-edge trim and etch control
  adjustment
```

---

## 5.8 Summary & Key Takeaways

1. **The trim sets the hardware requirements.** Lateral trim needs ±1–2% uniformity and very low ion energy. The etch is comparatively forgiving.

2. **In-situ trim–etch dominates.** Split-chamber operation avoids wall memory but nearly doubles sequence time through transfers.

3. **ICP with independent bias is the standard source.** It gives high O-atom density at near-zero ion energy for trim and controlled ion energy for etch. CCP raises the trim ratio. Remote sources lower it at the cost of rate.

4. **O-atom reaction probability on resist is low.** O flux near the feature is nearly isotropic, but resist area still loads the plasma.

5. **Staircase is a tool-count driver.** A 50k WSPM fab needs on the order of 20 platforms with an efficient scheme, and several times that with a naive one.

6. **Chamber matching is a placement requirement.** Contacts are printed without regard to chamber, so tread positions must match chamber to chamber.

---

## Study Questions

1. A split-chamber scheme needs 45 s per transfer. For a mask with 11 etches and 10 trims, compute the transfer overhead and compare it with an in-situ scheme using 7 s transitions.

2. Compute the Bohm ion flux for n₀ = 5 × 10¹⁰ cm⁻³, T_e = 4 eV, h = 0.4, and O₂⁺ ions (32 amu).

3. At 120 mTorr and 15% dissociation, estimate the O-atom flux at 300 K. If lateral trim rate scales linearly with O flux, how does the rate compare with the 80 mTorr, 10% example?

4. A fab runs 80,000 WSPM with 5 staircase mask passes per wafer and 25 minutes of chamber time per pass. With 6-chamber platforms at 85% availability, how many platforms are needed? How many are saved if the trim time per cycle drops by 20 s for a mask with 8 trims?

5. A chamber's ESC runs 0.15 °C warmer than its fleet mates. With a trim-rate sensitivity of 6%/°C, compute the cumulative tread-position offset at the last of 8 trims of 0.60 µm. Propose a correction.

---

**Previous Chapter:** [Chapter 4: Pair-Etch and Trim Chemistries](./04-etch-trim-chemistries.md)  
**Next Chapter:** [Chapter 6: Ion Energy & Bias Control for Layer-by-Layer Etch](./06-ion-energy-control.md)

---

**Chapter 5 Development Status:** Complete  
**Version:** 1.0
