# Trips

> Request the trips for a vehicle <br>
> <a href="https://cloud.pegasusgateway.com/api/trips?vehicles=45&from=2015-12-25T10:05:00&to=2015-12-25T11:00:00&fields=distance,mean_speed,duration,moving,start_time&distance=km&time=minute&speed=kph">https://cloud.pegasusgateway.com/api/trips?vehicles=45&from=2015-12-25T10:05:00&to=2015-12-25T11:00:00&fields=distance,mean_speed,duration,moving,start_time&distance=km&time=minute&speed=kph</a>

```json
[
    {
        "distance": 13.405,
        "mean_speed": 43.28,
        "start_time": "2015-12-25 10:04:05",
        "moving": true,
        "duration": 18.58
    },
    {
        "distance": 0,
        "mean_speed": 0,
        "start_time": "2015-12-25 10:22:40",
        "moving": false,
        "duration": 5.87
    },
    {
        "distance": 25.33,
        "mean_speed": 36.49,
        "start_time": "2015-12-25 10:28:32",
        "moving": true,
        "duration": 41.65
    }
]
```

With the trips api we can get summarized data about a collection of events over a time span grouped in a convenient and natural way.

For each vehicle's trip we can request for example the summary for counters changes and have information of important trip's events (like for maximum speed).<br>
This information is already calculated and stored for each trip, so it is much more efficient to get this kind of data using the trips api than the more general rawdata or counters api, and of course you don't have to deal with how to partition the raw data to get trips' information.

These are the currently supported query params to get trips data:

 Param    | Description
--------: | -------
ids       | Trips of interest
from      | Time from YYYY-MM-DD[Thh:mm:ss]
to        | Time to YYYY-MM-DD[Thh:mm:ss]
duration  | ISO_8601 Duration format (PnYnMnDTnHnMnS, PnW, PT) (ex: P1D = Past 1 Day)
vehicles  | Vehicles to get data from
fields    | Which data fields do you want to include
contained | Whether to return only fully contained trips within 'from' and 'to'
distance  | unit to return distance values in (default: meters)
volume    | unit to return volume values in (default: liters)
time      | unit to return time values in (default: second)
speed     | unit to return speed values in (default: distance_unit/time_unit)
export    | format to export the trips data (csv or tsv)


<aside class="warning">
All requested conditions defined by the parameters are <em>logically ANDed</em>, so invalid conditions will return no trips (eg: requesting <em>ids=12</em> and <em>vehicles=34</em> but trip with <em>id=12</em> belongs to vehicle with <em>id=56</em>).
</aside>

<aside class="notice">
In the near future, the trips API will support the advanced and flexible quering options present in rawdata API, like filtering, transforms and async.
</aside>

##Fields

> View the trips keys/fields and their description <br>
> <a href="https://cloud.pegasusgateway.com/api/trips/keys">https://cloud.pegasusgateway.com/api/trips/keys</a>

```json
{
    "mean_speed": {
        "units": "meter/second",
        "short_desc": "Mean speed",
        "long_desc": "Mean speed calculated with trip's distance and duration values",
        "type": "number"
    },
    "duration": {
        "units": "second",
        "short_desc": "Trip duration",
        "long_desc": "‘Total duration of the trip",
        "type": "number"
    },
    "distance": {
        "units": "meter",
        "short_desc": "Trip distance",
        "long_desc": "Preferred trip distance (ecu_dist or dev_dist)",
        "type": "number"
    },
    ...
}
```

> Request only the units and the type for the trip keys
> <a href="https://cloud.pegasusgateway.com/api/trips/keys?data=units,type">https://cloud.pegasusgateway.com/api/trips/keys?data=units,type</a>

```json
{
    "mean_speed": {
        "units": "meter/second",
        "type": "number"
    },
    "duration": {
        "units": "second",
        "type": "number"
    },
    "distance": {
        "units": "meter",
        "type": "number"
    },
    ...
}
```

Getting the resource `trips/keys` you can check for the available fields definitions for the trips.

For this resource you can optionally request the data of interest, eg:

`GET https://cloud.pegasusgateway.com/api/trips/keys?data=units,type`

The available options for the `data` parameter are:

* all <span style="font-size:80%">(default)</span>
* none
* type
* short_desc
* long_desc
* units

When `data=none` then no data for the fields is requested and only an array of available fields names is returned.

<br>
> View the distance, duration, ignition time, idle time and the average speed per trip <br>
> <a href="https://cloud.pegasusgateway.com/api/trips?from=2016-10-17&to=2016-10-18&vehicles=598,599&fields=distance,duration,ignition,idle,mean_speed">https://cloud.pegasusgateway.com/api/trips?from=2016-10-17&to=2016-10-18&vehicles=598,599&fields=distance,duration,ignition,idle,mean_speed</a>

```json
{
  "mean_speed": 13.17,
  "duration": 3483,
  "idle": 0,
  "distance": 45876,
  "ignition": 3483
}
```

When getting trips, if the *fields* parameter is undefined, then all available trips' fields data will be returned by default.<br>
You can request for a specific subset of trip information by defining the *fields* paramater.

`GET /trips?fields=moving,distance,mean_speed,duration...`

`GET /trips?fields=["moving","distance","mean_speed","duration"]...`

<br>
Notice that one of the possible fields is the `moving` trip's property. This flag indicates if the trip object corresponds to a *"trip"* or an *"stop"*.<br>
This is offered like this because in the more general use case we want to get the events summary and partitioning within a time range, so both types (*trips* and *stops*) are returned.

<aside class="notice">
When filtering gets available you will be able to query explicitly for "trips" or "stops" simply by filtering with <b>moving</b> or <b>not moving</b> correspondingly.
</aside>
