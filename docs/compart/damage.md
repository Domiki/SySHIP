# Damage

Marks which compartments are assumed breached and flooded for a damage-stability case.
Whatever is checked here becomes a deduction from buoyant volume — scaled by each
compartment's own **Permeability** (set per-compartment in [Volume](volume.md)) — the next
time you run [Stability](stability.md). Leave everything unchecked for an ordinary
**intact**-stability run; check one or more compartments and the Stability criteria window
starts with the **MARPOL Annex I** / **ICLL Type A** damage conditions instead of the intact
ones.

![Damage tab, one compartment flooded](../assets/screenshots/compart-damage-flooded.png)

Drag one or more compartments from the **Mesh List** into the **Flooded compartments** drop
box to flood them (the hull and cutting surfaces can't be dropped here — a hint explains why
if you try). Each row shows its permeability read-only (from [Volume](volume.md)); click the
× to remove a compartment from the flooded set.

At every trial heel/trim/draft of the stability solve, the part of each flooded compartment
below the waterline is cut out, multiplied by its permeability, and subtracted from the
buoyant volume (lost buoyancy). Any cargo loaded in a flooded compartment is treated as
lost and left out of the weight.

Flooded compartments get a red outline in the 3D View. When the equilibrium result is shown
in [Stability](stability.md), they also show a translucent sea-colored fill up to the
equilibrium waterline, tilted to the equilibrium heel/trim.

Openings (down-flooding points) are not modeled, so the flooding angle is not used in the
criteria.