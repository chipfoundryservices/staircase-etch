# Appendix F: Endpoint & Metrology Reference

Quick reference for optical emission signals, endpoint and fault-detection logic, and metrology methods used in staircase etch.

---

## F.1 OES Lines

```
Species   λ (nm)          Step(s)          Behavior / use
──────────────────────────────────────────────────────────────────────────────
CN        388.3           Oxide, nitride   Rises at oxide→nitride; falls at
                                           nitride→oxide. Primary clearing signal
N₂        337.1, 357.7    Nitride          Nitride etching; confirms CN
CO        483.5, 519.8    Oxide; trim      Oxide etching; trim bulk-resist
                                           removal (rises after crust)
SiF       440.0           Both etches      Weak interface contrast; total rate
F         685.6, 703.7    Etch; trim       Clearing (less consumption); wall
                                           and crust F release in trim
H         656.3 (Hα)      Nitride; trim    HFC chemistry; resist H in trim
OH        308.9           Trim             Resist oxidation
O         777.4, 844.6    Trim             Rises as resist consumption falls
                                           (strip endpoint)
C₂        516.5           Etch             Polymer-rich conditions
Ar        750.4, 811.5    All              Actinometry reference; window
                                           transmission
```

---

## F.2 Normalized Signals

```
Signal                      Definition                 Use
───────────────────────────────────────────────────────────────────────────
Oxide clearing              CN/Ar rise in oxide step   Confirms oxide cleared
Nitride clearing            CN/Ar fall in nitride step Confirms nitride cleared
Trim start                  CO/Ar rise after crust     Starts trim timer
                                                       (optional)
Trim amount (proxy)         ∫ (CO/Ar − baseline) dt    Resist volume removed;
                                                       mostly vertical
Wall release                F/Ar peak at trim start    Wall-inventory tracking
Strip endpoint              CO/Ar → baseline, O/Ar     Resist gone
                            plateau
```

---

## F.3 Endpoint Signal-to-Noise

```
ΔI/I ≈ f_open · χ
SNR ≈ (f_open · χ) / (σ_sample / √(N_avg))

Example: f_open = 0.05, χ = 0.40, σ = 0.2%, 10 samples
  SNR ≈ 0.02 / 0.00063 ≈ 32

Minimum usable SNR for clearing confirmation at 5σ threshold: ~10
Multivariate (PCA over many lines): ×2–4 effective SNR
```

---

## F.4 Endpoint and FDC Logic (Template)

```
For each etch level L:
  t_ox,exp, t_N,exp      expected clearing times (from level table)
  tol                    ± window (e.g., ±25%)

  Oxide step:
    detect CN/Ar rise (derivative threshold)
    if no rise by t_ox,exp × (1 + tol): FAULT "oxide not cleared"
    if rise before t_ox,exp × (1 − tol): WARN "early clear"
    step ends at max(t_min, t_detect × (1 + OE_frac))

  Nitride step:
    detect CN/Ar fall
    if no fall by t_N,exp × (1 + tol): FAULT "nitride not cleared"
    step ends at max(t_min, t_detect × (1 + OE_frac))

  Trim:
    detect CO/Ar rise → t_start
    trim ends at t_start + t_trim,APC (or fixed total time)
    check ∫CO within ±x% of reference: else WARN "trim anomaly"

Sequence:
  confirmed etches = level-table count; else HOLD
  resume only per Appendix C.6
```

---

## F.5 Layer Counting in Deep Etches

```
Method: count CN/Ar oscillation periods (one per pair)

Usable while A/A₀ = exp(−2π²σ_D²/p²) ≥ ~0.5
  → σ_D ≤ ~10 nm for p = 55 nm
  → depth ≤ ~12 pairs at σ_R = 1.5%

Deeper etches: split into sub-steps with selective landing
(resynchronization), each counted separately (Chapter 15.3.3)

Cross-checks: time window per sub-step, total elapsed time vs. depth
estimate, signal pattern shape
```

---

## F.6 Metrology Methods

```
Method                      Measures                     Resolution /       Notes
                                                         capability
──────────────────────────────────────────────────────────────────────────────────────────
CD-SEM (top-down)           Tread widths, edge           ~1 nm precision;   Edge detection on
                            positions, corner arcs       µm-scale FOV       topographic steps;
                                                                            charging on oxide
Optical image metrology     Edge positions vs.           ~1–3 nm (overlay-  Fast; integrated
                            reference marks              type marks)        possible
AFM                         Step heights, riser          ~0.5 nm height     Slow; tip shape
                            profiles, feet                                  limits riser angle
Stylus profilometer         Step heights, level count    ~1–5 nm height     Needs wide treads
                            on test staircases                              (≥ 10–20 µm)
Spectroscopic ellipsometry  Tread oxide thickness;       ~0.1–0.5 nm        Needs treads wider
                            resist thickness             thickness          than the spot
                                                                            (30–50 µm)
Reflectometry               Resist thickness (budget,    ~1–5 nm            Blanket monitors
                            vertical trim rate)
Cross-section SEM           Riser angle, foot, landing   ~2–5 nm            Destructive;
                            interface                                       sampling only
TEM / STEM-EDS              Interface landing, residues, <1 nm             Destructive;
                            tread oxide                                     qualification
Electrical test             Per-level contact            Functional         After metal;
                            resistance, WL–WL leakage                       final confirmation
```

---

## F.7 Test Structures

```
Structure                    Purpose                         Design notes
──────────────────────────────────────────────────────────────────────────────
Wide-tread test staircase    Level count (profilometer),     Treads 20–50 µm;
  (scribe)                   step height, tread oxide        same masks as
                             (ellipsometry)                  product
Placement marks              Tread edge position vs. a       Formed in an early
                             common reference                staircase mask;
                                                             survive later etches
Corner test                  Corner arc radius, blunting     Inside and outside
                                                             corners at product
                                                             geometry
Narrow-opening array         Resist-aspect-ratio effect      Openings 2–20 µm
Stitch test                  Stitch tread width              Spans every mask
                                                             boundary
Electrical level chain       Per-level connectivity after    Contact + metal per
                             metal                           level; WL–WL leakage
                                                             pairs
```

---

## F.8 Sampling Plan (Illustrative)

```
Measurement                         Frequency                  Sites
──────────────────────────────────────────────────────────────────────────
Tread widths/positions (last mask)  Every lot, 1–2 wafers      5–13
Stitch treads                       Every lot, 1 wafer         5
Level count (test staircase)        Every lot, 1 wafer         3
Tread oxide (test staircase)        Daily per chamber          9
Radial trim profile (monitor)       Weekly per chamber, post-  25–49
                                    PM
Cross-section                       Qualification, excursions  as needed
Electrical level test               Every lot at e-test        all die (sample
                                                               structures)
```

---

**Appendix F Version:** 1.0  
**Last Updated:** 2026-10-04
