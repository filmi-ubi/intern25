# Project Log — May 14, 2025

## Project: Internal Space Definition Tool (4601 Lyman Dr)

### What I'm Working On

I'm building a tool to digitally map the internal layout of my company’s building (4601 Lyman Dr). The goal is to define and label all interior spaces; like rooms, hallways, and exits, so our system can understand the structure of the building and how people or objects move through it.

Eventually, this map will be exported into a YAML format that our tracking system can use to understand where things are happening spatially.

---

### What I'm Doing

I'm exploring two implementation paths:

**Option A: QGIS**
- Load a floorplan image of the building
- Align (geo-rectify) the image to a base map
- Draw closed polylines for building perimeters and interior spaces
- Label each line segment with its type (e.g., TRAVERSABLE, NOT_TRAVERSABLE)
- Export the result and convert to YAML

**Option B: Custom Web Tool**
- Build a drawing tool in React using the HTML `<canvas>` element
- Allow users to manually draw and label building geometry
- Handle export and validation in the browser

Right now, I’m leaning toward QGIS. During our meeting, my CEO, Eric, emphasized that route and asked me to start learning it. The follow-up email was more relaxed and left the implementation approach up to me.

---

### The End Goal

The system should be able to:
- Know where all rooms, hallways, and boundaries are
- Identify which line segments represent doors or exits
- Use that data to link object tracking to real, named spaces
- Output a yaml file for our tracking system.

---

### What I’ve Learned

- **Polyline** = A connected sequence of line segments (e.g., outlining a room)
- **Closed polyline** = A shape where the first and last points connect (i.e., a loop)
- **Geo-rectify** = Aligning a floorplan image to a real-world map or coordinate system
- **React canvas** = A way to draw on screen using the HTML `<canvas>` element inside a React app

---

### Status Update

- Installed QGIS
- Downloaded base imagery of 4601 Lyman Dr, created by Eric
- Imported the floorplan image
- Starting to geo-rectify and align it with the real-world building footprint
- Next: draw building perimeter and define the first interior space
