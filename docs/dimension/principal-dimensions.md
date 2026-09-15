# Principal Dimensions

![Principal Dimensions tab](../assets/screenshots/dimension-principal-dimensions.png)

Click **Solve** to compute the new design's principal dimensions from the Owner's
Requirements and Parent Ship entered in [Requirements](requirements.md). This closes both
the **Weight Equation** (displacement = lightweight + deadweight) and the **Volume
Equation** (cargo hold capacity) simultaneously, producing the "Design Ship" that every
other DIMENSION sub-tab — and HULL's Variation panels, and COMPART's Loading tab — can
read from.

The button is disabled with a hint ("Set a parent ship in the Requirements tab first.")
until a Parent Ship exists.

## What Solve does

- Detects the **carrier type** (deadweight vs. volume), per the Requirements tab's setting
  or auto-detection.
- **Deadweight carrier** (e.g. tankers, bulkers): fixes L/B and C<sub>B</sub> at the
  parent's own ratios, then numerically solves for L so the volume-equation-implied D and
  the weight equation both close exactly.
- **Volume carrier** (e.g. container ships): solves L, B, D directly from the cargo-hold
  Volume Equation (keeping L/B and B/D at the parent's ratios), then computes C<sub>B</sub>
  in closed form from the Weight Equation.
- Rescales every hull-form/freeboard detail (bulb area, transom area, superstructure
  dimensions, etc.) proportionally from the parent, using the new L/B/D/T ratios.
- Clears any previously computed Freeboard, Resistance curve, and Propeller result, since
  they all depend on the design ship and are now stale.

## Results

- **Carrier type / Weight method** — echoes which mode was actually used.
- **Principal Particulars** (read-only): LOA, LBP, B molded, D molded, T<sub>d</sub>,
  T<sub>s</sub> (m), C<sub>B</sub> at T<sub>d</sub>, Displacement (ton).
- **Lightweight** (read-only): Structural W<sub>s</sub>, Outfit W<sub>o</sub>, Machinery
  W<sub>m</sub> (ton), LWT total, and Building cost (from the Requirements tab's cost
  model).
- **Equation Closure** (read-only): the Weight Equation error (%) and Volume Equation
  error (%) — how precisely the numeric solver closed each equation. Both should be
  essentially 0; this is the solve's own self-check.

Once solved, the resulting LBP/B/D/C<sub>B</sub> becomes the "DIMENSION recommended" value
offered by HULL's Length/Breadth/Depth/C<sub>P</sub> Variation panels, and the lightweight
total (W<sub>s</sub>+W<sub>o</sub>+W<sub>m</sub>) becomes the one-click lightship estimate
in COMPART's Loading tab.
