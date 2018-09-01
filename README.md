# GeoJSON Terminator

Create a GeoJSON of overlay day and night regions.
It's derivated from [Leaflet-Terminator](https://github.com/joergdietrich/Leaflet.Terminator) project

## Demo

You can see a [demo in action]() using OpenLayers

It generates a 'pseudo-GeoJSON'.

## Why 'pseudo'?

Coordinates longitude range go from -360 to 360 whereas it should be -180, + 180

API like OpenLayers or Leaflet can consume this GeoJSON although it's invalid from the GeoJSON spec viewpoint.

If you need a valid GeoJSON in another context, you may use something like GDAL/OGR to clip GeoJSON to a valid range with:

    ogr2ogr -f "GeoJSON" output.geojson input.geojson \
            -clipsrc -180 90 180 90

## Usage


### With OpenLayers standalone build

    var geoJSON = new GeoJSONTerminator();
    var timeLayer = new ol.layer.Vector({
      source: new ol.source.Vector({
        features: (new ol.format.GeoJSON()).readFeatures(geoJSON, {
          featureProjection: 'EPSG:3857'
        })
      }),
      style: new ol.style.Style({
        fill: new ol.style.Fill({
          color: 'rgb(0, 0, 0)'
        }),
        stroke: null
      }),
      opacity: 0.5
    });
    map.addLayer(timeLayer);

## With npm

