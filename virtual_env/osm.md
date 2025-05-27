## OSM Tile Server Progress Log

### 5/19/25 — Tile Server Working

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



## OSM Tile Server Progress Log

### 5/21/25 — Initial Setup with Docker

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

### 5/22–5/24/25 — Moved to Kyle's Test Server
- Transitioned from Docker to a dedicated Ubihere test server at 10.0.0.116
  - usr/osm_project 

- Installed PostgreSQL 14 with PostGIS extension

- Created osm database and imported Ohio data using osm2pgsql:

```sh
osm2pgsql -c -d osm -U osm -H localhost -S /usr/share/osm2pgsql/default.style ohio-latest.osm.pbf
```
- Import completed successfully. Confirmed same row counts as before.

### 5/25–5/26/25 — Vector Tile Generation
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

### 5/27/25 — Tile Server and Web Map
- Set up mbtileserver to serve vector tiles:

```sh
./mbtileserver -d ~/osm_project/tiles -p 8000
```
- Verified tile service at:

```sh
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



