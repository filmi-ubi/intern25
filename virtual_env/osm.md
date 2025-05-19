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
