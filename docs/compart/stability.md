# Stability

Solves a free-trim equilibrium at each heel angle in a **user-configurable range** to build
the ship's righting-arm (**GZ**) curve, then evaluates that curve against IMO/MARPOL/ICLL
stability criteria. With nothing checked in [Damage](damage.md), this is the ordinary
**intact stability** case; with one or more compartments flooded there, it's a
**damage-stability** case instead — the same GZ solve either way, just judged against
different criteria.

![Stability tab](../assets/screenshots/compart-stability-default.png)

Stability uses the [Loading](loading.md) tab's current total weight and combined CG as the
ship's weight — there's no separate weight input here beyond seawater density. It also
uses the hull's AP/FP (captured at import) to convert the solved trim angle into a linear
trim distance.

1. Set **Seawater density (t/m³)** (default 1.025).
2. Set the **Heel** range — Start / Step / End (deg). Unlike a fixed sweep, you choose the
   resolution and extent yourself; 0° is always included even if Start is greater than 0, and
   the sweep is capped at 181 points.
3. Optionally add **Openings** (see below).
4. Click **Run**.

## Openings

**Openings** lists down-flooding points — hull openings (doors, vents, hatches) whose
immersion determines the **flooding angle** \(\phi_f\) used by the intact-stability area
checks and by the damage-stability range/final-waterline checks. Each opening has a **Name**
and an **x / y / z (m)** position; **+ Opening** adds one, and the × button removes it. With
no openings defined, \(\phi_f\) falls back to the angle of vanishing stability (where the GZ
curve last crosses zero), and the results note this substitution.

## Results

- **GZ curve chart** — GZ (m) vs. heel angle (deg), one point per swept angle, drawn inline
  once a run finishes.
- **View Details** opens a separate window with the full breakdown:
    - The GZ curve again, at full size, plus a **Download CSV** of every swept point (Heel,
      Draft, Trim, LCB, TCB, VCB, GZ, converged) and a note if any heel point failed to fully
      converge.
    - **Equilibrium** — Draft (m), Trim (m if the hull's LBP is known, otherwise the raw
      trim angle), the solved equilibrium **Heel** (0° for an upright-stable loading, greater
      than 0° for a listing/asymmetric one, labeled port/starboard), LCB/TCB/VCB (m), the
      **deck-edge freeboard** at equilibrium, the **flooding angle** \(\phi_f\), and — if any
      Openings are defined — each opening's freeboard at equilibrium (highlighted if it's at
      or below the waterline).
    - One section per evaluated **ruleset**, each showing **All satisfied** or **Not
      satisfied** and a row per individual check (a ✓/✗ mark, the actual value, and the
      required value). Which ruleset(s) run depends on whether anything is flooded in
      [Damage](damage.md):
        - **Nothing flooded** → **Intact — IMO Res.A.749(18)** only. Checks Area A (0–30°) ≥
          0.055 m·rad, Area A+B (0° to min(40°, \(\phi_f\))) ≥ 0.09 m·rad, Area B (30° to that
          same upper bound) ≥ 0.030 m·rad, GZ at 30° ≥ 0.20 m, angle of maximum GZ ≥ 25°, and
          initial \(GM_0\) ≥ 0.15 m (read off the GZ curve's own slope at the smallest
          nonzero swept heel).
        - **One or more compartments flooded** → both **Damage — MARPOL Annex I Reg.28** and
          **Damage — ICLL Reg.27 (Type A)** run instead (the intact ruleset is skipped for
          that run). Each checks the equilibrium heel against a limit (25°/30° for MARPOL,
          15°/17° for ICLL — the relaxed limit applies if the deck edge isn't immersed), the
          range of positive GZ beyond equilibrium (≥ 20°), the maximum residual GZ within that
          range (≥ 0.1 m), the area under the curve within that range (≥ 0.0175 m·rad), and —
          if Openings are defined — that the final waterline stays below the lowest opening.
    - A **Damage extents (reference)** table (whenever the hull's AP and depth are known,
      whether or not anything is actually flooded) lists the MARPOL/ICLL side, bottom,
      bottom-raking, and ICLL side damage-extent formulas (longitudinal/transverse/vertical)
      computed from the hull's own \(L_f\), FP′, breadth, and the [Loading](loading.md) tab's
      deadweight — for you to compare against wherever you actually modeled the breach in
      [Damage](damage.md); SySHIP does not automatically place or size a damage case for you.

Once run, the 3D View tilts the hull/compartments to the equilibrium heel and trim and
draws a translucent sea plane at the equilibrium draft.

!!! warning "Sanity-check very light loading conditions"
    A loading condition far outside the hull's normal displacement range (for example, a
    lightship weight with no cargo/ballast loaded at all) can push the equilibrium solver
    into an extreme, physically implausible result, or fail to converge cleanly. If a run
    produces a wildly deep draft or an oddly tilted 3D preview, check that your
    [Loading](loading.md) condition's total weight is realistic for the hull before trusting
    the result.