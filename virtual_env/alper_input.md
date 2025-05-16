# 4601 Object Tracking System (Initial Logic Summary)

This document outlines the object detection and space-tracking logic for 4601 Lyman Dr. It’s written to clarify how we define building boundaries, track objects across visible and non-visible spaces, and maintain internal consistency over time.

## Building Perimeter and Traversals

The building boundary is defined using a vector polyline around the main structure (excluding the Lyman conference area). Each edge segment can be:

- **Non-traversable**: Static walls.
- **Traversable (red)**: External doorways where objects enter or exit the building.

Every traversable segment is **shared with at least one internal space**. This is critical for defining when an object enters ("Seminal") or exits ("Terminal") the building.

## Space Definitions

Spaces are drawn as vector polygons inside the building. Each one is:

- **Visible or Non-Visible** – depending on camera coverage.
- **Constrained or Unconstrained** – depending on how many ways in/out exist.

Definitions:
- **Visible**: Cameras can reliably detect and localize objects.
- **Non-Visible**: No reliable detection; requires inference based on entry/exit behavior.
- **Constrained**: Only one entry/exit; better for tracking continuity.
- **Unconstrained**: Multiple paths; introduces ambiguity.

## Object Lifecycle: Core Concepts

The system operates on a **1-second update loop**. Each second, we track objects and update tables based on their presence, disappearance, or reappearance.

### Seminal Object
Created when an object is **first detected in a Visible space** that borders the building (via a red Traversable segment).

We store:
- Object ID
- Space it was created in
- Vector segment it entered through

### Transitioning Object
When an object **moves from a Non-Visible space into a Visible space**, we generate a new Object ID and mark the boundary segment it crossed. Since we lost track during the Non-Visible phase, this is treated as a new appearance.

### Terminal Object
When an object **disappears from a Visible space** through a red Traversable segment (external exit), it becomes a Terminal Object. This is logged with its final position and exit vector.

## Tracking Continuity and Reconciliation

Objects may "disappear" into Non-Visible spaces and re-emerge elsewhere. When that happens, the system does not assume continuity. Instead:

- The Non-Visible space count increases
- A new object is created upon reappearance
- Reconciliation happens *later*, by comparing entry and exit times, locations, and optionally, visual snapshots (handled manually or by DNNs)

The total number of **Seminal = Terminal** object records defines a full tracking cycle for the day. IDs may not match, but counts must.

## Example: Day-in-the-Life

1. Object 1 enters through Logistics at 09:14:01 → Seminal Object created.
2. Object 1 transitions into UbiTrax → now in Non-Visible space.
3. Later, Object 5 is created in Operations from UbiTrax → Transitioning Object.
4. Object 5 exits through "Parking_Exit" → Terminal Object.

Building count is updated every step:
- Always tracked to match space-level totals.
- Assumes object count stays constant unless entry or exit occurs.

## Summary

This framework gives us second-by-second awareness of where objects are, where they’re likely going, and when they leave. It allows ambiguity (via Non-Visible spaces) but provides a structure to reconcile and validate movements.

The end goal is to produce consistent, reconcilable object logs with minimal drift from reality—even if visual gaps exist.

