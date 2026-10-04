# Chapter 14: Advanced Staircases — Chop Masks, Split Cells & Multi-Deck

## Overview

A single-layer staircase grows by one tread and one etch for every word line. At 64 layers this was manageable. At 136 levels it means 16 thick-resist masks, 136 etches, about 120 trims, and roughly 80 µm of staircase at each end of the array. At 300 levels it would mean more than 30 masks and nearly 0.2 mm of staircase. No fab builds staircases that way at high layer counts.

This chapter covers the schemes that break the link between layer count and staircase cost. Multi-layer steps etch several pairs per cycle. Chop masks etch selected regions by fixed depths so that the same trim–etch staircase appears at several depth offsets. Split-cell layouts use the width of each block to hold several staircase rows offset by one layer each. Multi-deck stacks add an inter-deck layer to the staircase. The chapter then surveys alternative methods and ends with a comparison framework for choosing a scheme.

**Learning Objectives:**
- Explain why multi-layer steps alone do not give every layer a tread
- Design a binary chop-mask set that produces 2^c depth offsets
- Lay out a split-cell staircase and compute its length reduction
- Combine trim–etch, X-chops, and Y-chops into a complete scheme and count masks, etches, and trims
- Identify the process challenges of deep chop etches: masking, landing, tall risers, and overlay
- Describe staircase options for multi-deck stacks
- Evaluate alternative staircase methods and select a scheme for a given layer count

---

## 14.1 The Scaling Problem

```
Single-layer trim–etch, reference process (9 levels per mask):

Levels   Masks   Etches   Trims   Length (w = 0.60 µm)
──────────────────────────────────────────────────────
  70       8       70      62       42 µm
 136      16      136     120       82 µm
 250      28      250     222      150 µm
 310      35      310     275      186 µm
```

Every column grows linearly. The schemes in this chapter attack mask count, cycle count, and length together.

---

## 14.2 Multi-Layer Steps

### 14.2.1 Etching m Pairs per Cycle

If each etch in a trim–etch sequence removes m pairs instead of one, each tread is m pairs below its neighbor:

```
m = 4: treads at levels 0, 4, 8, 12, ...

Levels per mask with n trims: m · (n + 1)
  Reference (n = 8, Ch. 3.4.3): 4 × 9 = 36 levels per mask
```

The resist budget barely changes because trim loss dominates (Chapter 3.3).

### 14.2.2 The Catch

A staircase of 4-pair steps exposes only every fourth layer. Layers 1, 2, 3, 5, 6, 7, ... have no tread. Multi-layer steps must be combined with a way to offset some regions by 1, 2, or 3 pairs. That is the job of chop masks.

---

## 14.3 Binary Chop Masks

### 14.3.1 The Idea

A **chop mask** is a resist mask that exposes selected regions for one deep etch of a fixed number of pairs, with no trim. With c chop masks etching 1, 2, 4, ..., 2^(c−1) pairs, every combination of exposures gives a distinct depth offset:

```
c = 2 chop masks: A etches 1 pair, B etches 2 pairs

Region   Exposed to A?   Exposed to B?   Offset (pairs)
──────────────────────────────────────────────────────
Z0       no              no              0
Z1       yes             no              1
Z2       no              yes             2
Z3       yes             yes             3

c chop masks → 2^c offsets: 0, 1, ..., 2^c − 1
```

### 14.3.2 Combining With Multi-Layer Steps

If the staircase is built from steps of m = 2^c pairs and divided into 2^c regions with offsets 0 to 2^c − 1, every level is covered exactly once:

```
Depth at X-step i in region z:  D = m · i + z,  z = 0..m−1

m = 4 (two chop masks), X-steps i = 0..33:
  Levels covered: 0..135 (136 levels) ✓
```

### 14.3.3 Chops in the X-Direction

Chop masks can also multiply X-steps. A trim–etch mask forms several identical short staircases side by side. X-chops then lower whole staircases by multiples of the staircase depth:

```
One trim–etch mask: four identical 9-step staircases (m = 4 pairs/step,
  each spanning 36 levels), placed side by side in x

X-chop C: etches 36 pairs on staircases 2 and 4
X-chop D: etches 72 pairs on staircases 3 and 4

Staircase   Offset (pairs)   Levels covered
──────────────────────────────────────────
S1          0                0–35
S2          36               36–71
S3          72               72–107
S4          108              108–143
```

---

## 14.4 Split-Cell (Two-Dimensional) Staircases

### 14.4.1 Using the Block Width

A word line in a block is a plate that spans the block's whole width between two slits. A contact can land anywhere across that width. A **split-cell** staircase divides each block's staircase region into rows along y, each row offset by one or more layers by chop masks:

```
Plan view of one block's staircase (4 rows, m = 4):

  y ↑   ┌─────┬─────┬─────┬─────┬─────┬─────┐
        │ Z3  │ 3   │ 7   │ 11  │ 15  │ ... │   ← row Z3: offset 3
        ├─────┼─────┼─────┼─────┼─────┼─────┤
        │ Z2  │ 2   │ 6   │ 10  │ 14  │ ... │   ← row Z2: offset 2
        ├─────┼─────┼─────┼─────┼─────┼─────┤
        │ Z1  │ 1   │ 5   │ 9   │ 13  │ ... │   ← row Z1: offset 1
        ├─────┼─────┼─────┼─────┼─────┼─────┤
        │ Z0  │ 0   │ 4   │ 8   │ 12  │ ... │   ← row Z0: offset 0
        └─────┴─────┴─────┴─────┴─────┴─────┘
                 x → (X-steps of 4 pairs, width w each)

Each cell is a landing site for the level shown.
```

### 14.4.2 Length Reduction

```
Single-row length:     L₁ = N · w
Split cell, R rows:    L_R = (N / R) · w + chop-edge allowances

136 levels, w = 0.60 µm:
  R = 1:  L = 81.6 µm
  R = 4:  L = 34 × 0.60 = 20.4 µm (+ ~1–2 µm allowances)
  R = 8:  L = 17 × 0.60 = 10.2 µm (+ allowances)
```

### 14.4.3 What Limits the Number of Rows

```
Constraint                                      Effect
─────────────────────────────────────────────────────────────────────────
Block width (slit pitch, a few µm)              Each row needs contact +
                                                margins in y
Y-chop edge placement (overlay, riser taper)    Each row boundary costs
                                                landing width
Slit placement through the staircase            Rows must avoid slit
                                                exclusion zones
Number of chop masks (log₂ R)                   More masks per extra
                                                doubling
```

### 14.4.4 Trim–Etch Resist for Split Cells

In split-cell schemes, the trim–etch resist edge should run straight across all rows in y. If the trim–etch resist had edges along x inside the staircase region, the trim would also pull them back in y and blur the row boundaries. Row offsets come only from chop masks, which do not trim.

---

## 14.5 A Complete Scheme

### 14.5.1 Three Schemes for 136 Levels

```
Scheme A: single-layer trim–etch
  16 trim–etch masks; 136 etches; 120 trims; L ≈ 82 µm

Scheme B: 4-pair X-steps + 2 Y-chops (4-row split cell)
  Trim–etch masks: ⌈34 / 9⌉ = 4 (34 X-steps of 4 pairs)
  Chop masks: 2 (1 pair, 2 pairs)
  Etches: 34 (4-pair) + 2 chop etches
  Trims: 30
  L ≈ 21 µm

Scheme C: 4-pair X-steps + 2 Y-chops + 2 X-chops
  Trim–etch masks: 1 (four side-by-side 9-step staircases)
  Chop masks: 4 (Y: 1, 2 pairs; X: 36, 72 pairs)
  Etches: 9 (4-pair) + 4 chop etches
  Trims: 8
  L ≈ 4 × 9 × 0.60 + 3 × ~1.0 (X-chop edge allowances) ≈ 25 µm
```

### 14.5.2 Comparison

```
Metric                    Scheme A     Scheme B     Scheme C
─────────────────────────────────────────────────────────────────
Masks                     16           6            5
Trims                     120          30           8
Pair-etch cycles          136          34           9
Deepest single etch       1 pair       4 pairs      72 pairs (~4 µm)
Staircase length          82 µm        21 µm        25 µm
Trim–etch chamber time    ~16 × 23 min ~4 × 24 min  ~1 × 24 min
  (Ch. 5.5)               ≈ 6.1 h      ≈ 1.6 h      ≈ 0.4 h + chops
Main risk                 Cost, area   Y-chop       Deep chop landing,
                                       overlay      tall risers
```

Scheme C moves the difficulty from many easy cycles to a few hard etches. The 36- and 72-pair chop etches are 2–4 µm deep, must land on exact interfaces, and leave tall risers that cost area and complicate contact etch and fill.

---

## 14.6 Deep Chop Etch Challenges

### 14.6.1 Masking

```
Chop depth 72 pairs = 3.96 µm of stack
Stack-to-resist selectivity S_R ≈ 4 (non-selective main etch)
  Resist consumed ≈ 1.0 µm, plus faceting at the edge

Options:
  Thick resist (≥ 4–6 µm) with sloped-edge correction
  Hard mask (e.g., amorphous carbon, Book #19) patterned over the
    existing staircase topography; costs deposition and its own etch
```

### 14.6.2 Landing

From Chapter 11.7, a single timed main etch of 880–1760 nm cannot land within ±15 nm at 3σ. Deep chops need **layer counting**:

```
1. Non-selective main etch with OES monitoring of oxide/nitride
   alternation (Chapter 15): count k interfaces
2. Stop the main etch after (m − 1) full pairs plus most of the m-th
3. Selective oxide and nitride landing steps

Count errors are catastrophic (every level in the chopped region is
shifted), so counting is cross-checked: time window, total depth
estimate, and signal pattern must all agree.
```

### 14.6.3 Tall Risers

```
Riser height 3.96 µm, riser angle 85°:
  Riser run = 3.96 × cot(85°) = 0.35 µm
Riser angle 80°:
  Riser run = 3.96 × cot(80°) = 0.70 µm

Plus overlay (3σ) of the chop mask to the staircase: ~0.03–0.05 µm
Plus foot at the base: ~0.05–0.1 µm

X-chop edge allowance: ~0.5–1.0 µm per edge
```

Tall risers also create deep, narrow gaps for the fill dielectric if they sit close to other features, and they put word-line contacts at very different depths side by side (Chapter 16).

### 14.6.4 Overlay

Each chop edge must fall in a planned gap, not on a landing tread. Chop masks are aligned to a common reference so that their errors do not chain (Chapter 12.5.2). Y-chop edges divide rows and directly reduce each row's landing width:

```
Row width budget (y, illustrative):
  Contact d_c = 0.20 µm + 2 × margin 0.10 µm = 0.40 µm
  Y-chop edge allowance: 2 × (overlay 0.03 + riser run (1–2 pairs,
    ~0.02) + foot 0.02) ≈ 0.14 µm
  Row width ≈ 0.54 µm minimum
  4 rows ≈ 2.2 µm of block width
```

---

## 14.7 Multi-Deck Stacks

### 14.7.1 Why Decks

Channel holes through 200–300+ layers are too deep to etch in one step, so the stack is built in two or three decks. Each deck's channel holes are etched separately, and an inter-deck layer (thicker dielectric, sometimes with a plug landing layer) joins them.

### 14.7.2 Staircase Options

```
Option                            Description                      Notes
──────────────────────────────────────────────────────────────────────────────────
Full-stack staircase after all    One staircase through all decks  Most common; inter-
  decks                                                            deck layer needs its
                                                                   own etch step
Per-deck staircase                Lower deck staircase formed,     Upper deck must be
                                  filled, and planarized before    removed over the
                                  upper deck deposition            lower staircase
                                                                   (deep etch); more
                                                                   process steps
Separate staircase regions        Lower and upper deck treads in   Uses chop-like deep
  per deck                        different x ranges               removal of the upper
                                                                   deck
```

### 14.7.3 The Inter-Deck Level

```
Inter-deck layer (illustrative): 100–200 nm oxide, sometimes with a
  thin poly-Si or nitride landing layer for channel plugs

Staircase implications:
  - A per-level etch step with different chemistry or time
  - Its riser is taller than a normal pair → wider riser run
  - Its tread may have no word line (no contact needed), or carries a
    dummy level
  - In chop schemes, the inter-deck layer breaks the regular pitch, so
    chop depths must be counted in layers, not in nanometers
```

---

## 14.8 Alternative Staircase Methods

```
Method                         Principle                          Status / issues
──────────────────────────────────────────────────────────────────────────────────────
Trim with capped resist        Thin cap on the resist top stops   Research; cap overhang
                               vertical loss (r → ~0)             removal, particles
Trimmable hard mask            Lateral recess of a sacrificial    Process complexity;
                               layer under a protective cap       selectivity to stack
Grayscale (gray-tone)          Sloped or stepped resist profile   Landing control poor
  lithography                  transferred by ~1:1 etch           without selective
                                                                  stops; research
Quasi-ALE landing              Cyclic modify/remove steps to      Slow; useful for final
                               land deep chops precisely          nanometers only
Insulated through-stack         Contacts etched through upper     Avoids staircase area;
  contacts                     layers, lined, opened at target    extremely demanding
                               layer                              selectivity; research
Staircase in CMOS-under-array  Staircase over periphery CMOS      Area reuse; adds
  / bonded designs             (or on a separate bonded wafer)    integration constraints
```

---

## 14.9 Choosing a Scheme

```
Criterion               Favors more trim–etch            Favors more chops
──────────────────────────────────────────────────────────────────────────────
Mask cost               —                                Fewer masks
Chamber time            —                                Far fewer trims
Staircase area          (Split cell reduces both)        Split cell + X-chops
Landing robustness      Many shallow selective etches    —
Placement control       Short sequences per mask         Fewer trims overall
Deep-etch capability    Not needed                       Required (counting,
                                                         tall risers)
Contact etch            Gradual depth progression        Large depth jumps
                                                         between neighbors
```

The industry trend has been toward multi-layer steps with split cells (Scheme B-like) as the base, with X-chops added as layer counts rise, together with investment in layer counting and deep-etch landing. Chapter 16 compares schemes in cost terms.

---

## 14.10 Summary & Key Takeaways

1. **Single-layer staircases do not scale.** Masks, cycles, and length all grow linearly with levels.

2. **Multi-layer steps need offsets.** Steps of m pairs cover every layer only when combined with regions offset by 0 to m − 1 pairs.

3. **Binary chop masks make offsets cheaply.** c chop masks give 2^c offsets.

4. **Split cells use block width.** R rows cut staircase length by about R, limited by block width, chop overlay, and slits.

5. **X-chops replace trim–etch masks with deep etches.** They save masks and chamber time but require layer counting, tall-riser control, and careful overlay.

6. **Multi-deck stacks add an irregular level.** The inter-deck layer needs its own step and breaks the regular pitch for chop depths.

7. **Scheme choice is a cost-and-risk trade.** The best scheme depends on layer count, deep-etch capability, and the cost of masks versus chamber time.

---

## Study Questions

1. Design a chop-mask set for m = 8 (8-pair X-steps). How many chop masks are needed, what depths do they etch, and which of the 8 rows does each expose?

2. For 232 levels with w = 0.55 µm, compute the staircase length for (a) a single row, (b) a 4-row split cell, and (c) an 8-row split cell. Include 0.8 µm of chop-edge allowance per row boundary for (b) and (c) only if the boundaries run in x.

3. Using the reference trim–etch budget (9 X-steps per mask), count masks, trims, and etches for 232 levels with m = 4 and two Y-chops, with and without X-chops (choose the X-chop depths).

4. A 64-pair X-chop has a 2% depth uncertainty (1σ). Show that a single timed main etch cannot land it reliably. How many interfaces must OES layer counting resolve, and what is the consequence of a count error of one?

5. A chop riser of 3.5 µm has angle 82°. Compute the riser run. With overlay 40 nm (3σ) and foot 80 nm, what edge allowance is needed?

6. An inter-deck layer is 150 nm oxide. In a scheme with 4-pair X-steps, describe how the X-step that spans the inter-deck layer differs from a regular step and how the recipe must change.

---

**Previous Chapter:** [Chapter 13: Loading, Pattern Dependence & Uniformity](./13-loading-uniformity.md)  
**Next Chapter:** [Chapter 15: Layer Counting, Metrology & Advanced Process Control](./15-layer-counting-metrology-apc.md)

---

**Chapter 14 Development Status:** Complete  
**Version:** 1.0
