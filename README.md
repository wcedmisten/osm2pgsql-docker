# osm2pgsql-docker

This project is meant to provide the tools necessary to run a postgresql container with postGIS,
and allow for OSM data queries.

# Getting Started

## Download the OSM data

I used this link for my region. Can be either the .pbf or .osm format, but I used the pbf
https://download.geofabrik.de/north-america/us-south.html


# Run PostGIS

```bash
docker-compose up
```

# Importing OSM data

```bash
PGPASSWORD=password osm2pgsql --create --verbose -U postgres -H localhost -S osm2pgsql.style /path/to/data.osm.pbf
```
