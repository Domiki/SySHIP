# Optimize

![Optimize tab](../assets/screenshots/dimension-optimize.png)

Automatically searches the two free dimension ratios — L/B and either C<sub>B</sub>
(deadweight carriers) or B/D (volume carriers) — for the combination that best satisfies a
chosen objective, subject to the freeboard regulation and a sane Froude-number range.

## Setup

- **Objective**: Minimize building cost / Minimize effective power (EHP) at service speed /
  Weighted: cost + λ × EHP.
- **Lambda** (weight on EHP) — shown only for the Weighted objective.
- **Custom bounds** checkbox — unchecked (default) searches ±15% around the parent ship's
  own ratios; checked reveals manual **L/B min/max** and **C<sub>B</sub> min/max** (or
  **B/D min/max**, depending on carrier type) fields.

Click **Optimize** (needs a Parent Ship from [Requirements](requirements.md); no prior
Solve required).

## How it searches

A derivative-free **COBYLA** optimizer runs the search — chosen because the objective
embeds a bisection solve whose coarse tolerance breaks gradient-based methods like SLSQP —
with two inequality constraints enforced at every trial: **freeboard margin ≥ 0** and
**Froude number between 0.05 and 0.4**.

## Results

- **Converged** (Yes/No) and **Evaluations** count.
- **Chart:** objective value per evaluation (convergence trace).
- **Chart:** freeboard margin per evaluation (must stay ≥ 0).
- **Chart:** objective value vs. L/B, and vs. C<sub>B</sub> (or B/D) — every trial point.
- **Optimized Design**: LBP, B molded, D molded, C<sub>B</sub>, Building cost, and the
  final objective value.

On success, this **replaces** the active Design Ship — Principal Dimensions, Freeboard, and
Resistance & Propeller now reflect the optimized result, and you should re-run them (and
Engine Selection) to refresh their own outputs.
