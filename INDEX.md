# Index: Book #23 Navigation Guide

## Quick Navigation

**Total Content:** 16 chapters + 7 appendices + glossary  
**Estimated Read Time:** 22–30 hours for the complete book; 6–10 hours for a focused reading path

| Part | Chapters | Theme |
|------|----------|-------|
| I | 1–4 | Fundamentals: architecture, stack, trim–etch physics, chemistry |
| II | 5–9 | Hardware: reactor, ion energy, gas switching, temperature and chuck, walls |
| III | 10–14 | Phenomena: profile, landing, resist budget and error, loading, advanced schemes |
| IV | 15–16 | Production: layer counting, metrology, APC, contacts, integration, cost |

---

## Part I: Fundamentals (Chapters 1–4)

### Chapter 1: [3D NAND Architecture & the Word-Line Staircase](./chapters/01-staircase-architecture.md)
**Estimated Time:** 60 min | **Difficulty:** Foundation | **Reading Level:** All roles  
**Focus:** Why does 3D NAND need a staircase, and what must it deliver?

**Key Topics:**
- Planar NAND to vertical NAND: word lines become buried sheets
- The staircase as a wiring solution: one tread per word line
- Tread width, riser height, staircase length, and area penalty
- Where the staircase sits in the flow and on the die
- Specification sheet for a modern staircase

**Prerequisites:** None (foundational)  
**Cross-References:** Contact-Hole Etch companion volume (word-line contacts)  
**Critical Equations:** Staircase length L = N·w; area fraction; landing margin  
**Study Questions:** 5 calculations on staircase length, area, and specification

---

### Chapter 2: [The Alternating Stack — Materials, Deposition & Stress](./chapters/02-stack-materials.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Process/Integration roles  
**Focus:** What is the staircase cut into, and how does the stack set up the etch?

**Key Topics:**
- SiO₂/Si₃N₄ (ON) and SiO₂/poly-Si (OP) stacks
- PECVD deposition: thickness control, hydrogen, interface quality
- Layer thickness variation and its effect on landing
- Film stress, wafer bow, and warpage
- Select gates, dummy layers, and the top and bottom of the stack

**Prerequisites:** Chapter 1  
**Cross-References:** Silicon Nitride Etch companion volume; Books #6–10  
**Critical Equations:** Stoney bow; cumulative thickness variation; etch-time variation  
**Data Tables:** Stack film properties (Appendix A)  
**Study Questions:** 5 calculations on stack thickness, stress, and bow

---

### Chapter 3: [Trim–Etch Physics — Building Steps From Resist Pullback](./chapters/03-trim-etch-physics.md)
**Estimated Time:** 90 min | **Difficulty:** Advanced | **Reading Level:** Process/Research roles  
**Focus:** How does one resist block make many steps?

**Key Topics:**
- Geometry of one trim–etch cycle
- Isotropic trim: lateral and vertical loss, trim ratio
- Resist loss during the pair etch
- Steps per mask from the resist budget
- Cumulative edge position and two-dimensional corner rounding

**Prerequisites:** Chapters 1–2; Book #20 (ashing kinetics)  
**Cross-References:** Books #3, #5 (ion-surface interactions)  
**Critical Equations:** N_max = (T₀ − T_min)/(r·w + δ_E); x_k = x₀ − Σ w_i; corner radius R_k = Σ w_i  
**Study Questions:** 6 calculations on cycle geometry and resist budget

---

### Chapter 4: [Pair-Etch and Trim Chemistries](./chapters/04-etch-trim-chemistries.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process/Research roles  
**Focus:** Which gases etch the pair, which gases trim the resist, and why?

**Key Topics:**
- C₄F₆/C₄F₈/O₂/Ar oxide etch selective to nitride
- CH₃F, CH₂F₂, CHF₃ with O₂ for nitride selective to oxide
- Single-step non-selective pair etch
- O₂ trim with N₂, CF₄, and H₂O additives
- The fluorinated resist crust and trim induction time

**Prerequisites:** Chapter 3; Books #6–10 (fluorocarbon chemistry)  
**Cross-References:** Book #20 (O₂ plasma); Book #22 (nitride etch selective to oxide)  
**Critical Equations:** Effective F/C ratio; trim rate vs. O-atom flux; selectivity  
**Data Tables:** Reaction products and volatility (Appendix B)  
**Study Questions:** 5 calculations on chemistry selection and selectivity

---

## Part II: Hardware Design (Chapters 5–9)

### Chapter 5: [Reactor Architecture for Trim–Etch Sequences](./chapters/05-reactor-architecture.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Equipment/Process roles  
**Focus:** What reactor alternates cleanly between anisotropic etch and isotropic trim?

**Key Topics:**
- In-situ versus split-chamber trim–etch
- ICP, CCP, and remote sources for trim
- Ion flux, radical flux, and decoupling
- Throughput of long cycle sequences
- Platform configuration and chamber matching

**Prerequisites:** Chapters 1–4  
**Cross-References:** Books #11–15 (reactor engineering)  
**Critical Equations:** Cycle time; residence time; throughput model  
**Study Questions:** 5 calculations on reactor selection and throughput

---

### Chapter 6: [Ion Energy & Bias Control for Layer-by-Layer Etch](./chapters/06-ion-energy-control.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Equipment/Research roles  
**Focus:** How do we etch exactly one layer and stop, and keep the trim ion-free?

**Key Topics:**
- Ion energy windows for selective oxide and nitride steps
- Self-bias, plasma potential, and residual ion energy during trim
- Pulsed bias and tailored waveforms for landing
- Ion-driven resist erosion and faceting during the pair etch

**Prerequisites:** Chapter 3; Books #1–5  
**Cross-References:** Books #11–15 (RF delivery and pulsing)  
**Critical Equations:** Y(E) = A(√E − √E_th); selectivity vs. energy; time-averaged energy  
**Study Questions:** 5 calculations on ion energy design

---

### Chapter 7: [Gas Switching, Pressure & Step Transitions](./chapters/07-gas-switching-pressure.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Process/Equipment roles  
**Focus:** How do we move between etch and trim without corrupting either?

**Key Topics:**
- Trim and etch operating regimes
- Gas exchange time, residence time, and pressure settling
- Plasma-on versus plasma-off transitions
- Residual oxygen and residual fluorocarbon
- Center/edge gas tuning for trim uniformity

**Prerequisites:** Chapters 4–5  
**Cross-References:** Books #11–15 (gas delivery)  
**Critical Equations:** Exponential gas exchange; pump-down time; transition overhead  
**Study Questions:** 5 calculations on switching time and transients

---

### Chapter 8: [Wafer Temperature, Bow & Electrostatic Chuck Design](./chapters/08-wafer-temperature-esc.md)
**Estimated Time:** 70 min | **Difficulty:** Intermediate | **Reading Level:** Equipment/Process roles  
**Focus:** Why is a staircase tread a thermometer?

**Key Topics:**
- Arrhenius sensitivity of trim rate
- Wafer heat balance in trim and etch
- Multi-zone ESCs and radial trim tuning
- Chucking strongly bowed wafers
- Thermal transients over alternating steps

**Prerequisites:** Chapters 3–7  
**Cross-References:** Book #20 (ashing temperature)  
**Critical Equations:** d(ln R)/dT = E_a/(kT²); wafer heat balance; He gap conductance  
**Study Questions:** 5 calculations on thermal design

---

### Chapter 9: [Chamber Conditioning & Wall Memory](./chapters/09-chamber-conditioning.md)
**Estimated Time:** 60 min | **Difficulty:** Intermediate | **Reading Level:** Equipment/Manufacturing roles  
**Focus:** How does each step inherit the wall left by the last one?

**Key Topics:**
- Fluorocarbon deposits from pair etch, oxygen cleaning during trim
- Fluorine release during trim and tread-oxide loss
- First-cycle and first-wafer effects
- Seasoning, waferless autoclean, and PM recovery
- Wall materials and particle control

**Prerequisites:** Chapters 4, 7  
**Cross-References:** Book #20 (chamber coatings)  
**Critical Equations:** Wall-loss probability and radical density; wall inventory balance  
**Study Questions:** 5 calculations on wall memory and drift

---

## Part III: Process Phenomena (Chapters 10–14)

### Chapter 10: [Step Profile Control — Edge Taper, Footing & Corner Rounding](./chapters/10-step-profile-control.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** How do we make treads flat, risers steep, and corners usable?

**Key Topics:**
- Riser angle and usable tread width
- Resist edge slope and transfer into the step
- Footing, stringers, and residues at step bases
- Corner rounding and its effect on layout

**Prerequisites:** Chapters 3–4, 6  
**Critical Equations:** Usable tread w_u = w − p·cot(θ); corner radius; footing clearance  
**Study Questions:** 6 calculations on profile and usable area

---

### Chapter 11: [Layer Landing, Selectivity & Step-Height Accuracy](./chapters/11-layer-landing-selectivity.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** How does every step stop on the right layer?

**Key Topics:**
- Landing requirement and tread-oxide budget
- Overetch sizing for each half of the pair etch
- Repeated exposure of outer treads
- Missed and doubled steps: mechanisms and probability

**Prerequisites:** Chapters 3–4, 6  
**Cross-References:** Book #22 (nitride etch selective to oxide)  
**Critical Equations:** Required selectivity vs. overetch; per-step miscount probability  
**Study Questions:** 6 calculations on landing and selectivity

---

### Chapter 12: [Resist Budget & Cumulative Placement Error](./chapters/12-resist-budget-error.md)
**Estimated Time:** 80 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** How many steps per mask, and how accurate is the last one?

**Key Topics:**
- Resist budget and its components
- Trim-ratio control and resist choice
- Systematic and random tread-width error
- Cumulative edge-placement error
- Mask-to-mask stitching and overlay

**Prerequisites:** Chapters 3, 8, 10  
**Critical Equations:** σ_x,k = √(k·σ_w² + ...); systematic error k·δ; stitch error budget  
**Study Questions:** 6 calculations on error budgets

---

### Chapter 13: [Loading, Pattern Dependence & Uniformity](./chapters/13-loading-uniformity.md)
**Estimated Time:** 75 min | **Difficulty:** Advanced | **Reading Level:** Process/Integration roles  
**Focus:** Why does tread width depend on where it is?

**Key Topics:**
- Macroloading of trim by resist area
- Changing open area during a sequence
- Within-wafer, extreme-edge, and wafer-to-wafer uniformity
- Die-level layout effects
- Compensation strategies

**Prerequisites:** Chapters 3, 7–8, 12  
**Cross-References:** Book #20 (ashing loading)  
**Critical Equations:** Loading model R = R₀/(1 + κ·A_r); radial trim profile  
**Study Questions:** 5 calculations on loading and compensation

---

### Chapter 14: [Advanced Staircases — Chop Masks, Split Cells & Multi-Deck](./chapters/14-advanced-staircase.md)
**Estimated Time:** 100 min | **Difficulty:** Expert | **Reading Level:** Process/Integration/Research roles  
**Focus:** How do we make 200+ landing levels without a 200-step staircase?

**Key Topics:**
- Multi-layer steps and binary chop masks
- Split-cell (two-dimensional) staircases
- Multi-deck stacks and inter-deck staircases
- Hard-mask staircases, ALE-assisted landing, and other alternatives
- Choosing a scheme: masks, cycles, area

**Prerequisites:** Chapters 1–13  
**Cross-References:** Book #19 (carbon hard masks)  
**Critical Equations:** Levels = N_x·2ⁿ; length reduction; mask-count model  
**Study Questions:** 6 calculations on advanced schemes

---

## Part IV: Production Scale (Chapters 15–16)

### Chapter 15: [Layer Counting, Metrology & Advanced Process Control](./chapters/15-layer-counting-metrology-apc.md)
**Estimated Time:** 75 min | **Difficulty:** Intermediate | **Reading Level:** Process/Manufacturing roles  
**Focus:** How do we know each step landed, and that each tread is where it should be?

**Key Topics:**
- OES signatures of oxide and nitride clearing
- Layer counting and miscount detection
- Tread-width, step-height, and edge-placement metrology
- Feed-forward and feedback APC on trim

**Prerequisites:** Chapters 3–4, 11–13  
**Cross-References:** Books #11–15 (endpoint)  
**Critical Equations:** Signal-to-noise vs. open area; EWMA controller  
**Study Questions:** 5 calculations on endpoint and APC

---

### Chapter 16: [Word-Line Contacts, Integration, Yield & Cost of Ownership](./chapters/16-contacts-integration-yield-coo.md)
**Estimated Time:** 80 min | **Difficulty:** Intermediate | **Reading Level:** All roles  
**Focus:** How does the staircase serve the rest of the flow and the bottom line?

**Key Topics:**
- Staircase fill and CMP
- Replacement gate at the staircase
- Word-line contact etch to many depths at once
- Defect signatures and yield correlation
- Throughput, mask count, and cost-of-ownership model

**Prerequisites:** Chapters 1–15  
**Cross-References:** Contact-Hole Etch companion volume; Book #20 (residue)  
**Critical Equations:** Contact overetch ratio; cost per wafer; scheme cost comparison  
**Study Questions:** 5 calculations on integration and cost

---

## Appendices

| Appendix | Title | Use |
|----------|-------|-----|
| [A](./appendices/A-stack-material-properties.md) | Stack Material Properties | Density, stress, etch rates, resist properties |
| [B](./appendices/B-chemistry-reaction-data.md) | Chemistry & Reaction Data | Gas properties, products, trim kinetics |
| [C](./appendices/C-standard-procedures.md) | Standard Operating Procedures | Daily checks, qualification, PM recovery |
| [D](./appendices/D-process-windows.md) | Process Windows & Lookup Tables | Starting recipes, sensitivities |
| [E](./appendices/E-staircase-geometry-calculations.md) | Staircase Geometry & Error-Budget Calculations | Worked derivations |
| [F](./appendices/F-endpoint-metrology-reference.md) | Endpoint & Metrology Reference | OES lines, metrology capabilities |
| [G](./appendices/G-troubleshooting-guide.md) | Troubleshooting Guide | Symptom → cause → action |

Glossary: [GLOSSARY.md](./GLOSSARY.md)

---

## Suggested Reading Paths

### Process Engineer (≈10 hours)
1 → 3 → 4 → 10 → 11 → 12 → 13 → Appendix D, E, G

### Equipment Engineer (≈9 hours)
1 → 3 → 5 → 6 → 7 → 8 → 9 → 15 → Appendix C, F

### Integration Engineer (≈8 hours)
1 → 2 → 12 → 14 → 16 → Appendix A, E, G

### Device Engineer (≈5 hours)
1 → 2 → 11 → 16

### Researcher (≈10 hours)
3 → 4 → 6 → 9 → 14 → Appendix B

---

## Cross-Reference Map to Other Books

| Book | Topic | Relevant Chapters |
|------|-------|-------------------|
| Books #1–5 | Plasma Physics Fundamentals | Ch. 3, 5, 6 |
| Books #6–10 | Dielectric & Fluorocarbon Etch | Ch. 4, 11 |
| Books #11–15 | Advanced Plasma Engineering | Ch. 5, 6, 7, 15 |
| Book #19 | Carbon Hard Mask Etch | Ch. 14 |
| Book #20 | Photoresist Ashing | Ch. 3, 4, 8, 9, 13 |
| Book #22 | Spacer Etchback | Ch. 4, 11 |
| Companion | Silicon Nitride Etch | Ch. 2, 4, 11 |
| Companion | Contact-Hole Etch | Ch. 16 |

---

## Study Questions Summary

**Total Study Questions:** 5–6 per chapter × 16 chapters ≈ 86 questions  
**Nature:** Mostly calculation-based  
**Topics:** Staircase geometry, resist budget, trim kinetics, landing and selectivity, error accumulation, loading, mask schemes, APC tuning, cost analysis

Examples:
- Compute staircase length and die-area fraction for a given layer count and tread width
- Size steps per mask from resist thickness, trim ratio, and pair-etch resist loss
- Estimate tread-width change from a 1 °C wafer temperature shift
- Compute cumulative edge-placement error after k trims with systematic and random components
- Count masks and cycles for a chop-mask, split-cell staircase
- Compare cost per wafer for two staircase schemes

---

## How to Use This Index

1. **First time?** Read PREFACE.md, then this INDEX, then Chapter 1.
2. **Focused reading?** Pick your role from the reading paths above.
3. **Reference mode?** Jump to the chapter. Use Appendix G for symptoms.
4. **Deep dive?** Read Chapters 1–16 in order and work the study questions.

---

**Index Version:** 1.0  
**Last Updated:** 2026-10-04  
**Next:** Begin [Chapter 1: 3D NAND Architecture & the Word-Line Staircase](./chapters/01-staircase-architecture.md)
