# osm2pgsql-docker

This project is meant to provide the tools necessary to run a postgresql container with postGIS,
and allow for OSM data queries.

# Getting Started

## Download the OSM data

I used this link for my region. Can be either the .pbf or .osm format, but I used the pbf
https://download.geofabrik.de/north-america/us-south.html


## Run PostGIS

```bash
docker-compose up
```

## Importing OSM data

```bash
PGPASSWORD=password osm2pgsql --create --verbose -U postgres -H localhost -S osm2pgsql.style /path/to/data.osm.pbf
```

## Querying Data Near a Point

### Nearest Roads to a Point

```sql
SELECT osm_id, name, maxspeed, highway,
  way <-> ST_Transform(ST_GeomFromText('POINT(-80.894784 36.681079)',4326),3857) AS dist
FROM
  planet_osm_roads
WHERE highway is NOT NULL
ORDER BY
  dist
LIMIT 10;
```

| osm_id    | name              | maxspeed | highway    | dist               |
|-----------|-------------------|----------|------------|--------------------|
| 20578703  | East Stuart Drive | NULL     | trunk      | 4.336059506144272  |
| 41926181  | East Stuart Drive | NULL     | trunk      | 20.281258637710497 |
| 263295758 | East Stuart Drive | NULL     | trunk      | 1402.957073053372  |
| 644313116 | NULL              | NULL     | trunk_link | 2243.9384596112036 |
| 966025927 | NULL              | NULL     | trunk_link | 2261.0034865201956 |
| 552200549 | Carrollton Pike   | 55 mph   | trunk      | 2440.3706777221914 |
| 966025923 | Carrollton Pike   | 55 mph   | trunk      | 2444.5512902932564 |
| 206766806 | New River Trail   | NULL     | cycleway   | 2573.804859725056  |
| 644313104 | NULL              | NULL     | trunk_link | 2645.3253628254465 |
| 19924130  | NULL              | NULL     | trunk_link | 2673.590137372333  |

### Nearest Point Features to a Point

```sql
SELECT name, "addr:housename", "addr:housenumber",
  way <-> ST_Transform(ST_GeomFromText('POINT(-80.885767 36.687606)',4326),3857) AS dist
FROM
  planet_osm_point
ORDER BY
  dist
LIMIT 10;
```

| name                      | addr:housename | addr:housenumber | dist               |
|---------------------------|----------------|------------------|--------------------|
| Gold and Silver Exchange  | NULL           | NULL             | 54.31610082610827  |
| Galax Realty              | NULL           | NULL             | 55.19403220320814  |
| NULL                      | NULL           | NULL             | 65.39219861929232  |
| NULL                      | NULL           | NULL             | 67.32008091972078  |
| Blue Ridge Eye Care       | NULL           | NULL             | 72.9340503667209   |
| BB&T                      | NULL           | NULL             | 81.73132156170799  |
| Tobacco and Candle Outlet | NULL           | NULL             | 113.92252499330681 |
| Cox Realty and Auction    | NULL           | NULL             | 144.12588306677372 |
| First Citizens Bank       | NULL           | NULL             | 156.60491338013532 |
| Cummings Auto Sales       | NULL           | NULL             | 157.6340662110268  |