# Appendix A: Stack Material Properties

Representative properties of the films a staircase etch encounters. Values are typical ranges for production-type films and depend strongly on deposition conditions. Use them for estimates. Measure your own films for calibration.

---

## A.1 Stack Dielectrics

```
Property                         SiO₂ (PECVD TEOS)    SiO₂ (PECVD SiH₄/N₂O)  Si₃N₄ (PECVD SiH₄/NH₃)
─────────────────────────────────────────────────────────────────────────────────────────────────
Density (g/cm³)                  2.15–2.25            2.20–2.28              2.6–2.9
Atom density (10²² /cm³)         ~6.6 (Si 2.2)        ~6.7                   ~8–9 (Si ~3.6)
H content (at.%)                 1–5                  2–6                    8–20
Refractive index (633 nm)        1.45–1.47            1.46–1.48              1.95–2.05
Dielectric constant              4.0–4.2              4.1–4.3                6.5–7.5
Intrinsic stress (MPa)           −100 to −250         −150 to −300           −300 to +600
                                 (compressive)        (compressive)          (tunable)
Young's modulus (GPa)            60–75                65–80                  150–220
CTE (10⁻⁶ /K)                    0.5–1.0              0.5–1.0                2.5–3.5
Thermal conductivity (W/m·K)     ~1.1–1.4             ~1.2–1.4               ~2–5
100:1 HF wet etch (nm/min)       3–8                  2–6                    0.3–2
Hot H₃PO₄ (160 °C) (nm/min)      0.05–0.2             0.05–0.2               4–8
```

---

## A.2 Word-Line and Stack Conductors

```
Property                         Doped poly-Si        W (CVD)          Mo (CVD/ALD)
──────────────────────────────────────────────────────────────────────────────────────
Density (g/cm³)                  2.33                 19.3             10.2
Resistivity (µΩ·cm)              ~1000–3000           8–15 (thin)      10–20 (thin)
Stress (MPa)                     −200 to +200         +500 to +1500    +300 to +1000
                                                      (tensile)
Use in staircase context         OP stack WL;         Replacement      Replacement
                                 etched in staircase  WL (after        WL (after
                                                      staircase)       staircase)
```

---

## A.3 Photoresists for Staircase Masks

```
Property                         KrF (PHS-based)        i-line (novolac)
─────────────────────────────────────────────────────────────────────────
Typical thickness for staircase  5–12 µm                5–15 µm
Density (g/cm³)                  1.1–1.2                1.2–1.3
Carbon density (10²² C/cm³)      ~4.4                   ~4.8
Sidewall angle (as developed)    80–88°                 75–85°
O₂-plasma trim E_a (eV)          0.35–0.50              0.40–0.60
Lateral trim rate, reference     0.3–0.6                0.2–0.5
  ICP O₂/N₂ (µm/min)
Stack-to-resist selectivity      4–8 (oxide step)       5–10 (oxide step)
                                 2–4 (nitride step)     3–5 (nitride step)
Bake sensitivity of trim rate    ~0.3–1%/°C of bake     ~0.3–1%/°C of bake
                                 temperature            temperature
```

---

## A.4 Liner, Fill, and Hard-Mask Films

```
Film                             Role                         Notes
─────────────────────────────────────────────────────────────────────────────────
SiN liner (20–50 nm, PECVD/ALD)  Contact etch stop            Must be isolated from
                                                              riser nitride edges
                                                              (Ch. 16.2.2)
Oxide liner (5–20 nm)            Isolation under SiN liner    Conformal ALD or PECVD
HDP-CVD oxide                    Staircase fill               Good gap fill; plasma
                                                              damage to treads minimal
O₃/TEOS (SACVD) oxide            Staircase fill               Low stress; softer;
                                                              faster contact etch
Flowable oxide                   Fill of tall risers          Needs cure; shrinkage
Amorphous carbon (Book #19)      Hard mask for deep chops     O₂-trimmable; deposited
                                                              over topography
```

---

## A.5 Silicon Substrate (for Bow and Thermal Estimates)

```
Property                              Value (Si, 300 mm)
───────────────────────────────────────────────────────────
Thickness                             775 µm
Biaxial modulus E/(1−ν), (100)        ~180 GPa
Young's modulus (in-plane avg.)       ~130 GPa
Poisson's ratio                       ~0.28
Density                               2330 kg/m³
Specific heat                         700 J/kg·K
Areal heat capacity (ρ·c_p·t)         ~1264 J/m²·K
Thermal conductivity                  ~150 W/m·K
Flexural rigidity D                   ~5.5 N·m
```

---

## A.6 Reference Stack Used in This Book

```
Layer / quantity                     Value
──────────────────────────────────────────────────────
Oxide per pair                       25 nm
Nitride per pair                     30 nm
Pair pitch p                         55 nm
Word-line pairs                      128
Select/dummy levels                  8
Total levels N                       136
Stack height (levels × p)            ~7.5 µm
Net stack stress                     −40 MPa (example)
Bow (Stoney)                         ~190 µm
Tread width w                        0.60 µm
Resist T₀ (KrF)                      8.0 µm
Trim lateral rate                    0.40 µm/min
Trim ratio r                         1.3
```

---

**Appendix A Version:** 1.0  
**Last Updated:** 2026-10-04
