# Project Log — May 12, 2025

## Project: Internal Space Definition Tool (4601 Lyman Dr)

### Context

This project builds on a preliminary spatial layout created by Misha, which visually mapped the camera zones and general room structure of the 4601 Lyman building. While Misha’s version provided a useful visual reference for internal areas and camera coverage, it was not built using vector geometry or connected data.

My contribution focuses on translating that visual structure into a clean, scalable vector-based system using QGIS. I created precise, closed polygons and line segments that define each room and hallway, tag traversable vs. non-traversable edges, and assign unique UUIDs to all exit points. This vectorized structure allows for geometric continuity, logical edge sharing, full spatial coverage, and export to YAML — making it compatible with Ubihere’s backend object tracking and space reasoning systems.

### What I'm Working On

I'm building a tool to digitally map the internal layout of my company’s building (4601 Lyman Dr). The goal is to define and label all interior spaces (like rooms, hallways, and exits) so our system can understand the structure of the building and how people or objects move through it.

This space model will serve as the backbone of our object detection and tracking system. It will ultimately be exported into a structured YAML file that defines the building perimeter, traversable exits, and internal spaces in a machine-readable way.

---

### What I'm Doing

I'm exploring two implementation paths:

**Option A: QGIS**
- Load a floorplan image of the building
- Align (geo-rectify) the image to a base map
- Draw closed polylines for building perimeters and internal spaces
- Label each leg (line segment) as either `TRAVERSABLE` or `NOT_TRAVERSABLE`
- Export the result and convert it to a YAML file

**Option B: Custom Web Tool**
- Build a drawing tool in React using the HTML `<canvas>` element
- Allow users to manually draw and label building geometry
- Handle export and validation in the browser

Right now, I’m leaning toward QGIS. During our meeting, my CEO, Eric, emphasized that route and asked me to start learning it. The follow-up email was more flexible, but QGIS seems more efficient for now.

---

### The End Goal

The system should be able to:
- Know where all rooms, hallways, and boundaries are
- Identify which line segments represent doors or exits
- Link object tracking data to real, labeled spaces
- Output a clean YAML file with the following structure:
  - A `BUILDING` with a `PERIMETER` made of line segments
  - Internal `SPACES` with their own closed perimeters
  - Traversable edges labeled with unique UUIDs

This YAML file will be used by the SWE and tracking pipeline to reconcile object identities and manage building state throughout the day.

---

## What I Was Given vs. What I’m Building

### What I Was Given
- A geo-rectified floorplan of 4601 Lyman
- A rough perimeter polygon titled `"Ubihere-4601Lyman"`
- A partial YAML structure showing:
  - Building perimeter with labeled `TRAVERSABLE` vs `NOT_TRAVERSABLE` legs
  - Use of `uuid`s for exits
  - A placeholder internal space called `Big Unknown`
- A starter HTML/JS tool for drawing polygons (not production-ready)

### What I'm Building
- Broke the outer building polygon into individual **line segments**
- Labeled each segment as either `TRAVERSABLE` or `NOT_TRAVERSABLE`
- Assigned **unique UUIDs** to all `TRAVERSABLE` segments (e.g., `West_Garage_Door`)
- Created the first internal space: `Big Unknown`, sharing traversable edges with the building
- Will subdivide the interior into named rooms and spaces
- Ensuring 100% space coverage with clean edge continuity
- Applying:
  - **Color fills** to distinguish rooms
  - **Line color styles** for `TRAVERSABLE` (e.g., red) vs `NOT_TRAVERSABLE` (e.g., black)
- Preparing the data for export into the YAML format used by the object tracking system

### Summary
Taking the initial WIP and turning it into a fully vectorized, labeled, and structured space definition tool that meets backend ingestion requirements.
What I Was Given vs. What I’m Building

What Eric gave me:
	•	A geo-rectified floorplan of 4601 Lyman
	•	A rough perimeter polygon titled "Ubihere-4601Lyman"
	•	A work-in-progress idea of how the building perimeter should be structured in YAML, including concepts like TRAVERSABLE legs, uuids, and a placeholder space (Big Unknown)
	•	A basic HTML tool mockup (not fully functional) as a starting point for vectorizing space definitions

What he wants me to do:
	•	Break the outer building perimeter into individual line segments
	•	Label each leg as either TRAVERSABLE or NOT_TRAVERSABLE
	•	Assign a unique uuid to each TRAVERSABLE segment (e.g., doorways, exits)
	•	Create internal spaces starting with a Big Unknown polygon, and later break it into clearly defined rooms, hallways, etc.
	•	Make sure every space is fully enclosed and shares edges with its neighbors — no gaps, no overlaps
	•	Apply color fills to each space and color-code edge types (TRAVERSABLE vs. NOT_TRAVERSABLE)
	•	Ensure the final output can be exported to a YAML file with a clean structure the backend can ingest

In short: I’m taking his WIP base and turning it into a fully vectorized, labeled, color-coded, and exportable space definition system.
---

### New Context from Eric (5/13/25)

Eric clarified that this YAML structure will be used to **enforce object creation and reconciliation logic**.

- Object tracking should **start when the building is empty** and **end when the building is empty**.
- Only **terminal objects** (those that fully exit the building) should be on the start list.
- This means the space definition needs to be reliable enough to:
  - Represent entry and exit points (with `TRAVERSABLE` legs)
  - Support day-start and day-end object state validation
- Each `TRAVERSABLE` edge must have a **unique name (UUID)** like `West_Exit_Garage_Door`.

This reinforces that the tool I’m building is not just for visualization—it’s a core part of the logic layer for how our system understands space, movement, and object permanence.

---

### What I’ve Learned

- **Polyline** = A connected sequence of line segments (e.g., outlining a room)
- **Closed polyline** = A shape where the first and last points connect (i.e., a loop)
- **Geo-rectify** = Aligning a floorplan image to a real-world map or coordinate system
- **React canvas** = A way to draw on screen using the HTML `<canvas>` element inside a React app
- **Terminal objects** = Objects that fully exit the building and should be used as session boundaries in tracking logic
- **UUID (for traversable legs)** = A unique identifier assigned to each doorway or access point (e.g., `East_Conference_Exit`)

---

### Status Update 5/14/25 

- Installed QGIS
- Downloaded and reviewed the 4601 building floorplan
- Imported both the building image and a georeferenced base map
- Currently geo-rectifying the floorplan to match the real-world layout
- Next:
Create a draft of the building perimeter with at least two TRAVERSABLE legs (like doors), and one internal space ("Big Unknown").
  - Draw the full building perimeter as a closed polyline
  - Identify and label at least two traversable edges with UUIDs
  - Define the first internal space ("Big Unknown")
  - Begin structuring the first draft of the YAML file

## Actual work 
**5/14/25 @ 2:00 pm**
I took the geo-rectified floorplan and initial building perimeter Eric provided and broke it into individual edge segments using QGIS. This allows me to label each segment as either TRAVERSABLE or NOT_TRAVERSABLE, and assign unique UUIDs to any exits or entry points (e.g., West_Garage_Door, East_Side_Conference_Exit). The outer perimeter is fully defined and titled "Ubihere-4601Lyman".

Next, I’m creating the first internal space (Big Unknown) which shares all TRAVERSABLE legs with the outer perimeter. After that, I’ll begin subdividing the interior into individual rooms and spaces. Each space will be uniquely named, typed, and color-filled to allow for clear separation and YAML export. I’m enforcing full continuity and coverage to make sure every square foot is accounted for, and all shared edges are cleanly snapped and aligned.

The end goal is a fully labeled, color-separated, and logically structured building map that exports to the YAML format used by the object tracking system.

**5/15/25**
I created and exported a complete vectorized layout of the 4601 building using a georeferenced TIFF sourced from Ohio’s GIS portal. The PNG originally provided by Eric couldn’t be properly aligned in QGIS, so I substituted in the state-provided GeoTIFF to ensure accurate coordinate registration (EPSG:4326). Eric later confirmed this was the correct approach.

Using QGIS, I finalized three distinct GeoTIFF outputs:

- One with the floorplan cleanly rendered over satellite imagery

- One showing the internal layout with color-coded room types

- One identical to the above but with no fill for a minimal reference layer

Each version maintains correct spatial alignment and is suitable for integration into the environment builder.

Additionally, I exported the full layout as a .geojson file, preserving all segment and space metadata for future parsing and YAML transformation. I also provided simplified .png snapshots for quick visual reference and packaged the original Ohio TIFF data separately for transparency.

All files have been delivered to Misha for integration into the new React-based tool. I’m now preparing to begin collaborative work on that front.

