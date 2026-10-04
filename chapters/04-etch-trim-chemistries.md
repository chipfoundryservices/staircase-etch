# Chapter 4: Pair-Etch and Trim Chemistries

## Overview

A staircase cycle uses at least two chemistries that pull in opposite directions. The pair etch is fluorocarbon-based. It deposits polymer, relies on that polymer for selectivity, and leaves fluorocarbon on every surface it touches. The trim is oxygen-based. It burns polymer and resist, and it would happily attack fluorocarbon on the walls and release the fluorine trapped there. Running both in the same chamber, dozens of times per wafer, means choosing each chemistry for its own job and also for what it leaves behind for the other.

This chapter covers the selective oxide etch that stops on nitride, the selective nitride etch that stops on oxide, the single-step non-selective alternative, the oxygen-based trim, and the fluorinated crust that the etch leaves on the resist for the trim to deal with.

**Learning Objectives:**
- Explain the steady-state polymer mechanism behind oxide-to-nitride and nitride-to-oxide selectivity
- Compare C₄F₆, C₄F₈, CH₃F, CH₂F₂, and CHF₃ chemistries using the effective F/C ratio
- Weigh two-step selective pair etching against single-step non-selective etching
- Describe O₂-based trim kinetics and the effects of N₂, H₂O, and fluorine additions
- Explain how the fluorinated resist crust creates a trim induction time and how that affects tread width
- Select a chemistry set for a given stack and staircase scheme

---

## 4.1 Chemistry Roles in a Cycle

```
Step                    Job                                  Chemistry family
────────────────────────────────────────────────────────────────────────────────
Oxide etch              Remove ~25 nm SiO₂, stop on Si₃N₄    Fluorocarbon (C₄F₆,
                                                             C₄F₈) + O₂ + Ar
Nitride etch            Remove ~30 nm Si₃N₄, stop on SiO₂    Hydrofluorocarbon
                                                             (CH₃F, CH₂F₂, CHF₃)
                                                             + O₂ + Ar
— or —
Pair etch (one step)    Remove the whole pair at ~1:1,       CF₄/CHF₃/Ar or tuned
                        time-controlled                      C₄F₈/CHF₃/O₂
Crust breakthrough      Remove fluorocarbon skin from        Short O₂ or O₂/N₂
  (optional)            resist before the trim               step with low bias
Trim                    Pull resist back by w                O₂ (+N₂, H₂O),
                                                             zero bias
Final strip             Remove remaining resist after the    O₂ or O₂/N₂/H₂ ash
                        last etch of a mask
```

### 4.1.1 The Effective F/C Ratio

The polymerizing tendency of a fluorocarbon gas is often summarized by an effective fluorine-to-carbon ratio, with hydrogen counted as removing one fluorine each (as HF):

```
(F/C)_eff = (n_F − n_H) / n_C

Gas        n_F   n_H   n_C   (F/C)_eff   Character
───────────────────────────────────────────────────────────────
CF₄        4     0     1     4.0         Etching, weakly polymerizing
CHF₃       3     1     1     2.0         Moderately polymerizing
C₄F₈       8     0     4     2.0         Polymerizing
C₄F₆       6     0     4     1.5         Strongly polymerizing
CH₂F₂      2     2     1     0.0         Strongly polymerizing (H-rich)
CH₃F       1     3     1     −2.0        Very strongly polymerizing

Lower (F/C)_eff → thicker steady-state polymer → higher selectivity
to materials that cannot consume the polymer, and more risk of etch stop.
O₂ addition raises the effective ratio by burning carbon.
```

---

## 4.2 Oxide Etch Selective to Nitride

### 4.2.1 Mechanism

In fluorocarbon plasmas, a thin CₓF_y film forms on every surface. Its steady-state thickness depends on what the substrate can do to it:

```
Surface      What consumes the polymer                 Steady-state film
──────────────────────────────────────────────────────────────────────────
SiO₂         Oxygen released from the oxide during     Thin (~1–2 nm)
             etching reacts with C (→ CO, COF₂)        → etching continues
Si₃N₄        Nitrogen forms CN and FCN, but less       Thicker (~2–4 nm)
             effectively than oxygen removes carbon    → etching slows
Resist       Hydrocarbon surface; fluorinated by        Variable; ion-
             the plasma; eroded by ions and O          dependent
```

Ions deliver energy through the polymer film. A thin film lets enough energy through to keep etching. A thicker film absorbs most of it. The **oxide-to-nitride selectivity** comes from this difference in steady-state polymer thickness.

### 4.2.2 Representative Conditions

```
Parameter                  Value (illustrative)
──────────────────────────────────────────────────────
C₄F₆ / O₂ / Ar             15 / 12 / 300 sccm
Pressure                   20 mTorr
Source power (ICP)         1000 W
Bias power                 250 W (mean ion energy ~200 eV)
Wafer temperature          20 °C

Results:
  SiO₂ rate                ~120 nm/min
  SiO₂ : Si₃N₄             ~10–15 : 1
  SiO₂ : resist            ~5–8 : 1
```

C₄F₆ is preferred over C₄F₈ when the highest selectivity to nitride is needed, because its lower F/C ratio gives a thicker protective film on nitride. The O₂/C₄F₆ ratio is the main selectivity knob. Too little O₂ and the oxide etch stops on its own polymer. Too much and nitride selectivity collapses.

### 4.2.3 Staircase Considerations

Compared with high-aspect-ratio oxide etch, the staircase oxide step is easy in one way and hard in another. The etched areas are wide terraces, so there is no ion-angle or neutral-transport limit, and the step can run at modest ion energy. But the step must stop on a nitride layer only 30 nm thick, and it does so up to dozens of times per mask. The resist selectivity also matters more than in most oxide etches, because resist loss adds to the budget in every cycle.

---

## 4.3 Nitride Etch Selective to Oxide

### 4.3.1 Mechanism

Selective nitride etch uses hydrogen-rich hydrofluorocarbons. Hydrogen scavenges fluorine as HF, lowering the effective F/C ratio and promoting polymer. Nitrogen from the film forms volatile HCN and CN-containing products, which consume polymer carbon on nitride surfaces. Oxide has no such route at low ion energy, and the polymer on oxide is thicker:

```
Surface      Steady-state HFC film     Etch behavior
────────────────────────────────────────────────────────
Si₃N₄        Thin (~0.5–1.5 nm)        Etches (HCN, SiF₄ products)
SiO₂         Thicker (~2–4 nm)         Etch slows: protected
Resist       Variable                  Eroded by O₂ in the mix
```

This is the same chemistry used for spacer etchback (Book #22) and for selective nitride removal generally (Silicon Nitride Etch companion volume).

### 4.3.2 Representative Conditions

```
Parameter                  Value (illustrative)
──────────────────────────────────────────────────────
CH₃F / O₂ / Ar             60 / 40 / 100 sccm
Pressure                   30 mTorr
Source power (ICP)         800 W
Bias power                 150 W (mean ion energy ~120 eV)
Wafer temperature          20 °C

Results:
  Si₃N₄ rate               ~150 nm/min
  Si₃N₄ : SiO₂             ~6–12 : 1
  Si₃N₄ : resist           ~2–4 : 1
```

### 4.3.3 The Oxygen Problem

The nitride step needs substantial O₂ to keep the polymer thin on nitride. That O₂ also attacks resist, both vertically and **laterally**. The nitride step therefore trims the resist a little:

```
Lateral resist loss during the nitride step (illustrative):
  ~5–15 nm per step, depending on O₂ fraction and temperature

Over a mask of 9 etches: 45–135 nm of extra pullback,
distributed one increment per cycle
```

This "trim during etch" adds to every tread width. It is systematic and can be calibrated out of the trim time, but only if it is stable. When the O₂ flow drifts or the wall state changes, it becomes a hidden tread-width variable (Chapter 12).

---

## 4.4 Single-Step Pair Etch

### 4.4.1 The Concept

Instead of two selective steps, one step etches oxide and nitride at nearly equal rates and stops on time:

```
Chemistry (illustrative):  CF₄ / CHF₃ / Ar  =  100 / 50 / 200 sccm, 20 mTorr
  SiO₂ rate     ~180 nm/min
  Si₃N₄ rate    ~170 nm/min
  Ratio         ~1.05 : 1

Pair (55 nm) etch time:  ~19 s
```

### 4.4.2 Comparison

```
Aspect                    Two-step selective          Single-step non-selective
──────────────────────────────────────────────────────────────────────────────────
Landing mechanism         Interface stop (selective   Time (plus endpoint
                          slowdown at each interface)  confirmation)
Landing accuracy          Set by selectivity and      Set by rate stability ×
                          interface width; tolerant   time; less tolerant of
                          of rate drift               rate drift
Steps per cycle           2 (+ transition)            1
Time per cycle            Longer                      Shorter
Resist selectivity        Oxide step good, nitride    Moderate throughout
                          step poor (O₂)
Tread surface             Clean oxide stop surface    Partly etched oxide; may
                                                      vary ± several nm
Sensitivity to layer      Low (each layer cleared     High (thickness change
  thickness change        to interface)               shifts landing)
Typical use               Single-layer steps where    Multi-layer steps and
                          landing must be exact       chop etches, with a
                                                      selective final step
```

### 4.4.3 Hybrid Schemes

Production staircases often combine the two: a fast non-selective main etch through most of the pair (or most of several pairs for multi-layer steps), followed by a short selective step that lands on the target interface:

```
Multi-layer step (m = 4 pairs, 220 nm):
  1. Non-selective main etch: ~190 nm (time)
  2. Selective nitride step: clears remaining nitride, stops on oxide
  Landing set by step 2; total time well below 4 × (two-step cycle)
```

The selective finishing step corrects for rate variation in the main etch, provided the main etch never cuts through the target layer.

---

## 4.5 The Trim Chemistry

### 4.5.1 Oxygen-Atom Ashing

The trim is a resist-ashing process (Book #20) run for controlled lateral removal. Oxygen atoms abstract hydrogen from the polymer, form peroxy radicals, and break the chain into volatile CO, CO₂, and H₂O:

```
Trim rate model (radical-limited, Arrhenius surface reaction):

  R = k₀ · Γ_O · exp(−E_a / k_B T)

where:
  Γ_O   = O-atom flux to the resist surface
  E_a   = apparent activation energy, ~0.3–0.6 eV for KrF
          (PHS-based) and i-line (novolac) resists
  T     = resist (wafer) temperature
```

Two consequences dominate trim control:

1. **Trim rate is proportional to local O-atom flux.** Anything that changes O-atom density (power, pressure, recombination on walls, loading by resist area) changes the trim.
2. **Trim rate is exponentially sensitive to temperature.** At E_a = 0.45 eV and T = 300 K, a 1 °C change shifts the rate by ~5.8% (Chapter 8).

### 4.5.2 Representative Conditions

```
Parameter                  Value (illustrative)
──────────────────────────────────────────────────────
O₂ / N₂                    800 / 80 sccm
Pressure                   80 mTorr
Source power (ICP)         1500 W
Bias power                 0 W
Wafer temperature          20 °C

Results:
  Lateral trim rate        ~0.40 µm/min
  Vertical trim rate       ~0.52 µm/min (r ≈ 1.3)
  Tread oxide loss         < 0.2 nm per trim (no fluorine)
```

### 4.5.3 Additives

```
Additive             Effect on trim                         Risk
─────────────────────────────────────────────────────────────────────────────────
N₂ (5–15%)           Raises O-atom density modestly          Minor
                     (reduces recombination, adds
                     dissociation paths); improves
                     uniformity
H₂O or H₂ (few %)    Raises rate; OH and H assist            Changes resist surface;
                     abstraction                             moisture on walls
CF₄ (1–5%)           Raises rate strongly (2–5×); F          Etches every exposed
                     abstracts H and opens the polymer       tread oxide and riser
                                                             nitride during every
                                                             trim → avoided or
                                                             tightly limited
Ar / He              Dilution; helps stability               Lower rate
```

The fluorine trade-off deserves emphasis. In a photoresist ash, a few percent CF₄ is a standard rate booster. In a staircase trim, **all the treads formed so far are exposed during every trim.** Fluorine thins every exposed tread oxide and attacks every exposed riser nitride, by a slightly different amount in each cycle. The next selective pair etch erases most of this structurally (Chapter 11.2), but the trim itself becomes harder to control: its rate depends on fluorine content, the fluorine content depends on wall state, and the resist surface becomes fluorinated, which shifts the trim ratio. Most staircase trims are fluorine-free by design, and the fluorine that arrives anyway, released from the walls, is a control problem (Chapter 9).

### 4.5.4 Riser Oxidation

The trim exposes the nitride edge of every riser to oxygen atoms. Nitride oxidizes slowly at room temperature in O₂ plasma, forming an oxynitride skin of ~1–2 nm. This has two minor effects: the next nitride step must break through a thin oxynitride at the riser corner, and the skin can affect the later hot-phosphoric nitride removal at the staircase edge. Both are usually negligible, but they accumulate on outer treads.

---

## 4.6 The Fluorinated Crust and Trim Induction

### 4.6.1 What the Etch Leaves on the Resist

During the pair etch, the resist surface is bombarded by ions and coated by fluorocarbon. The result is a thin modified layer:

```
Layer (from surface inward)          Thickness       Origin
───────────────────────────────────────────────────────────────────
CₓF_y polymer film                   1–5 nm          Deposition from
                                                     the plasma
Fluorinated, cross-linked resist     5–20 nm (top)   Ion bombardment +
                                     1–5 nm (side)   F incorporation
Bulk resist                          —               Unmodified
```

The modified layer is thicker on the top surface, which receives the ion flux, than on the sidewall.

### 4.6.2 Induction Time

Oxygen atoms remove fluorinated, cross-linked material more slowly than bulk resist. The trim starts slowly and then reaches its steady rate:

```
Lateral trim with induction time:

  w = R_L · (t_trim − t_ind)     for t_trim > t_ind

Representative: t_ind,side ≈ 3–8 s, t_ind,top ≈ 10–30 s
```

### 4.6.3 Two Effects of the Crust

The crust both helps and hurts:

```
Effect                       Consequence
──────────────────────────────────────────────────────────────────────────
Top crust (longer t_ind)     Delays vertical loss in each trim → lowers
                             the effective trim ratio r. Some processes
                             deliberately use the etch to "harden" the
                             top and save resist budget
Sidewall crust (shorter       Adds a delay to every lateral trim; if
  but variable t_ind)        t_ind varies, tread width varies:
                               Δw = R_L · Δt_ind
```

```
Example: R_L = 0.40 µm/min = 6.7 nm/s
  Δt_ind = ±1 s from etch-to-etch variation in polymer
  Δw = ±6.7 nm per tread

This is comparable to the entire per-tread width budget of a tight
staircase (Chapter 12).
```

### 4.6.4 Managing the Crust

```
Approach                         How it helps                    Cost
────────────────────────────────────────────────────────────────────────────
Short crust-breakthrough step    Removes sidewall polymer        A few seconds;
  (O₂, low bias, 3–5 s)          reproducibly before the         some vertical
                                 timed trim                      resist loss
Endpointed trim start (OES       Starts the trim clock when      Requires a
  CO/O transition)               bulk resist removal begins      clean signal
Stable etch polymer              Keeps t_ind constant cycle to   Wall control
                                 cycle                           (Ch. 9)
Exploit top crust                Keep it on the top to lower r   Interaction with
                                                                 lithography resist
                                                                 thickness
```

---

## 4.7 Final Strip

After the last etch of each mask, the remaining 1–2 µm of resist is stripped. The strip can run in situ, as a long trim with a higher rate, or in a dedicated asher. Either way, the fluorinated crust and any polymer on the treads must be removed completely, because the next mask's resist is coated directly over the staircase. Residual fluorocarbon on the treads can cause resist adhesion problems or show up later as residue under the fill dielectric (Chapter 16).

```
Strip (illustrative):  O₂/N₂/H₂ (forming gas) ash, 250 °C, downstream source
  Removes resist and crust; F residue reduced by H₂ chemistry
  Followed by wet clean (dilute organic or SC1-type) if needed
```

---

## 4.8 Chemistry Selection Summary

```
Staircase scheme                   Recommended chemistry set
────────────────────────────────────────────────────────────────────────────────
Single-layer steps, tight landing  C₄F₆/O₂/Ar oxide step + CH₃F/O₂/Ar nitride
                                   step; fluorine-free O₂/N₂ trim; short crust
                                   breakthrough
Multi-layer steps (m = 2–8)        Non-selective CF₄/CHF₃ main etch +
                                   selective landing step; O₂/N₂ trim
Chop etches (deep, no trim)        Non-selective main etch for m·p, selective
                                   landing step; resist or hard-mask selectivity
                                   critical
OP stacks                          C₄F₈/O₂/Ar oxide step + HBr/O₂ poly step;
                                   O₂ trim (residual Br on walls managed)
```

---

## 4.9 Summary & Key Takeaways

1. **Selectivity is a polymer balance.** Oxide etches through a thin film and nitride stops under a thick one in C₄F₆/O₂. In CH₃F/O₂ the situation is reversed.

2. **The nitride step trims the resist.** Its O₂ content gives a small lateral pullback in every etch, which adds to tread width.

3. **Single-step pair etch is faster but lands on time.** Hybrid schemes use a fast main etch and a short selective landing step.

4. **The trim is radical-limited and temperature-sensitive.** Trim rate follows O-atom flux and changes several percent per degree.

5. **Fluorine in the trim etches every exposed tread.** Staircase trims avoid deliberate fluorine additions.

6. **The etch leaves a crust that delays the trim.** The top crust lowers the effective trim ratio. Variable sidewall crust makes tread width vary.

---

## Study Questions

1. Calculate (F/C)_eff for C₃F₈ and for a 1:1 mixture (by molecules) of CH₂F₂ and CF₄. Rank them, together with the gases in Section 4.1.1, from most to least polymerizing.

2. A two-step pair etch uses an oxide step with S(ox:resist) = 6 and a nitride step with S(SiN:resist) = 3. The pair is 25 nm oxide and 30 nm nitride, each with 20% overetch. Compute the resist loss per cycle, δ_E. Using the budget equation of Chapter 3 with T₀ = 8.0 µm, T_min = 1.0 µm, r = 1.3, and w = 0.60 µm, does this change n_max from the reference?

3. The trim has E_a = 0.45 eV and the wafer is at 20 °C. Compute the fractional change in trim rate for +1 °C. At R_L = 0.40 µm/min and a 90 s trim, what tread width change results?

4. A trim recipe includes 3% CF₄, which roughly triples the trim rate and etches exposed SiO₂ at 0.6 nm per trim. The CF₄ MFC holds ±2% of setpoint, and the trim rate rises ~4% for each 10% increase in CF₄ flow. Estimate the tread-width variation this adds at w = 0.60 µm. Compare it with a fluorine-free trim whose rate varies ±0.5% with O₂ flow. Why is the oxide loss itself not the main concern?

5. Lateral trim induction time varies between 3 and 6 s from cycle to cycle. At R_L = 0.40 µm/min, what tread-width range results? Propose two ways to reduce it and explain the cost of each.

---

**Previous Chapter:** [Chapter 3: Trim–Etch Physics — Building Steps From Resist Pullback](./03-trim-etch-physics.md)  
**Next Chapter:** [Chapter 5: Reactor Architecture for Trim–Etch Sequences](./05-reactor-architecture.md)

---

**Chapter 4 Development Status:** Complete  
**Version:** 1.0
