## OSM Tile Server Progress Log 

# Terminal Commands to get website running locally:
```bash
 cd Documents/osm_project/repos/mbtileserver: ./mbtileserver --port 8081 ohio-segmented.mbtiles
 cd Documents/osm_project/viewer: python3 proxy_server.py &
  - If port is being used: sudo lsof -i :PORT_IN_USE -> sudo kill <PID>
 website: http://localhost:8085/map.html
```

# Terminal Commands to connect to tileserver
```bash
cd Documents/osm_project: tileserver-gl ohio-complete.mbtiles
website: http://localhost:8080/
```

# Terminal Commands to get custom backend running on TestServer:
```bash
 cd osm_project/repos/mbtileserver: ./mbtileserver --port 8081 ohio-segmented.mbtiles
 cd osm_project/viewer: python3 proxy_server.py &
 website: http://10.0.0.116:8080/map.html
```

### Leaflet Implementation: 

## 5/19/25 — Tile Server Working

- Set up a local OpenStreetMap tile server using `overv/openstreetmap-tile-server` (Docker)
- Created volume to hold the database:

  ```bash
  docker volume create osm-data
  ```

- Downloaded Ohio PBF from Geofabrik:

  ```bash
  wget https://download.geofabrik.de/north-america/us/ohio-latest.osm.pbf
  ```

- Imported it into the container:

  ```bash
  docker run --rm \
    -v osm-data:/data/database \
    -v "$(pwd)/ohio-latest.osm.pbf:/data/region.osm.pbf" \
    overv/openstreetmap-tile-server import
  ```

- Import completed successfully. No errors. `planet-import-complete` file was generated.
- Confirmed PostgreSQL tables were populated via:

  ```bash
  docker exec -it <container_id> bash
  su - renderer
  psql -d gis -c "SELECT COUNT(*) FROM planet_osm_polygon;"
  ```

- Got: `2467366` rows in `planet_osm_polygon`
- Started the tile server:

  ```bash
  docker run -p 8080:80 \
    -v osm-data:/data/database \
    -d overv/openstreetmap-tile-server run
  ```

- Tile server successfully rendered map. Verified `http://localhost:8080` loads Ohio data.


### MBtiles Implementation:

## 5/21/25 — Initial Setup with Docker

- Set up a local OpenStreetMap tile server using `overv/openstreetmap-tile-server` (Docker) and leaflet
- Created volume to hold the database:

  ```bash
  docker volume create osm-data
  ```
- Downloaded Ohio PBF from Geofabrik:

```sh
wget https://download.geofabrik.de/north-america/us/ohio-latest.osm.pbf
```
- Imported data into the container:

```sh
docker run --rm \
  -v osm-data:/data/database \
  -v "$(pwd)/ohio-latest.osm.pbf:/data/region.osm.pbf" \
  overv/openstreetmap-tile-server import
```
  
- Import completed successfully. planet-import-complete file was generated.

- Verified PostgreSQL tables populated:

```sh
docker exec -it <container_id> bash
su - renderer
psql -d gis -c "SELECT COUNT(*) FROM planet_osm_polygon;"
```
- Got: 2467366 rows in planet_osm_polygon

- Started the tile server:
```sh
docker run -p 8080:80 \
  -v osm-data:/data/database \
  -d overv/openstreetmap-tile-server run
```
- Tile server working at http://localhost:8080 with Ohio data rendering properly.

## 5/22–5/24/25 — Moved to Kyle's Test Server
- Transitioned from Docker to a dedicated Ubihere test server at 10.0.0.116
  - usr/osm_project 

- Installed PostgreSQL 14 with PostGIS extension

- Created osm database and imported Ohio data using osm2pgsql:

```sh
osm2pgsql -c -d osm -U osm -H localhost -S /usr/share/osm2pgsql/default.style ohio-latest.osm.pbf
```
- Import completed successfully. Confirmed same row counts as before.

## 5/25–5/26/25 — Vector Tile Generation
- Installed tilemaker to generate vector tiles instead of raster ones

- Created and configured:

  - config-openmaptiles.json for tile settings

  - process-openmaptiles.lua for data processing

- Generated MBTiles:

```sh
tilemaker --input ohio-latest.osm.pbf --output ohio.mbtiles \
  --config config-openmaptiles.json --process process-openmaptiles.lua
```
- **Result: ohio.mbtiles (598MB) with zoom levels 0–14**

## 5/27/25 — Tile Server and Web Map
- Set up mbtileserver to serve vector tiles:

```sh
./mbtileserver -d ~/osm_project/tiles -p 8000

- Verified tile service at:

http://10.0.0.116:8000/services/ohio/tiles/{z}/{x}/{y}.pbf
```

- Encountered CORS issues when accessing from browser

- Built a Python proxy server to:

- Add CORS headers

- Decompress gzipped tiles

- Forward requests from frontend to tile server

- Created web map using MapLibre GL JS

- Successfully displayed roads, buildings, water, and other geographic features in Ohio

## Final Setup
- Pipeline overview:


- OSM Data → PostgreSQL → Tilemaker → MBTiles → MBTileserver → CORS Proxy → Browser
- All components are self-hosted and running locally

- Final web map available at http://10.0.0.116:8081/map.html

- No external services (e.g., Mapbox or Google Maps) required


## 5/28/25 — Columbus Vector Tiles + MapLibre Setup
- Focused on smaller, faster tiles by generating vector tiles specifically for Columbus

- Used BBBike to download a compact .osm.pbf file for just the Columbus region

- Placed the file at: ~/Documents/ubi/osm/data/data.osm.pbf

- Ran tilemaker with OpenMapTiles schema:

```sh
./tilemaker \
  --input ~/Documents/ubi/osm/data/data.osm.pbf \
  --output columbus.mbtiles \
  --config config.json \
  --process process.lua
```

- **Output: columbus.mbtiles (32MB) with zoom levels 0–14**

- Successfully parsed roads, landuse, buildings, and water layers

- Moved columbus.mbtiles into tilesets/ for serving via Consbio’s mbtileserver:

```sh
./mbtileserver
Verified vector tile service at:
```
```sh
http://localhost:8000/services/columbus/tiles/{z}/{x}/{y}.pbf
Built frontend using MapLibre GL JS:
```

- Created map.html file and served it via:

- python3 -m http.server 8081
- Page accessible at:
```sh
http://localhost:8081/map.html
Encountered styling/rendering issues:
```

- No layers were visible due to missing or misconfigured source-layer name in HTML

- Checked MapLibre dev tools but nothing showed — suspecting malformed style or tile schema


**Next Steps:**

1. Inspect actual source-layer names in columbus.mbtiles using tools like tile-join or mbview

2. Confirm tile response headers and structure match vector tile expectations

3. Build a working style.json and extract layer metadata from tileset

## 5/29/25 — Pivot: Moving OSM Processing Back to Local Laptop

Decision: Shifted OpenStreetMap processing workflow from Ubihere TestServer (10.0.0.116) back to personal development laptop.

Why:

- System limitations on TestServer:

  - Only 1GB RAM available vs 9.3GB free on my laptop
  - TestServer already using 3.1GB of swap, indicating resource strain
  - Tight storage on server vs 827GB free locally

- Performance needs:
  - OSM tile generation (especially with tilemaker) is resource-intensive
  - My machine handles imports and MBTiles generation faster and more reliably

- Workflow clarity:
  - Server is shared—limited control, potential interruptions
  - Laptop gives full root access, no blockers
  - Cleaner division of labor: local = compute, server = serve
- Updated Plan:
  - Sync project files from TestServer to local
  - Clean up /usr/osm_project on server to free disk space
  - Run intensive tasks (e.g. generating vector tiles for surrounding states) locally
  - Use TestServer only to host final *.mbtiles via mbtileserver

**Bottom line: My laptop is simply better equipped for OSM processing. This pivot streamlines work and avoids fighting server constraints.**

## 5/30/25 — Dual Proxy Architecture & Individual Map Pages

**Created separate Python proxy servers for Ohio and Midwest tilesets:**

- ohio_proxy_server.py running on port 8085
- midwest_proxy_server.py running on port 8086


**Built individual HTML map pages:**

- ohio_map.html - Single Ohio map interface
- midwest_map.html - Single Midwest map interface


**Both maps working independently with proper CORS handling and gzip decompression**
- Verified tile serving pipeline: MBTileserver → Python Proxy → HTML Frontend

## 6/01-6/03/25 — GL Tile Server Integration

1. Set up GL tile server as alternative hosting method**
2. Tested tile serving through GL server before React migration**
3. Compared performance between MBTileserver and GL tile server approaches**
4. Validated vector tile delivery through both serving methods**

## 6/4-6/5/25 — React Migration & Dual Map Development

- Spoke with Eric about transitioning from HTML to React implementation
- Created new React application (fleet_tracker) with MapLibre GL JS
- Built flexible MapComponent.js accepting tileset prop ('ohio' or 'midwest')
- Developed App.js with side-by-side dual map layout
**Implemented:**

- Professional fleet tracking UI styling
- Status indicators for both maps
- Independent zoom controls with Midwest limited to zoom level 12 and Ohio limited to 14
- Navigation and scale controls for both map instances
- Real-time load status monitoring
- Connected React frontend to existing proxy architecture
- Optimized dual map performance with simultaneous tile loading
- Created production-ready mapping platform with complete offline capability

**Final Architecture:**
OSM Data → Tilemaker → MBTiles → MBTileserver → Python Proxies → React Frontend

- Ohio: 354MB tileset via port 8085 proxy
- Midwest: 2.1GB tileset via port 8086 proxy
- React app on port 3000 displaying professional dual-map interface

## 6/10/25 — OSRM Integration & Option B Implementation

- Integrated local OSRM routing server directly into React MapComponent
- Added routing controls and click-to-route functionality to Ohio map only
- Implemented real-time route calculation with performance metrics display
- Created unified Option B demonstration (custom tiles + local OSRM routing)

**Implemented:**

1. "Test OSRM Routing" button with toggle functionality
2. Two-click routing interface (start point → end point → automatic calculation)
3. Visual route markers (green start, red end) with GeoJSON route overlay
4. Real-time performance display showing distance, duration, and response time
5. Route visualization as bright green line overlay on custom Ohio tiles
6. Error handling and loading states for route calculations

**OSRM API Integration:**

- Direct fetch to localhost:5000 OSRM server from React frontend
- GeoJSON route geometry rendered as MapLibre layer
- Sub-50ms response times for local Ohio routes
- Automatic map bounds fitting to calculated routes

**Production Result: Complete Option B system - Ohio custom tiles with integrated local OSRM routing, demonstrating offline fleet routing capability with superior performance vs external APIs. Ready for Trax testing.**

