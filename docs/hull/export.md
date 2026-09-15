# Export

![Export tab](../assets/screenshots/hull-export.png)

The Export sub-tab has two independent downloads:

## Export STL

Downloads the current hull surface as `hull_export.stl` — the full triangulated mesh,
including every Fairing/Variation edit applied so far. Enabled once station lines exist.

## Export Lines

Downloads `hull_lines.zip`, containing a DXF drawing for each of the three lines-plan
views (Body / Water / Sheer Plan). Each DXF is flattened onto its own 2D plane (so it opens
correctly, right-side-up, in any DXF viewer — not just ones that default to a 3D top-down
view) and uses simple `POLYLINE`/`VERTEX` entities for maximum compatibility with older CAD
tools. Enabled as soon as any line exists (you don't need the full Import wizard finished —
station lines alone are enough).

!!! note "Coming soon"
    A future update will let you export auxiliary drawing elements (axes, AP/FP, draft
    line, station labels, line names) to their own DXF layers, and add a pre-export
    options dialog for choosing what to include.
