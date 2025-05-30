# OSM Initial Demo  

## Pre-Meeting Setup  

### Start My Servers

**Terminal 1**
```bash
cd ~/Documents/osm_project/repos/mbtileserver 
./mbtileserver --port 8081 ohio-segmented.mbtiles 
```
 
**Terminal 2** 
```bash

cd ~/Documents/osm_project/viewer 
python3 proxy_server.py
```


## 2. Open My Browser Tabs (in this order) 
  - http://localhost:8085/map.html (my custom setup)
  - TileServer GL version (my production alternative)
  - http://localhost:8085/services/ohio (TileJSON metadata) 

## 3. Test Everything 

- Zoom/pan both maps
- Verify roads, buildings, water all visible
- Check that tiles load smoothly 


## 4. Demo 1: My Custom Tile Server (60 seconds) 

- Navigate to: localhost:8085/map.html 

  - Say: "This is Ohio running on our self-hosted infrastructure" 

- Pan around Columbus area - show detail levels 

- Zoom in/out - demonstrate smooth performance 

  - Point out features: "Roads, buildings, water - all from OSM data" 

- Show the Technical Endpoint (30 seconds) 

- Switch to: inspect -> network 

  - Point at specific point in the map  

- “This endpoint provides the tile URLs - localhost:8085/tiles/{z}/{x}/{y}.pbf" 

  - I believe this is the vector tile URL Evan requested for UbiViewer integration  

```bash
Ex: https://ubimaptiler.com/services/maptiler-osm-2020-02-10-v311-planet/tiles/{z}/{x}/{y}.pbf
```

## 5. Demo 2: TileServer GL (30 seconds) 

- Switch to TileServer GL tab 

  - "I also have this running on TileServer GL for production deployment" 

- Show same map data: "Handles both vector and raster tiles if thats something you’d be interested in. This wouldn’t be for the commericial deployment for the fleet trackers but for funsies” 

## 6. My Status Summary  

- "The infrastructure is built and tested for Ohio. Vector tiles are generating properly, the server is stable, and we have deployment options ready." 

## 7. Ask for Direction (30 seconds) 

- "What do you need from me next?" 

  - "Should I deploy this to the Hilliard server?" 

- Should I keep working locally for the rest of the United States before I move onto the Hilliard server? 

- "Do you want me to start processing the full USA dataset?" 

- "What's the priority order for rollout?" 

- When should I check in with Evan?  

 

## 8. Technical Questions & Answers 

**1. "How much data is this?"** 

"Ohio is 600MB. Full USA would be about 30GB total." 

**2. "How long to process other states?"** 

"Each state takes 2-4 hours to process. I can run multiple states in parallel." 

**3. "What about performance?"** 

Show the smooth zooming/panning "Tiles load fast locally. Much faster than leaflet. Production server will be even better." 

**4. "Integration with UbiViewer?"** 

"Evan just needs this tile URL. The format is standard - MapLibre GL handles everything. What more can I do to integrate this into Ubiviewer" 

**5. "What about OSRM?"** 

"That's the routing service for GPS snapping. Different component - I can set that up next." 

 

## 9. Details 

**Architecture Overview**

"OSM data goes through Tilemaker to create vector tiles, stored as MBTiles, served by our custom server with CORS handling." 

**Scalability**

"Same process works for any region. We can segment by state or region for faster loading." 

**Deployment Options** 

"I have both custom server and TileServer GL working. Either can deploy to production." 

 

## 10. Likely Outcomes & My Responses 

**"Deploy this to production"** 

"I need access to the Hilliard server and we can have this running in 2-3 days." 

**"Start on full USA"** 

"I'll begin processing state-by-state. Should take 2-3 weeks for complete coverage." 
