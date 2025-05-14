# Project Log — May 12, 2025

## Project: Internal Space Definition Tool (4601 Lyman Dr)

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

5/14/25 @ 2:00 pm
I just finished breaking down the building perimeter into individual edge segments in QGIS. Now I’m setting up the attributes so each segment can be marked as either TRAVERSABLE or NOT_TRAVERSABLE, just like you described in the YAML structure. The building is fully geo-rectified, and the outer polyline is titled ‘Ubihere-4601Lyman.’ Next, I’m going to tag the door segments with unique UUIDs and start building the internal Big Unknown space that shares those traversals.

