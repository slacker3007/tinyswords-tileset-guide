# Tiny Swords Asset Guide — Long-Term Roadmap

This document is the reference plan for growing the asset guide from the current
**v0.0.0.1** terrain tileset snapshot into full Tiny Swords pack coverage. Each
milestone documents **one** asset category in `guide.json`-style rules (and
extends the example renderer where applicable), so progress stays small and
verifiable.

**Current release:** `0.0.0.2` — trees and bushes  
**Next milestone:** `0.0.0.3` — rocks

---

## Versioning scheme

Four-segment versions (matches `0.0.0.1`):

| Pattern | Meaning |
| --- | --- |
| `0.0.0.x` | Incremental terrain-world milestones (steps 1–4) |
| `0.0.1.0` | **Terrain world complete** (step 5) |
| `0.0.1.x` | Entities and effects (steps 6–8) |
| `0.0.1.4` | UI elements (step 9) |
| `1.0.0.0` | Full-pack release with schema and validation (step 10) |

Each milestone bumps the last segment. Tag releases on GitHub when a milestone
ships.

---

## Already covered (v0.0.0.1)

[`guide.json`](guide.json) documents:

- Shadow, water background, water foam
- Full terrain tilemap system: RuleTile-style 4-neighbour autotile, 5 elevation
  levels, cliffs, stairs/ramps, shadows, foam
- Hard placement invariants (`elevationGradient`, autopad)
- Accessibility rules and ramp density

**Not yet fully specced:** road overlay (`Tilemap_color6`), rocks, clouds,
resource semantics, buildings, units, particle FX, UI.

---

## Already covered (v0.0.0.2)

[`Terrain/Resources/Wood/Trees/guide.json`](Terrain/Resources/Wood/Trees/guide.json)
and [`Terrain/Decorations/Bushes/guide.json`](Terrain/Decorations/Bushes/guide.json)
document:

- Animated trees (`Tree1`–`Tree4`, 8-frame sheets from `Trees.aseprite`) and static
  stumps (`Stump1`–`Stump4`)
- Animated bushes (`Bushe1`–`Bushe4`, 8-frame sheets from `Bushes.aseprite`)
- Per-sprite anchors (bottom-centre), 1-tile footprints, y-sorted `props` render layer
- Placement rules: land only; not on water, cliff bases, or ramps; prefer interior
  centre grass
- Example renderer scatters trees, bushes, and stumps on the 200×200 map

---

## Milestones

### Phase A — Terrain world

#### 1. v0.0.0.2 — Trees and bushes **(done)**

**Scope:** Animated trees (8-frame) and static stumps + animated bushes.

| Asset | Location |
| --- | --- |
| `Tree1`–`Tree4`, `Stump1`–`Stump4` | `Terrain/Resources/Wood/Trees` |
| `Bushe1`–`Bushe4` (animated) | `Terrain/Decorations/Bushes` |

**Deliverables:**

- Per-sprite metadata: dimensions, ground anchor, 1-tile collision footprint
- Render layer: y-sorted props above ground tiles
- Tree and bush animation blocks (8 frames, 10 fps from Aseprite)
- Placement rules: on grass elevation tiers; never on water, cliff bases, or ramp
  cells
- Example: scatter trees/bushes on the rendered map

---

#### 2. v0.0.0.3 — Rocks **(NEXT)**

**Scope:** Ground rocks and animated water rocks.

| Asset | Location |
| --- | --- |
| `Rock1`–`Rock4` | `Terrain/Decorations/Rocks` |
| `Water Rocks_01`–`04` (animated) | `Terrain/Decorations/Rocks in the Water` |

**Deliverables:**

- Footprints, land vs water placement rules
- Water-rock animation metadata
- Layering relative to trees/bushes
- Example: scatter on the rendered map

---

#### 3. v0.0.0.4 — Ambient overlays

**Scope:** Sky/cloud drift and water ambient props.

| Asset | Location |
| --- | --- |
| `Clouds_01`–`08` (animated) | `Terrain/Decorations/Clouds` |
| `Rubber duck` (animated) | `Terrain/Decorations/Rubber Duck` |

**Deliverables:**

- Formal **top overlay** render layer (above all world props)
- Cloud drift / parallax notes
- Water-only placement for rubber duck
- Example: optional cloud strip and duck on water

---

#### 4. v0.0.0.5 — Resource semantics

**Scope:** Gameplay metadata layered on terrain props (harvestable resources).

| Asset | Location |
| --- | --- |
| Wood (tree → stump states) | `Terrain/Resources/Wood` |
| `Gold Stone 1`–`6`, `Gold_Resource`, highlights | `Terrain/Resources/Gold` |
| `Sheep` (Idle/Move/Grass), `Meat Resource` | `Terrain/Resources/Meat` |
| `Tool_01`–`04` | `Terrain/Resources/Tools` |

**Deliverables:**

- Resource type definitions: yields, states, highlight sprites
- Interaction anchor points
- Links from tree/stump entries in step 1

---

#### 5. v0.0.1.0 — Road overlay complete

**Scope:** Finish `Tilemap_color6` road network rules.

**Deliverables:**

- Road-layer autotile (reuse or extend `flatGroundLookup` on overlay layer)
- Where roads may run (elevation tiers, junctions with stairs/ramps)
- Render pass for road overlay
- Example map with a road network

**Milestone:** terrain world is fully documented and renderable.

---

### Phase B — Entities and effects

#### 6. v0.0.1.1 — Buildings

**Scope:** All faction-colored structures.

| Buildings | Colors |
| --- | --- |
| Castle, Barracks, Archery, House1–3, Monastery, Tower | Blue, Red, Purple, Yellow, Black |

**Deliverables:**

- Multi-tile footprints, placement (flat ground only)
- Anchor, y-sort, optional drop shadow
- Per-folder `guide.json` under `Buildings/`
- Docs-first; rendering optional

---

#### 7. v0.0.1.2 — Units

**Scope:** Playable and NPC units (5 colors each).

| Unit | Animation sets |
| --- | --- |
| Warrior | Idle, Run, Attack1–2, Guard |
| Lancer | Directional Idle/Run/Attack/Defence |
| Archer | Idle, Run, Shoot + Arrow projectile |
| Monk | Idle, Run, Heal + Heal_Effect |
| Pawn | Idle/Run/Interact variants (axe, hammer, knife, pickaxe, wood, gold, meat) |

**Deliverables:**

- Frame sizes, 10 fps, direction mapping
- Anchor points; note Aseprite source in Blue-only folder
- Per-color variant block
- Docs-first; rendering optional

---

#### 8. v0.0.1.3 — Particle FX

**Scope:** Combat and ambient effects.

| Asset | Notes |
| --- | --- |
| `Dust_01`–`02` | Combine for large dust |
| `Fire_01`–`03` | Loop for continuous fire |
| `Explosion_01`–`02` | Combine for large blasts |
| `Water Splash` | Character-in-water trigger |

**Deliverables:**

- One-shot vs loop, combination recipes
- Anchor and layer above units
- Docs-first; rendering optional

---

### Phase C — UI and release

#### 9. v0.0.1.4 — UI elements

**Scope:** Menus, HUD, and chrome.

| Category | Assets |
| --- | --- |
| Banners / papers / table | 9-slice stretch regions |
| Bars | BigBar, SmallBar (base + fill) |
| Buttons | Square/round, blue/red, pressed/regular |
| Cursors | `Cursor_01`–`04` |
| Icons | `9_elemental_towers_icons` atlas, tower icons |
| Ribbons / swords | 5 faction colors, horizontal stretch |

**Deliverables:**

- Stretch regions and state machines per control type
- Per-folder guide under `UI Elements/`
- Docs-only

---

#### 10. v1.0.0.0 — Schema and release

**Scope:** Package the full guide for tooling and IDE consumption.

**Deliverables:**

- JSON Schema for the guide format
- Top-level index linking all per-folder guides
- Small validation script (`validate_guides.py` or similar)
- Combined **kitchen-sink** demo scene: terrain + props + resources + buildings +
  units
- Git tag `v1.0.0.0`

---

## Conventions (apply from step 1 onward)

So later milestones stay small, each new `guide.json` reuses these shared concepts:

| Concept | Purpose |
| --- | --- |
| **Anchor point** | Where the sprite meets the ground tile (typically bottom-centre) |
| **Tile footprint** | How many 64×64 cells the prop occupies for collision/placement |
| **Render layers** | Ordered stack: water → ground → cliffs → props → shadows → foam → overlay |
| **Animation block** | `frameCount`, `fps`, `loop`, `sourceFile` (Aseprite when present) |
| **Faction-color block** | Same geometry, different palette file paths (buildings/units) |

**File layout:** one `guide.json` per asset-pack folder, mirroring
`TinySwords/` structure (e.g. `Terrain/Decorations/Bushes/guide.json`).

---

## Assumptions

- **Scope:** whole Tiny Swords pack; terrain-first ordering.
- **Rendering:** Phase A milestones (1–5) extend the example map renderer;
  Phase B–C (6–10) are docs-first unless noted.
- **Art:** never redistributed in this repo; guides reference paths inside a
  licensed local copy of the pack.

---

## How to use this roadmap

1. Pick the **Next** milestone (currently **0.0.0.3 — Rocks**).
2. Implement guide rules + example updates for that category only.
3. Bump [`VERSION`](VERSION), update this file’s “Current release / Next” lines,
   and tag on GitHub.
4. Repeat — no multi-category mega-releases.
