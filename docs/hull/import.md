# Importing a Hull

The Import sub-tab turns a raw STL hull surface into a working SySHIP model: scaled and
oriented correctly, with AP/FP set, and with the station, waterline, and buttock lines
that every later stage (Variation, Fairing, Export, COMPART) depends on.

STL files should represent **one half of the hull** (starboard or port side only, split at
the centerline). SySHIP always treats Y = 0 as the centerline and mirrors that half to draw
or export the "Full Ship."

The Import panel walks through five stages in order. You can't skip ahead — each stage's
"Create" button advances to the next one — but the 3D View and the Body/Water/Sheer Plan
panels update live as you go, so you can check the result of every step immediately.

## 1. New from Basis Ship

![Empty drop zone](../assets/screenshots/hull-import-dropzone.png)

Click or drag-and-drop an **STL file** onto the drop zone. On first launch, SySHIP ships
with a few sample hulls (`kvlcc.stl`, `kvlcc2.stl`, `kcs.stl`) — the file browser opens
there by default so you can try the app immediately without your own geometry.

Once a file loads, the panel shows the raw X/Y/Z extents of the mesh and a per-axis
**Scale**, **Offset**, and **Flip** control:

![Scale/offset/flip controls](../assets/screenshots/hull-import-scale.png)

| Control | Effect |
|---|---|
| **Scale** | Multiplies that axis's coordinates (use it to convert a model-scale STL to full scale, or to fix an axis that came in in the wrong unit). |
| **Offset (m)** | Shifts that axis after scaling — typically used to move the origin to AP or to the centerline. |
| **Flip** | Mirrors that axis (sign-flips it) — use it if the imported hull is upside-down, bow-aft, or on the wrong side of the centerline. |

A **preview row** shows what the X/Y/Z range will be *after* the transform, so you can
confirm the numbers before committing. Click **Apply Scale** to bake in the transform and
move to the next stage. (You can always start over with the **Reset** button next to the
loaded filename, at the cost of clearing all Import/Variation progress.)

## 2. Create Section Lines

![Section line stage](../assets/screenshots/hull-import-section-lines.png)

Enter the **AP** (aft perpendicular) and **FP** (forward perpendicular) positions in meters,
measured along the hull's long axis. SySHIP then automatically slices the hull into the
21 standard **station lines** (Station 0 at AP through Station 20 at FP, evenly spaced) —
this is the same 0–20 station numbering used throughout naval architecture lines
drawings, and the same numbering the Body Plan and the HULL tree view use.

## 3. Create Waterlines

![Waterline stage](../assets/screenshots/hull-import-waterline-stage.png)

- **Design Draft (T<sub>d</sub>)** — enter the design draft in meters. This is the single
  most important number in the project: it drives the hydrostatics calculation (displacement,
  wetted surface, \(C_B\)/\(C_P\)/\(C_M\)/\(C_{WP}\), \(LCB\)/\(TCB\)/\(VCB\), and the
  \(C_P\) curve shown in the Properties panel) and is used later by Variation and COMPART.
- **Waterline Positions (m)** — a comma-separated list of Z positions, where each entry is
  either a single value or a `start:end:step` range. For example:
  - `2, 4.5, 8` creates three waterlines at exactly those heights.
  - `0:20:4` creates waterlines every 4 m from 0 to 20 m (0, 4, 8, 12, 16, 20).
  - You can mix both forms in one entry, e.g. `0:20:4, 20.5`.
  - Leaving the start or end of a range empty (e.g. `:20:4`) defaults to the hull's own
    Z bounds.

## 4. Create Buttock Lines

![Buttock stage](../assets/screenshots/hull-import-buttock-stage.png)

Same idea as waterlines, but for **buttock lines** — vertical cuts at fixed Y (beam)
positions, used for the Sheer Plan. The **Buttock Positions (m)** field accepts the same
comma-list / `start:end:step` syntax as waterline positions.

## 5. Import Complete

![Import complete](../assets/screenshots/hull-import-complete.png)

A read-only summary of the final X/Y/Z range of the imported, transformed hull. From here
the **Variation**, **Fairing**, and **Export** sub-tabs become available in the left sub-tab
list, and the full working view (below) is populated.

## The working view

Once section lines exist, the main area always shows the same four-pane layout —
**Body Plan** (Y/Z), **3D View**, **Water Plan** (X/Y), **Sheer Plan** (X/Z) — regardless
of which HULL sub-tab (Import/Variation/Fairing/Export) is active:

![Full working grid with Full Ship and Fairness heatmap enabled](../assets/screenshots/hull-3d-full-ship.png)

### 2D Plan views (Body / Water / Sheer)

Each 2D plan is an interactive, zoomable/pannable SVG drawing:

- Drag to pan, scroll to zoom, or use the header's **−** / **+** buttons.
- **Fit View** (the circular-arrow icon) resets the pan/zoom to frame all visible lines.
- Double-click a pane's title bar to **maximize** it to fill the whole working area; double-click
  again (or the ⤡ button) to return to the 4-pane grid.
- Hovering a line shows its name in a tooltip; clicking a line selects it (it's then
  highlighted across all views, including the 3D View), and shows its live cursor
  position (in the selected plan's two axes) in the status bar at the bottom of the pane.
- The **Water Plan** and **Sheer Plan** show dashed **AP**/**FP** reference lines. The
  **Sheer Plan** additionally shows a solid horizontal line at the design draft (the
  waterline used for hydrostatics).
- The **Body Plan** mirrors each station line about the centerline automatically so the
  full cross-section (not just the imported half) is always visible.

![Maximized Body Plan](../assets/screenshots/hull-body-plan.png)

### 3D View

![3D view toolbar close-up](../assets/screenshots/hull-3d-toolbar.png)

The toolbar above the 3D viewport, left to right:

| Icon/control | Name | Effect |
|---|---|---|
| 〰 | **Wireframe** | Toggles the plain wireframe overlay of the surface mesh. |
| ■ | **Surface** | Toggles the shaded surface. The outward-facing side is drawn in your chosen color; the *inward*-facing side (visible through openings, or on the far side once "Full Ship" is on) is drawn darker and less saturated, so you can always tell which side of the hull you're looking at. |
| ▢ | **Mesh** | Toggles a dense wireframe drawn directly over the triangulated surface mesh (as opposed to the sparse Wireframe layer above, which only shows the station/water/buttock lines). |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M8 1.5v13" stroke-dasharray="1.6 1.6"/><path d="M8 3c2.5 0 4 1.5 4 5s-1.5 5-4 5"/><path d="M8 3c-2.5 0-4 1.5-4 5s1.5 5 4 5"/></svg> | **Full Ship** | Mirrors everything currently drawn (surface, lines) across the centerline, so you see the complete vessel instead of just the modeled half. |
| (color swatch) | **Surface Color** | Picks the hull's shaded surface color. |
| − / + | **Zoom Out / Zoom In** | Dollies the camera toward/away from the current orbit target. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3h10"/><path d="M3 3v5a5 5 0 0 0 10 0V3"/></svg> | **Section View** | Snaps the camera to look straight down the ship's long axis (\(X\)) — a body-plan view. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M2 10h12"/><path d="M2 10c1-4 11-4 12 0"/></svg> | **Elevation View** | Snaps the camera to look straight down the beam axis (\(Y\)) — a profile/sheer view. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M2 6h12"/><path d="M2 6v2a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V6"/></svg> | **Plan View** | Snaps the camera to look straight down from above (\(Z\)) — a waterplane view. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M13 8a5 5 0 1 0-1.5 3.6"/><path d="M13 4.5V8H9.5"/></svg> | **Fit View** | Reframes the camera to fit all visible geometry. |

Camera controls: left-drag to orbit, scroll to zoom, right-drag (or two-finger drag) to pan.
A small **axis gizmo** (the colored cube in the bottom-right corner) always shows the
current viewing direction relative to the ship's X/Y/Z axes — click any of its faces,
edges, or corners to snap the camera to that exact view.

When the Fairing sub-tab's curvature heatmap is active, a **Fairness** legend appears
under the 3D View (see [Fairing](fairing.md)).

### Lines tree (right panel)

The right-hand panel's **Lines** tab (next to **Properties**) lists every line in the
model, grouped by type:

![Lines tree view](../assets/screenshots/hull-lines-tree.png)

- **Primary Lines** — the Center Line, Side Tangent Line, Deck Side Line, and Bottom
  Tangent Line, derived automatically from the hull surface once it's scaled.
- **Section Lines** — the station lines (numbered 0–20). You can type a new station
  number in the add field to create one on demand, or click the pencil/× icons next to
  an existing line to rename/renumber or delete it.
- **Waterlines** / **Buttock Lines** — same add/rename/delete controls, using a Z or Y
  position in meters instead of a station number.

Clicking any row selects that line everywhere it's drawn (2D plans and 3D View).

## Project actions (sticky footer)

The bar at the bottom of the HULL tool panel is available from every HULL sub-tab:

| Icon | Action |
|---|---|
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linejoin="round"><rect x="2" y="2" width="12" height="12" rx="1"/><path d="M4.5 2v3.5h5.5V2"/><rect x="5" y="9" width="6" height="4"/></svg> | **Save** — writes the project to the last-used file (or behaves like Save As if none yet). |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linejoin="round"><path d="M2 4.5h4l1.2 1.5H14v7a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1v-8.5z"/></svg> | **Load** — opens a saved `.zip` project file. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M8 1.5v7M5.5 6.5 8 9l2.5-2.5"/><path d="M2.5 10v3a1 1 0 0 0 1 1h9a1 1 0 0 0 1-1v-3"/></svg> | **Save As** — prompts for a file location; the project is written as a `.zip`. |
| <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M3 8a5 5 0 1 1 1.5 3.6"/><path d="M3 4.5V8h3.5"/></svg> / <svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M13 8a5 5 0 1 0-1.5 3.6"/><path d="M13 4.5V8H9.5"/></svg> | **Undo / Redo** — step the hull model's edit history backward/forward (this also covers Fairing and Variation edits). |
