# Chapter 6: Ion Energy & Bias Control for Layer-by-Layer Etch

## Overview

Ion energy does two opposite jobs in a staircase sequence. In the pair etch, ions are the tool. They drive the etch through the polymer on the layer being removed, and their energy decides how cleanly the etch stops on the layer below. In the trim, ions are a nuisance. Every ion that strikes the resist top adds vertical loss without adding lateral pullback, which raises the trim ratio and spends resist budget.

This chapter treats both. It shows how polymer-protected thresholds create a selectivity window in the pair etch, why the width of the ion energy distribution matters as much as its mean, how pulsed and tailored-waveform bias help land each step, and how even the small ion energies of a "zero-bias" trim set the trim ratio.

**Learning Objectives:**
- Apply the threshold yield model to the oxide and nitride halves of a pair etch
- Explain the selectivity window created by a polymer-protected effective threshold
- Quantify how ion energy distribution width reduces selectivity
- Use pulsed and tailored-waveform bias for soft landing
- Estimate the residual ion energy at a floating or unbiased wafer during trim
- Relate trim-phase ion flux to the trim ratio and to steps per mask

---

## 6.1 Two Opposite Needs

```
Phase          Ions are...        Target ion energy     Key metric
──────────────────────────────────────────────────────────────────────────
Oxide etch     The etch driver    ~150–250 eV, narrow   Ox:SiN selectivity,
                                                        rate
Nitride etch   The etch driver    ~80–150 eV, narrow    SiN:ox selectivity
Landing        Must be gentle     Near the protected    Overetch loss in the
  (end of                         threshold of the      stop layer
  each step)                      stop layer
Trim           Unwanted           As low as possible    Trim ratio r
```

---

## 6.2 The Selectivity Window in the Pair Etch

### 6.2.1 Yield Model With a Protected Threshold

Ion-enhanced etch yield follows a threshold law (Book #22, Chapter 3):

```
Y(E) = A · (√E − √E_th)    for E > E_th, otherwise 0
```

For the layer being etched, the polymer film is thin, and E_th is the usual etch threshold. For the stop layer, the thicker steady-state polymer absorbs part of each ion's energy. The stop layer therefore behaves as if it had a much higher **effective threshold**, set by the chemistry.

### 6.2.2 Oxide Step Example

```
Oxide (thin polymer):        A_ox = 0.12,  E_th,ox = 30 eV
Nitride under C₄F₆ polymer:  A_n  = 0.08,  E_th,n,eff = 170 eV
(illustrative values)

E (eV)   Y_ox     Y_n      S = Y_ox / Y_n
──────────────────────────────────────────
 180     0.953    0.030    ~31
 200     1.040    0.088    ~12
 250     1.240    0.222    ~5.6
 300     1.421    0.343    ~4.1

(atom densities taken as equal for simplicity)
```

The window is narrow. Just above the nitride's effective threshold, selectivity is very high, but the oxide rate is only a little lower than at higher energies. **The best place to run the oxide step is just above the stop layer's protected threshold.** A few tens of eV higher costs a factor of two or more in selectivity.

### 6.2.3 The Same Logic for the Nitride Step

In CH₃F/O₂, the roles reverse: the oxide carries the thicker film. The nitride step runs at lower ion energy (80–150 eV) because the protective film on oxide is thinner than the C₄F₆ film on nitride, and its effective threshold is correspondingly lower.

---

## 6.3 Ion Energy Distribution Width

### 6.3.1 Why Width Matters

A sinusoidal RF bias does not deliver one ion energy. It delivers a distribution, often bimodal, whose width depends on bias frequency and ion mass:

```
IED splitting (approximate, thin collisionless sheath):

  ΔE ≈ (2 · e · V_rf / π) · (ω_i / ω)   when ω ≫ ω_i

  ω_i = ion plasma frequency, ω = bias angular frequency

Lower bias frequency → wider IED. At 400 kHz–2 MHz, the IED can be
nearly as wide as the full sheath voltage swing.
```

### 6.3.2 Effect on Selectivity

Using the oxide-step model above with a mean energy of 200 eV:

```
IED (equal-weight peaks)    ⟨Y_ox⟩    ⟨Y_n⟩     S
─────────────────────────────────────────────────────
Mono-energetic, 200 eV      1.040     0.088     ~12
Bimodal 160 / 240 eV        1.031     0.098     ~10.5
Bimodal 120 / 280 eV        1.004     0.148     ~6.8
```

At the same mean energy, the high-energy wing of a broad IED crosses the nitride's protected threshold and does most of the damage to selectivity. The low-energy wing adds little oxide etching. **A narrow IED buys selectivity without losing rate.**

### 6.3.3 Getting a Narrow IED

```
Approach                         IED effect                    Notes
──────────────────────────────────────────────────────────────────────────────
Higher bias frequency            Narrower; less splitting       Lower energy per
  (13.56 MHz or above)                                         watt; standing-wave
                                                               limits at high f
Tailored (non-sinusoidal)        Near mono-energetic peak       Requires waveform
  bias waveform                                                capable generator
Pulsed DC bias with              Narrow, set by pulse voltage   Charging on
  RF source                                                    insulating stacks
                                                               managed by pulse
                                                               timing
```

For staircase etch, where the features are wide and charging is mild, tailored and pulsed-DC bias approaches work well.

---

## 6.4 Soft Landing

### 6.4.1 Two-Stage Steps

Each selective step can be split into a main etch and a landing stage:

```
Oxide step:
  Main etch:     E ≈ 220 eV, until ~80% of the oxide is removed
  Landing:       E ≈ 180 eV (or pulsed bias at lower duty), through
                 the interface and the overetch

Nitride step:
  Main etch:     E ≈ 130 eV
  Landing:       E ≈ 90 eV, through the interface and the overetch
```

The landing stage runs close to the stop layer's protected threshold, where selectivity is highest. It is slower, so it is kept short.

### 6.4.2 Pulsed Bias

Pulsing the bias on and off at a duty cycle D lowers the time-averaged ion energy without changing the IED during the on-time:

```
Time-averaged energy flux to the surface:

  ⟨E · Γ⟩ = D · E_on · Γ_i + (1 − D) · E_off · Γ_i

  E_off ≈ plasma potential energy (~15 eV), below every etch threshold

Example: E_on = 200 eV, E_off = 15 eV, D = 0.5
  Average energy per ion = 0.5 × 200 + 0.5 × 15 = 107.5 eV
  But the off-phase ions do not etch; etching happens at 200 eV
  during the on-phase only.
```

The useful effect of pulsed bias is **not** a lower ion energy. It is that the off-phase lets polymer build up on the stop layer, so the on-phase starts from a thicker protective film each cycle. Pulsed bias raises selectivity through polymer dynamics at the cost of rate (roughly proportional to D).

### 6.4.3 Landing and Layer Thickness Variation

A soft landing helps when layer thickness varies. Regions that clear early spend more time in the landing stage, where the stop layer is lost slowly. Chapter 11 develops the overetch budget.

---

## 6.5 Ion Energy in the Trim

### 6.5.1 "Zero Bias" Is Not Zero Energy

With no bias power applied, the wafer surface still sits below the plasma potential. Ions cross that sheath and arrive with energy:

```
Floating-wafer ion energy (collisionless sheath, Maxwellian electrons):

  E_i ≈ e(V_p − V_f) ≈ (k_B T_e / 2) · [1 + ln(M_i / (2π m_e))]

For O₂⁺ (M = 32 amu), T_e = 3 eV:
  ln(32 × 1823 / 6.283) = ln(9285) = 9.14
  E_i ≈ 1.5 × (1 + 9.14) ≈ 15 eV

Additional contributions:
  Capacitive coupling from the ICP coil (without a Faraday shield)
  raises and RF-modulates V_p → ion energies of 20–40 eV
```

### 6.5.2 How Trim-Phase Ions Raise r

Low-energy ions do not sputter resist, but they enhance oxidation where they strike. They strike the top surface, not the sidewall:

```
Trim ratio with an ion contribution:

  R_L ≈ R_n                     (sidewall: neutrals only)
  R_V ≈ R_n + Y_i · Γ_i / n_C   (top: neutrals + ion-enhanced)

  r = R_V / R_L ≈ 1 + (Y_i · Γ_i) / (R_n · n_C)

Example (reference trim):
  Neutral carbon removal flux R_n · n_C ≈ 2.9 × 10¹⁶ C/cm²·s (Ch. 5.4.3)
  Γ_i ≈ 1.0 × 10¹⁶ ions/cm²·s
  Y_i ≈ 1 C per ion at ~15–20 eV (ion-enhanced oxidation, illustrative)

  r ≈ 1 + (1.0 × 10¹⁶) / (2.9 × 10¹⁶) ≈ 1.34
```

This simple model reproduces the reference trim ratio. It shows that **most of the excess vertical loss comes from the ion flux during trim**, not from geometry alone.

### 6.5.3 Lowering the Trim Ratio

```
Lever                              Effect on Γ_i / Γ_O       Cost
──────────────────────────────────────────────────────────────────────────────
Higher pressure (80 → 200 mTorr)   Lower T_e and n_e per O;   Slower rate, possibly
                                   ions lose energy in        worse uniformity
                                   collisional sheath
Faraday-shielded ICP               Removes capacitive V_p     Hardware
                                   modulation; E_i → ~15 eV
Pulsed source (ICP on/off,         Ion flux falls in the      Rate reduced by less
  ~1–10 kHz)                       afterglow faster than O    than the duty cycle
                                   density (O lifetime ≫
                                   pulse period)
Remote plasma source               Ion flux ≈ 0               Lower rate (Ch. 5.3.3)
Grid or ion filter                 Ion flux ≈ 0               Hardware; particles
```

```
Example: pulsed ICP trim at 50% duty, 5 kHz
  O-atom density falls ~15% (lifetime ≈ ms ≫ 0.1 ms off-time)
  Ion flux (time-averaged) falls ~50%
  R_L: 0.40 → 0.34 µm/min
  r ≈ 1 + 0.5 × 1.0 × 10¹⁶ / (0.85 × 2.9 × 10¹⁶) ≈ 1.20

  Steps per mask (reference budget, Ch. 3.4):
    r · w + δ_E = 1.20 × 0.60 + 0.014 = 0.734
    n_max = ⌊6.986 / 0.734⌋ = 9   (was 8)
```

One more step per mask from a pulsing change, at the cost of ~18% more trim time. Whether that trade is worth making depends on the scheme's mask count and tool count (Chapters 14 and 16).

---

## 6.6 Resist Erosion by Ions During the Pair Etch

During the pair etch, ions strike the resist top and its upper edge. Two effects follow:

```
1. Vertical erosion δ_E (Ch. 3.3), set by stack-to-resist selectivity.
   Higher ion energy lowers S_R and raises δ_E roughly as √E.

2. Faceting of the resist top corner. Sputter-type yields peak at
   55–70° incidence, so the top corner develops a facet that grows
   with every etch. The trim partly rounds it off between etches.
```

```
Facet growth per etch (illustrative):
  Facet height increment ≈ δ_E · (Y(θ_f)/Y(0) − 1) / sin(θ_f)

  δ_E = 14 nm, Y(θ_f)/Y(0) = 2, θ_f = 60°
  ≈ 14 × 1 / 0.866 ≈ 16 nm per etch

Over 9 etches: ~0.15 µm of facet on a ~2–8 µm thick resist block.
The facet stays far from the resist base and does not move the edge.
```

For single-layer steps, resist faceting is minor. For chop etches and multi-layer steps, which etch hundreds of nanometers per exposure at higher energy, the facet can reach the base of a thin resist and pull the edge back (Chapter 14).

---

## 6.7 Bias Control and Drift

```
Drift source                         Effect                       Detection
──────────────────────────────────────────────────────────────────────────────
Chuck/edge ring wear                 Edge sheath shape changes →   Edge tread
                                     edge ion energy and angle     metrology
Wall condition (impedance)           Delivered bias voltage at     V_dc / V_pp
                                     fixed power shifts            trend
Match position drift                 Power delivered changes       Reflected power,
                                                                   match position
Polymer on chuck cover               Capacitance change → V_dc     V_dc trend
```

**Bias voltage control** (closing the loop on measured wafer voltage rather than on delivered power) holds ion energy steady as chamber impedance drifts. For the selectivity window of Section 6.2, where 20–30 eV changes selectivity by a factor of two, voltage control is strongly preferred.

---

## 6.8 Summary & Key Takeaways

1. **The stop layer has a protected threshold.** Polymer on the stop layer raises its effective threshold, creating a narrow selectivity window just above it.

2. **IED width matters as much as mean energy.** The high-energy wing of a broad IED does most of the selectivity damage.

3. **Soft landing uses the window.** A main etch at higher energy followed by a landing stage near the protected threshold gives rate and selectivity.

4. **Pulsed bias works through polymer.** The off-phase rebuilds polymer on the stop layer, raising selectivity at the cost of rate.

5. **The trim always has some ion energy.** A floating wafer sees ~15 eV ions, more with capacitive coupling.

6. **Trim-phase ions set the trim ratio.** Lowering ion flux in the trim (pressure, shielding, pulsing, remote sources) adds steps per mask.

---

## Study Questions

1. Using the oxide-step model of Section 6.2.2, compute the selectivity at 190 eV and at 220 eV. What energy gives a selectivity of exactly 10? Express the oxide rate at that energy relative to the rate at 250 eV.

2. An IED has equal-weight peaks at 170 and 230 eV. Compute ⟨Y_ox⟩, ⟨Y_n⟩, and the selectivity, and compare them with the mono-energetic 200 eV case.

3. Compute the floating-wafer ion energy for Ar⁺ (40 amu) at T_e = 2.5 eV and for O₂⁺ at T_e = 4 eV.

4. A trim has R_n · n_C = 3.5 × 10¹⁶ C/cm²·s, Γ_i = 1.4 × 10¹⁶ cm⁻²·s⁻¹, and Y_i = 1.2. Compute r. If a Faraday shield halves Y_i, what is the new r, and how many steps per mask does the reference budget allow?

5. The bias generator runs in power control. After a wet clean, the delivered bias voltage at fixed power rises 12%. Using the yield model, estimate the change in oxide-step selectivity if the step was running at 200 eV. What control change would prevent this?

---

**Previous Chapter:** [Chapter 5: Reactor Architecture for Trim–Etch Sequences](./05-reactor-architecture.md)  
**Next Chapter:** [Chapter 7: Gas Switching, Pressure & Step Transitions](./07-gas-switching-pressure.md)

---

**Chapter 6 Development Status:** Complete  
**Version:** 1.0
