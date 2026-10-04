# Glossary: Staircase Etch

Terms are defined as they are used in this book. Chapter references point to the main discussion.

---

## A

**Activation energy (E_a):** Apparent energy barrier of the resist trim reaction, ~0.3–0.6 eV. Sets the trim's temperature sensitivity, E_a/(k_B·T²), about 6%/°C at 0.45 eV and 20 °C. (Ch. 4.5.1, Ch. 8.1)

**ALE / quasi-ALE (Atomic Layer Etching):** Cyclic etching in self-limiting (or nearly self-limiting) modify and remove half-steps. Proposed for precise landing of deep chop etches. (Ch. 14.8)

**APC (Advanced Process Control):** Run-to-run adjustment of recipe parameters, mainly trim time, using feedback from tread metrology and feed-forward from product, resist, track, and tool data. (Ch. 15.6)

**Array:** The region of the die containing memory strings. The staircase sits at its word-line ends. (Ch. 1.5)

---

## B

**Block:** Group of memory strings sharing a set of word-line plates, bounded by slits. The staircase serves each block. (Ch. 14.4)

**Bow (wafer bow):** Out-of-plane deflection of the wafer, caused mainly by stack stress. Estimated with the Stoney equation. Affects chucking, temperature, lithography, and overlay. (Ch. 2.5, Ch. 8.4)

**Bridge condition:** Intermediate gas condition used during plasma-on transitions so the plasma stays lit while chemistries change. (Ch. 7.3.2)

---

## C

**Chop etch / chop mask:** A deep etch of a fixed number of pairs through a mask that exposes selected regions, with no trim. Binary chop sets of c masks give 2^c depth offsets. *Y-chops* offset rows in split-cell staircases. *X-chops* offset whole staircases along the word-line direction. (Ch. 14.3)

**CMOS-under-array (CuA):** Architecture with periphery circuits beneath the memory array. The staircase is still needed to bring word lines to the routing layer. (Ch. 1.5.1)

**CN emission (388.3 nm):** Emission from CN radicals formed when nitride etches in fluorocarbon or hydrofluorocarbon plasma. Rises when the oxide step reaches nitride and falls when the nitride step clears. The primary layer-clearing signal. (Ch. 15.1)

**CO emission (483.5, 519.8 nm):** Emission from CO. Tracks oxide etching in the etch steps and resist oxidation in the trim. (Ch. 15.1)

**Corner arc:** The curved tread edge formed at an inside corner of the staircase opening. Its radius equals the cumulative trim. (Ch. 3.6.2, Ch. 10.5)

**Corner blunting:** Extra recession of outside (convex) resist corners caused by higher local O flux. (Ch. 3.6.3)

**Corner exclusion zone:** Layout region near opening corners where contacts are not placed, because treads there are curved. (Ch. 10.5.2)

**Crust:** Fluorinated, ion-modified layer on the resist surface left by the pair etch. Thicker on the resist top than on the sidewall. Causes trim induction time. (Ch. 4.6)

**Crust breakthrough:** Short O₂-based step before the timed trim that removes the sidewall crust reproducibly. (Ch. 4.6.4)

**Cumulative placement error:** Error in the position of a tread edge, equal to the sum of the width errors of every trim before it. (Ch. 3.5, Ch. 12.4)

---

## D

**Deck:** One separately deposited and channel-etched portion of a multi-deck stack. (Ch. 14.7)

**Depth group:** A subset of word-line contacts, by depth range, etched with its own mask to limit liner wait time. (Ch. 16.3.3)

**Doubled step:** One etch removing two pairs in some region, putting every tread exposed at that time one level deep. Rare in selective etching. (Ch. 11.5)

**Dummy word line:** Word-line level not used for data storage, placed near select gates or deck interfaces. Still needs a staircase level. (Ch. 2.6)

---

## E

**Effective F/C ratio:** (n_F − n_H)/n_C of a feed gas. Lower values polymerize more. (Ch. 4.1.1)

**Effective (protected) threshold:** The higher apparent ion-energy threshold of a stop layer covered by thick steady-state polymer. Creates the selectivity window of the pair etch. (Ch. 6.2)

**Etch-stop liner:** Conformal film (often SiN) deposited over the finished staircase so word-line contact etch can stop before opening the tread. (Ch. 16.1, 16.2.2)

**EWMA (Exponentially Weighted Moving Average):** Feedback filter used to update the estimated trim rate from measurements. (Ch. 15.6.2)

---

## F

**Field:** The region beyond a mask's starting resist edge, etched in every etch of that mask. It becomes the starting surface of the next mask. (Ch. 3.1.3)

**Floating-wafer ion energy:** Ion energy at an unbiased wafer, ~15 eV for O₂⁺ at T_e = 3 eV, and higher with capacitive coupling. Sets the trim ratio. (Ch. 6.5.1)

**Foot (riser foot):** Unetched material at the base of a riser. Accumulates on outer risers that are etched many times. Not removed by the trim. (Ch. 10.3)

---

## G

**Gate-first:** 3D NAND scheme using an oxide/poly-Si (OP) stack in which the poly-Si layers are the word lines. (Ch. 2.1.2)

---

## I

**IED (Ion Energy Distribution):** Distribution of ion energies at the wafer. A narrow IED improves pair-etch selectivity at a given mean energy. (Ch. 6.3)

**Induction time (t_ind):** Delay before the trim reaches its steady rate, caused by the crust. Variation in t_ind is tread-width noise. (Ch. 4.6.2)

**Inter-deck layer:** Thicker dielectric (sometimes with a plug landing layer) between decks. Needs its own staircase step. (Ch. 14.7.3)

---

## L

**Landing:** Stopping an etch on the correct interface. In a pair etch, the nitride step lands on the next oxide. (Ch. 11.1)

**Layer counting:** Counting oxide/nitride transitions by OES during an etch to confirm or control depth in pairs. (Ch. 15.3)

**Level:** Depth position in pairs below the top of the stack. Each word line needs one level in the staircase. (Ch. 1.3, Ch. 11.1)

**Liner–word-line short:** Failure in which hot phosphoric acid removes a nitride liner through its contact with riser nitride edges, and metal fill then connects word lines through the liner cavity. (Ch. 16.2.2)

**Loading (macroloading):** Dependence of trim rate on total resist area, or of etch rate on exposed area, through consumption of reactive species. (Ch. 13.1, 13.2)

---

## M

**Micromasking:** Residue islands on a newly exposed strip that block the next etch locally, leaving pillars. (Ch. 10.4)

**Missed step:** An etch that fails to clear a layer somewhere, leaving every tread exposed at that time one level shallow. Not healed by later cycles. (Ch. 11.2.2)

**Multi-layer step:** A tread-to-tread height of m pairs, made by etching m pairs per cycle. Must be combined with offsets so every layer has a tread. (Ch. 14.2)

---

## O

**OES (Optical Emission Spectroscopy):** Monitoring of plasma emission. Used for layer clearing, layer counting, trim start detection, and fault detection. (Ch. 15)

**ON stack:** Alternating SiO₂/Si₃N₄ stack used in replacement-gate 3D NAND. (Ch. 2.1.1)

**OP stack:** Alternating SiO₂/poly-Si stack used in gate-first 3D NAND. (Ch. 2.1.2)

**Overetch (OE):** Etch time beyond nominal clearing. Generous OE is safe in intermediate cycles because selective etching self-corrects. (Ch. 11.3)

---

## P

**Pair:** One oxide layer and one nitride (or poly-Si) layer. The unit removed by one pair etch. (Ch. 2.1)

**Pair etch:** The anisotropic etch removing one pair (or m pairs) wherever resist is absent. Two-step selective or single-step non-selective. (Ch. 4.1)

**Pair pitch (p):** Thickness of one pair, which is the riser height of a single-layer step. Reference: 55 nm. (Ch. 1.3.1)

**Pillar defect:** A column of unetched stack left under a particle or residue on a tread, carried down by later etches. (Ch. 9.8.2)

**Plasma-on transition:** Change between steps without extinguishing the plasma, with bias off during the gas change. (Ch. 7.3.2)

**Punch-through:** Contact etch breaking through the liner and tread oxide, through the target word line, and into the next. Most likely at shallow contacts. (Ch. 16.3.2)

---

## R

**Raised landing pad:** Locally thickened layer at contact sites on each tread that becomes thicker metal after replacement gate and absorbs contact overetch. (Ch. 16.3.3)

**Replacement gate:** Removal of stack nitride through slits with hot phosphoric acid and refilling with W or Mo to form word lines. (Ch. 1.1.2, Ch. 16.2)

**Resist aspect ratio:** Resist thickness divided by opening width. High values slow the lateral trim at the base of narrow openings. (Ch. 13.4)

**Resist budget:** Starting resist thickness minus all losses over a mask sequence. Must stay above T_min at the worst point on the wafer. (Ch. 3.4, Ch. 12.1)

**Riser:** The near-vertical face between two treads, one pair (or m pairs) high. (Ch. 1.2.3)

**Riser run:** Horizontal extent of a tapered riser, p·cot θ. (Ch. 10.1)

**Row:** One of the y-direction subdivisions of a split-cell staircase, offset from its neighbors by chop masks. (Ch. 14.4)

---

## S

**Select gate (SGD, SGS):** Drain- and source-side select transistors of the NAND string, formed from stack levels at the top and bottom. Need their own staircase levels. (Ch. 2.6)

**Self-correction:** Property of two-step selective pair etching by which overetch in one cycle is corrected by the next, as long as no step removes two complete layers. (Ch. 11.2)

**Side treads:** Treads formed along the y-directed edges of a staircase opening by the isotropic trim. Dead area unless used by a split-cell scheme. (Ch. 3.6.1, Ch. 10.5.3)

**Slit:** Trench through the stack separating blocks, used for replacement gate and common source. Runs through the staircase region. (Ch. 1.5.2)

**Split cell (two-dimensional staircase):** Staircase in which several rows across the block width are offset by one or more layers, reducing staircase length. (Ch. 14.4)

**Stack:** The full deposited set of alternating layers, plus cap, select-gate, dummy, and inter-deck layers. (Ch. 2)

**Staircase length (L):** Extent of the staircase along the word-line direction, ≈ N·w for a single-row staircase. (Ch. 1.3.2)

**Stitch tread:** The tread between the last edge of one trim–etch mask and the first of the next. Carries two overlay errors and a full mask of trim error. (Ch. 12.5)

**Support pillar (dummy channel hole):** Oxide-filled pillar through the staircase that supports the oxide layers during replacement gate. (Ch. 16.1.3)

---

## T

**T_min:** Minimum resist thickness that can still mask a pair etch reliably. (Ch. 3.4, Ch. 12.1.3)

**Tread:** Flat top surface of one level in the staircase, where a word-line contact lands. (Ch. 1.2.3)

**Tread oxide:** The oxide layer forming each tread's surface, over the nitride that becomes the word line. Must remain ≥ spec after the last etch of a mask. (Ch. 11.1.2)

**Tread width (w):** Distance between successive resist edges, set by one lateral trim. Reference: 0.60 µm. (Ch. 1.3)

**Trim:** Isotropic O₂-based plasma step that pulls the resist edge back by one tread width. (Ch. 3.2, Ch. 4.5)

**Trim–etch:** The repeated sequence of pair etch and resist trim that builds a staircase from one resist mask. (Ch. 3)

**Trim ratio (r):** Ratio of vertical to lateral resist loss in the trim. Typically 1.1–2.0. The main lever on steps per mask. (Ch. 3.2, Ch. 6.5.2)

---

## U

**Usable tread width (w_u):** Flat, clean tread width available for landing, w − p·cot θ − foot − top-edge loss. (Ch. 10.1.1)

---

## W

**WAC (Waferless Autoclean):** Chamber cleaning plasma with no wafer. Staircase chambers need fluorine WAC steps to remove SiOₓF_y and oxygen steps for polymer. (Ch. 9.4.2)

**Wall inventory:** Amount of polymer on chamber walls between steps. Grows in etch and shrinks in trim. Its balance predicts first-cycle effects and drift. (Ch. 9.3)

**Word line (WL):** Horizontal conductor plate at one level of the stack, shared by all strings in a block. Each needs one staircase level and one contact path. (Ch. 1.1)

**Word-line contact:** Vertical contact landing on a tread to connect a word line to the decoder routing. (Ch. 16.3)

---

**Glossary Version:** 1.0  
**Last Updated:** 2026-10-04
