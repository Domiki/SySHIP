# Damage

Marks which compartments are assumed breached and flooded for a damage-stability case.
Whatever is checked here becomes a deduction from buoyant volume — scaled by each
compartment's own **Permeability** (set per-compartment in [Volume](volume.md)) — the next
time you run [Stability](stability.md). Leave everything unchecked for an ordinary
**intact**-stability run; check one or more compartments to switch that run to the
**MARPOL Annex I** / **ICLL Type A** damage-stability criteria instead of the intact ones.

![Damage tab, one compartment flooded](../assets/screenshots/compart-damage-flooded.png)

Drag one or more compartments from the **Mesh List** into the **Flooded compartments** drop
box to flood them (the hull and cutting surfaces can't be dropped here — a hint explains why
if you try). Each row shows its permeability read-only (from [Volume](volume.md)); click the
× to remove a compartment from the flooded set.

A flooded compartment's shape is scaled about its own centroid down to
permeability<sup>1/3</sup> of its linear size (equivalent to exactly the permeability
fraction of its volume) and subtracted from submerged buoyant volume at every trial
heel/trim/draft during the stability solve.

Flooded compartments get a red outline in the 3D View; after a Stability run, they also
show a translucent sea-colored fill up to the equilibrium waterline, tilted to the
equilibrium heel/trim.

The **Openings** (down-flooding points) used to compute the flooding angle and the
final-waterline check for both intact and damage criteria are set on the
[Stability](stability.md) tab, not here.