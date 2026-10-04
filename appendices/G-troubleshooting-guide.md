# Appendix G: Troubleshooting Guide

Symptom-driven guide for staircase-etch excursions. For each symptom: likely causes ranked from most to least common, checks to separate them, and corrective actions. Chapter references point to the underlying physics.

---

## G.1 All Treads Too Wide or Too Narrow (Uniform Offset)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. ESC temperature offset (calibration,    Zone temps; sensor wafer; He leak   Recalibrate; APC
   chiller drift) (Ch. 8)                  trend; patterned trim monitor       offset meanwhile
2. Trim O₂ MFC offset (Ch. 7.6)            MFC rate-of-rise check              Recalibrate/replace
3. New resist lot or track bake shift      Lot ID; hotplate logs; lot          Lot offset in APC;
   (Ch. 13.5)                              qualification data                  fix hotplate
4. Product loading not in APC (new         Resist coverage vs. reference       Product offset
   product) (Ch. 13.1)                     product
5. Induction time changed (etch polymer    CO/Ar rise time in trim trace       Crust breakthrough;
   change, crust) (Ch. 4.6)                                                    restore etch
6. Source power delivery (coil, window)    Forward/reflected power; Ar         RF calibration;
                                           actinometry                         window replacement
```

## G.2 Tread Width Drifts Within a Mask (Index-Dependent)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Wall inventory not at steady state      F/Ar peak at each trim start;       Raise liner T; WAC
   (low removal fraction f) (Ch. 9.3)      width vs. trim index                and conditioning;
                                                                               per-level offsets
2. Thermal transient after etch steps,     Wafer T trace (if available);       Per-level trim
   larger for thick special layers         width vs. preceding etch length     offsets
   (Ch. 8.5)
3. Resist aspect ratio in narrow           Width vs. index in narrow vs. open  Layout rule; per-
   openings (Ch. 13.4)                     regions                             region bias
4. Nitride-step lateral resist loss        O₂ flow trend; wall state           Stabilize nitride
   changing (Ch. 4.3.3)                                                        step
```

## G.3 Placement Error Grows With Tread Index; Width Looks Fine

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Small systematic trim bias (0.5–1%)     Sum of width deviations vs.         APC recalibration;
   accumulating (Ch. 12.4)                 index; compare chambers             target cumulative
                                                                               position
2. Chamber mismatch (Ch. 5.6)              Placement by chamber                Per-chamber offsets;
                                                                               hardware matching
3. Resist edge recession during etch       Resist sidewall angle; etch         Steeper resist edge;
   (sloped resist) (Ch. 3.7)               resist loss                         lower etch ion energy
```

## G.4 Radial or Edge Tread Errors

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Bowed wafers: poor edge contact →       Bow measurement; He leak per        Staged chucking; He
   hot edge (Ch. 8.4)                      wafer; correlation with edge error  zoning; bow-class
                                                                               offsets
2. Radial O profile shift (gas split,      Patterned monitor radial map        Gas split; coil ratio
   coil, window) (Ch. 7.5, 13.3)
3. ESC zone drift                          Zone temps; sensor wafer            Zone offsets
                                                                               (Ch. 15.6.5)
4. Bare-ring loading at the edge           EBR width; edge die layout          ESC edge zone
   (Ch. 13.1.4)
5. Edge-ring wear (etch tilt, landing at   Ring hours; edge riser tilt         Ring adjust/replace
   edge)
```

## G.5 Level Offset (Missed or Doubled Step)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Step skipped or repeated after abort/   Tool event log; OES evidence per    Fix resume logic
   resume (Ch. 11.5.1)                     etch; offset begins at one tread    (Appendix C.6)
2. Underetch (overetch too small,          OES clearing times vs. step end;    Increase OE (cheap
   thick layer, slow region) (Ch. 11.4)    test staircase step heights         in intermediate
                                                                               cycles)
3. Etch stop from excess polymer (O₂ low,  O₂ MFC; CN/Ar rise missing;         Restore O₂; WAC
   wall over-polymerized)                  wall state
4. Level-table error (new recipe)          Recipe version; level table check   Change control
                                                                               (Appendix C.5)
5. Local: residue/micromask on a strip     Offset only in patches; SEM of      Crust breakthrough;
   (Ch. 10.4)                              strips                              over-trim
6. Deep chop count error (Ch. 14.6.2)      Chop sub-step OES counts            Sub-step resync;
                                                                               cross-checks
```

Diagnostic rule: a **global** missed etch k shifts every tread exposed at etch k (the outer treads of the mask and all later masks) by one level. A **local** miss shifts only the affected region.

## G.6 Resist Runs Out (Thin Resist, Edge Breakdown, Late-Cycle Defects)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Trim ratio higher than planned (ion     Vertical vs. lateral monitors;      Lower trim ion flux
   energy in trim, capacitive coupling,    r map                               (pulsing, pressure);
   bias timing) (Ch. 6.5, 7.3.3)                                               fix bias timing
2. Coat thinner at some locations          Coat thickness map; topography      Coat recipe; T₀
                                           over earlier masks
3. Pair-etch resist loss up (selectivity)  δ_E per etch                        Etch tuning
4. Crust breakthrough too long             Step time; OES                      Shorten; endpoint
5. Too many trims for the budget           Detailed budget (Ch. 12.1)          Reduce n per mask
```

## G.7 Feet, Stringers, Pillars, or Rough Treads

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Corner polymer accumulation on outer    Cross-section of inner vs. outer    Foot-clean step;
   risers (Ch. 10.3)                       risers                              nitride-step O₂
2. Residue on newly exposed strips         SEM of strips; induction time       Crust breakthrough;
   (crust, resist foot) (Ch. 10.4)         variation                           over-trim at end
3. Particles (SiOₓF_y flakes, polymer,     Particle monitors; defect maps vs.  WAC with fluorine;
   plasma-off drop) (Ch. 9.8)              PM/WAC history                      plasma-on
                                                                               transitions
4. Incomplete strip before next mask       Residual C/F on treads (XPS)        Strip endpoint
   (Ch. 4.7)
```

## G.8 Tread Oxide Too Thin (After Final Etch)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Last-etch nitride-step overetch too     Last-etch OE; SiN:ox selectivity    Limit last-etch OE
   long or selectivity low (Ch. 11.3.4)    monitor                             only; tune O₂
2. Strip or clean attacking oxide          Strip chemistry (F?); clean         Fluorine-free strip;
   (fluorine in strip, aggressive clean)   recipe                              milder clean
3. Wall fluorine during final strip        F/Ar during strip                   WAC/conditioning
   (Ch. 9.2)                                                                   before strip
```

## G.9 Corner Problems (Contacts Fail Near Opening Corners)

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Arc radius larger than layout allowed   Corner test structure               Update exclusion
   (more trims per mask) (Ch. 10.5)                                            zone; fewer trims
2. Convex corner blunting (higher flux)    Outside-corner metrology            Layout allowance
3. Side treads intruding on active rows    Plan-view SEM                       Opening y-edges
   (Ch. 3.6.1)                                                                 outside active area
```

## G.10 Chamber-to-Chamber Mismatch

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. ESC calibration                         Sensor wafer on each chamber        Recalibrate
2. Liner/wall temperature, wall material   Liner T; first-cycle behavior       Match hardware
   age
3. RF delivery (coil, window, match)       Power calibration; Ar actinometry   Calibrate; replace
4. MFC differences                         Rate-of-rise                        Calibrate
→ Interim: per-chamber APC offsets from patterned monitors
```

## G.11 First-Wafer or Post-Idle Effects

```
Likely causes                              Checks                              Actions
─────────────────────────────────────────────────────────────────────────────────────────────
1. Wall state after idle/WAC (Ch. 9.5)     First vs. later wafer metrology     Conditioning step;
                                                                               dummy wafer
2. Chuck/liner cooler after idle (Ch. 8.5) Idle time correlation               Warm-up plasma
3. WAC ending state varies                 WAC recipe final step               Fixed conditioning
```

## G.12 Word-Line Failures at Electrical Test (by Signature)

```
Signature                                  Likely staircase cause              First check
─────────────────────────────────────────────────────────────────────────────────────────────
Periodic in level (each mask boundary)     Stitch tread (Ch. 12.5)             Stitch metrology;
                                                                               overlay
Rising with index within each mask         Trim bias (Ch. 12.4)                Cumulative placement
Edge die, outer treads                     Edge trim (Ch. 13.3.4)              Edge tread metrology
Abrupt offset from level k outward         Missed etch k (Ch. 11.2.4)          OES record of etch k
Shallow levels only                        Contact punch-through (Ch. 16.3)    Liner/tread oxide;
                                                                               contact etch time
Deepest levels only, opens                 Contact underetch                   Contact etch
Many WLs shorted together                  Liner–WL path (Ch. 16.2.2)          Liner isolation
                                                                               cross-section
Random single levels                       Pillars, particles (Ch. 9.8)        Defect inspection
                                                                               history
One chamber only                           Chamber matching                    Chamber split
```

---

## G.13 General Excursion Procedure

```
1. Contain: hold affected chambers and lots; identify the first affected
   wafer (APC and metrology history)
2. Classify: global vs. local, index-dependent vs. uniform, radial vs.
   uniform, one chamber vs. fleet
3. Check the cheapest evidence first: OES traces per step, tool event
   logs, MFC/ESC/RF logs, recipe and level-table version, resist lot and
   track logs
4. Run monitors: patterned trim monitor, blanket etch rates and
   selectivity, particles, test staircase level count
5. If unresolved: short-loop wafers with full metrology and cross-section
6. Restore: fix root cause → re-qualify (Appendix C.4) → release with
   enhanced sampling for the next lots
7. Document: root cause, signature, detection lag; update FDC limits,
   APC models, and this guide
```

---

**Appendix G Version:** 1.0  
**Last Updated:** 2026-10-04
