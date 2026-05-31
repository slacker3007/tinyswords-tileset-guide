# TinySwords Tileset Guide

**Version 0.0.0.1**

A machine-readable usage guide for the **TinySwords** terrain tileset
(`Terrain/Tileset`), designed so an IDE / map tool can place tiles correctly and
procedurally generate valid maps. This repo contains the authored guide and
reference imagery only — **not** the TinySwords art assets themselves (those are
covered by their own license and are not redistributed here).

## Contents

| File | Description |
| --- | --- |
| `guide.json` | The full tileset specification: asset metadata, the RuleTile-style 4-neighbour autotile lookup, elevation system (5 levels + road overlay), cliff/stair/ramp rules, shadow & water-foam placement, and the hard placement invariants. |
| `Tilemap_color1_grid.png` | `Tilemap_color1` overlaid with a 64×64 grid and **raw row-major sheet IDs (1–54)**. |
| `Tilemap_color1_guide_ids.png` | The same sheet annotated with the **semantic piece IDs** used in `guide.json` (e.g. `FG 5` centre grass, `EG 21` water cliff, `ST 25` ramp top) and each tile's role. |
| `example-map.png` | A rendered 200×200 example map produced from `guide.json` (random multi-elevation terrain with a left-side sea, valley water channels, and frequent access ramps). |

## Key concepts captured in `guide.json`

- **Grid:** every tile is 64×64 px; ground sheets are 576×384 (9×6 cells).
- **Elevation:** 5 levels (`Tilemap_color1`..`color5`, lowest→highest); `color6` is a road overlay, not an elevation.
- **Autotiling:** a 4-neighbour (N/E/S/W) same-elevation bitmask selects the piece via `flatGroundLookup`.
- **South-only cliffs (the big rule):** the art only has **south-facing** cliffs.
  So land elevation may only step **down toward the south**; within any
  horizontal run of land elevation is constant, and any east/west/north
  elevation change must be separated by **water**. (`placementConstraints.elevationGradient`.)
- **Cliffs:** a single base row (pieces 17–20 onto land, 21–24 into water) plus a
  lip substitution gives a consistent 2-tile cliff face; tall drops are stepped
  into 1-tile staircases by the autopad algorithm.
- **Stairs / ramps:** 2-part side ramps bridge exactly one elevation step, with
  side-ramp/cliff-end tile substitutions, a walkable exit apron, and
  facing-side clearance.
- **Accessibility:** every elevated region must reach the tier below via a ramp,
  and `rampDensity` keeps ramps frequent (no cell more than 30 tiles from one).
- **Shadows** sit only under elevated ground; **water foam** rings elevation-0
  shorelines.

## Versioning

`0.0.0.1` — first snapshot of the terrain tileset guide (autotiling, elevation,
cliffs, stairs/ramps, shadows, foam, placement invariants, ramp density) with
annotated reference sheets and a rendered example map.

> Note: the TinySwords asset pack is **not** included in this folder. Place the
> guide alongside your licensed copy of the tiles to use it.
