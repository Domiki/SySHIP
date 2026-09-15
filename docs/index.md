# SySHIP

SySHIP is a desktop application for early-stage naval architecture design: sizing a new
ship from owner's requirements, importing and fairing a hull surface, reshaping it toward
target dimensions, and laying out internal compartments for hydrostatics, loading, and
stability analysis.

The app is organized into three main tabs, meant to be worked roughly in order (though
none of them require the others to have been used first):

| Tab | What it's for |
|---|---|
| [**DIMENSION**](dimension/index.md) | Size a new design from owner's requirements and a parent ship: principal dimensions, freeboard, resistance/propeller, engine selection, and automatic optimization. |
| **HULL** | Import an STL hull surface, scale/orient it, generate its lines plan, reshape it with [Variation](hull/variation.md) and [Fairing](hull/fairing.md), and [export](hull/export.md) STL/DXF. |
| **COMPART** | Build internal compartments/tanks inside an imported hull and run hydrostatics, tank capacity, loading, damage, and stability calculations — see the [COMPART overview](compart/index.md). |

## Download

Get the latest installer from the [Releases](https://github.com/Domiki/SySHIP/releases) page.

## Where to start

- [Installation](installation.md)
- [Importing a Hull](hull/import.md) — the fastest way to see the app working, using one
  of the bundled sample hulls.
- Having trouble launching the app? See the [FAQ](faq.md).
