# Chapter 7: Gas Switching, Pressure & Step Transitions

## Overview

A staircase mask sequence contains about three regime changes per cycle (oxide etch to nitride etch, nitride etch to trim, trim to oxide etch), so a nine-etch mask has more than twenty-five transitions. Each one moves the chamber between conditions that differ by a factor of four in pressure, by an order of magnitude in oxygen content, and from hundreds of eV of ion energy to almost none. What happens during those few seconds sets the start of every step: the polymer state the etch begins with, the crust the trim must break through, and the fluorine or oxygen that lingers into the next step.

This chapter covers the operating regimes, the physics of gas exchange and pressure settling, the choice between plasma-off and plasma-on transitions, the effect of residual species, and the use of center/edge gas tuning to flatten the trim.

**Learning Objectives:**
- Specify the gas, pressure, and power regimes of each staircase step
- Compute gas exchange and pressure settling times from chamber volume, flow, and pumping speed
- Compare plasma-off and plasma-on transitions and design a transition sequence
- Identify the effects of residual oxygen and fluorocarbon at each step boundary
- Use center/edge gas distribution to tune radial trim uniformity
- Estimate how mass-flow accuracy maps into trim rate and selectivity

---

## 7.1 The Operating Regimes

```
Parameter            Oxide etch        Nitride etch      Trim
───────────────────────────────────────────────────────────────────────
Pressure             20 mTorr          30 mTorr          80 mTorr
Main gases           C₄F₆/O₂/Ar        CH₃F/O₂/Ar        O₂/N₂
Total flow           327 sccm          200 sccm          880 sccm
O₂ fraction          ~4%               ~20%              ~91%
Source power         1000 W            800 W             1500 W
Bias                 ~200 eV           ~120 eV           0
Duration             ~14 s             ~16 s             ~90 s

(Reference values, illustrative)
```

Every transition changes several of these at once. The order in which they change matters.

---

## 7.2 Gas Exchange and Pressure Settling

### 7.2.1 Chamber Gas Exchange

Once new gases flow into a well-mixed chamber, the old composition decays exponentially:

```
c(t) = c_new + (c_old − c_new) · exp(−t / τ)

τ = p · V / Q   (residence time; 0.2–0.3 s in the reference chamber)

Time to reach 1% residual:  t₀.₀₁ = τ · ln(100) = 4.6 · τ

  Trim → etch (τ ≈ 0.20 s):  ~0.9 s
  Etch → trim (τ ≈ 0.29 s):  ~1.3 s
```

Residence time alone would allow transitions in a second or two.

### 7.2.2 What Actually Sets Transition Time

```
Contributor                          Typical time      Notes
──────────────────────────────────────────────────────────────────────────
MFC response (setpoint change)       0.3–1.0 s         Faster with pressure-
                                                       based MFCs
Gas line transport (MFC to          0.05–0.5 s         Depends on line volume
  showerhead)                                          and upstream pressure
Throttle valve travel and            1–3 s             Largest pressure jumps
  pressure settling                                    take longest
Chamber pump-down (when pressure     V/S ≈ 0.2 s per   Fast at high
  falls)                             e-fold            conductance
Plasma ignition and match tuning     0.5–2 s           Avoided with plasma-on
  (plasma-off transitions)                             transitions
RF stabilization (power, bias        0.5–1 s
  voltage within tolerance)
```

```
Pump-down time constant example:

  Effective pumping speed at 20 mTorr:
    S = Q / p = 4.14 Torr·L/s / 0.020 Torr = 207 L/s
  τ_pump = V / S = 40 L / 207 L/s = 0.19 s

  80 → 20 mTorr after flow reduction:
    t ≈ τ_pump · ln(80/20) ≈ 0.27 s (if the throttle valve keeps up)
```

In practice, pressure control loops and RF stabilization set the floor at about 2–4 s per transition for a well-tuned system. Early staircase recipes often used 8–15 s.

### 7.2.3 Transition Overhead per Mask

```
Per cycle: 3 transitions (ox → nit, nit → trim, trim → ox)
Reference: 3 s + 8 s + 8 s = 19 s per cycle

Mask of 9 etches and 8 trims: ~8 × 19 + 3 ≈ 155 s
  ≈ 13% of the 1165 s process time

With optimized 3 s transitions throughout:
  8 × 9 + 3 = 75 s → saves ~80 s per mask (~7%)
```

---

## 7.3 Plasma-Off vs. Plasma-On Transitions

### 7.3.1 Plasma-Off

```
Sequence: end step → RF off → switch gases → settle pressure →
          RF on (ignite) → tune → start next step

Advantages:
  Clean separation of chemistries
  No mixed-chemistry plasma
Disadvantages:
  Ignition transient: reflected-power spikes, unstable first second
  Particles: when the plasma goes out, negatively charged particles
    trapped at the sheath edge can fall onto the wafer
  Longer: ignition and tuning add 1–3 s
  Wafer temperature dips while RF is off (Ch. 8)
```

### 7.3.2 Plasma-On

```
Sequence: ramp gases and pressure with the source on (often through a
          short "bridge" condition, e.g., Ar/O₂), bias off, then set the
          next step's bias

Advantages:
  No ignition events, no particle drop from plasma extinction
  Faster, steadier wafer temperature
Disadvantages:
  Mixed chemistry during the ramp (e.g., O₂ + residual CH₃F)
  Requires careful bias timing so that ions do not hit the wafer
  at high energy in the wrong chemistry
```

Production staircase recipes generally use plasma-on transitions with bias turned off during the gas change.

### 7.3.3 Bias Timing

```
Rule                                         Reason
─────────────────────────────────────────────────────────────────────────
Bias off before O₂ rises at the start of     Biased ions in O₂ erode the
  the trim                                   resist top fast → wastes
                                             resist budget (raises r)
Bias on only after etch gases reach          Biased ions in O₂-rich mix
  composition at the start of an etch        etch resist and land
                                             unselectively
Bias ramp (0.3–0.5 s) rather than step       Avoids V_dc overshoot and
                                             match transients
```

```
Example cost of a timing error:
  Bias at 200 eV left on for 1.5 s into the trim gas mix
  Biased O₂ resist etch rate ≈ 2 µm/min vertical
  Extra resist loss ≈ 2 × 1.5/60 = 0.05 µm per cycle
  Over 8 cycles: 0.4 µm — half a cycle of resist budget
```

---

## 7.4 Residual Species at Step Boundaries

```
Boundary          Residual            Effect                          Mitigation
────────────────────────────────────────────────────────────────────────────────────
Trim → oxide      O atoms, O₂         Thin polymer at start of        Short purge; let
  etch            (walls O-rich)      oxide etch; resist erodes a     C₄F₆ establish
                                      little faster for ~1 s          polymer before
                                                                      bias on
Oxide → nitride   C₄F₆ fragments      Thicker polymer at the start    Brief transition;
  etch                                of the nitride step → slow      nitride step time
                                      start on nitride riser and      accounts for it
                                      tread
Nitride → trim    CH₃F fragments,     Polymer deposits on the resist  Bias off; purge or
                  HF, CN; F from      during the first seconds of     crust-breakthrough
                  walls               trim → longer induction time    step; stable
                                      (Ch. 4.6)                       timing
Trim (steady)     F released from     Etches tread oxide and riser    Heated liner, wall
                  wall polymer by O   nitride during every trim       control (Ch. 9)
```

The nitride-to-trim boundary is the most sensitive for tread width. Any residual hydrofluorocarbon that deposits in the first seconds of the trim adds to the induction time, and if the residual amount varies, the tread width varies with it.

---

## 7.5 Center/Edge Gas Tuning for Trim

### 7.5.1 Why the Trim Rate Profile Varies

```
Source                             Typical radial signature
─────────────────────────────────────────────────────────────────────
O-atom generation (ICP coil        Peaked under the coil (often at
  geometry)                        mid-radius)
O-atom loss on walls and           Lower density near the wall → edge
  chamber edge                     slow
Resist consumption (loading)       Depletion where resist area is
                                   dense
Gas injection pattern              Center-fed: center-rich; edge-fed:
                                   edge-rich
Wafer temperature profile          Edge often hotter on bowed wafers
                                   → edge fast (Ch. 8)
```

### 7.5.2 Split Injection

Most staircase chambers split gas between a center injector and edge injectors, with an adjustable ratio:

```
Edge gas fraction f_e     Trim rate, edge / center (illustrative)
────────────────────────────────────────────────────────────────
0.2                       0.96
0.3                       0.98
0.4                       1.00
0.5                       1.02
0.6                       1.04

Sensitivity ≈ 0.02 per 0.1 change in f_e
```

```
Example: tread width at the edge is 2.5% narrow (edge trim slow).
  Required change: +0.025 / 0.02 per 0.1 → Δf_e ≈ +0.125
  New edge fraction: 0.40 → ~0.52
```

Gas tuning shifts the broad radial profile. The extreme edge (last 5–10 mm) is usually dominated by the edge ring and wafer temperature, and is tuned with those (Chapters 8 and 13).

### 7.5.3 Pressure as a Profile Knob

Raising trim pressure shortens the O-atom diffusion length and makes the profile follow local generation and loss more closely. Lowering it smooths the profile by diffusion but lowers ion damping (Chapter 6.5). The pressure is chosen jointly for trim ratio and uniformity.

---

## 7.6 Mass-Flow Accuracy

### 7.6.1 Sensitivity Coefficients

```
Parameter (reference)        Change          Effect (illustrative)
──────────────────────────────────────────────────────────────────────────────
O₂ in trim (800 sccm)        +1%             Lateral trim rate +0.5%
                                             (O density sublinear in flow)
N₂ in trim (80 sccm)         +10%            Lateral trim rate +0.5–1%
O₂ in oxide step (12 sccm)   +0.5 sccm       Ox:SiN selectivity −10–15%
                             (+4%)
C₄F₆ (15 sccm)               −0.5 sccm       Ox:SiN selectivity −10%
                             (−3%)
O₂ in nitride step (40 sccm) +2 sccm (+5%)   SiN:ox selectivity −10%;
                                             lateral resist loss in etch
                                             +10%
```

### 7.6.2 Implications

1. **Small flows control selectivity.** The O₂ and C₄F₆ flows in the oxide step are tens of sccm. An MFC sized for 500 sccm with ±1% of full-scale accuracy cannot hold them to ±0.5 sccm. Use MFCs sized to the flow, with accuracy specified as a percentage of setpoint.

2. **The trim is less sensitive to flow than to temperature.** A 1% O₂ flow error moves the trim about 0.5%. A 0.1 °C wafer temperature error moves it about 0.6% (Chapter 8).

3. **Flow errors are systematic.** An MFC that reads 1% high does so every cycle, so its error accumulates in tread position. Rate-of-rise MFC checks belong in the daily chamber check (Appendix C).

---

## 7.7 Summary & Key Takeaways

1. **Each mask has dozens of transitions.** Their reproducibility, more than their speed, decides the start state of every step.

2. **Residence time is short; control loops set the pace.** MFC response, throttle settling, and RF stabilization dominate transition time.

3. **Plasma-on transitions are preferred.** They avoid ignition transients and particle drop, provided bias is off during the gas change.

4. **Bias timing protects the resist.** A second or two of biased oxygen at a transition costs a meaningful fraction of the resist budget.

5. **The nitride-to-trim boundary sets tread width reproducibility.** Residual hydrofluorocarbon lengthens and varies the trim induction time.

6. **Center/edge gas split tunes the radial trim profile.** The extreme edge needs temperature and edge-ring tuning as well.

---

## Study Questions

1. A chamber of 55 L runs the trim at 100 mTorr and 1000 sccm. Compute τ and the time to reach 0.1% residual of the previous gas.

2. During a trim-to-etch transition, pressure falls from 80 to 20 mTorr with effective pumping speed 250 L/s at the lower pressure. Compute the pump-down time constant and estimate the time to come within 5% of the final pressure.

3. A recipe uses 10 s transitions. If they are cut to 4 s each, how much time is saved per mask with 11 etches and 10 trims (three transitions per cycle)? What fraction of a 25 min chamber pass is this?

4. The trim bias is turned off 0.8 s late. If biased O₂ etches resist vertically at 2.5 µm/min, how much extra resist is lost per cycle and over 10 cycles? How many reference cycles of resist budget is that?

5. Edge treads are 1.8% wider than center treads because the edge trims fast. Using the sensitivity in Section 7.5.2, what change in edge gas fraction corrects this? What other cause should be checked before changing gas distribution?

---

**Previous Chapter:** [Chapter 6: Ion Energy & Bias Control for Layer-by-Layer Etch](./06-ion-energy-control.md)  
**Next Chapter:** [Chapter 8: Wafer Temperature, Bow & Electrostatic Chuck Design](./08-wafer-temperature-esc.md)

---

**Chapter 7 Development Status:** Complete  
**Version:** 1.0
