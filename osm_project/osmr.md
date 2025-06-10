# OSRM Progress Log

## Terminal Commands to get OSRM running locally:

```bash
cd Documents/osm_project
docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm=MLD /data/ohio-roads-only.osrm --port 5000 --ip 0.0.0.0
```

**Test endpoint:** `http://localhost:5000/route/v1/driving/-82.9988,39.9612;-84.5120,39.1031`

**Comparison tool:** `[http://localhost:8080/comparison_route.html`](http://127.0.0.1:5500/viewer/comp_route.html)`

---

## Implementation Timeline:

### 6/6/25 — Initial OSRM Setup
* Downloaded Ohio OSM data from Geofabrik:
```bash
wget https://download.geofabrik.de/north-america/us/ohio-latest.osm.pbf
```

* Used Docker OSRM backend to process Ohio data:
```bash
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/ohio-latest.osm.pbf
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-partition /data/ohio-latest.osrm
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-customize /data/ohio-latest.osrm
```

* Started OSRM server:
```bash
docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm=MLD /data/ohio-latest.osrm --port 5000 --ip 0.0.0.0
```

* Verified server working with test route Columbus → Cincinnati
* Response time: ~8ms locally vs ~150ms public OSRM

### 6/7/25 — Performance Testing Setup
* Built Leaflet-based comparison tool to test local vs public OSRM
* Created preset routes for Ohio testing:
  - Columbus → Cincinnati (200km)
  - Columbus → Cleveland (185km) 
  - Columbus → Toledo (190km)
  - Short urban routes (5km)

* Performance results consistently showed 5-10x faster response times:
  - Local OSRM: 5-15ms average
  - Public OSRM: 50-200ms average

### 6/8/25 — Route Quality Validation
* Compared route accuracy between local and public OSRM
* Both systems using same OSM data source
* Route distances within 1-2% variance (expected due to different processing times)
* Local routes properly following Ohio road networks

### 6/9/25 — Option B Implementation Complete
* Refined comparison tool to clearly demonstrate Option B (Hybrid OSRM + PostgreSQL approach)
* Added performance benchmarking with multiple test routes
* Documented offline capability - works without internet connection
* System ready for Trax local testing

### 6/10/25 — Final Integration Testing
* Verified OSRM server stable under load testing
* Confirmed integration potential with map tiler system
* Both mapping (Option B maps) and routing (Option B OSRM) working together
* Complete offline fleet tracking solution operational

---

## Key Files:
- `ohio-latest.osm.pbf` - Source OSM data for Ohio
- `ohio-latest.osrm` - Processed OSRM routing data
- `comparison_route.html` - Option B testing interface
- `car.lua` - OSRM car profile for routing

## Performance Metrics:
- **Local OSRM average response:** 7.6ms
- **Public OSRM average response:** 563.9ms  
- **Performance improvement:** 98.7% faster
- **Coverage area:** Complete Ohio road network
- **Offline capability:** Fully functional without internet

## Option B Status: 
**COMPLETE** - Local OSRM routing server operational, integrated with map tiling system

Do a deep dive on post GIS x-y vector polylines
- determine what i can be bound to
- research gis database capabiities for storing vector data
- pull osmr vector data
