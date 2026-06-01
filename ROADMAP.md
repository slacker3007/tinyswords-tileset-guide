# Tiny Swords Asset Guide — Long-Term Roadmap

This document is the reference plan for growing the asset guide from the current
**v0.0.0.1** terrain tileset snapshot into full Tiny Swords pack coverage. Each
milestone documents **one** asset category in `guide.json`-style rules (and
extends the example renderer where applicable), so progress stays small and
verifiable.

**Current release:** `0.0.0.4` — resource semantics (visuals)  
**Next milestone:** `0.0.1.1` — buildings

---

## Versioning scheme

Four-segment versions (matches `0.0.0.1`):

| Pattern | Meaning |
| --- | --- |
| `0.0.0.x` | Incremental terrain-world milestones (steps 1–3) |
| `0.0.1.x` | Entities and effects (steps 5–7) |
| `0.0.1.4` | UI elements (step 8) |
| `1.0.0.0` | Full-pack release with schema and validation (step 9) |

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

**Not yet fully specced:** road overlay (`Tilemap_color6`), buildings, units,
particle FX, UI.

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

## Already covered (v0.0.0.3)

[`Terrain/Decorations/Rocks/guide.json`](Terrain/Decorations/Rocks/guide.json)
and [`Terrain/Decorations/Rocks in the Water/guide.json`](Terrain/Decorations/Rocks%20in%20the%20Water/guide.json)
document:

- Static ground rocks (`Rock1`–`Rock4`, 64×64 single-tile sprites)
- Animated water rocks (`Water Rocks_01`–`04`, 16-frame sheets from
  `Water Rocks_0N.aseprite`)
- Per-sprite anchors (bottom-centre), 1-tile footprints, y-sorted `props` render layer
- Placement rules: ground rocks on land only; water rocks on open water (8-neighbour
  water cells); layering notes relative to trees/bushes
- Example renderer scatters ground rocks on interior grass and water rocks on open
  water in the 200×200 map

---

## Already covered (v0.0.0.4)

[`Terrain/Resources/Gold/Gold Stones/guide.json`](Terrain/Resources/Gold/Gold%20Stones/guide.json)
and [`Terrain/Resources/Meat/Sheep/guide.json`](Terrain/Resources/Meat/Sheep/guide.json)
document:

- Static gold stones (`Gold Stone 1`–`6`, 128×128) with paired 6-frame highlight
  animations (`Gold Stone N_Highlight.png`, `Gold Stones.aseprite`)
- Animated sheep states: idle (6 frames), move (4 frames), grass (12 frames) from
  `Sheep.aseprite`
- Per-sprite anchors (bottom-centre), 1-tile footprints, y-sorted `props` render layer
- Placement rules: gold stones and sheep on walkable land / interior grass
- Example renderer scatters gold stones and sheep (idle/grass) on the 200×200 map;
  gameplay pickup and harvest semantics remain in backlog

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

#### 2. v0.0.0.3 — Rocks **(done)**

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

#### 3. v0.0.0.4 — Resource semantics (visuals) **(done)**

**Scope:** Gold stone props and sheep animation states — sprite metadata only (no
yields, harvest states, or pickup semantics yet).

| Asset | Location |
| --- | --- |
| `Gold Stone 1`–`6`, `Gold Stone N_Highlight` | `Terrain/Resources/Gold/Gold Stones` |
| `Sheep_Idle`, `Sheep_Move`, `Sheep_Grass` (from `Sheep.aseprite`) | `Terrain/Resources/Meat/Sheep` |

**Deliverables:**

- Per-folder `guide.json` under `Gold Stones/` and `Sheep/`
- Per-sprite dimensions, bottom-centre anchors, footprints
- Animation blocks for sheep states (frame count, fps, layout from Aseprite)
- Highlight sprites paired with each gold stone variant
- Placement rules: gold stones on walkable land; sheep on interior grass
- Example renderer: scatter gold stones and a small sheep group on the map

---

### Phase B — Entities and effects

#### 5. v0.0.1.1 — Buildings

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

#### 6. v0.0.1.2 — Units

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

#### 7. v0.0.1.3 — Particle FX

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

#### 8. v0.0.1.4 — UI elements

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

#### 9. v1.0.0.0 — Schema and release

**Scope:** Package the full guide for tooling and IDE consumption.

**Deliverables:**

- JSON Schema for the guide format
- Top-level index linking all per-folder guides
- Small validation script (`validate_guides.py` or similar)
- Combined **kitchen-sink** demo scene: terrain + props + resources + buildings +
  units
- Git tag `v1.0.0.0`

---

## Backlog

Deferred items — no version assigned until promoted. Give the next free
`0.0.0.x` segment when pulling one into the active milestone list.

| Item | Original scope | Notes |
| --- | --- | --- |
| **Road overlay** | Finish `Tilemap_color6` road network rules | Road-layer autotile, road placement (elevation tiers, stairs/ramp junctions), render pass, example road network; was milestone `0.0.1.0` |
| **Ambient overlays** | `Clouds_01`–`08`, `Rubber duck` | Sky/cloud drift, top overlay layer, parallax; deferred pending decision |
| **Tools** | `Tool_01`–`04` in `Terrain/Resources/Tools` | Static tool props; was bundled with resource semantics |
| **Resource pickups** | `Gold_Resource`, `Gold_Resource_Highlight`, `Meat Resource` | Gameplay pickup semantics and interaction anchors |
| **Wood resource states** | Tree → stump transitions, wood yields | Links harvest behaviour to the trees guide from step 1 |

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
- **Rendering:** Phase A milestones (1–3) extend the example map renderer;
  Phase B–C (5–9) are docs-first unless noted.
- **Art:** never redistributed in this repo; guides reference paths inside a
  licensed local copy of the pack.

---

## How to use this roadmap

1. Pick the **Next** milestone (currently **0.0.1.1 — Buildings**).
2. Implement guide rules + example updates for that category only.
3. Bump [`VERSION`](VERSION), update this file’s “Current release / Next” lines,
   and tag on GitHub.
4. Repeat — no multi-category mega-releases.
