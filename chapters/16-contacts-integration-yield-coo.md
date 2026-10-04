# Chapter 16: Word-Line Contacts, Integration, Yield & Cost of Ownership

## Overview

The staircase exists for one customer: the word-line contact. Everything the staircase etch does (counting levels, placing treads, protecting tread oxide, keeping risers clean) is judged by whether a row of contacts with depths ranging from a fraction of a micron to more than eight microns can each land on exactly one word line and touch nothing else. Between the staircase etch and that contact etch come a strip, a liner, several microns of fill, CMP, slit etch, and replacement gate, and each of them can undo good staircase work.

This final chapter follows the staircase through the rest of the flow. It covers the fill and liner, the replacement gate as it acts in the staircase region, and word-line contact etch to many depths at once, then catalogs defect modes and their yield signatures. It ends with a cost-of-ownership model that compares staircase schemes, and with the principles that run through the book.

**Learning Objectives:**
- Describe post-staircase processing: strip, liner, fill, CMP, and support pillars
- Explain the liner–word-line isolation problem during replacement gate
- Quantify the punch-through risk in single-step word-line contact etch and the remedies
- Map defect types and electrical signatures to staircase root causes
- Build a cost-per-wafer model for a staircase scheme including masks, chamber time, and area
- Compare schemes on total cost and risk

---

## 16.1 After the Last Etch

### 16.1.1 Sequence

```
1. Final resist strip and post-etch clean (Chapter 4.7)
2. Etch-stop liner: conformal SiN (or alternative), 20–50 nm, over the
   whole staircase
3. Staircase fill: thick oxide (HDP-CVD, O₃/TEOS, or flowable oxide),
   thicker than the stack height
4. CMP to planarize fill to the top of the stack
5. Support pillars ("dummy channel holes") through the staircase
6. Slit etch through the stack, including the staircase region
7. Replacement gate: nitride removal through slits, W (or Mo) fill
8. Word-line contact etch and fill
```

### 16.1.2 Fill and CMP

```
Fill thickness ≥ stack height + margin
  Reference: 7.5 µm stack → ~8–9 µm fill deposited over the staircase

Concerns:
  Voids at the base of tall risers (chop edges, Ch. 14.6.3), where fill
    must cover a step of microns
  Stress from thick fill → added bow
  CMP dishing over the wide, low staircase area next to the high array
  Fill density and wet-etch rate → contact etch rate (Section 16.3)
```

Staircase layout affects fill: gradual single-layer staircases fill easily, while chop schemes with micron-tall risers need fill processes with good gap and step coverage.

### 16.1.3 Support Pillars

During replacement gate, nitride is removed from between the oxide layers. In the array, channel pillars hold the oxide layers apart. In the staircase region there are no channels, so **dummy pillars** are etched through the staircase and filled with oxide to support the layers. They must be placed in the tread layout so that they never coincide with a contact landing site:

```
Each tread hosts:  word-line contacts + support pillars + clearances
→ Pillar layout is another claimant on tread area (Chapter 1.4)
```

---

## 16.2 Replacement Gate in the Staircase Region

### 16.2.1 What Happens

Hot phosphoric acid enters through the slits and removes nitride laterally, including from under every tread. The cavities are then lined (barrier, high-k in some schemes) and filled with tungsten or molybdenum. The tread oxide over each word line becomes the only layer between the metal word line and the liner and fill above.

### 16.2.2 The Liner–Word-Line Isolation Problem

A silicon nitride etch-stop liner on the staircase touches the exposed nitride edge of every riser:

```
At each riser, the liner contacts the end of the nitride layer:

   tread oxide k     ┊ liner (SiN) ┊
   ═══ nitride k ════╪═════════════╪  ← liner touches nitride edge
   tread oxide k+1   ┊             ┊

During replacement gate, H₃PO₄ removes nitride k and reaches the liner
through that contact. If it removes the liner, the metal fill follows
the liner path → every word line connected to every other through the
liner cavity → catastrophic short.
```

```
Remedies                                     Trade-off
───────────────────────────────────────────────────────────────────────
Thin oxide liner first, then SiN liner       Separates SiN liner from
                                             nitride edges; adds a step
Riser nitride recess and oxide seal before   Extra etch and deposition
  the liner
Liner of a material not etched in hot        Must still act as contact-
  H₃PO₄ (e.g., certain oxides, SiCN)         etch stop
No liner; contact etch stops on thicker      Contact etch landing harder
  tread features (raised pads)               (Section 16.3)
```

The staircase etch influences this through the riser profile: feet and residues at the riser base (Chapter 10.3) can leave nitride where the isolation oxide does not cover it.

---

## 16.3 Word-Line Contact Etch

### 16.3.1 The Problem

All word-line contacts are usually etched in one step through the fill oxide:

```
Contact depths (reference, single deck, ~1 µm overburden above the stack):
  Shallowest (top tread):  ~1.0 µm
  Deepest (bottom tread):  ~1.0 + 7.5 = 8.5 µm

The shallow contacts reach the liner in a small fraction of the etch time.
They then sit on the liner while the deep contacts finish.
```

### 16.3.2 Punch-Through Estimate

```
Oxide etch rate in contacts (with ARDE):
  Shallow: ~0.8 µm/min; deep: average ~0.4 µm/min

  Time for deepest: 8.5 / 0.4 ≈ 21 min (+ overetch)
  Time for shallowest: 1.0 / 0.8 ≈ 1.25 min

Shallow contacts sit on the liner for ~20 min.
Liner loss = R_ox · t_wait / S(ox:SiN)
  = 0.8 µm/min × 20 min / S = 16 µm / S

  S = 50:   320 nm of liner → punched through (liner 40 nm)
  S = 400:  40 nm of liner → just consumed; no margin

After the liner: 25 nm tread oxide, then the word line (~30 nm W),
then 25 nm oxide, then the next word line.
```

Single-step contact etch with a uniform liner would need oxide-to-nitride selectivity of several hundred over 20 minutes at the shallow contacts. That is beyond reliable practice.

### 16.3.3 Remedies

```
Approach                               How it helps                        Cost
───────────────────────────────────────────────────────────────────────────────────
Raised landing pads (thicker           Thicker metal or nitride at each   Staircase process
  nitride on each tread before         contact site absorbs overetch      steps; pad
  liner, which becomes thick W                                            patterning
  after replacement)
Multiple contact masks by depth        Each group has a smaller depth     1–3 extra litho and
  group (e.g., 2–4 groups)             range → shorter wait on liner      etch steps
Graded liner (thicker over upper       Protection matched to wait time    Liner process
  treads)                                                                 complexity
Highly selective, polymerizing         Raises S(ox:SiN) on the liner       ARDE, etch stop in
  landing chemistry                                                       deep contacts
Liner open as a separate low-damage    Controls final punch through       Extra step
  step after the main etch             tread oxide
```

```
Example: two depth groups instead of one
  Group 1: levels 0–67 (depths 1.0–4.7 µm)
    Deepest at ~0.55 µm/min average: 8.5 min; shallowest 1.25 min
    Wait on liner ≈ 7.3 min → loss = 0.8 × 7.3 / S = 5.8 µm / S
    S = 150 → 39 nm (at the limit)
  Group 2: levels 68–135 (4.8–8.5 µm)
    Shallowest 4.8 µm reaches liner at ~9 min; deepest at ~21 min
    Wait ≈ 12 min at the bottom-of-hole rate (~0.4 µm/min)
    → loss = 0.4 × 12 / S = 4.8 µm / S; S = 150 → 32 nm

Two groups plus a modestly thicker liner (or raised pads) bring the
problem into reach.
```

### 16.3.4 What the Staircase Contributes

```
Staircase property            Effect on contact etch
─────────────────────────────────────────────────────────────────────────
Tread placement               Contact landing on the riser → touches the
                              adjacent word line (upper riser) or runs down
                              to the lower tread (lower edge) → short
Tread oxide uniformity        Variation in the last layer before the word
                              line → variable liner-open overetch
Level count                   Miscount → every contact beyond the error
                              lands on the wrong word line
Riser feet and residues       Liner/fill irregularities at the landing
                              edge
Step-height regularity        Contact depth is the sum of stack layers;
                              systematic thickness signatures shift deep
                              contacts by up to ~100 nm (Chapter 2.3.3)
```

---

## 16.4 Defect Modes and Yield Signatures

```
Defect / failure               Staircase root cause         Electrical signature
───────────────────────────────────────────────────────────────────────────────────
Miscount (missed or doubled    Underetch, skipped step,     Every level beyond k
  step)                        resume error (Ch. 11)        addresses the wrong WL;
                                                            whole die or region fails
Contact-to-riser short         Tread placement error        WL(k)–WL(k±1) shorts,
                               (Ch. 12), corner arcs        increasing with tread
                               (Ch. 10.5)                   index within a mask;
                                                            peaks at stitch treads
Punch-through                  Thin tread oxide; contact    WL(k)–WL(k+1) shorts at
                               etch (Section 16.3)          shallow levels
Contact open                   Underetched deep contacts;   Open WLs at deepest levels
                               fill density variation
Pillar defects                 Particles, micromasking      Random single-level
                               (Ch. 9.8, 10.4)              shorts/misconnects
Liner path short               Liner–WL isolation           Many WLs shorted together
                               (Section 16.2.2)
Fill voids                     Tall risers (chop schemes)   Shorts between adjacent
                                                            contacts along a void
High WL resistance at          Thin or damaged tread         Slow program/read on
  staircase                    region; incomplete W fill     specific levels
```

### 16.4.1 Reading the Signature

```
Pattern                                      Points to
──────────────────────────────────────────────────────────────────────────
Failures periodic in level (every 9th, at    Stitch treads (Ch. 12.5)
  each mask boundary)
Failures rising with tread index inside      Trim-rate bias (systematic
  each mask                                  placement error)
Failures at the wafer edge, outer treads     Edge trim rate, edge
                                             temperature (Ch. 13.3.4)
Failures at one staircase side only          Layout asymmetry or contact
                                             overlay
Abrupt level offset from tread k outward     Missed etch k (Ch. 11.2.4)
Failures by chamber                          Chamber matching (Ch. 5.6)
Shallow-level shorts only                    Contact punch-through
```

---

## 16.5 Throughput and Cost of Ownership

### 16.5.1 Cost per Etch Chamber Pass

```
Platform (6 chambers), illustrative:
  Capital                     $12 M, 5-year depreciation → $2.4 M/yr
  Maintenance and parts       $0.8 M/yr
  Consumables (rings, liners,
    ESC refurbishment)        $0.4 M/yr
  Gases, power, facilities    $0.2 M/yr
  ─────────────────────────────────────────
  Total                       $3.8 M/yr

Throughput: 15.6 trim–etch passes/h (Chapter 5.5)
  Annual passes: 15.6 × 612 h/month × 12 = 114,600

Cost per trim–etch pass: $3.8 M / 114,600 ≈ $33
```

### 16.5.2 Cost per Mask Step

```
Step type         Litho (thick     Etch          Strip/clean    Total
                  resist)                        + metrology
─────────────────────────────────────────────────────────────────────────
Trim–etch mask    $40              $33           $10            $83
Shallow chop      $40              $15           $10            $65
Deep chop         $45              $60           $15            $120
  (sub-stepped,
  counted)
```

### 16.5.3 Scheme Comparison (136 Levels)

```
                       Scheme A          Scheme B           Scheme C
                       (single-layer)    (4-pair + Y-chops) (+ X-chops)
──────────────────────────────────────────────────────────────────────────
Trim–etch masks        16 × $83          4 × $83            1 × $83
Shallow chops          —                 2 × $65            2 × $65
Deep chops             —                 —                  2 × $120
Process cost/wafer     $1,328            $462               $453

Staircase length       82 µm             21 µm              25 µm
Area fraction          3.9%              1.0%               1.2%
  (2 staircases,
  4 mm array)
Area value at          $156              $40                $48
  $4,000/wafer
Total (process +       $1,484            $502               $501
  area)
Main risk              Tool count        Y-chop overlay     Deep-chop count,
                                                            tall risers, fill
```

Schemes B and C cost about one-third of Scheme A. Between them, the cost difference is small, so the choice rests on risk and on capability: whether the fab has reliable deep-etch counting and void-free fill over tall risers. As layer counts rise toward 300, Scheme A scales out of reach entirely, while the gap between B-type and C-type schemes widens in favor of more chops:

```
Rough scaling to 310 levels (illustrative):
  A: 35 masks → ~$2,900 + area ~$340
  B: 9 trim–etch masks + 2 chops → ~$880 + area ~$90
  C: 2 trim–etch masks + 2 Y-chops + 3 X-chops → ~$650 + area ~$110
```

### 16.5.4 Yield Weight

```
Yield is worth more than process cost:
  Wafer value $4,000; 1% staircase-related yield loss = $40 per wafer,
  about the cost of one trim–etch mask.

  A scheme that saves $20/wafer in process cost but adds 0.5% yield loss
  is break-even. Deep-chop schemes must earn their place by keeping
  miscount and void risk below that level.
```

---

## 16.6 Principles of Staircase Etch

The chapters of this book reduce to a short list:

1. **The trim draws the treads.** Tread width is lateral trim. Trim is radical-limited ashing, so temperature, oxygen flux, loading, and wall fluorine set the dimension (Chapters 3, 4, 8, 9, 13).

2. **Placement accumulates.** Each tread edge is the sum of the trims before it. Systematic and spatial errors, linear in tread index, dominate (Chapter 12).

3. **The selectivity counts the layers.** Two-step selective etching corrects overetch automatically but cannot recover from underetch. Overetch generously, and confirm every clearing (Chapters 11, 15).

4. **The resist budget sets the price.** Steps per mask come from resist thickness divided by trim loss, and the trim ratio is the lever. Placement limits how far the budget can usefully be stretched (Chapters 3, 6, 12).

5. **The chamber alternates between two worlds.** Fluorocarbon etch and oxygen trim leave opposite wall states, and every step inherits the last (Chapters 7, 9).

6. **Area is the economic driver.** Multi-layer steps, chop masks, and split cells break the linear growth of staircase length and cost with layer count (Chapters 1, 14, 16).

7. **The staircase serves the contact.** Tread placement, tread oxide, level count, and riser cleanliness are judged by word-line contact landing (Chapter 16).

---

## 16.7 Summary & Key Takeaways

1. **Post-staircase steps can undo staircase work.** Fill voids, liner paths, and support-pillar placement all interact with staircase geometry.

2. **The liner must not connect word lines.** Replacement gate can turn a nitride liner touching riser edges into a metal short path.

3. **Single-step contact etch to all levels is punch-through limited.** Shallow contacts wait on the liner for most of the etch. Depth groups, raised pads, and graded liners make it workable.

4. **Failure signatures point to causes.** Periodic, index-dependent, edge, side, and chamber patterns each trace to a specific part of the staircase process.

5. **Advanced schemes cost about a third as much.** Most of the saving is in masks and chamber time, and area adds to it.

6. **Yield outweighs process cost.** A percent of yield is worth about a trim–etch mask.

---

## Study Questions

1. The deepest word-line contact is 10 µm and the shallowest 0.8 µm, with ARDE rates of 0.35 and 0.85 µm/min respectively. Compute the shallow contacts' wait on the liner and the liner loss at S = 200. How thick must the liner be?

2. Split the contacts of Question 1 into three depth groups of equal depth range. Estimate the maximum wait on the liner in each group, assuming rates fall linearly with depth from 0.85 to 0.35 µm/min. What selectivity keeps liner loss below 30 nm?

3. Explain the liner–word-line short mechanism in your own words. Which riser-profile defect from Chapter 10 could defeat an oxide-first liner remedy, and why?

4. Failure analysis shows WL–WL shorts concentrated at levels 9, 18, 27, ... and rising slightly with level inside each group of nine. Identify two root causes and the metrology you would request to confirm them.

5. Recompute the cost of Scheme B for 232 levels using the reference budget (9 X-steps per mask, m = 4, two Y-chops). Compare it with a Scheme C using 2 trim–etch masks, 2 Y-chops, and 2 X-chops.

6. A deep-chop scheme saves $35 per wafer over a Scheme B-type process. What maximum additional yield loss can it tolerate at a wafer value of $4,500?

---

**Previous Chapter:** [Chapter 15: Layer Counting, Metrology & Advanced Process Control](./15-layer-counting-metrology-apc.md)

---

**Chapter 16 Development Status:** Complete  
**Version:** 1.0
