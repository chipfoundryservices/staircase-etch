# Appendix E: Staircase Geometry & Error-Budget Calculations

Worked derivations and calculation templates for the quantitative relationships used in this book. Each section states the model, its assumptions, and a worked example using the reference process (p = 55 nm, w = 0.60 µm, T₀ = 8.0 µm, r = 1.3, R_L = 0.40 µm/min).

---

## E.1 Staircase Length and Area

```
Single-row length:     L = N · w + L_margin
Split cell, R rows:    L_R = ⌈N / R⌉ · w + (chop-edge allowances)
With X-chops into k    L ≈ k · (levels per copy / m) · w + (k − 1) · a_x
  copies:              a_x = X-chop edge allowance

Area fraction with n_s staircases and array length L_array:
  f = n_s · L / (L_array + n_s · L)

Example: N = 136, w = 0.60 µm, L_array = 4.0 mm, n_s = 2
  Single row: L = 81.6 µm → f = 163.2 / 4163.2 = 3.92%
  4 rows:     L = 34 × 0.60 = 20.4 µm → f = 40.8 / 4040.8 = 1.01%
```

---

## E.2 Minimum Tread Width

```
w_min = d_c + m_up + m_down

Each margin (root-sum-square of independent terms, linear for others):
  m = √(δ_OL² + δ_TP² + δ_CD²) + δ_taper + δ_clr

  δ_taper = p · cot θ

Example: d_c = 0.20, δ_OL = 0.030, δ_TP = 0.060, δ_CD = 0.015,
         θ = 80°, δ_clr = 0.050 µm
  √(0.0009 + 0.0036 + 0.000225) = 0.0687
  δ_taper = 0.055 × 0.1763 = 0.0097
  m = 0.0687 + 0.0097 + 0.050 = 0.128
  w_min = 0.20 + 2 × 0.128 = 0.457 µm
```

---

## E.3 Levels Formed by a Trim–Etch Sequence

```
A sequence of n trims and n + 1 etches with m pairs per etch:

  Surfaces after the sequence (from the array outward):
    under resist:               level 0 (relative to mask start)
    strip from trim n:          level m
    strip from trim n − 1:      level 2m
    ...
    strip from trim 1:          level n · m
    field:                      level (n + 1) · m

  Levels added per mask: (n + 1) · m

Masks for N levels (single-row, m = 1): ⌈N / (n + 1)⌉
```

---

## E.4 Resist Budget

```
Simple:
  T_n = T₀ − n · r · w − (n + 1) · δ_E ≥ T_min
  n_max = ⌊(T₀ − T_min − δ_E) / (r · w + δ_E)⌋

  δ_E = (m · p) / S_R

Detailed (worst point):
  T_n,worst = T₀ − Δ_coat − Δ_topo − Δ_descum
              − (n + 1) · δ_E − n · δ_cb − n · r_max · w

Example (simple): (8.0 − 1.0 − 0.014) / (0.78 + 0.014) = 8.80 → 8
Example (detailed): see Chapter 12.1.1 → 7
```

---

## E.5 Trim Ratio From Ion Flux

```
r ≈ 1 + (Y_i · Γ_i) / (R_n · n_C)

  R_n · n_C = neutral-driven carbon removal flux at the sidewall

Example: Γ_i = 1.0 × 10¹⁶ cm⁻²s⁻¹, Y_i = 1, R_n · n_C = 2.9 × 10¹⁶
  r ≈ 1.34

Lateral trim carbon flux from rate:
  R_n · n_C = (R_L in cm/s) × (carbon density)
  0.40 µm/min = 6.67 × 10⁻⁷ cm/s; × 4.4 × 10²² = 2.9 × 10¹⁶ C/cm²·s
```

---

## E.6 Corner Geometry

```
Inside corner of the opening (initially sharp), after i trims:
  R_i = Σ_{j=1}^{i} w_j + R_litho

Outside corner blunting (corner/edge rate ratio κ):
  b_i ≈ (κ − 1) · Σ w_j

Exclusion length along each edge from the corner:
  L_excl ≈ R_n + d_c / 2 + margin

Example: n = 8, w = 0.60, R_litho = 0.3, d_c = 0.20, margin = 0.5 µm
  R₈ = 5.1 µm; L_excl ≈ 5.7 µm
```

---

## E.7 Usable Tread Width

```
w_u = w − p_r · cot θ − e_f − e_t

  p_r = riser height (p or m·p)
  e_f(i) = e_f,0 + (n − i) · δ_f   for the riser at edge x_i

Example (outermost riser of an 8-trim mask, single-layer):
  e_f = 5 + 8 × 2 = 21 nm; e_t = 5 nm; θ = 80°
  w_u = 600 − 9.7 − 21 − 5 = 564 nm
```

---

## E.8 Cumulative Placement Error

```
Edge position error after k trims:
  ε_k = Σ_{i=1}^{k} e_i

Variance with independent random part σ_w and per-trim linear terms:
  E(k)² = (a · k)² + 9 · k · σ_w² + OL²      (3σ-equivalent)

  a = √(β² + s² + c² + d²) per trim (bias, spatial, chamber, drift)

Example: β = 1.8, s = 3.0, c = 3.0, d = 1.2 nm per trim → a = 4.76 nm
  σ_w = 2.4 nm, OL = 30 nm
  E(8) = √(38.1² + 415 + 900) ≈ 53 nm
```

---

## E.9 Stitch Tread Error

```
w_stitch = x₀^(m+1) − Σ w_i^(m+1) − x₀^(m)

σ_stitch² ≈ σ_OL(m)² + σ_OL(m+1)² + (trim error of mask m+1 at k = n)²

Example (3σ): 30, 30, 43 nm → √(900 + 900 + 1849) ≈ 60 nm
```

---

## E.10 Temperature Sensitivity

```
(1/R) dR/dT = E_a / (k_B T²)

Tread width error:  Δw = w · (E_a / k_B T²) · ΔT
Placement error:    Δx_k = k · Δw

Example: E_a = 0.45 eV, T = 293 K, ΔT = 0.1 °C
  Sensitivity = 0.45 / (8.617 × 10⁻⁵ × 293²) = 0.0608 /K
  Δw = 600 × 0.0608 × 0.1 = 3.6 nm
  Δx₈ = 29 nm
```

---

## E.11 Overetch and Miss Probability

```
Overetch requirement:
  OE ≥ (1 + u_d)(1 + u_e) − 1 + u_i + u_s + u_t

Stop-layer loss:
  Δ_stop ≈ R_etch · (t_OE + t_nonuniform) / S

Miss probability per site, per etch:
  P_miss = Φ(−Z),  Z = (t_step − t_clear) / σ_clear

  Z = 4: 3.2 × 10⁻⁵;  Z = 5: 2.9 × 10⁻⁷;  Z = 6: 9.9 × 10⁻¹⁰

Example: t_clear = 12.5 s, σ = 0.5 s, systematic 1.5 s, Z = 6
  t_step = 12.5 + 3.0 + 1.5 = 17.0 s (36% overetch)
```

---

## E.12 Timed-Etch Depth Drift

```
σ_D,k = √k · σ_R · p      ΔD_k = k · β · p

Example: σ_R = 1.5%, β = 1%, k = 36
  σ_D = 6 × 0.825 = 5.0 nm; ΔD = 19.8 nm
```

---

## E.13 Multi-Layer Main-Etch Window

```
Main etch must end within the m-th nitride: window ≈ ± t_N / 2

Depth error (3σ) = 3 · σ_rel · m · p

Largest m that fits: m_max = (t_N / 2) / (3 · σ_rel · p)

Example: t_N = 30 nm, σ_rel = 1.2%, p = 55 nm
  m_max = 15 / (3 × 0.012 × 55) = 15 / 1.98 = 7.6 → m ≤ 7
```

---

## E.14 OES Oscillation Damping (Layer Counting)

```
A / A₀ = exp(−2π² σ_D² / p²),   σ_D ≈ σ_R · D

Depth at which A/A₀ falls to a threshold A_t:
  σ_D,max = (p / π) · √(ln(1/A_t) / 2)
  D_max = σ_D,max / σ_R

Example: p = 55 nm, A_t = 0.5, σ_R = 1.5%
  σ_D,max = 17.5 × √(0.693 / 2) = 17.5 × 0.589 = 10.3 nm
  D_max = 10.3 / 0.015 = 687 nm ≈ 12 pairs
```

---

## E.15 Chop Combinatorics

```
c binary chop masks with depths 1, 2, ..., 2^(c−1) (in units of pairs,
or in units of the step height for X-chops) produce 2^c offsets.

Split cell with R = 2^c rows and X-steps of m = R pairs covers every
level: D = m · i + z, z ∈ {0, ..., R − 1}

X-chops with depths q, 2q, ..., 2^(c_x − 1)·q (q = levels per copy)
produce 2^(c_x) copies covering 2^(c_x) · q levels.
```

---

## E.16 Wafer Bow (Stoney)

```
κ = 6 · σ_f · t_f · (1 − ν_s) / (E_s · t_s²);  δ = κ R² / 2

Example: σ_f t_f = 300 N/m, E_s/(1−ν) = 180 GPa, t_s = 775 µm, R = 0.15 m
  κ = 0.0167 m⁻¹; δ ≈ 188 µm
```

---

## E.17 Wall Inventory

```
M_k = (1 − f) M_(k−1) + m_d,   B_k = f · M_k = m_d [1 − (1 − f)^k]

Cumulative placement offset from wall drift (steady-state calibration):
  Δx_k = Δw* · Σ_{j=1}^{k} [B_j / B* − 1] = −Δw* · Σ (1 − f)^j

Example: f = 0.3, Δw* = 13 nm, k = 8 → Σ(0.7^j) = 2.20 → Δx₈ ≈ −29 nm
```

---

## E.18 Loading

```
Trim:  R = R₀ / (1 + κ A_r);  ΔR/R = −[κ A_r / (1 + κ A_r)] · ΔA_r / A_r
Etch:  R = R₀ / (1 + κ_E f_open)

Example: κ = 0.286, A_r 0.90 → 0.85: ΔR/R = +1.1%
```

---

## E.19 Word-Line Contact Liner Loss

```
Liner loss at shallow contacts = R_ox,shallow · t_wait / S(ox:liner)

  t_wait = t_deep − t_shallow

Example: 0.8 µm/min × 20 min / S; S = 400 → 40 nm
```

---

## E.20 Cost per Wafer

```
Cost per pass = annual platform cost / annual passes
Scheme cost  = Σ_masks (litho + etch + strip/clean/metrology)
               + area fraction × wafer value

Example (136 levels, Chapter 16.5.3):
  Scheme A: 16 × $83 + 3.9% × $4000 ≈ $1,484
  Scheme B: 4 × $83 + 2 × $65 + 1.0% × $4000 ≈ $502
```

---

**Appendix E Version:** 1.0  
**Last Updated:** 2026-10-04
