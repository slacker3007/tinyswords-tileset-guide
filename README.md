# TinySwords Tileset Guide

**Version 0.0.0.1**

A machine-readable usage guide for the **Tiny Swords** terrain tileset
(`Terrain/Tileset`), designed so an IDE / map tool can place tiles correctly and
procedurally generate valid maps. This repo contains the authored guide and
reference imagery only — **not** the Tiny Swords art assets themselves. The pack
is free (name-your-own-price) from its creator **Pixel Frog**; grab it directly:
<https://pixelfrog-assets.itch.io/tiny-swords>. We don't bundle the art here
because the license forbids redistributing/repackaging it (see
[Credits & license](#credits--license)).

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

## Credits & license

The terrain tiles described by this guide are from the **Tiny Swords** asset
pack by **Pixel Frog**:

- Asset pack: <https://pixelfrog-assets.itch.io/tiny-swords>
- Author: Pixel Frog — <https://pixelfrog-assets.itch.io>

All credit for the artwork goes to Pixel Frog. Crediting isn't required by the
license, but it helps and is always welcome.

**Asset license (quoted from the Tiny Swords page):**

> Feel free to use this asset pack in both personal and commercial projects,
> modifying the assets as needed.
> Crediting is not required, but it helps and is always welcome.
> You may not redistribute, resell, or repackage the assets, even if the files
> are modified.

Because of the no-redistribution clause, **the Tiny Swords art is not included
in this folder** — download it from the link above and place this guide
alongside your own copy of the tiles.

> The `guide.json`, the annotated reference images, and the example map in this
> folder are our own authored work.
