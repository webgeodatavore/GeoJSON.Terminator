# GeoJSON Terminator

Create a GeoJSON of overlay day and night regions.
It's derivated from [Leaflet-Terminator](https://github.com/joergdietrich/Leaflet.Terminator) project

## Demo

You can see a [demo in action](https://rawgit.com/webgeodatavore/GeoJSON.Terminator/master/demo/index.html) using OpenLayers

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

    // At the beginning
    import VectorLayer from 'ol/layer/Vector';
    import VectorSource from 'ol/source/Vector';
    import GeoJSON from 'ol/format/GeoJSON';
    import Style from 'ol/style/Style';
    import Stroke from 'ol/style/Stroke';
    import Fill from 'ol/style/Fill';
    import GeoJSONTerminator from "@webgeodatavore/geojson.terminator";


    // Then after map instanciation

    const geoJSON = new GeoJSONTerminator();
    const timeLayer = new VectorLayer({
      source: new VectorSource({
        features: (new GeoJSON()).readFeatures(geoJSON, {
          featureProjection: 'EPSG:3857'
        })
      }),
      style: new Style({
        fill: new Fill({
          color: 'rgb(0, 0, 0)'
        }),
        stroke: null
      }),
      opacity: 0.5
    });
    map.addLayer(timeLayer);