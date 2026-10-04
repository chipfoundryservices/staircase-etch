# Appendix C: Standard Operating Procedures

Representative procedures for running and qualifying a staircase trim–etch chamber. Limits are illustrative. Each fab sets its own from baseline data, typically mean ± 3σ of a stable reference period. Because tread placement accumulates trim-rate error, trim-related limits are tighter than in most etch modules.

---

## C.1 Daily Chamber Check

```
Time required: ~25 minutes (plus monitor wafer run time)
Performed by: Equipment technician; reviewed by process engineer

1. Vacuum and leak (3 min)
   ☐ Base pressure ≤ spec (e.g., < 0.5 mTorr after 5 min pump)
   ☐ Rate-of-rise ≤ spec (e.g., < 1 mTorr/min, valves closed)
     Fail → leak check before any production

2. Pressure gauge zero (2 min)
   ☐ Zero capacitance manometers at base pressure; record offsets
   ☐ Offset jump > 0.2 mTorr → investigate (trim pressure accuracy)

3. Gas delivery (4 min)
   ☐ MFC zero readings at no flow (within ±0.5% FS)
   ☐ Rate-of-rise flow verification on O₂ (trim), O₂ (etch, small MFC),
     C₄F₆, and CH₃F MFCs; each within ±1% of setpoint
     (O₂ trim MFC error is a systematic tread-width error, Ch. 7.6)

4. RF and match (3 min)
   ☐ Standard Ar step: bias voltage within ±3% of baseline
   ☐ Standard O₂ trim step: source forward/reflected power, match
     preset positions within limits
   ☐ Transition test: etch → trim → etch with plasma on; no reflected
     power spikes above limit

5. ESC and thermal (4 min)
   ☐ Zone temperatures at setpoint ±0.1 °C (idle)
   ☐ He leak with test wafer ≤ spec (e.g., < 1 sccm); He leak with a
     bowed reference wafer ≤ spec (bow-class check)
   ☐ Liner/ceiling heaters at setpoint ±2 °C

6. OES baseline (2 min)
   ☐ Ar 750 nm intensity in standard step vs. last clean (window
     transmission)
   ☐ CO/Ar ratio in standard trim on a resist coupon or monitor

7. Trim monitor (patterned or blanket; see C.2)
   ☐ Lateral (or vertical proxy) trim rate within ±0.5% of target
   ☐ Uniformity within limit

8. Record and release
   ☐ All checks logged in the tool's daily record
   ☐ Any failure → chamber held; process engineer notified
```

---

## C.2 Trim-Rate Monitor

### C.2.1 Blanket (Vertical Proxy)

```
1. Coat bare Si monitor with staircase resist, 2–3 µm, standard bake
2. Measure thickness (49 sites) by optical reflectometry
3. Run standard trim step (e.g., 60 s) after a standard etch step
   (to reproduce the crust)
4. Re-measure; compute vertical rate map
5. Limits: mean within ±1% of baseline; radial range within limit

Limitation: measures R_V, not R_L. A change in trim ratio is invisible.
Use patterned monitors (C.2.2) at least weekly.
```

### C.2.2 Patterned (Lateral, Reference)

```
1. Use monitor wafers with large resist blocks (staircase-like edges)
   on an oxide or ON-stack substrate
2. Run 3 cycles of the production etch–trim sequence
3. Strip resist; measure tread widths by CD-SEM at 9–25 sites
4. Compute lateral rate per site: R_L = (w − λ_etch) / (t_trim − t_ind)
5. Limits: mean within ±0.5% of target; radial range ±0.5%

Patterned monitors are the reference for APC rate calibration and for
ESC zone tuning (Ch. 8.6, Ch. 15.6).
```

---

## C.3 Weekly Radial Profile and Matching Check

```
1. Patterned trim monitor (C.2.2) at 25–49 sites
2. Compute radial profile; compare with fleet target profile
3. If any radial zone deviates by > 0.3%: compute ESC zone offset
   (Ch. 15.6.5) with damped gain; apply; verify on a second monitor
4. Chamber-to-chamber: compare mean lateral rate with fleet; update
   per-chamber APC offset if difference > 0.2%
5. Etch monitors: blanket oxide and nitride rates and selectivity on
   test wafers; within ±3% of baseline
```

---

## C.4 Preventive Maintenance Recovery

```
After wet clean, part replacement (liner, edge ring, window), or ESC work:

1. Pump-down, leak check, bake-out with liner at temperature (≥ 2 h)
2. Long WAC: fluorine step (NF₃/O₂) + oxygen step + conditioning step
3. ESC: temperature calibration with instrumented wafer (if ESC or
   edge ring touched); He leak check, including bowed reference wafer
4. Seasoning: run full trim–etch sequence on resist-coated dummies
   (10–25 wafers; Ch. 9.6)
5. Monitors after every 5 seasoning wafers:
   ☐ Lateral trim rate (patterned) and uniformity
   ☐ Oxide/nitride rates and selectivity
   ☐ Particles (adders at ≥ spec size)
6. Release criteria:
   ☐ Lateral trim rate within ±0.5% of pre-PM baseline on 3 consecutive
     monitors
   ☐ Radial profile within limits
   ☐ Particles within spec
   ☐ First product lot: full tread metrology on first wafer before
     continuing
7. Edge ring replacement: reset ring-hour counter in APC (Ch. 13.3.4)
```

---

## C.5 Recipe Level-Table Change Control

The level table (per-level step times, chemistries, and trim times) defines the staircase. An error in it is a miscount.

```
1. Every change reviewed by two engineers
2. Automated check that:
   ☐ Number of etches = intended levels for each mask
   ☐ Special levels (cap, select gates, inter-deck) map to correct
     step definitions
   ☐ Per-level trim offsets within allowed range
3. First wafer after change: level-count verification on test
   staircase (profilometer/AFM) and tread metrology before lot release
4. Version recorded in wafer history
```

---

## C.6 Abort and Resume Procedure

```
On any in-sequence abort (RF fault, pressure fault, chuck fault):

1. Tool records: last step with confirmed OES clearing evidence; step
   in progress; elapsed time
2. Wafer state classification:
   a. Abort during etch, clearing not confirmed → resume by re-running
      that etch from the start (self-correcting; Ch. 11.5.1)
   b. Abort during trim → re-run trim with time reduced by the elapsed
      portion only if the induction time had passed; otherwise re-run
      full trim; flag for metrology
   c. Abort during a transition → resume at the next step only if the
      previous step's evidence is complete
   d. Any ambiguity → hold wafer; no automatic resume
3. Held wafers: metrology on test staircase (level count) and tread
   widths before decision
4. Never resume by starting the next trim after an unconfirmed etch
```

---

## C.7 New Product Introduction (Trim Calibration)

```
1. Obtain resist coverage and exposed area per mask from design data
2. Estimate trim-rate offset from the loading model (Ch. 13.1) and
   etch step times from exposed area (Ch. 13.2)
3. Run short-loop wafers through the first trim–etch mask:
   ☐ Measure all tread widths and positions at 13+ sites
   ☐ Measure test staircase step heights (level count)
   ☐ Cross-section 1 wafer: riser profile, foot, tread oxide
4. Adjust trim time and per-level offsets; repeat once
5. Run full sequence on 2–3 wafers; verify stitch treads
6. Set APC product offsets; release for pilot lots with full metrology
```

---

## C.8 Resist Lot Qualification

```
1. On receipt of a new resist lot, coat 2 monitor wafers with standard
   track recipe
2. Run patterned trim monitor (C.2.2) on a reference chamber with known
   current offset
3. Compute lot trim-rate offset vs. reference lot
4. Accept if |offset| ≤ 1.5%; enter offset into APC feed-forward
5. Reject or escalate if > 1.5% (placement impact ~70 nm at tread 8)
```

---

## C.9 Track Hotplate Monitoring

```
1. Monthly hotplate temperature verification with sensor wafer
   (each plate used for staircase resist bake)
2. Limit: ±0.5 °C from setpoint across the plate
3. Log plate ID per wafer; APC uses plate offsets if established
   (Ch. 13.5)
```

---

**Appendix C Version:** 1.0  
**Last Updated:** 2026-10-04
