# Geo

### GDAL
get shapefile metadata
```
$ orginfo -al shapefile.shp
```

### ogr2ogr
```bash
$ ogr2ogr \
    -f GeoJSON \
    -where "STATE = 'CA'" \
    -s_srs crs:84 \
    -t_srs crs:84 \
    output.geojson \
    input.shp

$ ogr2ogr \
    -f GeoJSON \
    -where "STATEFP10 = '06'" \
    -s_srs crs:84 \
    -t_srs crs:84 \
    zip.geojson \
    tl_2010_06_zcta510.shp

$ ogr2ogr \
    -f GeoJSON \
    -s_srs crs:84 \
    -t_srs crs:84 \
    state.geojson \
    tl_2010_06_state10.sh
```

### topojson
```bash
$ topojson \
    -p statefp=STATEFP \
    -p name=NAME \
    -o output.topojson \
    input.geojson

$ topojson \
    -p zipcode=ZCTA5CE10 \
    -o zip.topojson \
    zip.geojson
```

combine multiple files
```
$ topojson \
    -o foo.topojson \
    -p zipcode=ZCTA5CE10 \
    tl_2010_06_zcta510/zip.geojson \
    tl_2010_06_state10/state.geojson

$ topojson \
  -o uk.json \
  --id-property SU_A3 \
  --properties name=NAME \
  -- \
  subunits.geojson \
  places.geojson
```
