# Chapter 11: Layer Landing, Selectivity & Step-Height Accuracy

## Overview

A staircase is a counting device. Each etch must lower every exposed surface by exactly one pair. If one etch removes nothing in some region, every tread exposed at that moment is one level shallow for the rest of the wafer's life. If one etch removes two pairs, they are one level deep. Either way, word-line contacts land on the wrong word lines.

This chapter explains how a two-step selective pair etch counts layers reliably. Its central result is that selective etching is **self-correcting for overetch but not for underetch**. An etch that goes too far is pulled back to the right interface by the next cycle. An etch that stops short loses a level permanently. That asymmetry decides how overetch should be sized, which failure modes matter, and how the single-step and multi-layer alternatives must be guarded.

**Learning Objectives:**
- State what correct landing means for each etch and for the finished tread
- Explain why two-step selective etching self-corrects overetch but not underetch
- Size overetch for each half of the pair etch from thickness, rate, interface, and residue terms
- Compute the probability of a missed step from the overetch margin
- Identify the mechanisms of missed and doubled steps, including tool faults
- Analyze landing for single-step timed etching and multi-layer steps

---

## 11.1 What Landing Means

### 11.1.1 Per-Etch Requirement

```
Number the pairs from the top: pair j = (oxide_j over nitride_j).
A surface "at level L" has pairs 1..L removed and shows oxide_(L+1).

Each etch must take every exposed surface from level L to level L + 1:
  1. Clear oxide_(L+1) completely, everywhere exposed
  2. Clear nitride_(L+1) completely, everywhere exposed
  3. Stop in oxide_(L+2), leaving it in place
```

### 11.1.2 Final-Tread Requirement

The last etch of a mask lowers every exposed tread at once, then the resist is stripped. The surface each tread keeps is therefore set by the **last etch of the mask** and by what follows it (strip, cleans, later processing):

```
Final tread oxide = t_ox − (nitride-step overetch loss in the last etch)
                        − (strip loss) − (clean loss)

Reference: t_ox = 25 nm, required ≥ 15 nm → loss budget 10 nm
```

The tread oxide matters because it protects the nitride (the future word line) through fill and CMP and because word-line contact etch must open a known oxide thickness before reaching the metal (Chapter 16).

---

## 11.2 Self-Correction in Two-Step Selective Etching

### 11.2.1 Why Overetch Heals

Each half of the pair etch stops on the other material. Consider what happens when either half goes too far:

```
Event in etch k                         Result after etch k + 1
────────────────────────────────────────────────────────────────────────────
Oxide step overetches partway into      Nitride step removes the rest of
  nitride_(L+1)                         nitride_(L+1), stops on oxide_(L+2).
                                        Correct.
Nitride step overetches partway into    Etch k + 1: oxide step removes the
  oxide_(L+2)                           rest of oxide_(L+2), nitride step
                                        removes nitride_(L+2). Correct level.
Nitride step goes through all of        Etch k + 1: oxide step stops on the
  oxide_(L+2) and partway into          exposed nitride; nitride step clears
  nitride_(L+2)                         nitride_(L+2) and lands on oxide_(L+3).
                                        Correct level.
```

As long as one step never removes **two complete layers**, the next etch lands on the correct interface. Overetch errors do not accumulate.

### 11.2.2 Why Underetch Does Not Heal

```
Event in etch k                         Result
────────────────────────────────────────────────────────────────────────────
Oxide step leaves residual oxide_(L+1)  Nitride step (selective to oxide)
  (e.g., 3 nm)                          cannot clear it in time; nitride_(L+1)
                                        stays. Etch k + 1 then clears oxide
                                        remnant and nitride_(L+1), landing on
                                        oxide_(L+2): ONE LEVEL SHORT forever.
Nitride step leaves residual            Etch k + 1: oxide step stops on the
  nitride_(L+1)                         residual nitride; nitride step clears
                                        it and stops on oxide_(L+2): ONE LEVEL
                                        SHORT forever.
```

A two-step selective etch removes at most one oxide and one nitride per cycle. If either is left in place, the cycle is spent on clearing remnants and the level is lost.

### 11.2.3 The Design Consequence

**Overetch generously.** Because overetch costs are erased by the next cycle (except in the last etch of a mask), the overetch of intermediate cycles can be sized for a very low probability of underetch. Only the last etch of each mask needs its overetch limited to protect the final tread oxide.

### 11.2.4 Which Treads a Missed Etch Affects

```
If etch k misses (globally), every surface exposed during etch k is
one level short: the field and the treads exposed by trims 1 through
k − 1. Treads exposed by trims k and later are correct.

Signature: an abrupt one-level offset beginning at a specific tread
and continuing outward to the end of the mask, and through every
later mask (whose levels are all built on the shifted field).
```

A local miss (at the wafer edge, on a residue patch, under a particle) produces the same offset only in that region.

---

## 11.3 Sizing the Overetch

### 11.3.1 Terms

```
OE ≥ (1 + u_d)(1 + u_e) − 1 + u_i + u_s + u_t

  u_d  layer thickness non-uniformity (thickest point vs. nominal)
  u_e  etch-rate non-uniformity (slowest point vs. nominal)
  u_i  interface transition (graded oxynitride) as a fraction of the layer
  u_s  start delay on newly exposed strips (residue, crust), as a
       fraction of the step time
  u_t  timing / endpoint uncertainty
```

### 11.3.2 Oxide Step Example

```
Oxide 25 nm at 120 nm/min → t_nom = 12.5 s

  u_d = 0.03, u_e = 0.04
  u_i = 1.5 nm / 25 nm = 0.06
  u_s = 1.0 s / 12.5 s = 0.08
  u_t = 0.03

OE ≥ (1.03)(1.04) − 1 + 0.06 + 0.08 + 0.03 = 0.071 + 0.17 = 0.24

Minimum OE ≈ 24% (3.0 s). Nitride loss where the oxide cleared first,
at Ox:SiN = 12:
  ≈ 120 nm/min × (3.0 + 0.9) s / 60 / 12 ≈ 0.65 nm
  (0.9 s is the extra time early-clearing areas see from non-uniformity)
```

### 11.3.3 Nitride Step Example

```
Nitride 30 nm at 150 nm/min → t_nom = 12.0 s

  u_d = 0.04, u_e = 0.04, u_i = 0.05, u_s = 0.05, u_t = 0.03

OE ≥ (1.04)(1.04) − 1 + 0.05 + 0.05 + 0.03 = 0.082 + 0.13 = 0.21

Minimum OE ≈ 21% (2.5 s). Oxide loss at SiN:ox = 8:
  ≈ 150 × (2.5 + 1.0) / 60 / 8 ≈ 1.1 nm
```

### 11.3.4 Generous Overetch for Intermediate Cycles

```
Nitride step with 100% overetch (12 s extra):
  Oxide loss ≈ 150 × (12 + 1.0) / 60 / 8 ≈ 4.1 nm of 25 nm

The next oxide step simply has 4 nm less to clear. No harm to the count.

Last etch of the mask (protecting final tread oxide):
  Limit OE so that nitride-step oxide loss + strip + clean ≤ 10 nm
  e.g., OE 40% → ~1.8 nm; strip ~0.5 nm; clean ~2 nm → 4.3 nm ✓
```

---

## 11.4 Probability of a Missed Step

### 11.4.1 Clearing-Time Distribution

The local time to clear a layer varies across sites on the wafer and from cycle to cycle. Treat it as approximately normal with mean t_c and standard deviation σ_c. The step misses at a site if its clearing time exceeds the step time t_s:

```
P_miss = P(t_c,local > t_s) = Φ(−Z),   Z = (t_s − t_c) / σ_c
```

### 11.4.2 How Small Must It Be?

```
Landing sites per wafer:
  ~300 die × 2 staircases × ~150 levels × ~20 contacts per level
  ≈ 1.8 × 10⁶ contact landing sites

Each site has been through tens of etches.

For < 0.1 miss-affected contact per wafer, the per-site, per-etch miss
probability must be well below 10⁻⁹ (strongly correlated sites relax
this, but the order of magnitude stands).

Z needed:
  Φ(−Z) = 10⁻⁹ → Z ≈ 6.0
```

### 11.4.3 Overetch Needed

```
Oxide step: t_c = 12.5 s nominal; local σ_c from thickness, rate, and
start-delay variation ≈ 0.5 s

  t_s ≥ t_c + 6 σ_c + (systematic terms) = 12.5 + 3.0 + ~1.5 = 17 s
  → ~36% overetch

Nitride step similarly → ~35–45% overetch
```

Rare-event control is the real reason staircase recipes run with what looks like excessive overetch. The self-correcting property makes it affordable.

### 11.4.4 Tails Are Not Normal

Real miss events come mostly from non-Gaussian tails: a particle, a residue patch, a crust fragment, or a transient tool fault. Overetch protects against the Gaussian part. The tails need defect control (Chapter 9.8), clean strips (Chapter 10.4), and fault detection (Section 11.5).

---

## 11.5 Missed and Doubled Steps: Mechanisms

```
Mechanism                             Result     Scope       Defense
─────────────────────────────────────────────────────────────────────────────────
Insufficient overetch (drift,         Missed     Regional    Generous OE; rate
  thick layer, slow edge)                                    monitors
Residue / micromask on a new strip    Missed     Local strip Crust breakthrough;
                                                             over-trim (Ch. 10.4)
Particle on a tread                   Missed     Local       Particle control
                                      (pillar)               (Ch. 9.8)
Etch-stop polymer (O₂ MFC low,        Missed     Global      MFC checks; OES
  over-polymerizing wall)                                    clearing confirmation
Step skipped (plasma fails to         Missed     Global      FDC: per-step OES
  ignite; step aborted and recipe                            signature check
  resumes at the next step)
Step run twice (abort and resume      Doubled    Global      Recipe resume logic
  repeats an etch)                                           tied to OES-confirmed
                                                             step completion
Gross overetch through two layers     Doubled    Regional    Selectivity; never
  in one step (selectivity collapse)                         rely on time alone
Wrong recipe level (per-level step    Missed or  Global      Recipe management;
  table misaligned)                   doubled                level counting (Ch. 15)
```

### 11.5.1 Recipe Abort and Resume

Tool faults during a staircase sequence are not rare over millions of cycles. The resume logic decides whether a fault becomes a scrapped wafer:

```
Rule set (illustrative):
  1. Each etch records OES evidence of oxide and nitride clearing.
  2. On abort, the tool records the last etch with confirmed clearing.
  3. Resume restarts the interrupted etch (not the next one) and lets
     selectivity absorb any partial work already done.
  4. Never resume with a trim if the preceding etch was not confirmed.
  5. Any ambiguity → hold the wafer for metrology before continuing.
```

Rule 3 uses self-correction on purpose. Re-running a partly completed selective etch only adds overetch, which heals.

---

## 11.6 Single-Step Timed Pair Etch

### 11.6.1 Errors Accumulate

A non-selective pair etch removes a depth set by time and rate, with no interface to stop on. Errors carry from cycle to cycle:

```
Depth after k etches:  D_k = Σ_{i=1}^{k} R_i · t_i

Random rate variation σ_R (fraction), systematic bias β:
  σ_D,k = √k · σ_R · p
  ΔD_k = k · β · p

Reference p = 55 nm, σ_R = 1.5%, β = 1%:
  k = 9:   σ_D = 3 × 0.825 = 2.5 nm,  ΔD = 9 × 0.55 = 5.0 nm
  k = 36:  σ_D = 6 × 0.825 = 5.0 nm,  ΔD = 36 × 0.55 = 19.8 nm
```

By 36 cycles, the systematic term alone is most of a nitride layer. Without a reset, the staircase drifts off the interfaces.

### 11.6.2 Periodic Selective Reset

Single-step schemes therefore add a selective landing at intervals: every cycle (hybrid scheme, Chapter 4.4.3) or every few cycles. The reset pulls the surface back to a known interface. Its overetch must cover the accumulated drift since the last reset.

---

## 11.7 Landing for Multi-Layer Steps

### 11.7.1 The Window

A multi-layer step removes m pairs per cycle: a non-selective main etch of most of the depth, then selective steps to land. The main etch must:

```
1. Clear the oxide of the m-th pair (otherwise the selective nitride step
   cannot proceed → missed level)
2. Not clear the m-th pair's nitride and the next oxide entirely
   (otherwise the selective steps land one pair deep → doubled level)

Target: end the main etch near the middle of the m-th nitride.
Window: roughly ± half the nitride thickness, ±15 nm in the reference.
```

### 11.7.2 Main-Etch Depth Accuracy

```
Depth error (3σ, combining rate, thickness, and timing):
  ≈ 3 × 1.2% × m · p (illustrative)

m     Depth (nm)   3σ error (nm)   Fits ±15 nm window?
──────────────────────────────────────────────────────
2     110          ±4.0            Yes
4     220          ±7.9            Yes
8     440          ±15.8           Marginal
16    880          ±31.7           No
32    1760         ±63.4           No
```

Steps up to about eight pairs can be landed with a single main etch plus a selective finish. Deeper etches, such as the chop etches of Chapter 14, need endpoint-based layer counting (Chapter 15) or must be split into sub-steps, each with its own selective landing.

---

## 11.8 Step-Height Accuracy

With selective landing, every riser height equals one pair thickness as deposited. Step height accuracy is then a deposition property (Chapter 2.3), not an etch property. The etch contributes only through the final tread-oxide loss, which differs slightly between treads at different radii on the wafer:

```
Tread surface depth below the stack top, tread at level L:
  D_L = Σ_{j=1}^{L} p_j + (oxide loss in the last etch)

The etch term is a few nanometers and nearly the same for all treads in
a mask. Word-line contact etch must handle the deposition term
(up to ~100 nm systematic at the deepest tread, Chapter 2.3.3).
```

---

## 11.9 Summary & Key Takeaways

1. **The staircase is a counter.** Each etch must clear exactly one oxide and one nitride everywhere and stop on the next oxide.

2. **Selective etching heals overetch, not underetch.** An etch that goes too far is corrected by the next cycle. An etch that leaves a remnant loses a level permanently.

3. **So overetch generously.** Intermediate cycles can carry 35–100% overetch at little cost. Only the last etch of each mask must protect the final tread oxide.

4. **Missed steps are rare events.** Per-site miss probabilities must be near 10⁻⁹, which takes about six standard deviations of clearing margin, plus defect control for non-Gaussian tails.

5. **Tool faults need resume logic.** Restarting an interrupted selective etch is safe. Skipping or repeating one is not.

6. **Timed etches drift.** Single-step and multi-layer schemes need selective resets, and deep chop etches need layer counting or splitting.

---

## Study Questions

1. A surface at level 12 shows oxide_13. Describe the surface after the next etch if (a) the oxide step leaves 2 nm of oxide_13, (b) the nitride step removes 20 nm of oxide_14, and (c) the nitride step removes all of oxide_14 and 10 nm of nitride_14. Which cases lose a level?

2. For a nitride layer of 32 nm etched at 140 nm/min with u_d = 0.05, u_e = 0.03, u_i = 0.05, u_s = 0.06, and u_t = 0.02, compute the minimum overetch and the oxide loss at SiN:ox = 7.

3. Clearing time for the oxide step has mean 13.0 s and σ_c = 0.6 s. What step time gives Z = 6? What miss probability results if the step is cut to 15.0 s?

4. A single-step timed etch has σ_R = 2% and β = 0.5% per cycle on a 55 nm pair. After how many cycles does the 3σ random depth error plus systematic error exceed 15 nm?

5. During cycle 5 of a 9-etch mask, the plasma fails to ignite in the oxide step and the tool continues with the nitride step and the trim. Describe the resulting staircase. Which treads are affected? How would OES reveal the fault?

6. A multi-layer scheme uses m = 6. With a depth error of 1.0% (1σ), does a single main etch fit the ±15 nm window at 3σ? What is the largest m that fits?

---

**Previous Chapter:** [Chapter 10: Step Profile Control — Edge Taper, Footing & Corner Rounding](./10-step-profile-control.md)  
**Next Chapter:** [Chapter 12: Resist Budget & Cumulative Placement Error](./12-resist-budget-error.md)

---

**Chapter 11 Development Status:** Complete  
**Version:** 1.0
