# Geofences
Geofences are user defined areas on the map that can be used together with triggers and tasks for a more personalized experience, for example to let you know when a vehicle reaches a particular location on the map.  Geofences are also available when you request the [reversegeo](?json#reverse-geocoding) information so it's included in the address response.  

Geofences can be grouped into [geofence_types](?json#geofence-types) (also referred to as geofence collections)

The geofence API is based on the geojson spec, for more information on geojson visit [geojson.org](http://geojson.org/)

Please note that this api is [paginated](?json#pagination). 

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence)

<aside class="info">The geojson spec does not officially handle Circle type geofences, however there are <a href="https://github.com/geojson/geojson-spec/wiki/Proposal---Circles-and-Ellipses-Geoms">proposals in place</a></aside>

## View 

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence-GetGeofences)

In order to view geofences you have to have the [scope](?json#scopes) `geofences` assigned to your user.

All geofences are encoded using a [lossy compression algorithm](https://developers.google.com/maps/documentation/utilities/polylinealgorithm).

All geofences will also have a `bbox`, or [bounding box](http://geojson.org/geojson-spec#bounding-boxes).

## Create 

> Create a circular fence of 105 meter radius around 24.78238, -82.28900

> Assign a custom property to this geofence

> POST [api/geofences](https://cloud.pegasusgateway.com/api/geofences)

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Circle",
        "radius": 105,
        "coordinates": [
            -82.289,
            24.78238
        ]
    },
    "properties": {
        "name": "warehouse 234",
        "visibility": "groups",
        "groups": [
            500
        ],
        "custom": {
            "foo": "bar",
            "something": true
        },
        "types": [],
        "description": "this is a sample description"
    }
}
```

> Create a square polygon geofence

> Associated to geofence_type 55

> POST [api/geofences](https://cloud.pegasusgateway.com/api/geofences)

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [
            [
                [
                    -81.07642,
                    26.38707
                ],
                [
                    -80.96106,
                    26.27383
                ],
                [
                    -81.11487,
                    26.21471
                ],
                [
                    -81.23023,
                    26.29353
                ],
                [
                    -81.07642,
                    26.38707
                ]
            ]
        ]
    },
    "properties": {
        "name": "Square Polygon",
        "visibility": "all",
        "types": [
            55
        ],
        "description": "Warehouse with small packages"
    }
}
```

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence-CreateGeofence)

When creating a geofence you have to specify the Geometry type you want to create, the API supports `Polygon` of 3 or more coordinates (up to 800 coordinates), `Circle`, or `LineString` types.

**Polygon** <br>
`geometry.coordinates` = 
<code>[[<code style="color:red">[lon1,lat1]</code>
,[lon2,lat2],[lon3,lat3],
<code style="color:red">[lon1,lat1]</code>]]</code>

<aside class="info">
Keep in mind that for Polygon types the last lon,lat pair has to be equal to the first, this tells the API that it's closed at that point.
</aside>

**Circle** <br>
`geometry.radius` = radius in meters (minimum 50 meters)<br>
`geometry.coordinates` = <code>[lon,lat]</code>

**LineString** <br>
`geometry.radius` = radius in meters<br>
`geometry.coordinates` = <code>[[lon1,lat1],[lon2,lat2]]</code>

when creating geofences you can specify custom properties using the `custom` key in the properties. This accepts any JSON object value.

The visibility of the geofences are as follow:

visibility | description
-----------|-------------
all | anyone with scope to see `geofences` has access to see them 
groups | only people within the group you belong to have access to see the geofence
private | only your user has access to see the geofence

If you set the visibility to "groups" you'll have to specify in an array called: `groups` the group IDs you'd like this geofence to belong to.


## Updating Geofences

> Update name, radius, and add a custom property

> PUT [api/geofences/:id](https://cloud.pegasusgateway.com/api/geofences/12)

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Circle",
        "radius": 52,
        "coordinates": [
            -80.289000061596184,
            25.78238262858029
        ]
    },
    "properties": {
        "name": "Warehouse 3",
        "custom": {
            "contact": "John Smith",
            "amount": 123
        }
    }
}
```

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence-UpdateGeofence)

When updating a geofence using the API, pass the geometry and properties object.

## Deleting Geofences

> Delete a fence

> DELETE [/api/geofences/:id](https://cloud.pegasusgateway.com/api/geofences/12)

> 204 NO CONTENT

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence-DeleteGeofence)

<aside class="warning">Note that when you delete a geofence this affects any alerts or reports that were previously made that depended on the geofence</aside>


## Geofence Types

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence_Types_Collections)

Geofence types are collections of geofences, a geofence can belong to one or many types.
These collections are useful for quickly generating reports and scheduling them too.

For example, let's say that you have a fleet of delivery trucks that make daily deliveries to specific locations.  These locations can be saved as a geofence_type called: `Clients`.  Thus when generating a trigger for Vehicle Inside Geofence and Idling for example, you want to create a single trigger that encompasses the `Clients` geofence type we created, rather than individual fences.  Note that any future geofences you add to the `Client` type automatically gets updated.

### Create

> Create a geofence type

```json
{
  "name": "Clients",
  "color": "#ff0404",
  "visibility": "groups",
  "groups": [5],
  "geofences": [
    25,
    1042,
    1044,
    1045,
    1414,
    1415,
    1416
  ],
  "icon": "material:location_city"
}
```

[Reference docs](https://cloud.pegasusgateway.com/api-static/docs/#api-Geofence_Types_Collections-CreateGeofenceTypes)

When creating a geofence type the important parameters are the `name`, `visibility` and `geofences`

You can optionally set a HEX color code for the geofences under the `color` parameter.
Also you can set an icon using the [material icon library](https://material.io/icons/)

`"icon": "material:location_city"`

or a custom URL <br>

`"icon": "url:https://goo.gl/bQFcW5"`


