# USACE MetaJSON 1.0.0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in RFC 2119.

## 1. Purpose

This specification attempts to create a standard for representing
metadata about multiple types of web-based layers, to aid clients
in configuration and browsing.

## 2. File format

MetaJSON manifest files use the JSON format as described in RFC 4627.

Implementations MUST treat unknown keys as if they weren't present.
However, implementations MUST expose unknown key/values in their API
so that API users can optionally handle these keys. Implementations MUST
treat invalid values for keys as if they weren't present. If the key is
required, implementations MUST treat the entire TileJSON manifest file
as invalid and refuse operation.

###//Notes
Need to take a look at all of the formats we want to support:

* WMS
* TMS
* ArcGIS Server
* Vector Tile (protobuf or geoJSON)
* Basic Vector data, geoJSON or other vector? (KML, GPX, etc...)

Each format requires it's own set of attributes that we need to track
in order to add it to the map.

```javascript
{
    // Information about the layer
    //
    // REQUIRED
    "tilejson": "2.1.0",
    // REQUIRED
    "name": "compositing",
    // REQUIRED
    "description": "A simple, light grey world.",
    // OPTIONAL
    // internal dataset version
    "version": "1.0.0",
    // OPTIONAL
    // do not interpret as html
    "attribution": "<a href='http://openstreetmap.org'>OSM contributors</a>",
    // OPTIONAL
    // legend object that tells us how to draw the legend
    // may point to the legend end-point of a service as well
    "legend": {
      // REQUIRED
      // see /legend.md for legend types
    },
    // OPTIONAL
    // array of field names, descriptions and types for the source data
    "fields":[
      // For each attribute, you should include a field record like below:
      {
        "name":"id",
        "type":"integer"
        "description":"Unique ID for each record"
      }
    ]
    //
    // How do you connect to the layer
    //
    // OPTIONAL. Default: "xyz". Either "xyz" or "tms".
    "scheme": "xyz",
    // REQUIRED. An array of tile endpoints. {z}, {x} and {y}, if present,
    "tiles": [
        "http://localhost:8888/admin/1.0.0/world-light,broadband/{z}/{x}/{y}.png"
    ],
    // OPTIONAL. Default: [].
    "grids": [
        "http://localhost:8888/admin/1.0.0/broadband/{z}/{x}/{y}.grid.json"
    ],
    // OPTIONAL. Default: null. Contains a mustache template to be used to
    // format data from grids for interaction.
    "template": "{{#__teaser__}}{{NAME}}{{/__teaser__}}",
    // OPTIONAL. Default: [].  URL(s) for downloading the source data, separate by file type
    "data": [
        "http://localhost:8888/admin/data.geojson"
    ],
    //
    // Standard viewport settings for the layer
    //
    // OPTIONAL. Default: 0. >= 0, <= 22.
    "minzoom": 0,
    // OPTIONAL. Default: 22. >= 0, <= 22.
    "maxzoom": 11,
    // OPTIONAL. Default: [-180, -90, 180, 90].
    "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ],
    // OPTIONAL. Default: null.
    "center": [ -76.275329586789, 39.153492567373, 8 ]
}
```


## 3. Caching

Clients MAY cache files retrieved from a remote server.
When implementations decide to perform caching, they MUST honor valid
cache control HTTP headers as defined in the HTTP specification for both
tile images and the TileJSON manifest file.
