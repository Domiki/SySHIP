# Freeboard

![Freeboard tab](../assets/screenshots/dimension-freeboard.png)

Computes the vessel's statutory minimum freeboard under the **International Convention on
Load Lines (ICLL) 1966, Type A** rules — the rule set for tankers and other liquid-cargo
ships with high subdivision and small deck openings — and checks whether the required
summer draft can actually be achieved within it.

Click **Compute Freeboard** (disabled with a hint until [Principal Dimensions](principal-dimensions.md)
has produced a design ship).

## Results table

Each row can be expanded (▸/▾) to show its own breakdown, where applicable:

| Row | Notes |
|---|---|
| Freeboard length (L<sub>f</sub>) (m) | |
| Tabular freeboard (mm) | Looked up from the official Reg. 28 Type-A table. |
| Correction for block coefficient (mm) | |
| Correction for depth (mm) | |
| Deduction for superstructure/trunks (mm) | Shown as negative; driven by the Forecastle/Poop/Trunk checkboxes and superstructure dimensions from Requirements. |
| Correction for sheer (mm) | Uses the AP/FP sheer ordinates and a Simpson's-rule comparison against the standard sheer curve. |
| Correction for minimum bow height (mm) | |
| **Calculated summer freeboard (mm)** | Sum of all rows above. |
| Depth for freeboard (m) | |
| Maximum permissible summer draft (m) | |
| Required summer draft (T<sub>s</sub>) (m) | From Requirements/Design Ship. |
| Margin (mm) | Maximum permissible minus required draft's freeboard-equivalent. Must be ≥ 0. |

## Regulation status

A badge reads **Satisfied** when the margin is non-negative, or **Not satisfied — increase
D or adjust C<sub>B</sub>** otherwise.

[Optimize](optimize.md) uses this same calculation as a hard constraint (margin ≥ 0) at
every trial point during its search.
