# Routes

[Reference docs](https://pegasus1.pegasusgateway.com/api-static/docs/#api-routes)

The routes API is useful for creating paths on a map that can be used to know if an entity passed through the path and reached a set destination at any point in time. 
Routes are comprised of 2 or more checkpoints. The first and last checkpoints are known as the origin and destination checkpoints, they are required to have a time.  The origin's time is also known as the *time of departure* and the destination time the *time of arrival*.

Routes can be scheduled to repeat on a daily basis.  Each checkpoint within a route can also have a minimum and maximum duration. 

When the route's time of departure starts (based on the `timezone` given and the first checkpoints `time`) the entity's progress is recorded in the `route_state` API, and once it reaches any checkpoints either by location or the scheduled time of arrival expires, the location of the entity is recorded, once the entity leaves a checkpoint that's not the last one it's also recorded.

Multiple routes can be assigned to the same entity, and the status of multiple routes can be updated simultaneously.  

## Create routes

> Create a route

> POST [api/entities/:id/routes](https://pegasus1.pegasusgateway.com/api/entities/1673/routes)

```json
{
  "active": true,
  "name": "Pick up package",
  "checkpoints": [
    {
      "name": "First checkpoint",
      "buffer": 150,
      "lat": 25.78363,
      "lon": -80.29349
    },
    {
      "name": "Arrival",
      "buffer": 158,
      "lat": 25.79587,
      "lon": -80.27858
    }
  ],
  "schedules": [
    {
      "active": true,
      "timezone": "America/New_York",
      "repeat": "Sun",
      "checkpoints": [
        {
          "time": "12:05"
        },
        {
          "time": "12:30"
        }
      ]
    }
  ]
}
```

[Reference docs](https://pegasus1.pegasusgateway.com/api-static/docs/#api-routes-CreateRoute)

Routes are created within the [entities](?json#entities) api, POST `/entities/:id/routes`

**Required parameters**

* `active` - true if the route is active or not
* `checkpoints` - array of 2 or more checkpoints 
	* `lat` - latitude 
	* `lon` - longitude
	* `buffer` - radius (meters)
* `schedules` - times of departures and arrivals of the checkpoints
	* `active` - true if schedule is active or not
	* `timezone` - [timezone](?json#timezone)
	* `repeat` - time of the week which the route will repeat
	* `checkpoints` - array of the times and durations per checkpoint
		* `time` - time for checkpoint (`HH:mm`)

**Optional parameters**

* `name` - name of the route
* `enforce` - true if the route is enforced (relevant for alerts)
* `color` - HEX code for the color of the route
* `description` - notes or descriptions of the route
* `checkpoints`
	* `name` - for each checkpoint
* `schedules` 
	* `checkpoints`
		* `min_duration` - minimum duration in minutes
		* `max_duration` - maximum duration in minutes
		* `mandatory` - true if the checkpoint is mandatory
	* `valid_until` - timestamp in which the route will go inactive (YYYY-MM-DDTHH:mm:ss.000Z)
* `path` - object with a path for the entity to follow as it goes through the destination 
	* `buffer` - radius of the path (meters)
	* `encoding` - [polyline encoded path](https://developers.google.com/maps/documentation/utilities/polylinealgorithm) 

## Route states 

> get an entities' route states

> [/route_states?entities=:id](https://pegasus1.pegasusgateway.com/api/route_states?entities=1673)

```json
[
  {
    "is_outside_path": false,
    "name": "Client visit route",
    "finalized": true,
    "route_end": "1903121725",
    "start_epoch": 1552410000,
    "updated_at": 1552415585.2653241,
    "route_id": 60,
    "end_epoch": 1552411500,
    "finalized_cpt": 6,
    "checkpoints": [
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "Departure",
        "finalized": true,
        "geometry": [
          -80.293308917201387,
          25.78377829683669,
          50
        ],
        "min_duration": 0,
        "time": 1552410000,
        "id": "cpt.start",
        "events": {
          "entered": {
            "system_time_epoch": 1552403066.3740399,
            "gpsknit_prev_lat": 25.783390000000001,
            "gpsknit_prev_event_time__epoch": 1552395072.0,
            "lon": -80.293149999999997,
            "label": "slpoff",
            "gpsknit_prev_lon": -80.293149999999997,
            "lat": 25.783390000000001,
            "id": 147726267731374,
            "event_time_epoch": 1552403035.0
          },
          "last": null,
          "exited": {
            "system_time_epoch": 1552410955.968734,
            "gpsknit_prev_lat": 25.783390000000001,
            "gpsknit_prev_event_time__epoch": 1552410920.0,
            "lon": -80.293329999999997,
            "label": "mblybrkon",
            "gpsknit_prev_lon": -80.293149999999997,
            "lat": 25.78332,
            "id": 147726275620968,
            "event_time_epoch": 1552410932.0
          }
        }
      },
      {
        "max_duration": 5,
        "mandatory": null,
        "name": "Discovery HQ",
        "finalized": true,
        "geometry": [
          -80.302509975325123,
          25.783440582343367,
          87
        ],
        "min_duration": 3,
        "time": 1552410180,
        "id": "cpt.1",
        "events": {
          "entered": null,
          "last": null,
          "exited": null
        }
      },
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "7th Street",
        "finalized": true,
        "geometry": [
          -80.301271314153468,
          25.777856051122349,
          97
        ],
        "min_duration": 0,
        "time": 1552410300,
        "id": "cpt.2",
        "events": {
          "entered": {
            "system_time_epoch": 1552411068.2377181,
            "gpsknit_prev_lat": 25.77758,
            "gpsknit_prev_event_time__epoch": 1552411057.0,
            "lon": -80.300820000000002,
            "label": "mblyspd",
            "gpsknit_prev_lon": -80.299199999999999,
            "lat": 25.77758,
            "id": 147726275733237,
            "event_time_epoch": 1552411066.0
          },
          "last": null,
          "exited": {
            "system_time_epoch": 1552411088.8534629,
            "gpsknit_prev_lat": 25.777660000000001,
            "gpsknit_prev_event_time__epoch": 1552411068.0,
            "lon": -80.304209999999998,
            "label": "aggdrv",
            "gpsknit_prev_lon": -80.301230000000004,
            "lat": 25.777699999999999,
            "id": 147726275753853,
            "event_time_epoch": 1552411083.0
          }
        }
      },
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "Miliam Diary Rd",
        "finalized": true,
        "geometry": [
          -80.31160340384308,
          25.777633864577687,
          62
        ],
        "min_duration": 0,
        "time": 1552410420,
        "id": "cpt.3",
        "events": {
          "entered": null,
          "last": null,
          "exited": null
        }
      },
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "12 Sth & 826",
        "finalized": true,
        "geometry": [
          -80.32114148139955,
          25.782763849356652,
          95
        ],
        "min_duration": 0,
        "time": 1552410600,
        "id": "cpt.4",
        "events": {
          "entered": {
            "system_time_epoch": 1552411389.4811511,
            "gpsknit_prev_lat": 25.78192,
            "gpsknit_prev_event_time__epoch": 1552411382.0,
            "lon": -80.320800000000006,
            "label": "trckpnt",
            "gpsknit_prev_lon": -80.320869999999999,
            "lat": 25.782399999999999,
            "id": 147726276054481,
            "event_time_epoch": 1552411384.0
          },
          "last": null,
          "exited": {
            "system_time_epoch": 1552411396.8575439,
            "gpsknit_prev_lat": 25.782399999999999,
            "gpsknit_prev_event_time__epoch": 1552411384.0,
            "lon": -80.321089999999998,
            "label": "mblyhdwrn",
            "gpsknit_prev_lon": -80.320800000000006,
            "lat": 25.784420000000001,
            "id": 147726276061857,
            "event_time_epoch": 1552411395.0
          }
        }
      },
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "12 Street & 84 AVE",
        "finalized": true,
        "geometry": [
          -80.33194627365593,
          25.782811673477919,
          100
        ],
        "min_duration": 0,
        "time": 1552410960,
        "id": "cpt.5",
        "events": {
          "entered": null,
          "last": null,
          "exited": null
        }
      },
      {
        "max_duration": 0,
        "mandatory": null,
        "name": "Arrival",
        "finalized": true,
        "geometry": [
          -80.335345403836939,
          25.784850600179006,
          50
        ],
        "min_duration": 0,
        "time": 1552411500,
        "id": "cpt.end",
        "events": {
          "entered": null,
          "last": null,
          "exited": null
        }
      }
    ],
    "path": {
      "buffer": 80,
      "encoding": "q{j|CnkaiNnBCNtb@ElEVjD[bBvA_BjHCbT]CbIp@~H?rO~CnSR~JsDuCgBbGaMbN{FlIgEhEUrR^rUOrg@WjDqM@?hTZB?i@"
    },
    "route_start": "1903121700",
    "exited_path": false,
    "color": "#FFC107"
  }
]
```

The `/route_states` api is given one or many entity IDs and it returns the current status and a history of all the routes and their states. 
The parameter `finalized` is used to indicate whether the route's time of arrival has expired or the entity has reached it's final checkpoint. Once that value is true the rest of the object does not get updated.  Thus representing the last state of the route at the time of completion. 

<aside class="warning"> 
At the moment the API returns the last 100 route states, while a route state browser API is built. 
</aside>

The following results are after the route is `finalized`

**Returned results** 

* `name` - name of the route
* `color` - color of the route
* `exited_path` - null, means that it was never inside the path, true means that it was at one point outside the route's path, false means that it stayed in route's path at all time
* `finalized` - true if the time of arrival expired or the entity reached its last checkpoint
* `finalized_cpt` - checkpoint progress, increases by one every time the checkpoints are finalized
* `is_outside_path` - true if the entity was outside the path.
* `route_start` - scheduled time of the start of the route `YYMMDDHHMM` (in route's timezone)
* `start_epoch` - scheduled start time of the route as an epoch
* `route_end` - scheduled time of the end of the route `YYMMDDHHMM` (in route's timezone)
* `end_epoch` - scheduled end time of the route as an epoch
* `route_id` - specific id for an entity's route
* `checkpoints` - array of the checkpoints for the routes
	* `geometry` - lon, lat, and radius of the checkpoint
	* `id` - identifier of the checkpoint `cpt.start`, `cpt.#`, or `cpt.end`
	* events - obect with events that correspond to the entrance / exit of an entity to a checkpoint
		* `entered` - checkpoint was visited with this info
		* `exited` - checkpoint exited with this info

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

## Viewing routes

> [/api/entities/:id/routes](https://pegasus1.pegasusgateway.com/api/entities/1673/routes)

```json
[
  {
    "edited_at": "2019-01-11T13:13:46.809564+00:00",
    "__updated": 1547212426.899816,
    "uuid": "KGRwMApTJ3BhdGgnCnAx",
    "color": "#CD0A0A",
    "created_at": "2018-10-26T01:54:00.751951+00:00",
    "description": "",
    "id": 18,
    "primary_id": 1673,
    "options": {},
    "checkpoints": [
      {
        "lat": 25.74513,
        "buffer": 204,
        "lon": -80.12540,
        "name": "Departure"
      },
      {
        "lat": 25.78412,
        "buffer": 108,
        "lon": -80.29345,
        "name": "Arrival"
      }
    ],
    "schedules": [
      {
        "active": true,
        "timezone": "America/New_York",
        "repeat": "Mon,Tue,Wed,Thu,Fri",
        "valid_until": null,
        "checkpoints": [
          {
            "max_duration": 0,
            "min_duration": 0,
            "optional": false,
            "time": "8:00"
          },
          {
            "max_duration": 0,
            "min_duration": 0,
            "optional": false,
            "time": "8:35"
          }
        ]
      }
    ],
    "active": true,
    "path": {
      "buffer": 95,
      "encoding": "ajc|Cx_thNsGjA^zViXdAEtWrBraC`IfjAoD?jCv}Bqi@tA{dDtEuGr^_Eo@"
    },
    "enforce": null,
    "name": "Daily weekday route"
  }
]
```

[Reference docs](https://pegasus1.pegasusgateway.com/api-static/docs/#api-routes-GetRoutes)

To view a list of the routes assigned to an entity you can use the `/entities/:id/routes` api



## Edit a route

> updating a route's times (hour later)

PUT [api/entities/:id/routes/:id](https://pegasus1.pegasusgateway.com/api/entities/1673/routes/18)

```json
{
    "edited_at": "2019-01-11T13:13:46.809564+00:00",
    "__updated": 1547212426.899816,
    "uuid": "KGRwMApTJ3BhdGgnCnAx",
    "color": "#CD0A0A",
    "created_at": "2018-10-26T01:54:00.751951+00:00",
    "description": "",
    "id": 18,
    "primary_id": 1673,
    "options": {},
    "checkpoints": [
        {
            "lat": 25.74513,
            "buffer": 204,
            "lon": -80.225409999999997,
            "name": "Client 1"
        },
        {
            "lat": 25.784120000000001,
            "buffer": 108,
            "lon": -80.293450000000007,
            "name": "Destination"
        }
    ],
    "schedules": [
        {
            "active": true,
            "timezone": "America/New_York",
            "repeat": "Mon,Tue,Wed,Thu,Fri",
            "valid_until": null,
            "checkpoints": [
                {
                    "max_duration": 0,
                    "min_duration": 0,
                    "optional": false,
                    "time": "9:00"
                },
                {
                    "max_duration": 0,
                    "min_duration": 0,
                    "optional": false,
                    "time": "9:35"
                }
            ]
        }
    ],
    "active": true,
    "path": {
        "buffer": 95,
        "encoding": "ajc|Cx_thNsGjA^zViXdAEtWrBraC`IfjAoD?jCv}Bqi@tA{dDtEuGr^_Eo@"
    },
    "enforce": null,
    "name": "Pick up package"
}
```

[Reference docs](https://pegasus1.pegasusgateway.com/api-static/docs/#api-routes-UpdateRoute)

A route can be edited to update just about anything as long as the finalized parameter is not marked as true. 

To update a route go to that specific route ID for a particular entity and update it

<aside class="info">
	It's recommended to always update a route using the entire payload of the GET request.
</aside>


## Delete a route

> delete a route

> DELETE [api/entites/:id/routes/:id](https://pegasus1.pegasusgateway.com/api/entities/1673/routes/172)

When deleting a route you have to pass it the route ID. 
<aside class="error"> 
Note that deleting an active route will void any triggers associated to it. 
</aside>
