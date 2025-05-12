**Eric's initial email** 

- I need a self-hosted map service for Open Street Map (OSM) which is really just a protocol for map formats which are created, curated, leveraged, and shared amongst a massive global community which hates that people have to use Google or Apple Maps.
We have a massive processing box, a webserver, and a multi-petabyte NAS in Hilliard we are starting to use for projects and this is one of them.
I have some folks looking at it but would need a good implementation and development lead full time on it as we start rolling out commercially.
I have lots of map experience and actually used to do development on the JMTK which was the military's first moving and vectored maps which later became ESRI and Google Maps, so will be able to help on the map standards/formats/etc. side but need a dedicated super star to lead the spin-up and commercial launch to support of fleet tracking product launching in a month.

*Notes*:

- The community is very active and there is amazing documentation on all aspects of creating a self serving solution.
https://switch2osm.org/
  - either Ubuntu or Docker with our own tiler
  - (https://switch2osm.org/serving-tiles/)
  - Download the whole country but start with Ohio or just Columbus.

- We have built the user interface system which can render from MBTiles (I want our own non-AWS implementation) and OSM and Google Maps (as needed). See below. You will work with an amazing front-end developer who will teach you a lot as well here. I believe he uses MapLibre but we will chat with him if you pick this one. The world's data is free and continuously updated. https://wiki.openstreetmap.org/wiki/Planet.osm#Processing_the_file
- The main project page defines everything about the maps:
  - https://wiki.openstreetmap.org/wiki/Main_Page

- OSRM is the open source solution we will use to connect our 1Hz max GPS updates to roads most likely traveled, place most likely parked, and calculate relevant leg-to-leg analytics they are interested in such as speeding drivers, etc.
We need an OSRM service running with each fleet instance which accesses our common OSM datasets.

- This will be a full time large responsibility (but with lots and lots of help available) for the summer and most likely through the school year and is very important to me.
