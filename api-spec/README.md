
# SpatioTemporal Asset Catalog API Specification

## Overview

A STAC API is the dynamic version of a SpatioTemporal Asset Catalog. It returns [STAC Items](../json-spec/json-spec.md) 
(GeoJSON objects with required links time stamp and links to assets) from search queries on a RESTful endpoint.

The core of the spec is a single endpoint:

```
/search/stac
```

It takes a JSON object that can filter on date and time:

```

{
  "bbox": [ 5.5, 46, 8, 47.4 ],
  "time": "2018-02-12T00:00:00Z/2018-03-18T12:31:12Z"
}
```

This tells the server to return all the catalog items it has that are from the second half of March, 2018 and 
that fall within this area:

![swiss_bbox](https://user-images.githubusercontent.com/407017/38382405-b5e69344-38be-11e8-90dc-35738678356d.png)

*map © OpenStreetMap contributors*

The return format is a [GeoJSON](http://geojson.org) feature collection with features compliant with the 
[json spec]((../json-spec/json-spec.md) for STAC. It returns to a limit optionally requested by the client, and includes 
pageable links to iterate through any results past that limit.

## Specification

The definitive specification for STAC is the [OpenAPI](http://openapis.org) 3.0 yaml document. This is available
in several forms. The most straightforward for an implementor new to STAC is the [STAC-standalone.yaml](STAC-standalone.yaml).
This gives a complete service with the main STAC endpoint. See the [online documentation](https://app.swaggerhub.com/apis/cholmesgeo/STAC-standalone/0.4.1) for the api rendered interactively.

An OpenAPI 2.0 (swagger) version is also available ([WFS3core+STAC-swagger2.yaml](WFS3core+STAC-swagger2.yaml)), which can be useful for autogenerating client and server code.

##### Warning

**The latest spec has not yet been fully validated by actual implementations. This will come soon, but if you
are new to STAC and are trying to use the spec we recommend you jump on the [stac gitter](https://gitter.im/SpatioTemporal-Asset-Catalog/Lobby)
to get help**

### Filtering

The core STAC API includes two filters - Bounding Box and time. All STAC Items require space and time, and thus any STAC
client can expect to be able to filter on them. Most data will include additional data that users would like to query on,
so there is a mechanism to also specify more filters. See the [Filtering](filters.md) document for additional information
on the core filters as well as how to extend them. It is anticipated that 'profiles' for domains (like earth observation
imagery) will require additional fields to query their common fields.

### WFS 3.0 Integration

At the [Ft Collins Sprint](https://github.com/radiantearth/community-sprints/tree/master/03072018-ft-collins-co) the
decision was made to integrate STAC with the [WFS 3 specification](https://github.com/opengeospatial/WFS_FES), as
they are quite similar in spirit and intention. It is anticipated that most STAC implementations will also implement 
WFS, and indeed most additional functionality that extends STAC will be done as WFS extensions. 

This folder thus also provides an [openapi fragment](STAC-fragment.yaml), as well as an [example service](https://app.swaggerhub.com/apis/cholmesgeo/STAC_WFS-example/0.4.1) ([yaml](WFS3core+STAC.yaml))
for those interested in what a server that implements both STAC and WFS would look like. Those interested in learning more
can read the [deeper discussion of WFS integration](wfs-stac.md).


### GET requests

The POST endpoint is required for all STAC API implementations. The fields of the JSON object can also be used
for a GET endpoint, which are included in the openapi specifications. The GET requests are designed to reflect the same
fields as the POST fields, and are based on WFS 3 requests. It is recommended for implementations to implement both, but 
only POST is required. 


