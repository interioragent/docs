---
title: Groups
layout: home
nav_order: 5
has_children: true
---

# Groups

Placing every object by absolute coordinate is tedious and brittle. Interior design is
inherently *relational* — "the chairs go **around** the table", "the desks form a **grid**",
"the nightstands sit **beside** the bed". IDSDL captures this with **groups**: spatial
abstractions that take high-level placement intent and resolve it into concrete coordinates.

## The group context

Every group is used as a Python context manager:

```python
with scene.RelativeGroup() as seating:
    sofa  = scene.AddAsset("a modern 3-seat sofa")
    table = scene.AddAsset("a rectangular wooden coffee table")
    seating.set_anchor(sofa)
    seating.place_on_front(table)
# <- the group compiles here, when the `with` block closes
```

Inside the block you **declare** placements; you do not compute positions. The placement
calls are *recorded*, not executed immediately. When the `with` block exits, the group
**compiles**:

1. all recorded placements are executed in a sensible order,
2. child groups are compiled first (bottom-up),
3. optimization runs (overlap resolution, and for some groups proportion/clearance checks),
4. the group **freezes** into a single rigid unit.

After compilation the group behaves like one object: it has its own bounding box, can be
rotated and placed, and can be handed to another group as if it were a single asset. This is
what makes **hierarchies** possible — see [Hierarchical Layout & Parent–Child
Relationships](hierarchical).

## The four group types

| Group | Purpose | Key methods |
|---|---|---|
| **[RelativeGroup](relative-group)** | Place objects *relative to an anchor* (in front of, beside, behind, on top of). | `set_anchor`, `place_on_front/left/right/back`, `place_on_*_corner`, `place_on_*_further`, `place_on_top`, `place_rug` |
| **[AroundGroup](around-group)** | Arrange objects *around an anchor* — rings, circles, arcs. | `set_anchor`, `place_rectilinear`, `place_circle`, `place_arc` |
| **[GridGroup](grid-group)** | Arrange objects in *regular patterns* — rows, grids, borders, arcs. | `place_row`, `place_grid`, `place_rectilinear`, `place_arc` |
| **[RoomGroup](room-group)** | The top-level container — a room with walls, grid positions, and wall-mounted objects. | `place_on_center/back/front/...`, `place_on_*_wall_*`, `place_walls`, `place_door`, `place_window_*` |

There is also **`BasicRoomGroup`**, a room with explicit dimensions and manual placement (used
for testing and precise control, documented on the [RoomGroup](room-group) page), and
**[SentenceASCIIGenerator](sentence-ascii-generator)**, which lays out copies of an object in
the shape of text.

## Anchors

`RelativeGroup` and `AroundGroup` are *anchored* groups: one object (set with
`set_anchor`) defines the local origin and orientation, and everything else is placed
relative to it. The anchor is itself part of the group. `GridGroup` is *anchorless* — it
arranges objects in a pattern around its own center.

## Common parameters

Two parameters appear across the pattern groups:

- **`sparsity`** (`AroundGroup`, `GridGroup`) — a value in `[0, 1]` controlling how tightly
  packed the objects are. `0` is dense (objects nearly touching), `1` is spread out.
- **`randomness`** (`GridGroup`) — a value in `[0, 1]` that jitters spacing to produce a
  more natural, less mechanical look.

Continue to the individual group pages for the full method and parameter reference, each
illustrated with top-down (floor-plan) and perspective renders.
