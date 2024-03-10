# Toronto Bikeshare Map

Using monthly refreshed data from [Open Toronto](https://open.toronto.ca/dataset/bike-share-toronto-ridership-data/), this repository creates a map visualizing bikeshare ridership in Toronto. 

The map will have sliders for time of day and seasonality, in addition to membership type. Frequency of trips is indicated by line intensity

## geojson and spatialite

Create a VIRTUAL table with the geojson

```sql
CREATE VIRTUAL TABLE fsa_geo USING VirtualGeoJSON( geojsons/toronto_fsa_nldelim.geojson );

-- testing the geometry

select cfsauid, st_contains(geometry, st_geomfromtext('POINT(-79.5 43.6)')) as foo FROM fsa_geo order by foo desc limit 3;

-- result
M8V|1
M9R|0
M9V|0
```

### Virtual table mechanism

Creates an interface against which spatialite can apply SQL queries. Not a real table stored inside the database. When we run `ST_CONTAINS` on the virtual geojson table, spatialite is accessing the geojson to complete the query. `CREATE INDEX`, `ALTER`, and other altering statements cannot be made against virtual tables
