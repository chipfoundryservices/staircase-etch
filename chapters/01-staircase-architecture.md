# Chapter 1: 3D NAND Architecture & the Word-Line Staircase

## Overview

In planar NAND flash, every word line was a strip of polysilicon patterned on the wafer surface. Contacting it was routine: one contact at the end of each strip, all at the same depth. When the industry turned NAND on its side and stacked the cells vertically, the word lines became horizontal sheets buried at different depths in a tall stack of alternating films. The memory string now runs vertically through all of them. Every sheet still needs its own wire to a decoder transistor.

The **word-line staircase** is the structure that makes this possible. At the edge of the memory array, the stack is cut into terraces so that each buried layer has a patch of exposed surface, its **tread**. A vertical contact lands on each tread. Making those terraces is staircase etch.

This chapter explains why 3D NAND needs a staircase, what geometry it must have, what it costs in die area, and where it sits in the process flow. It ends with a specification sheet that frames the rest of the book.

**Learning Objectives:**
- Explain how vertical NAND turns word lines into buried layers that need individual landing pads
- Define tread width, riser height, staircase length, and area penalty
- Compute the staircase length and die-area fraction for a given layer count and tread width
- Derive the minimum tread width from contact size, overlay, and placement error
- Place staircase etch in the 3D NAND process flow
- State the key specifications that a staircase etch must meet

---

## 1.1 From Planar to Vertical NAND

### 1.1.1 The Scaling Wall

Planar NAND reached its lateral limits in the mid-2010s at half-pitches near 15 nm. Below that, the floating gates held too few electrons, neighboring cells coupled too strongly, and lithography cost per bit stopped falling. The way forward was to stop shrinking the cell sideways and start stacking it upward.

### 1.1.2 The Vertical String

In vertical (3D) NAND, the cell string runs perpendicular to the wafer:

```
Planar NAND string                    Vertical NAND string

  WL0  WL1  WL2  WL3                  ┌─┐  ← bit-line contact
  ┌┐   ┌┐   ┌┐   ┌┐                   │ │  SGD (drain select)
──┴┴───┴┴───┴┴───┴┴──  channel        │ │  WL127
  (word lines side by side            │ │  WL126
   on the surface)                    │ │   ...
                                      │ │  WL1
                                      │ │  WL0
                                      │ │  SGS (source select)
                                      └─┘  ← source
                                  (channel runs vertically through
                                   a stack of word-line layers)
```

The stack is built by depositing alternating layers. In the dominant **replacement-gate** scheme, the stack is SiO₂/Si₃N₄ ("ON"). Channel holes are etched through it and filled with the charge-trap layers and a polysilicon channel. Later, the nitride is removed through slits and replaced by tungsten (or molybdenum) word lines. In the older **gate-first** scheme, the stack is SiO₂/poly-Si ("OP"), and the polysilicon layers are the word lines from the start.

Either way, **each word line is a horizontal sheet at a specific depth**, shared by every string in a block.

### 1.1.3 Bit Density Through Layers

Bit density in 3D NAND scales mainly with layer count:

```
Generation (approx.)    Word-line layers   Decks   Stack height (approx.)
──────────────────────────────────────────────────────────────────────────
First commercial         24–32              1        ~1.5–2 µm
Early mainstream         48–64              1        ~3–4 µm
Mid generations          96–128             1–2      ~5.5–7.5 µm
Recent                   176–236            2        ~8–12 µm
Leading edge             280–300+           2–3      ~12–16 µm

(Layer counts include only word lines; select gates and dummy layers
 add several more pairs. Heights are illustrative.)
```

Every layer added to the stack is another word line that needs a landing pad. **The staircase grows with the stack.**

---

## 1.2 The Wiring Problem

### 1.2.1 Every Layer Needs a Contact

Each word line connects to a row-decoder pass transistor. The connection runs up from the word line through a contact, then over a metal routing layer to the decoder. Because word lines are stacked, a single contact landing on the top of the stack can only reach the top layer. Reaching layer *k* means reaching down through the *k − 1* layers above it without shorting to any of them.

### 1.2.2 Options for Reaching Buried Layers

```
Approach                         How it reaches layer k              Status
─────────────────────────────────────────────────────────────────────────────────
Staircase (terraced edge)        Expose a tread of layer k;          Industry
                                 contact lands from above            standard
Insulated deep contact           Etch through layers above, line     Explored;
  (through-stack)                the hole with insulator, open       difficult
                                 only at layer k                     selectivity
Edge contact per layer           Contact each layer's sidewall       Research
  (sidewall wiring)              at the stack edge
```

The staircase won because it turns a three-dimensional wiring problem into a simple rule: **every layer gets a patch of top surface, and every contact is a plain vertical contact landing on a horizontal surface.** The cost of that simplicity is the staircase itself: the etch sequence that makes it and the die area it occupies.

### 1.2.3 What the Staircase Looks Like

```
Cross-section along the word-line direction (simplified, 6 layers):

      memory array                 staircase region
  ◄──────────────────────►◄────────────────────────────────►

  ═══════════════════════╗                          ← WL5 (top)
  ═══════════════════════╩═══╗                      ← WL4
  ═══════════════════════════╩═══╗                  ← WL3
  ═══════════════════════════════╩═══╗              ← WL2
  ═══════════════════════════════════╩═══╗          ← WL1
  ═══════════════════════════════════════╩═══╗      ← WL0
  ───────────────────────────────────────────╨───── substrate / source
                          │   │   │   │   │   │
                          T5  T4  T3  T2  T1  T0    ← treads
                          ◄w► ◄w► ◄w► ◄w► ◄w► ◄w►

After fill and contact:

                          ┃   ┃   ┃   ┃   ┃   ┃     ← word-line contacts
                          ┃   ┃   ┃   ┃   ┃   ┃       (deeper to the right)
```

Each tread is the top surface of one pair. The **riser** between treads is one pair thick. The contact for word line *k* lands on tread *k*.

---

## 1.3 Staircase Geometry

### 1.3.1 Definitions

```
Symbol     Name                    Typical (illustrative)
──────────────────────────────────────────────────────────────────────
p          Pair pitch (riser       50–65 nm (reference: 55 nm)
           height per layer)
w          Tread width             0.4–1.2 µm (reference: 0.60 µm)
N          Number of landing       ~70–320 per deck stack, including
           levels                  select and dummy layers
L          Staircase length        L ≈ N · w (single-row staircase)
θ          Riser angle             70–88° from horizontal
d_c        Contact diameter        0.12–0.30 µm at landing
```

### 1.3.2 Staircase Length

For a simple single-row staircase, where each tread is one layer below the last:

```
L = N · w + L_margin

where L_margin covers the boundary to the array and the dummy region
at the far end.

Example (reference process, single-row):
  N = 128 word lines + 8 select/dummy levels = 136 levels
  w = 0.60 µm
  L = 136 × 0.60 = 81.6 µm (plus margins)

Example (300-layer stack, same tread):
  N ≈ 310 levels
  L = 310 × 0.60 = 186 µm
```

Length grows **linearly with layer count**. That is the central economic problem of staircase design.

### 1.3.3 Area Penalty

The staircase occupies the full width of each block along the bit-line direction. Its area fraction relative to the array is roughly the ratio of staircase length to array length along the word line:

```
f_area ≈ (n_s · L) / (L_array + n_s · L)

where n_s = number of staircases per array (1 or 2)
      L_array = array length along the word-line direction

Example: L_array = 4.0 mm, n_s = 2 (staircase at both ends)

  128-layer, single-row:  L = 81.6 µm
    f_area = 163.2 / 4163.2 = 3.9%

  300-layer, single-row:  L = 186 µm
    f_area = 372 / 4372 = 8.5%
```

Several percent of die area spent on non-storing silicon is a direct cost penalty, and it rises with every generation. Most of the advanced schemes in Chapter 14, including multi-layer steps, chop masks, and split-cell staircases, exist to cut L by a factor of 2–8.

### 1.3.4 Riser Height and Stack Height

```
Total staircase height (single deck):

  H_stack = N · p

  Reference: 136 × 55 nm = 7.48 µm

The deepest tread sits ~7.5 µm below the top of the stack. The
word-line contact to it must be ~7.5 µm deep plus the overburden
of the fill dielectric (Chapter 16).
```

---

## 1.4 Landing Requirements and Minimum Tread Width

### 1.4.1 The Landing Budget

A contact must land fully on its tread with margin from both the riser above it (the edge toward the array, where the next layer up begins) and the step edge below it (where the tread ends and the riser drops to the next layer down). Landing too close to the upper riser risks touching the layer above. Landing over the lower edge risks the contact running down the riser into the layer below.

```
Top view of one tread (word-line direction →):

  riser up                                     step edge down
     │◄──────────────── w ─────────────────────►│
     │   m_up   │◄──── d_c ────►│   m_down      │
     │          │    contact    │               │
     │          │               │               │
```

The tread must be at least:

```
w_min = d_c + m_up + m_down

where each margin must cover:
  δ_OL     contact-to-staircase overlay error (3σ)
  δ_TP     tread edge placement error (3σ, from cumulative trim)
  δ_taper  horizontal width lost to riser taper
  δ_clr    minimum electrical clearance
  δ_CD     contact CD variation (half-range)
```

### 1.4.2 Worked Example

```
Contact:                  d_c = 0.20 µm at landing
Overlay (3σ):             δ_OL = 0.030 µm
Tread placement (3σ):     δ_TP = 0.060 µm (each edge)
Riser taper (θ = 80°):    δ_taper = p · cot(θ) = 55 nm × 0.176 = 0.010 µm
Clearance:                δ_clr = 0.050 µm
Contact CD half-range:    δ_CD = 0.015 µm

Each margin (adding the errors linearly, conservative):
  m = 0.030 + 0.060 + 0.010 + 0.050 + 0.015 = 0.165 µm

w_min = 0.20 + 2 × 0.165 = 0.53 µm

Adding the independent errors in root-sum-square instead:
  √(0.030² + 0.060² + 0.015²) = 0.069 µm
  m = 0.069 + 0.010 + 0.050 = 0.129 µm
  w_min = 0.20 + 2 × 0.129 = 0.46 µm
```

The reference tread of 0.60 µm leaves some margin over either estimate. The example also shows where the leverage is. **Tread placement error is the largest single term**, and it comes almost entirely from the cumulative trim (Chapter 12). Every nanometer of trim control converts into tread width that can be given back as die area.

### 1.4.3 Tread Width vs. Area

```
Reducing w from 0.60 to 0.45 µm (128-layer, single row, 2 staircases,
L_array = 4.0 mm):
  L: 81.6 → 61.2 µm
  f_area: 3.9% → 3.0%

That is ~0.9% of array-plus-staircase area recovered, worth roughly
the same fraction of die cost, on every die, for the life of the product.
```

---

## 1.5 Where the Staircase Sits

### 1.5.1 On the Die

```
Plan view (one plane, simplified):

  ┌──────┬────────────────────────────────────────┬──────┐
  │      │                                        │      │
  │ stair│          memory array (blocks)         │stair │
  │ case │   ← word lines run left–right →        │case  │
  │      │                                        │      │
  └──────┴────────────────────────────────────────┴──────┘
     ▲                                                ▲
  row decoder routing                        row decoder routing
  (or decoder under array in CMOS-under-array designs)
```

Common placements:

- **Both ends** of the word lines, with each end contacting half the blocks or half the layers. This shortens each staircase or splits RC delay.
- **One end only**, used when routing favors it.
- **Center staircase**, where the staircase sits in the middle of a plane and word lines run outward from it. This halves the word-line RC.

In CMOS-under-array and wafer-bonded designs, the decoder circuitry sits beneath or beside the array, but the staircase is still needed to bring every word line up to the routing layer.

### 1.5.2 In the Process Flow

```
Representative replacement-gate 3D NAND flow (single deck):

  1. CMOS periphery (or CMOS wafer for bonded designs)
  2. Alternating ON stack deposition                       (Chapter 2)
  3. Hard mask, channel-hole lithography and HAR etch
  4. Channel-hole fill: blocking oxide, charge trap, tunnel
     oxide, poly-Si channel, core fill
  5. STAIRCASE ETCH (multiple trim–etch masks, chop masks) ← this book
  6. Staircase dielectric fill (thick oxide) and CMP       (Chapter 16)
  7. Slit etch through the stack
  8. Nitride removal through slits; W (or Mo) word-line fill
  9. Slit fill / common source
 10. Word-line contact etch on the staircase               (Chapter 16)
 11. Bit-line contacts, interconnect

(The order of steps 3–5 varies. Some flows form the staircase before
 the channel holes.)
```

Note what the staircase etches: **the stack as deposited, before replacement**. The treads are oxide over nitride. The nitride becomes the word line later. The tread oxide over each nitride layer is the layer the pair etch must stop on, and it is the layer the contact must later punch through.

---

## 1.6 Specification Sheet for a Modern Staircase

```
Parameter                        Target (illustrative)          Driven by
─────────────────────────────────────────────────────────────────────────────────
Landing levels                   All N, each exactly once       Function
Missed / doubled steps           0 per wafer (target < 1 ppm    Yield
                                 of steps)
Tread width                      w ± 30 nm (3σ, within mask)    Contact margin
Tread placement (cumulative)     ± 60 nm (3σ) at last tread     Contact margin
                                 of a mask
Riser angle                      ≥ 75–80°                       Usable tread
Tread-oxide remaining            ≥ 15 nm of 25 nm (no nitride   WL integrity,
                                 exposure)                      contact etch
Footing / stringers              None detectable                Shorts
Corner rounding radius           Within layout allowance        Contact placement
Within-wafer tread uniformity    ≤ 2% (1σ) of w                 Edge die yield
Mask-to-mask stitch error        ≤ ± 50 nm (3σ)                 Contact margin
Particles / defects              Fab defect spec                Yield
Throughput                       Platform target (Ch. 5, 16)    Cost
```

Every chapter that follows works toward one or more lines of this table.

---

## 1.7 Summary & Key Takeaways

1. **Vertical NAND buries the word lines.** Each one is a horizontal sheet at a different depth, and each still needs its own contact.

2. **The staircase is the wiring solution.** It gives every layer a tread, so every contact is a simple vertical contact landing on a flat surface.

3. **Length grows with layer count.** A single-row staircase is about N·w long. At 300 layers and 0.6 µm treads, that is nearly 0.2 mm per staircase.

4. **Area is the cost.** Staircases occupy several percent of the array, and that fraction rises each generation unless the scheme changes.

5. **Tread width is a margin budget.** Contact size, overlay, tread placement, and taper set the minimum. Tread placement, which comes from cumulative trim error, is usually the largest term.

6. **The staircase is etched in the as-deposited stack.** Treads are oxide over nitride. The nitride becomes the word line after replacement gate.

---

## Study Questions

1. A 3D NAND deck has 176 word lines plus 10 select and dummy levels, with a tread width of 0.55 µm. Compute the single-row staircase length. If the array is 3.5 mm long along the word line and there are staircases at both ends, what fraction of the array-plus-staircase length is staircase?

2. For the deck in Question 1 with a pair pitch of 58 nm, what is the depth of the deepest tread below the top of the stack? If the fill dielectric adds 0.8 µm of overburden, how deep is the deepest word-line contact?

3. Using the margin method of Section 1.4.2 with root-sum-square addition, compute w_min for d_c = 0.16 µm, δ_OL = 25 nm, δ_TP = 45 nm, θ = 78°, p = 55 nm, δ_clr = 40 nm, and δ_CD = 12 nm.

4. A design team proposes reducing tread width from 0.60 to 0.50 µm on a 236-layer product (246 total levels, two staircases, 4.0 mm array). How much staircase length is saved per side, and what is the change in area fraction? What must happen to tread placement error for the change to keep the same contact margin?

5. Explain why the tread oxide, not the nitride, is the surface each pair etch must stop on in a replacement-gate flow. What would go wrong if the staircase were instead etched to leave nitride exposed on every tread?

---

**Next Chapter:** [Chapter 2: The Alternating Stack — Materials, Deposition & Stress](./02-stack-materials.md)

---

**Chapter 1 Development Status:** Complete  
**Version:** 1.0
