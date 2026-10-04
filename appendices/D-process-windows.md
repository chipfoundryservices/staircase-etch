# Appendix D: Process Windows & Lookup Tables

Starting-point recipes and sensitivity tables for a 300 mm ICP staircase chamber with independent bias. All values are **illustrative starting points for a design of experiments**, not qualified conditions.

---

## D.1 Starting Recipes

### D.1.1 Oxide Step (Selective to Nitride)

```
Parameter               Start        Window          Primary effect
───────────────────────────────────────────────────────────────────────────
C₄F₆                    15 sccm      10–20           Polymer, selectivity
O₂                      12 sccm      8–16            Polymer balance (main knob)
Ar                      300 sccm     200–400         Dilution, ion flux
Pressure                20 mTorr     12–30           Polymer, uniformity
Source power            1000 W       700–1500        Density, rate
Ion energy (mean)       ~200 eV      170–250         Selectivity window
                                                     (Ch. 6.2)
Wafer temperature       20 °C        10–40           Polymer
Expected: SiO₂ ~120 nm/min; Ox:SiN 10–15; Ox:resist 5–8
```

### D.1.2 Nitride Step (Selective to Oxide)

```
Parameter               Start        Window          Primary effect
───────────────────────────────────────────────────────────────────────────
CH₃F                    60 sccm      40–80           Polymer, rate
O₂                      40 sccm      25–55           Selectivity (main knob);
                                                     lateral resist loss
Ar                      100 sccm     50–200          Dilution
Pressure                30 mTorr     20–50
Source power            800 W        500–1200
Ion energy (mean)       ~120 eV      80–150          Selectivity, foot
Expected: Si₃N₄ ~150 nm/min; SiN:ox 6–12; SiN:resist 2–4
```

### D.1.3 Single-Step Pair Etch (Non-Selective)

```
CF₄ / CHF₃ / Ar         100 / 50 / 200 sccm
Pressure                20 mTorr
Source / bias           1000 W / ~250 eV
Expected: SiO₂ ~180, Si₃N₄ ~170 nm/min (ratio ~1.05)
Tune CHF₃:CF₄ to set the ratio to 1.00 ± 0.05
```

### D.1.4 Crust Breakthrough

```
O₂ / N₂                 500 / 50 sccm
Pressure                40 mTorr
Source power            1000 W, bias 0 (or ≤ 20 W)
Time                    3–5 s (or OES: until CO/Ar rise)
```

### D.1.5 Trim

```
Parameter               Start        Window          Primary effect
───────────────────────────────────────────────────────────────────────────
O₂                      800 sccm     500–1500        O density
N₂                      80 sccm      0–150           Rate, uniformity
Pressure                80 mTorr     50–200          r (higher → lower r),
                                                     profile
Source power            1500 W       1000–2500       Rate
Bias                    0 W          0               Never biased
Pulsing (optional)      50% at 5 kHz 30–100%         Lower r (Ch. 6.5.3)
Wafer temperature       20 °C ±0.1   10–40           Rate (6%/°C)
Center/edge gas         0.40 edge    0.2–0.6         Radial profile
Expected: R_L ~0.40 µm/min, r ~1.3 (unpulsed), ~1.2 (pulsed)
```

### D.1.6 Final Strip and WAC

```
In-situ strip:   O₂/N₂ 1500/150 sccm, 150 mTorr, 2000 W, bias 0, to
                 OES endpoint (CO/Ar falls to baseline) + 20%
WAC:             (1) NF₃/O₂ 200/200 sccm, 100 mTorr, 1500 W, 20–40 s
                 (2) O₂ 1000 sccm, 100 mTorr, 2000 W, 20–40 s
                 (3) Conditioning: short etch-chemistry step or O₂,
                     fixed per recipe
```

---

## D.2 Sensitivity Tables

### D.2.1 Trim

```
Parameter change              Lateral rate     Trim ratio r    Uniformity
──────────────────────────────────────────────────────────────────────────
Wafer T +1 °C                 +6%              ~0              —
Source power +10%             +5–7%            +0.02           slight change
Pressure +20 mTorr            −3–5%            −0.03           center/edge shift
O₂ +10%                       +4–5%            ~0              —
N₂ fraction +5 pts            +2–4%            ~0              improves
Pulsing 100% → 50%            −15%             −0.10           —
Edge gas fraction +0.1        edge/center      ~0              +2% edge
                              +0.02
Wall F (first-cycle)          +5–20%           +/−             —
```

### D.2.2 Pair Etch

```
Parameter change              Etch rate        Selectivity     Foot
──────────────────────────────────────────────────────────────────────────
Oxide step O₂ +1 sccm         +2%              Ox:SiN −10%     −
Oxide step ion energy +20 eV  +5%              Ox:SiN −30–50%  −
Nitride step O₂ +5 sccm       +3%              SiN:ox −10%     less foot;
                                                               more lateral
                                                               resist loss
Nitride step ion energy       +6%              SiN:ox −20%     less foot
  +20 eV
Wafer T +5 °C                 +2–4%            ±               less polymer
```

---

## D.3 Resist Budget Lookup (Trims per Mask, n_max)

```
n_max = ⌊(T₀ − T_min − δ_E) / (r·w + δ_E)⌋, T_min = 1.0 µm, δ_E = 14 nm

            w = 0.45 µm             w = 0.60 µm             w = 0.80 µm
T₀ (µm)   r=1.1  r=1.3  r=1.6     r=1.1  r=1.3  r=1.6     r=1.1  r=1.3  r=1.6
──────────────────────────────────────────────────────────────────────────────
 6         9      8      6         7      6      5         5      4      3
 8         13     11     9         10     8      7         7      6      5
10         17     15     12        13     11     9         10     8      6
12         21     18     14        16     13     11        12     10     8
```

Use the detailed budget of Chapter 12.1 (worst-point coat, topography, crust breakthrough, r non-uniformity) before committing to a value. Expect the detailed budget to lose about one trim relative to this table.

---

## D.4 Overetch Lookup (Two-Step Selective Etch)

```
Purpose                                  Oxide step OE   Nitride step OE
──────────────────────────────────────────────────────────────────────────
Minimum (Gaussian terms only, Ch. 11.3)  20–25%          20–25%
Intermediate cycles (rare-event, Z ≈ 6)  35–60%          35–100%
Last etch of mask (protect tread oxide)  35–60%          30–40%
After PM / new product (first lots)      +10 pts         +10 pts
```

---

## D.5 Placement Budget Lookup (3σ at Last Tread, nm)

```
E(n) = √[(a·n)² + 9·n·σ_w² + OL²], σ_w = 2.4 nm, OL = 30 nm

a (nm/trim)   n = 6    n = 8    n = 10   n = 12
──────────────────────────────────────────────────
3.0           39       43       48       53
4.8           45       53       61       70
6.0           49       60       71       82
8.0           59       74       88       104
```

---

## D.6 Chop-Scheme Lookup

```
Levels   m (pairs/step)   Y-chops   X-chops   Trim–etch masks   Total masks
──────────────────────────────────────────────────────────────────────────────
 136     1                0         0         16                16
 136     4                2         0         4                 6
 136     4                2         2         1                 5
 232     4                2         0         7                 9
 232     4                2         2         2                 6
 232     8                3         0         4                 7
 310     4                2         0         9                 11
 310     4                2         3         2                 7
 310     8                3         2         2                 7

(Trim–etch masks assume 9 X-steps per mask.)
```

---

**Appendix D Version:** 1.0  
**Last Updated:** 2026-10-04
