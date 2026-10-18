# Export

![Export tab](../assets/screenshots/compart-export.png)

## Export STL

Choose what to export, set an optional origin, and click **Export STL**.

- **Hull shell only** downloads the hull exactly as it was imported from HULL — the original
  watertight shell, without any compartments or Modeling edits — as `<hull name>.stl`. Useful
  when you want the outer hull surface on its own, separate from whatever's been built inside
  it.
- **All compartments** exports every modeled compartment as a **single merged STL**
  (`compart_merged.stl`) — useful for opening the whole compartment model outside SySHIP (3D
  printing, CAD, CFD). Walls shared between adjacent compartments are merged into a single
  plate rather than kept as two overlapping surfaces; a success message reports how many
  shared walls were merged. This always merges every compartment together — there's currently
  no button to export a single compartment's mesh on its own.

**Origin (m)** shifts the exported geometry so that point becomes (0, 0, 0) in the file —
useful for lining the model up with an external tool's own origin (for example the ship's
A.P., centerline and baseline). Leave it at 0, 0, 0 to keep SySHIP's own coordinates.