# DIMENSION

DIMENSION is where a new ship design starts: you describe the owner's requirements and a
comparable "parent ship," and SySHIP solves for the new ship's principal dimensions,
checks freeboard, predicts resistance and propulsion, selects an engine, and can
automatically search for the best dimension ratios.

The tab has six sub-tabs, meant to be worked roughly top to bottom:

| Sub-tab | Purpose |
|---|---|
| [Requirements](requirements.md) | Enter the owner's requirements and the parent ship's particulars — the input to everything else. |
| [Principal Dimensions](principal-dimensions.md) | Solve for the new design's L/B/D/T/C<sub>B</sub> and lightweight. |
| [Freeboard](freeboard.md) | Check the statutory minimum freeboard (ICLL 1966, Type A) against the design. |
| [Resistance & Propeller](resistance-propeller.md) | Predict resistance/power (Holtrop & Mennen) and design a matching propeller (Wageningen B-series). |
| [Engine Selection](engine-selection.md) | Build an engine catalog and check the design's operating point against an engine's layout diagram. |
| [Optimize](optimize.md) | Automatically search the free dimension ratios for the best building cost / powering trade-off. |

Nothing else in SySHIP requires DIMENSION — you can go straight to importing an STL hull in
the HULL tab. But wherever DIMENSION *has* produced a design ship, HULL's Variation panels
and COMPART's Loading tab offer one-click buttons to reuse its recommended dimensions and
lightship weight.

!!! note
    DIMENSION's values are entered and displayed directly in their labeled unit (m, m³,
    ton, kW, rpm, knots, ...) — there's no separate unit-conversion step like HULL's
    millimeter-internal geometry.
