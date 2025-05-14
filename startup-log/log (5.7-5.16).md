
## 5/7/25

- Set up a Linux VM on a Mac (M2) using Multipass to run Ubuntu Installed Label Studio manually
- Annotated object detection data in YOLO format.
- Installed Docker Desktop and used it from the terminal.
- Cloned and ran a machine learning backend using docker compose, with a YOLO model that:
- Runs locally
- Talks to Label Studio
- Supports automated annotation
- Connected the backend to Label Studio to enable the Auto Annotate feature.
- Understood the difference between:
- Manual vs. automated labeling
- Local vs. containerized environments
- The YOLO model workflow and export format
- THEN SWITCHED TO LINUX FOR YOLO!

*Pivot:*
- Rebuilt a Linux dev environment from scratch
- Installed Docker, Label Studio, and a full backend
- Connected Mac and Linux with shared folder access
- imported BVy data set

## **5/8/25**

- Downloaded and set up ubuntu on vm 
- Onboarding and quickbooks finished

## **5/11/25**

- got work laptop and got all my virtual environments downloaded
- got automation for label studio to work 
    - dont forget to run backend while label studio is running for predictions to work 

want to do:
- possibly train the backend to be smarter? 




## 5/12/25

What I'm Doing — My Role Right Now
So, here’s where I’m at after the latest meeting with the CEO:

I'm working on building a frontend that visualizes object movement and identity across a mapped building layout. The backend stuff (like how data is stored or generated) is being handled by Noah — he’s setting up the source data either in MongoDB or something like Google Sheets. I'm not touching the heavy backend logic.

The Main Idea
- We're using vector mapping of the building, which I just learned is kind of like turning the blueprint into a digital map. Every part of the building has a defined space:

- Bounded spaces: rooms and areas enclosed by walls.

- Transitional spaces: known hallways and entrances.

- Unknowns: places we don't track actively — we kind of ignore those.

- The tech stack includes JavaScript and QGIS, which is a free tool used for mapping stuff (it's basically the open-source version of something called "Esri" — I’m still figuring that part out).

More In-Depth: How I Fit Into This
I’m responsible for:

- Pulling data from Noah’s backend (object IDs, images, movement logs).

- Visualizing it on a map that matches the building layout.

- Representing different areas (bounded, transitional, unknown).

- Building a visual identity-tracking system — so if someone leaves and comes back, we can figure out who they used to be and where they went.

- This also connects back to my earlier task:

- Showing 5-entry and 5-exit images per object,

- Matching unknowns to knowns when they return,

- And updating the table or visual space once that match is made.

Immediate jobs

- learn how to use qgis and how its used with the blueprint stuff

Questions I Still Have

1.  Mapping + Vector Stuff

- What exactly does "vector mapping" mean in the context of QGIS? Is it just a grid layout of rooms?

- How do I represent walls and constraints? Am I pulling these from the blueprint or is that already digitized for me?

  

2. Tech + Data

- Will Noah’s data include position info that maps to QGIS coordinates? Or do I need to connect object locations to specific rooms manually?

- Do I need to install QGIS locally, or is there a JS library I’ll be working with directly?

- How much should I know about coordinate systems or layers in QGIS? Should I just be treating them like tagged zones?

  

3. Unknown Zones

- What exactly counts as an "unknown" zone? Is that anything not marked as bounded or transitional?

- Do I still need to track entry/exit in those areas or are they just ignored completely?

4. Frontend Responsibilities

- When I’m visualizing this in React or HTML/CSS, am I literally drawing the floorplan? Or overlaying data onto an already-rendered map?

- Should I expect to build some kind of live timeline or path visualization? Or is it more static and session-based?

5. Next Steps
- Noah will send over a diagram showing the data structure and how the movement + object identity is laid out.

- Once I get that, I’ll look it over and figure out how to connect it to what I’m building on the frontend.

## 5/13/25 
 Summary: What I’m Doing (as of now)
Today Eric told me he wants me to build the Space Definition Tool for our building, 4601 Lyman Dr, Hilliard, OH. The output of this tool will be a detailed YAML file that defines the layout and logic of the space using vector mapping (likely from QGIS or a blueprint). Basically, I’ll be breaking down the building into tagged areas like:

- Bounded spaces (e.g., rooms enclosed by walls)

- Transitional spaces (e.g., doorways, hallways)

- Unknown spaces (stuff outside of our scope — not important)

- This YAML will serve as the structural backbone for track rectification/track merge, the tool Noah’s building that pieces together object tracks over the course of a day using entrance/exit logic.

How It All Connects
- Noah is coding the backend logic for track rectification, and Bushra is building the frontend skeleton. It is on the Ubihere github under 

- I’m working alongside them, but now I also own the spatial definition side — mapping out spaces that get used by the rectification logic.

- Once I finish the space definitions and generate the YAML, it’ll integrate with their work by telling the system how the building is structured, so it knows how to interpret movement data (e.g. entering Room A from Hallway 1 means X).

- I don’t need to worry about BVy or annotation stuff — Bushra confirmed that’s separate.

 Key Terms (as I understand them)
- QGIS: A tool used to define and tag building spaces on a map using vector data. Basically, it turns a blueprint into a structured layout.

- Vector Mapping: Labeling building regions (like rooms, doors, halls) as geometric shapes so we can use them in code.

- YAML Output: A structured text file that defines what each space is, its type (bounded/transitional), and probably its coordinates or labels.

- Track Rectification: The process of stitching together fragmented object tracks using spatial context — basically, matching object IDs over time as people walk through known doorways.

 Questions I Still Have
1. YAML Structure
What exactly needs to go in the YAML?

Do I need to define coordinates? Labels? Room types? Doorway connections?

2. QGIS Integration
Will I be working inside QGIS directly and exporting?

Or just referencing vector/blueprint data and writing YAML manually?

3. Space Logic
How do I differentiate transitional vs bounded in the YAML? Is that a field like type: transitional?

How do we represent connections (like Room A connects to Hallway B via Door 3)?

4. Frontend Visualization
Will this YAML be visualized anywhere in the tool I’m working on? Or is it purely backend logic?

5. Collaboration
Once I define the space layout, who’s using it next — just Noah's backend, or does Bushra also reference it in the UI?


**5/13/25**
