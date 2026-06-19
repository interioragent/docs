---
title: Constraints
layout: home
nav_order: 6
has_children: true
---

# Constraints

Placement gets objects *roughly* where they belong; **constraints** refine the layout so it
is physically valid and design-plausible. A constraint encodes a rule — "nothing should
overlap", "leave walking space in front of the sofa", "keep the nightstand within reach of
the bed", "don't block the view of the TV" — and the optimizer nudges objects until the rule
is satisfied.

IDSDL has two families of constraints.

## Gradient constraints

[Gradient constraints](gradient-constraints) are geometric and fast. Each one inspects the
current layout and produces a **gradient** — a direction and magnitude to move each object.
A solver applies these gradients iteratively until the layout settles. They run
automatically inside group compilation, and you can also add them explicitly.

| Constraint | Rule it enforces |
|---|---|
| `OverlapConstraint` | No two objects may intersect. |
| `OutOfBoundsConstraint` | Every object stays within the room walls. |
| `ClearanceConstraint` | Keep a minimum free space around an object. |
| `AccessConstraint` | Keep one object within a reachable distance band of another. |
| `VisibilityConstraint` | Keep the sightline between two objects clear. |

## VLM constraints

[VLM constraints](vlm-constraints) use a vision-language model to judge things geometry
alone can't — whether object **proportions** look right, whether the **room** feels the
right size, whether **wall-mounted** objects collide. They render the scene, ask the model
for feedback, and apply rescaling or surface textual guidance.

| Constraint | Rule it enforces |
|---|---|
| `ObjectProportionsConstraint` | Objects are sensibly sized relative to each other. |
| `RoomProportionsConstraint` | The room isn't too cramped or too empty. |
| `WallOverlapConstraint` | Wall-mounted objects don't share a slot. |

## How the solver works

The gradient solver treats each movable object as a particle:

1. **Accumulate gradients.** Every active constraint adds a force vector to each object.
2. **Pick moves.** The solver weights candidate moves by how much free space lies in that
   direction, so objects prefer to move into open areas rather than into walls or neighbours.
3. **Step.** A few objects are nudged along their gradients.
4. **Repeat** for a fixed number of steps, then a deterministic pass snaps apart any
   remaining overlaps and clamps anything still out of bounds.

Because the solver operates on a group's **direct children**, a frozen sub-group is moved as
a single rigid unit — see [Hierarchical Layout](hierarchical).

Continue to the two reference pages for each constraint, its parameters, and before/after
renders.
