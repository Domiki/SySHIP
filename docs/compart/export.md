# Export

![Export tab](../assets/screenshots/compart-export.png)

Exports every modeled compartment as a **single merged STL** — useful for opening the
whole compartment model outside SySHIP (3D printing, CAD, CFD).

Click **Export STL**. Any walls shared between adjacent compartments are welded/merged
into a single plate rather than kept as two overlapping surfaces; a success message reports
how many shared walls were merged. The result downloads as `compart_merged.stl`.

This is the only export on this tab, and it always merges every compartment together —
there's currently no button to export a single compartment's mesh on its own.
