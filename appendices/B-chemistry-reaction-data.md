# Appendix B: Chemistry & Reaction Data

Reference data for the gases, products, and surface reactions of staircase etch. Kinetic values are representative and depend on surface condition and plasma environment.

---

## B.1 Feed Gases

```
Gas      MW (g/mol)   (F/C)_eff   Role in staircase                   Notes
──────────────────────────────────────────────────────────────────────────────────────
C₄F₆     162.0        1.5         Oxide step (selective to nitride)  Strongly polymerizing;
                                                                     liquid-source handling
C₄F₈     200.0        2.0         Oxide step; non-selective main     Moderate polymer
CF₄      88.0         4.0         Non-selective pair etch; WAC       Weak polymer
CHF₃     70.0         2.0         Non-selective pair etch            Moderate polymer
CH₂F₂    52.0         0.0         Nitride step                       Strong polymer
CH₃F     34.0         −2.0        Nitride step (selective to oxide)  Very strong polymer;
                                                                     H-rich
O₂       32.0         —           Trim; polymer control in etch      —
N₂       28.0         —           Trim additive                      Raises O density,
                                                                     improves uniformity
Ar       40.0         —           Diluent; ion source in etch        Sputter-type faceting
He       4.0          —           Diluent; backside cooling          —
NF₃      71.0         —           WAC (fluorine)                     Removes SiOₓF_y
SF₆      146.1        —           WAC (fluorine)                     Removes SiOₓF_y
HBr      80.9         —           Poly-Si step (OP stacks)           Highly selective to
                                                                     oxide
```

---

## B.2 Etch and Trim Products

```
Product   Boiling point (°C)   Formed from                          Pumps away?
──────────────────────────────────────────────────────────────────────────────────
SiF₄      −86 (sublimes)       Oxide and nitride etch               Yes; can form SiOₓF_y
                                                                     on walls with O
CO        −191                 Oxide etch (O + C); resist trim      Yes
CO₂       −78 (sublimes)       Resist trim                          Yes
COF₂      −85                  Oxide etch; wall polymer oxidation   Yes
H₂O       100                  Resist trim                          Yes (adsorbs on walls)
HF        20                   HFC chemistry; trim with F           Mostly; adsorbs
HCN       26                   Nitride etch in HFC                  Yes
FCN       −46                  Nitride etch                         Yes
N₂        −196                 Nitride etch                         Yes
SiBrₓ     ~150 (SiBr₄)         Poly-Si step in HBr (OP stacks)      Partly; wall deposits
```

---

## B.3 Bond Energies (Approximate, Diatomic or Representative)

```
Bond      Energy (eV)    Relevance
──────────────────────────────────────────────────────────────
Si–O      ~8.3           Oxide strength; high ion threshold
Si–N      ~4.5–5         Nitride
Si–F      ~5.7           Driving force for SiF₄ formation
C–F       ~5.0           Polymer stability
C–H       ~4.3           Resist backbone; H abstraction in trim
C–C       ~3.6           Resist backbone
C=O       ~7.7 (in CO:   Volatile product formation in trim
          11.1)
O–H       ~4.4           OH formation in trim
H–F       ~5.9           Fluorine scavenging by H
```

---

## B.4 Selective Etch Mechanisms (Summary)

```
Step              Etching film   Stop film   Selectivity mechanism
──────────────────────────────────────────────────────────────────────────────────
C₄F₆/O₂/Ar        SiO₂           Si₃N₄       Film O consumes polymer on oxide;
                                             thicker CₓF_y film on nitride
CH₃F/O₂/Ar        Si₃N₄          SiO₂        H scavenges F; N forms HCN/CN, thin
                                             film on nitride; thicker film on oxide
HBr/O₂            poly-Si        SiO₂        Br etches Si; oxide needs high energy
                                             to break Si–O; SiBrₓOᵧ passivation
```

---

## B.5 Trim Kinetics

```
Rate law (radical-limited):  R = k₀ · Γ_O · exp(−E_a / k_B T)

Parameter                                   Representative value
──────────────────────────────────────────────────────────────────────────
E_a (KrF resist, O₂ plasma)                 0.35–0.50 eV
E_a (i-line resist, O₂ plasma)              0.40–0.60 eV
Temperature sensitivity at 20 °C            4–8 %/°C
Effective O reaction probability on resist  ~10⁻³–10⁻² (high-density ICP)
O consumed per carbon removed               ~2–3 (CO, CO₂, H₂O)
Rate enhancement from 1–5% F (CF₄ or        2–5×
  wall release)
Rate change with N₂ addition (5–15%)        +5–20%
Rate change with H₂O addition (few %)       +10–30%
Trim ratio r, ICP zero bias                 1.2–1.4
Trim ratio r, CCP                           1.6–2.2
Trim ratio r, remote source                 1.0–1.15
```

---

## B.6 Surface Recombination of O Atoms

```
Surface                       Recombination probability γ_O (approx.)
─────────────────────────────────────────────────────────────────────
Quartz / SiO₂ (clean)         10⁻⁴–10⁻³
Anodized Al                   10⁻³–10⁻²
Y₂O₃ / YOF                    10⁻³–10⁻²
Stainless steel               10⁻²–10⁻¹
Polymer-coated walls          Variable; polymer reacts with O
                              (consumes O and releases F)
```

Higher wall recombination lowers O density and trim rate. Changes in wall state after WAC or PM change γ_O and therefore the trim rate (Chapter 9).

---

## B.7 Gas-Phase Constants Used in Calculations

```
Quantity                                  Value
──────────────────────────────────────────────────────────────
Boltzmann constant k_B                    1.381 × 10⁻²³ J/K
                                          8.617 × 10⁻⁵ eV/K
Elementary charge e                       1.602 × 10⁻¹⁹ C
Atomic mass unit                          1.661 × 10⁻²⁷ kg
Electron mass / amu                       1/1823
1 sccm                                    4.48 × 10¹⁷ molecules/s
                                          0.01267 Torr·L/s
1 mTorr at 300 K                          3.22 × 10¹³ cm⁻³
Mean speed v̄ = √(8kT/πm), 300 K:
  O (16 amu)                              630 m/s
  F (19 amu)                              578 m/s
  O₂ (32 amu)                             445 m/s
Mean free path, O₂, 80 mTorr, 300 K       ~0.6–1 mm
```

---

## B.8 Reaction Summary by Step

```
Oxide step:
  SiO₂ + CₓF_y (ion-assisted) → SiF₄ + CO, CO₂, COF₂

Nitride step:
  Si₃N₄ + CHₓF_y (ion-assisted) → SiF₄ + HCN, FCN, N₂ + HF

Trim:
  (C₈H₈O)ₙ resist + O → CO, CO₂, H₂O
  CₓF_y (crust, wall) + O → COF₂, CO, CO₂ + F

Wall chemistry during trim:
  SiF₄ (residual) + O → SiOₓF_y (wall deposit) + F

Fluorine WAC:
  SiOₓF_y + F (from NF₃) → SiF₄ + O₂
```

---

**Appendix B Version:** 1.0  
**Last Updated:** 2026-10-04
