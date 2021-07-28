# Reverse Geocoding

There are two reversegeo APIs, the first one is `/api/reversegeo` this one returns the names of any geofences found in the location you pass it, as well as an address.  The second reversegeo api is `maps.pegasusgateway.com/reversegeo` which is more up to date in terms of the addresses it returns and is capable of returning bulk addresses in multiple language formats, read below to find out.

## api/reversegeo 

### GET 

> [api/reversegeo?lat=25.78362&lon=-80.293265&zones=true](https://cloud.pegasusgateway.com/api/reversegeo?lat=25.78362&lon=-80.293265&auth=af6193706622132cbc4f3c58e23ddbfba0b57ac6feaa292469608455)

```json
{
    "zones": [
        "DCT",
        "US - Florida"
    ],
    "location_full": "(DCT)(US - Florida) 33126, Miami-Dade County, Florida, US",
    "address": "33126, Miami-Dade County, Florida, US"
}
```

The GET request can be used to retrieve a single lat,lon result. The response will show you the:

- zones (name of any geofence that this point is in)
- location_full (zones + address)
- address (address of location)


GET `/api/reversegeo` params

URL params | description
----------:|-------------
`lat` | latitude
`lon` | longitude
`zones` | set to true to return the zones the point is in
`auth` | pegasus auth token (overrides Authenticate header)

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>


### POST 

> multiple lat lon pairs

> [api/reversegeo](https://cloud.pegasusgateway.com/api/reversegeo)

```json
[
  {
    "key": "one",
    "lat": 25.78362,
    "lon": -80.293265
  },
  {
    "key": "two",
    "lat": 25.79362,
    "lon": -80.293265
  }
]
```

> 200 OK

```json
{
    "two": {
        "zones": [
            "US - Florida"
        ],
        "location_full": "(US - Florida) 33126, Miami-Dade County, Florida, US",
        "address": "33126, Miami-Dade County, Florida, US"
    },
    "one": {
        "zones": [
            "DCT",
            "US - Florida"
        ],
        "location_full": "(DCT)(US - Florida) 33126, Miami-Dade County, Florida, US",
        "address": "33126, Miami-Dade County, Florida, US"
    }
}
```

With the post request you can pass multiple latitude, longitude pairs.

When you send multiple points you must specify a `key`, this will help you identify which json object corresponds to which lat, lon requested

POST `/api/reversegeo` params

params | description
------:|-------------
`lat` | latitude
`lon` | longitude
`key` | required key when using multiple lat lon

## maps/reversegeo

The `maps/reversegeo` reverse geocoding API allows you to convert lat,lon coordinates into a human-readable address. The API synchronizes daily with [OSM](https://www.openstreetmap.org/#map=5/38.007/-95.844) to bring the latest address information. The neat thing is this means you can edit something on OSM and see it reflected on the reversegeo API.  Another notable difference is that this API does not provide the zones information.

> maps API endpoint [https://maps.pegasusgateway.com/reversegeo](https://maps.pegasusgateway.com/reversegeo)


### GET 

> [`reversegeo?r=25.78362,-80.293265`](https://maps.pegasusgateway.com/reversegeo?r=25.78362,-80.293265&auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

```json
{
    "errors": {},
    "results": {
        "025.78362000:-080.29327000": "5835 Blue Lagoon Drive. MIAMI-DADE COUNTY, FL 33126 US"
    }
}
```

> [`reversegeo?r=32.0769,34.8151`](https://maps.pegasusgateway.com/reversegeo?r=32.0769,34.8151&auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

```json
{
  "errors": {},
  "results": {
    "032.07690000:0034.81510000": "כצנלסון. גבעתיים, מחוז תל אביב 53100 IL"
  }
}
```

> [`reversegeo?r=32.0769,34.8151&fmt={$one-line-short}`](https://maps.pegasusgateway.com/reversegeo?r=32.0769,34.8151&fmt={$one-line-short}&auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

```json
{
  "errors": {},
  "results": {
    "032.07690000:0034.81510000": "כצנלסון. גבעתיים"
  }
}
```

> [`reversegeo?r=32.0769,34.8151&lang=en`](https://maps.pegasusgateway.com/reversegeo?r=32.0769,34.8151&lang=en&auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

```json
{
  "errors": {},
  "results": {
    "032.07690000:0034.81510000": "Katsanelson. GIVATAYIM, TEL AVIV DISTRICT 53100 IL"
  }
}
```

> [`reversegeo?r=32.0769,34.8151&include_data=1&lang=en&return_as=debug&fmt={address}`](https://maps.pegasusgateway.com/reversegeo?r=32.0769,34.8151&include_data=1&lang=en&return_as=debug&fmt={address}&auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

```json
{
  "how": {
    "items": 1,
    "dt": 0.004695892333984375,
    "borders": [
      "israel-and-palestine"
    ],
    "rate": 212.95207148659625
  },
  "result": [
    {
      "lat": 32.0769,
      "lon": 34.8151,
      "_revgeo": {
        "dt": 0.004678964614868164,
        "result": {
          "text": "Katsanelson",
          "data": {
            "address": "Katsanelson",
            "boundary_ref_all_csv": "Givatayim, Tel Aviv District",
            "street": "Katsanelson",
            "boundary_admin_level_8": "Givatayim",
            "pcode": "53100",
            "boundary_all_two-line": "Givatayim, Tel Aviv District",
            "ccode": "il",
            "boundary_admin_level_4": "Tel Aviv District",
            "address_ns": "Katsanelson",
            "aprox": "",
            "address_sn": "Katsanelson",
            "boundary_ref_admin_level_4": "Tel Aviv District",
            "boundary_ref_admin_level_8": "Givatayim",
            "boundary_ref_b": "Tel Aviv District",
            "boundary_ref_a": "Givatayim",
            "boundary_a": "Givatayim",
            "boundary_b": "Tel Aviv District",
            "boundary_all_csv": "Givatayim, Tel Aviv District"
          },
          "reduced": {
            "boundaries": [
              {
                "admin_level": 8,
                "parent_place_id": 466107,
                "name": "Givatayim",
                "ref": null,
                "place_id": 461879,
                "osm_id": 1382923,
                "osm_type": "R",
                "type": "administrative",
                "extratags": {
                  "website": "http://www.givatayim.muni.il",
                  "contact:website": "http://www.givatayim.muni.il",
                  "wikipedia": "en:Giv'atayim",
                  "wikidata": "Q152413",
                  "place": "town",
                  "population": "57508"
                }
              },
              {
                "admin_level": 4,
                "parent_place_id": 462199,
                "name": "Tel Aviv District",
                "ref": null,
                "place_id": 466107,
                "osm_id": 1400916,
                "osm_type": "R",
                "type": "administrative",
                "extratags": {
                  "wikidata": "Q192811",
                  "wikipedia": "de:Bezirk Tel Aviv"
                }
              }
            ],
            "building": {
              "_inside": false,
              "admin_level": 15,
              "parent_place_id": 447044,
              "name": null,
              "distance": 78.6753360331113,
              "housenumber": "18",
              "place_id": 392216,
              "_same_street": false,
              "osm_type": "W",
              "street": "סירקין",
              "osm_id": 236768869,
              "_w_index": null,
              "extratags": null,
              "class": "building"
            },
            "pcode": "53100",
            "streets": [
              {
                "distance": 2.19657702619019,
                "parent_place_id": 129276,
                "name": "Katsanelson",
                "ref": null,
                "place_id": 407406,
                "osm_id": 248990288,
                "osm_type": "W",
                "type": "tertiary",
                "extratags": {
                  "oneway": "yes"
                }
              },
              {
                "distance": 13.2604261534137,
                "parent_place_id": 129276,
                "name": "Katsanelson",
                "ref": null,
                "place_id": 379054,
                "osm_id": 157019162,
                "osm_type": "W",
                "type": "tertiary",
                "extratags": {
                  "oneway": "yes"
                }
              }
            ],
            "ccode": "il"
          }
        }
      }
    }
  ]
}
```

For a single address request

 * Synth method is forced to `"amia_text"`

URL params | description
----------:|-------------
`r` | <lat>,<lon> pair
`auth` | pegasus auth token (overrides Authenticate header)
`fmt` | `synth.fmt`
`lang` | `synth.lang`
`include_data` | `synth.include_data`
`return_as` | same as on POST


### POST

> `fmt` options

> `{$multi-line}`

```
137 Pilkington Avenue 
SUTTON COLDFIELD
BIRMINGHAM
B72 GB
```

> `{$one-line}`

```
137 Pilkington Avenue. SUTTON COLDFIELD, B72 BIRMINGHAM GB
```

> `{$two-line}`

```
137 Pilkington Avenue
SUTTON COLDFIELD, B72 BIRMINGHAM GB
```

> `{$one-line-short}`

```
137 Pilkington Avenue. SUTTON COLDFIELD
```

> <hr>

> basic `"return_as": "dict"`

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "synth": {
    "method": "amia_text",
    "lang": "es"
  },
  "return_as": "dict"
}
```

> response 

```json
{
  "errors": {},
  "results": {
    "025.77110000:-080.27740000": "5050 West Flagler Street. MIAMI, 33134 FL US",
    "052.54874000:-001.81602000": "137 Pilkington Avenue. SUTTON COLDFIELD, B72 BIRMINGHAM GB",
    "004.67020000:-074.05410000": "Carrera 14 85-68. CHAPINERO, BOGOTÁ CO"
  }
}
```

> <hr>

> dict with custom keys

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774,
      "key": "mykey1"
    },
    {
      "lat": 52.54874,
      "lon": -1.81602,
      "key": "mykey2"
    },
    {
      "lat": 4.6702,
      "lon": -74.0541,
      "key": "mykey3"
    }
  ],
  "synth": {
    "method": "amia_text",
    "lang": "es"
  },
  "return_as": "dict"
}
```

> response

```json
{
  "errors": {},
  "results": {
    "mykey3": "Carrera 14 85-68. CHAPINERO, BOGOTÁ CO",
    "mykey2": "137 Pilkington Avenue. SUTTON COLDFIELD, B72 BIRMINGHAM GB",
    "mykey1": "5050 West Flagler Street. MIAMI, 33134 FL US"
  }
}
```

> <hr>

> return as `list` (experimental)

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "synth": {
    "method": "amia_text",
    "lang": "es"
  },
  "return_as": "list"
}
```

> response

```json
{
  "errors": [],
  "results": [
    "5050 West Flagler Street. MIAMI, 33134 FL US",
    "137 Pilkington Avenue. SUTTON COLDFIELD, B72 BIRMINGHAM GB",
    "Carrera 14 85-68. CHAPINERO, BOGOTÁ CO"
  ]
}
```

> <hr>

> formatting text

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": " {address}"
  },
  "return_as": "list"
}
```

> response

```json
{
  "errors": [],
  "results": [
    "5050 West Flagler Street",
    "137 Pilkington Avenue",
    "Carrera 14 85-68"
  ]
}
```

> <hr>

> text and data

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": " {address}",
    "include_data": true
  },
  "return_as": "list"
}
```

> response

```json
{
  "errors": [],
  "results": [
    {
      "text": "5050 West Flagler Street",
      "data": {
        "address_ns": "5050 West Flagler Street",
        "boundary_ref_county": "12-086",
        "boundary_ref_city": "Miami",
        ...
 ```

> <hr>

> override synth for a single lat-lon object

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 41.4063,
      "lon": 2.2046
    },
    {
      "lat": 32.0769,
      "lon": 34.8151,
      "synth": {
        "method": "amia_text",
        "lang": "en",
        "fmt": "{$multi-line}"
      }
    }
  ],
  "return_as": "dict",
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": "{$one-line}",
    "include_data": false
  }
}
```

> response

```json
{
  "errors": {},
  "results": {
    "025.77110000:-080.27740000": "5050 West Flagler Street. MIAMI, 33134 FL US",
    "041.40630000:0002.20460000": "Carrer de Pallars 318. BARCELONA, 08019 CT ES",
    "032.07690000:0034.81510000": "Katsanelson - Katsanelson\nGIVATAYIM\nTEL AVIV DISTRICT\n53100 IL\n"
  }
}
```

The POST method is used to pass single or multi line lat,lon reverse geocoding

* `Authenticate` header required: Any [token](#authentication) from a valid pegasus session
* Rates: Per user, site, and global. Rate control is not based on number of POST requests but on number of locations requests for reverse-geo processing. Enforced via `http 429`. On 429, current returned object is pretty crude, and gives no info on when the ban is lifted, however, indication of which rate is violated is given.

**Request object keys**


* `list`: A list of lat-lon objects to resolve (max 100 per call)
  * `lat-lon` Object with lat lon, optional `key`, optional `synth`
* `return_as`: String: `dict|list`: Indicates how to return results. `list` yields to a list with the same size
as the passe one. `dict` returns a dict whose keys are either the `key` passed on the lat-lon object, otherwise, an internally constructed key representaing the lat+lon object
* `synth`: Object. Controls how data results are returned/formatted. This object may be defined also per lat-lon object to override the request-wide set. This should be used when some of the lat-lon objects on list need to be synthetized different than the majority of items. 

synth keys: 

* `method`: String. Method to process:
  * `'std_text'`: return text. Legacy format
  * `'amia_text'`: return text. An `fmt` argument may be passed to format the result. An `include_data` boolean may be passed so that data objects, as well as text is returned.
  * `'reduced'`: Returns an useful object to play with.
  * If method is not passed, a long debug object is returned.
* `'lang'`: String (`en|es|it|fr|... etc`). When available, try to return names in this language.
* `'fmt'`: For `amia_text` method. An interpolation string to format the returned text. Some fields to pick: Interpolation: python formatter. Use `!u` and `!l` to convert upper, lower case, see combos below for examples


address formatting (fmt) |
:------------------|
| {boundary_ref_county} 
| {boundary_ref_city} 
| {street} 
| {ccode} 
| {address} 
| {boundary_county} 
| {boundary_ref_b} 
| {boundary_ref_a} 
| {boundary_a} 
| {boundary_city} 
| {pcode} 
| {boundary_b} 
| {boundary_ref_state} 
| {intersect} 
| {aprox} 
| {hnumber} 
| {boundary_state} 
| {address_sn} 


> <hr>

> {$one-line}

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "return_as": "dict",
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": "{$one-line}",
    "include_data": false
  }
}
```

> response

```json
{
  "errors": {},
  "results": {
    "012.15370000:-086.24860000": "27a Avenida N.E. - Carretera Norte. MANAGUA (MUNICIPIO),￼11005 MANAGUA NI",
    "007.10940000:-073.11670000": "Calle 55 - Carrera 21. BUCARAMANGA, SAN CO",
    "-34.88420000:-056.16940000": "Doctor Juan José de Amézaga - Cufré. MONTEVIDEO, 11800￼MONTEVIDEO UY"
  }
}
```

> <hr>

> {$two-line}

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "return_as": "dict",
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": "{$two-line}",
    "include_data": false
  }
}
```

> response

```json
{
  "errors": {},
  "results": {
    "012.15370000:-086.24860000": "27a Avenida N.E. - Carretera Norte\nMANAGUA (MUNICIPIO), 11005 MANAGUA NI",
    "007.10940000:-073.11670000": "Calle 55 - Carrera 21\nBUCARAMANGA, SAN CO",
    "-34.88420000:-056.16940000": "Doctor Juan José de Amézaga - Cufré\nMONTEVIDEO, 11800 MONTEVIDEO UY"  
  }
}
```

> <hr>

> {$multi-line}

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "return_as": "dict",
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": "{$multi-line}",
    "include_data": false
  }
}
```

> response

```json
{
  "errors": {},
  "results": {
    "012.15370000:-086.24860000": "27a Avenida N.E. - Carretera Norte\nMANAGUA (MUNICIPIO)\nMANAGUA\n11005 NI\n",
    "007.10940000:-073.11670000": "Calle 55 - Carrera 21\nBUCARAMANGA\nSANTANDER\nCO\n",
    "-34.88420000:-056.16940000": "Doctor Juan José de Amézaga - Cufré\nMONTEVIDEO\nMONTEVIDEO\n11800 UY\n"
  }
}
```

> <hr>

> {$one-line-short}

> body

```json
{
  "list": [
    {
      "lat": 25.7711,
      "lon": -80.2774
    },
    {
      "lat": 52.54874,
      "lon": -1.81602
    },
    {
      "lat": 4.6702,
      "lon": -74.0541
    }
  ],
  "return_as": "dict",
  "synth": {
    "method": "amia_text",
    "lang": "es",
    "fmt": "{$one-line-short}",
    "include_data": false
  }
}
```

> response

```json
{
  "errors": {},
  "results": {
    "012.15370000:-086.24860000": "27a Avenida N.E. - Carretera Norte. MANAGUA (MUNICIPIO)",
    "007.10940000:-073.11670000": "Calle 55 - Carrera 21. BUCARAMANGA",
    "-34.88420000:-056.16940000": "Doctor Juan José de Amézaga - Cufré. MONTEVIDEO"
  }
}
```

combos formatting | fields
:-----------------| ------
{$multi-line} | `{address} {boundary_a!u} {boundary_b!u} {pcode} {ccode!u}`
{$one-line} | *default when no fmt is passed* `{address}. {boundary_a!u}, {pcode} {boundary_ref_b!u} {ccode!u}`
{$two-line} | `{address} {boundary_a!u}, {pcode} {boundary_ref_b!u} {ccode!u}`
{$one-line-short} | `{address} {boundary_a!u}`

