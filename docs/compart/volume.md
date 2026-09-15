# Volume

For one compartment at a time, assigns a cargo/contents **category** (liquid, solid, or
void — with a density, permeability, and free-surface behavior) and computes a **sounding
table**: volume, weight, and center of gravity as a function of how full the tank is.

Click a compartment in the **Mesh List** first — Volume acts on whichever mesh is picked
there (not the hull, and not a plane).

![Volume tab, before picking a compartment](../assets/screenshots/compart-volume-no-pick.png)

## Assigning a category

- The picked mesh's own **Volume (m³)** and **Centroid X/Y/Z (m)** are shown read-only at
  the top.
- **Category** dropdown lists every category defined so far ("Name (SG density)"), plus
  "No category." Selecting one assigns it to the picked mesh immediately.
- **Open Category Editor** opens the category manager in its own window:

![Category Editor](../assets/screenshots/compart-category-editor.png)

  - **+ Category** adds a row (defaults: solid, density 1.0, reduction 1.0, FSM type
    "actual", permeability 1.0).
  - Per category: **Name**, **Type** (liquid/solid/void), **Density (t/m³)**,
    **Reduction** (0–1 — the loadable-volume fraction after structural/stiffener
    allowance), **FSM Type** (none/even/max/actual — how free-surface moment is
    computed for that tank), **Permeability** (0–1 — this is exactly the value the
    [Damage](damage.md) tab uses for that compartment's flooding).
  - **Apply** saves changes; **Cancel** reverts; **Save CSV** / **Load CSV** exchange the
    whole category list as a spreadsheet-friendly file.

## Computing the sounding table

- **Sounding Height (from tank bottom)** — start/step/end (m); auto-populated to the
  picked mesh's own height range.
- **Trim** / **Heel** ranges (m / deg), default 0/0/0.
- **Values to compute** — Volume (m³), Weight (ton), LCG/TCG/VCG (m), Free Surface Moment
  (ton·m). Volume, Weight, VCG, and FSM are checked by default.
- Click **Calculate**, then **Show** to open the results in a popup window (same
  Table/Curve/Export layout as [Hydro](hydro.md)'s results window).

The weight/CG this table reports at a given fill percentage is exactly what the
[Loading](loading.md) tab uses when you set that compartment's fill level.
