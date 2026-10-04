# Chapter 8: Wafer Temperature, Bow & Electrostatic Chuck Design

## Overview

Resist trim is a thermally activated reaction. Its rate rises by about 6% for every degree of wafer temperature. That makes a staircase tread a sensitive thermometer: a tenth of a degree across the wafer shows up as nanometers of tread width, and a degree shows up as tens of nanometers of tread position by the end of a mask. Wafer temperature is therefore one of the most important control variables in staircase etch, and the electrostatic chuck is one of the most important pieces of hardware.

Staircase wafers make this harder. 3D NAND stacks bow wafers by tens to hundreds of microns, the trim and etch steps put very different heat loads on the wafer, and the sequence alternates between them dozens of times. This chapter covers the temperature sensitivity of the trim, the wafer heat balance, multi-zone chuck design, chucking of bowed wafers, and the thermal transients of alternating steps.

**Learning Objectives:**
- Compute trim-rate sensitivity to temperature from the activation energy
- Translate wafer temperature non-uniformity into tread width and position error
- Build a wafer heat balance for trim and etch steps
- Explain how multi-zone ESCs and helium backside cooling control the radial profile
- Describe the problems of chucking bowed wafers and their thermal consequences
- Estimate thermal transients across a sequence of alternating steps

---

## 8.1 Temperature Sensitivity of the Trim

### 8.1.1 Arrhenius Sensitivity

```
R = R₀ · exp(−E_a / k_B T)

Fractional sensitivity:

  (1/R) · dR/dT = E_a / (k_B · T²)

E_a (eV)    T = 293 K (20 °C)    T = 323 K (50 °C)
────────────────────────────────────────────────────
0.30        4.1 %/°C              3.3 %/°C
0.45        6.1 %/°C              5.0 %/°C
0.60        8.1 %/°C              6.7 %/°C

(k_B = 8.617 × 10⁻⁵ eV/K)
```

Running the trim warmer slightly lowers the sensitivity, but only slightly. The rate is always several percent per degree.

### 8.1.2 What That Means for Treads

```
Reference: R_L = 0.40 µm/min, w = 0.60 µm, E_a = 0.45 eV, 20 °C
           sensitivity 6.1 %/°C

Temperature error     Tread width error     Position error after 8 trims
  (systematic)          (per tread)           (cumulative)
────────────────────────────────────────────────────────────────────────
0.1 °C                3.7 nm                29 nm
0.3 °C                11 nm                 88 nm
1.0 °C                37 nm                 293 nm
```

A systematic 0.3 °C offset uses most of the ±60 nm tread-placement allowance of Chapter 1. **Staircase trim needs wafer temperature control to about ±0.1 °C** in the systematic sense, both within a wafer and chamber to chamber. That is tighter than almost any other etch application requires.

### 8.1.3 The Pair Etch Is Less Sensitive

Pair-etch rates depend on temperature mainly through polymer deposition. Typical sensitivities are 0.5–2% per °C in rate and somewhat more in selectivity. Temperature matters for landing, but its effect on the staircase is an order of magnitude weaker than its effect on the trim.

---

## 8.2 Wafer Heat Balance

### 8.2.1 Heat In, Heat Out

```
Heat flux into the wafer:
  q_in = q_ion + q_rad + q_chem + q_light

  q_ion   = Γ_i · (E_i + E_ionization-recombination)  ion bombardment
  q_rad   = radical recombination at the surface
  q_chem  = reaction enthalpy (exothermic oxidation of resist)
  q_light = absorbed plasma radiation

Heat flux out (to the chuck):
  q_out = h_gap · (T_wafer − T_chuck)

  h_gap = He-gap conductance (W/m²·K), set by He pressure and gap
```

### 8.2.2 Trim vs. Etch Heat Loads

```
Step       Γ_i (cm⁻²s⁻¹)   E_i (eV)   q_ion       Other loads      q_in (total)
──────────────────────────────────────────────────────────────────────────────────
Oxide      1.3 × 10¹⁶      200        ~4.2 kW/m²  ~1 kW/m²         ~5 kW/m²
  etch
Nitride    1.0 × 10¹⁶      120        ~1.9 kW/m²  ~1 kW/m²         ~3 kW/m²
  etch
Trim       1.0 × 10¹⁶      15         ~0.4 kW/m²  ~1.5–2.5 kW/m²   ~2–3 kW/m²
                                      (incl.      (O recombination,
                                      recomb.)    resist oxidation)

q_ion example (oxide): 1.3 × 10²⁰ m⁻²s⁻¹ × 200 eV × 1.6 × 10⁻¹⁹ J/eV
                     = 4.2 × 10³ W/m²
```

### 8.2.3 Temperature Rise Above the Chuck

```
ΔT = q_in / h_gap

h_gap with 10 Torr He, good contact: ~500–1000 W/m²·K

  Oxide etch (5 kW/m², h = 700):   ΔT ≈ 7 °C
  Trim (2.5 kW/m², h = 700):       ΔT ≈ 3.6 °C

With 1% variation in h_gap across the wafer:
  Trim ΔT variation ≈ 0.036 °C → trim-rate variation ≈ 0.2%
With 10% variation in h_gap (e.g., poor contact at the edge of a
bowed wafer):
  Trim ΔT variation ≈ 0.36 °C → trim-rate variation ≈ 2.2%
```

The trim's absolute temperature rise above the chuck is modest, but **any non-uniformity in gap conductance becomes a non-uniformity in trim rate.** That is why chucking bowed wafers well matters so much (Section 8.4).

### 8.2.4 The Temperature That Matters

The trim reacts at the resist surface, but the resist is thin enough (a few µm, low thermal mass) that it sits within a fraction of a degree of the wafer. The relevant temperature is effectively the wafer surface temperature, which follows the chuck temperature plus q_in/h_gap.

---

## 8.3 Multi-Zone ESCs and Radial Tuning

### 8.3.1 Zones

```
ESC generation                    Zones           Use in staircase
────────────────────────────────────────────────────────────────────────
Dual-zone (center/edge)           2               Basic radial tilt
Multi-zone heater                 4–10 radial     Radial profile shaping
High-zone-count heater arrays     tens to         Radial and azimuthal
                                  hundreds        correction; die-level
                                                  fingerprint tuning
```

### 8.3.2 Tuning the Trim Profile With Temperature

Because the trim is so temperature-sensitive, ESC zones are a powerful trim-uniformity knob:

```
Example: lateral trim is 2.4% slow in a ring at r = 120–135 mm

Required ΔT in that zone:
  ΔT = 0.024 / 0.061 per °C ≈ +0.39 °C

Side effects to check:
  Pair-etch rate change in that ring: ~0.2–0.8% (small)
  Selectivity change: small but real; verify landing on monitors
```

### 8.3.3 Same Zone Settings for Etch and Trim?

The ESC cannot change its temperature profile quickly between steps. Zone temperatures are effectively fixed for a whole sequence, because the chuck's thermal time constant is tens of seconds to minutes. The chosen profile is therefore a compromise. In practice, the trim profile has priority because it is far more sensitive, and the etch is made robust to the resulting temperature profile through selectivity margin.

---

## 8.4 Chucking Bowed Wafers

### 8.4.1 The Problem

An ESC holds the wafer with an electrostatic pressure:

```
Coulombic (Johnsen-Rahbek aside) clamping pressure:

  P_clamp = (ε₀ · ε_r² · V²) / (2 · d²)   (dielectric layer thickness d)

Typical P_clamp: 2–10 kPa (15–75 Torr)

He backside pressure: 1–2.7 kPa (8–20 Torr)

Net force pulling the wafer down must exceed the He pressure plus
the force needed to flatten the wafer's bow.
```

A bowed wafer must be elastically flattened. The force needed rises with bow and with wafer stiffness:

```
Uniform pressure to flatten a bowed plate (order of magnitude):

  P_flat ≈ 64 · D · δ / R⁴,   D = E · t³ / (12 · (1 − ν²))

300 mm Si (t = 775 µm, E = 130 GPa, ν = 0.28):
  D ≈ 130 × 10⁹ × (775 × 10⁻⁶)³ / (12 × 0.92) ≈ 5.5 N·m

  δ = 200 µm:
  P_flat ≈ 64 × 5.5 × 2 × 10⁻⁴ / (0.15)⁴ ≈ 0.070 / 5.06 × 10⁻⁴
         ≈ 140 Pa (~1 Torr)
```

The flattening pressure is small next to the clamping pressure, so a 200 µm bow can in principle be chucked. The practical problems are elsewhere:

```
Problem                          Cause                              Consequence
──────────────────────────────────────────────────────────────────────────────────
Initial contact                  Bowed wafer touches the chuck     Clamping starts
                                 only at the center (concave)      from a small area;
                                 or edge (convex)                  may fail to pull in
He leak                          Gap at the edge seal              Chuck faults; low
                                                                   h_gap at the edge
Non-uniform h_gap                Residual gap where clamping       Radial temperature
                                 is weakest                        error → trim error
Dechuck / residual charge        Wafer springs back unevenly       Wafer movement,
                                                                   particles
Saddle (anisotropic) bow         Cannot be flattened by a radially Azimuthal temperature
                                 symmetric pull-in                 pattern
```

### 8.4.2 Mitigations

```
Approach                                   Effect
───────────────────────────────────────────────────────────────────────────
Higher chucking voltage / J-R chuck        Stronger pull-in; more residual charge
Staged chucking (ramp voltage, then He)    Pulls wafer in from center outward
                                           before backside pressure is applied
Zoned He pressure (inner/outer)            Lower outer He pressure where contact
                                           is weak; reduces leak
Bow-aware recipe (He pressure, zone        Compensates residual edge temperature
  temperature offsets by bow class)        error
Upstream bow control (backside films)      Removes the problem at the source
```

### 8.4.3 Monitoring

He leak rate during the sequence is a direct indicator of chucking quality. For staircase, it is also an indicator of edge temperature, and therefore of edge tread width. Leak rate should be logged per wafer and correlated with edge metrology (Chapter 15).

---

## 8.5 Thermal Transients Over Alternating Steps

### 8.5.1 Time Constants

```
Wafer thermal time constant on the chuck:

  τ_w = ρ · c_p · t / h_gap

Si: ρ = 2330 kg/m³, c_p = 700 J/kg·K, t = 775 µm
  ρ · c_p · t = 2330 × 700 × 7.75 × 10⁻⁴ = 1264 J/m²·K

  h_gap = 700 W/m²·K → τ_w ≈ 1.8 s
```

The wafer follows a heat-load change within a few seconds. The chuck surface, with its ceramic and bonding layers, responds over tens of seconds.

### 8.5.2 A Cycle's Temperature History

```
Wafer temperature above chuck set point (illustrative):

  Oxide etch (14 s)       +7 °C  (reaches ~steady state in ~5 s)
  Nitride etch (16 s)     +4.5 °C
  Transition (8 s)        falls toward +2 °C (low load)
  Trim (90 s)             +3.6 °C (steady after ~5 s)
  Transition (8 s)        falls toward +2 °C

At the start of each trim, the wafer is still cooling from the
etch-step temperature. The first ~5 s of trim run hotter than
steady state.
```

### 8.5.3 Effect on Tread Width

```
First 5 s of trim, average excess ≈ +1.0 °C above trim steady state
  Rate excess ≈ 6.1% over 5 s → equivalent to 0.3 s of extra trim
  At 6.7 nm/s → ~2 nm extra per tread

This is systematic, reproducible, and calibrated out by the trim time,
as long as the preceding etch steps and transition are reproducible.
```

The transient becomes a problem only when it varies, for example when the first cycle of a mask follows a long idle (cold chuck surface) or when one cycle has a longer etch step (e.g., a thicker select-gate layer). Per-level trim-time adjustments (Chapter 15) handle the predictable cases.

### 8.5.4 First-Wafer and Idle Effects

```
Condition                     Effect on first trims
─────────────────────────────────────────────────────────────────
Chamber idle > 30 min         Chuck surface and liner cooler →
                              first trims slower (and walls
                              differ; Ch. 9)
After PM                      ESC recalibration, new edge ring →
                              edge temperature shift
Lot start                     Thermal equilibrium of ceramics
                              not reached → slight drift over
                              first 2–3 wafers
```

Warm-up (dummy) wafers or a conditioning plasma before the first product wafer reduce these effects.

---

## 8.6 Temperature Metrology

```
Method                           Use
──────────────────────────────────────────────────────────────────────────
ESC zone thermocouples / RTDs    Control; do not measure the wafer
Instrumented (sensor) wafers     Calibrate wafer temperature vs. zone
                                 setpoints under plasma; chamber matching
Trim-rate monitor wafers          Indirect but most relevant: blanket resist
                                 trim rate maps (vertical) and patterned
                                 lateral trim (Ch. 15)
He leak and flow                 Indirect indicator of contact quality
```

Because trim rate is itself the most sensitive thermometer, many fabs calibrate ESC zones directly from patterned lateral-trim monitors rather than from sensor wafers alone.

---

## 8.7 Summary & Key Takeaways

1. **Trim rate changes about 6% per degree.** That sensitivity is built into O-atom ashing chemistry.

2. **Tread position needs ±0.1 °C systematic control.** A 0.3 °C offset uses most of a typical placement budget by the end of a mask.

3. **Gap conductance uniformity is temperature uniformity.** Any variation in contact between wafer and chuck becomes a trim-rate variation.

4. **Bowed wafers are the main threat to contact.** Edge leaks and incomplete pull-in raise edge temperature and trim rate.

5. **Multi-zone ESCs tune the trim profile.** Zone temperatures are fixed for a sequence, so the trim profile takes priority over the etch.

6. **Thermal transients are calibrated, not eliminated.** They matter when they change, after idle, PM, or unusual steps.

---

## Study Questions

1. For a resist with E_a = 0.38 eV, compute the trim-rate sensitivity at 15 °C and at 40 °C. How much would running the trim at 40 °C reduce the tread-width error from a 0.2 °C non-uniformity?

2. The oxide step delivers 1.5 × 10¹⁶ ions/cm²·s at 220 eV. Compute q_ion. With 1.2 kW/m² of other loads and h_gap = 600 W/m²·K, what is the wafer temperature rise above the chuck?

3. A bowed wafer has 15% lower h_gap in its outer 20 mm. With a trim heat load of 2.5 kW/m² and h_gap = 700 W/m²·K elsewhere, estimate the outer-ring temperature excess and the resulting tread-width error for a 0.60 µm tread at 6.1%/°C.

4. Compute the thermal time constant of a 775 µm Si wafer with h_gap = 400 W/m²·K. How long after an etch step does it take the wafer to settle within 0.1 °C of trim steady state if it starts 3 °C hot?

5. Explain why a lateral-trim monitor wafer can be a better ESC calibration tool for staircase than an instrumented temperature wafer. What are its limitations?

---

**Previous Chapter:** [Chapter 7: Gas Switching, Pressure & Step Transitions](./07-gas-switching-pressure.md)  
**Next Chapter:** [Chapter 9: Chamber Conditioning & Wall Memory](./09-chamber-conditioning.md)

---

**Chapter 8 Development Status:** Complete  
**Version:** 1.0
