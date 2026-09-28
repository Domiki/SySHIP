# Loading

Builds a loading condition: the ship's lightship weight/CG plus however full each
categorized compartment is, combining into a total weight and CG that
[Stability](stability.md) solves against.

![Loading tab](../assets/screenshots/compart-loading-lightship.png)

## Lightship

- **Weight (t)** and **CG X/Y/Z (m)**.
- If a DIMENSION design ship exists and the weight is still 0, a **"Use DIMENSION estimate
  (N t)"** button appears — one click fills it with W<sub>s</sub>+W<sub>o</sub>+W<sub>m</sub>
  from [DIMENSION → Principal Dimensions](../dimension/principal-dimensions.md).

## Loading compartments

Select one or more compartments in the **Mesh List** — only ones that already have a category
assigned (in [Volume](volume.md)) can be selected here; others are grayed out while this tab
is active. With a selection made:

- **Fill (%)** — a shared fill level for every selected compartment (shows "mixed" if the
  selection currently has different fill levels). Compartments always fill from the bottom
  upward.
- **Weight (t)** (or **Total weight (t)** for a multi-compartment selection) — the equivalent
  fill level's weight; editing it converts back to a fill percentage. A **Weight range: 0 –
  N t** hint below it shows the maximum this selection can hold, computed from each
  compartment's own volume, **Reduction**, **Filling**, and category density (see
  [Volume](volume.md) — the loadable volume at 100% fill is `volume × reduction × filling`,
  and its weight is that volume × the category's density).

Both fields update live and commit immediately — there's no separate Apply button.

## Summary

- **Deadweight** — sum of every loaded compartment's weight.
- **Total weight** — deadweight + lightship weight. This total, and the combined center of
  gravity behind it, is exactly what [Stability](stability.md) uses as the ship's weight/CG
  when solving for equilibrium.

Loaded compartments render as translucent colored solids in the 3D View, filled to the
correct surface for the current fill level.