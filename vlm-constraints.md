---
title: VLM Constraints
layout: home
parent: Constraints
nav_order: 2
---

# VLM Constraints

Some design judgments can't be made from bounding boxes alone. Is the coffee table
ridiculously large next to the sofa? Does the room feel cavernous around a single armchair?
Do two pictures fight for the same patch of wall? **VLM constraints** answer these by
rendering the scene and asking a vision-language model.

Each VLM constraint renders the relevant view(s), sends them to the model with a focused
prompt, and either:

- **applies a correction** automatically (e.g. a rescale factor for an object or the room),
  or
- **records textual feedback** on `scene.vlm_feedback` for a later pass or for the user.

These constraints run automatically at the end of the relevant group's compilation — object
proportions for anchored groups, room proportions and wall overlap for rooms — so you
usually don't call them by hand. They require a configured vision-language model and are
slower than gradient constraints.

```python
# triggered automatically during compile; the result accumulates here:
print(scene.vlm_feedback)
```

---

## ObjectProportionsConstraint

Renders an anchored group from four sides and asks the model whether any object is clearly
too large or too small relative to the others. The model replies with rescale instructions
such as `rescale coffee table by 0.5`, which are applied to bring the cluster into sensible
proportion (or `no rescale` if everything looks right).

Runs automatically when a `RelativeGroup` or `AroundGroup` compiles. This is why a seating
cluster whose retrieved coffee table happens to be oversized will be quietly corrected before
the group freezes.

---

## RoomProportionsConstraint

Renders the whole room and considers both its appearance and its **occupancy ratio** (the
fraction of floor area covered by furniture). An occupancy around `0.4` reads as comfortable;
much lower feels empty, much higher feels cramped. The model responds with a room rescale
instruction (e.g. `rescale room by 0.8`) so the room is resized to suit its contents.

Runs automatically when a `RoomGroup` compiles. It complements the `modulate_scale`
parameter: `modulate_scale` is your explicit size preference, while this constraint is an
automatic sanity check.

---

## WallOverlapConstraint

Checks that wall-mounted objects don't collide. The room tracks which objects occupy each
wall slot (`left`, `center`, `right` on each of the four walls); if two objects land in the
same slot, this constraint surfaces feedback describing the conflict and suggesting they be
moved to different positions.

```
back_wall slot 'center' has multiple occupants: 'a large abstract painting', 'a round mirror'.
```

Runs automatically when a `RoomGroup` compiles, and writes its findings to
`scene.vlm_feedback`.

---

## Working with feedback

All VLM feedback accumulates on `scene.vlm_feedback` as plain text. After exporting you can
inspect it to understand what the model flagged:

```python
with scene.RoomGroup() as room:
    room.place_on_back_wall_center(sofa, facing="front")
    room.place_on_front_left_corner(plant, facing="right")

print(scene.vlm_feedback or "(no issues flagged)")
```

This makes the VLM layer useful both as an automatic corrector and as an advisory signal you
can act on in your own program or surface to an end user.
