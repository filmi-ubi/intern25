# OSM Fleet Tracking - Technology Stack

## Core Technologies Used
### Data Processing

- Tilemaker - Vector tile generation from OSM data
- osmium-tool - OSM data manipulation and filtering
- osm2pgsql - OSM to PostgreSQL import tool

### Backend Tile Serving

- mbtileserver (Custom) - Go-based tile server for .mbtiles
- TileServer GL - Node.js/Express.js professional tile server
- PostgreSQL + PostGIS - Geospatial database (initial approach)

### Frontend Map Libraries

- MapLibre GL JS - WebGL vector map rendering
- Leaflet - JavaScript mapping library (Docker phase)

### Data Formats

- MBTiles - SQLite-based tile storage format
- OSM PBF - OpenStreetMap Protocol Buffer Binary format
- Vector Tiles (MVT) - Mapbox Vector Tile specification

### Development Tools

- Python HTTP Server - Local development server
- Docker - Containerized tile server (overv/openstreetmap-tile-server)
- CORS Proxy - Python-based proxy for cross-origin requests

### Deployment Platforms (just locally on Ubuntu NOT commercial)

- TestServer (10.0.0.116) - Initial deployment environment
- Local Development - Current primary environment
  

### APIs & Services

- Geofabrik - OSM data download source
- BBBike - Regional OSM extracts
- OpenMapTiles Schema - Vector tile layer specification

### Architecture Evolution
- Phase 1: Docker + Leaflet + Raster tiles
- Phase 2: PostgreSQL + PostGIS + Custom backend
- Phase 3: Tilemaker + MBTiles + Vector tiles
- Phase 4: TileServer GL + MapLibre GL + Professional setup
- Phase 5: Move to React!
