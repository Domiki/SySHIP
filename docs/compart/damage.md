# Damage

Marks which compartments are assumed breached and flooded for a damage-stability case.
Whatever is checked here becomes a deduction from buoyant volume — scaled by each
compartment's category **permeability** — the next time you run [Stability](stability.md).
Leave everything unchecked for an ordinary **intact**-stability run.

![Damage tab, one compartment flooded](../assets/screenshots/compart-damage-flooded.png)

Every compartment except the hull and cutting planes is listed (build them first in
[Modeling](modeling.md)). Check a compartment's box to flood it; a hint next to it shows
its permeability ("perm. 0.85") or **"perm. 1.00 (no category)"** if you haven't assigned
one in [Volume](volume.md) yet — permeability 1.00 means fully open to the sea, i.e. the
compartment floods completely with no allowance for structure or stored cargo displacing
water.

A flooded compartment's shape is scaled about its own centroid down to
permeability<sup>1/3</sup> of its linear size (equivalent to exactly the permeability
fraction of its volume) and subtracted from submerged buoyant volume at every trial
heel/trim/draft during the stability solve.

Flooded compartments get a red outline in the 3D View; after a Stability run, they also
show a translucent sea-colored fill up to the equilibrium waterline, tilted to the
equilibrium heel/trim.
