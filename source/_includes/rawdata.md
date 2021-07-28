# Rawdata

Raw data consists of a list of observations within a queried time range.
The rawdata API allows you to retrieve data, with a high degree of flexibility.
You can transform fields to your liking, perform small calculations, and resample data.

For large data sets, the request can be delegated to a job.
This will significantly improve the chances of retrieving the data set, circumventing timeouts and potential network issues.

The `rawdata` api has several query params that can be passed to specify the data need.

 Param | Description 
-------:| -------
`from` |  Date from YYYY-MM-DD[Thh:mm:ss]
`to` |  Date to YYYY-MM-DD[Thh:mm:ss]
`duration` |  ISO_8601 Duration format (PnYnMnDTnHnMnS, PnW, PT) (ex: P1D = Past 1 Day)
`vehicles` |  Vehicles to get data from
`groups` | Groups to get data from
`fields` | Which data fields do you want to include
`exclude` | Exclude the following fields
`types` | Event types to search for
`labels` | Filter by the following [labels](#labels), which are for describing the event generated
`codes` | Filter by event code (not recommended, use labels instead)
`distance` | unit to return distance values in (default: meters)
`volume` | unit to return volume values in (default: liters)
`time` | unit to return time values in (defalut: second)
`speed` | unit to return speed values in (default: distance_unit/time_unit)
`raw` | Show the rawdata format
`filter` | Create a custom filter for the results
`resample` | Activate resampling, can only be **event_time** or **system_time**
`freq` | Resampling frequency, required for resampling.
`how` | resampling method to use, defaults to 'mean'.
`fill` | How to fill empty rows after resampled (pad, ffill, bfill)
`group_by` | group events by specific fields before resampling.
`order` | Order the results by a particular field
`head` | Get top **n** events from result
`tail` | Get bottom **n** events from result
`export` | Export the data to json (default), html, csv, or tsv
`async` | Run the report in the background (returns a job id)

##Fields

> Get a list of all rawdata fields

> [/api/resources/rawdata/keys](https://cloud.pegasusgateway.com/api/resources/rawdata/keys)

```json
{
  "keys": {
    "io_exp_in1": {
      "short_desc": "exp in1",
      "long_desc": "I/Os expander input 1",
      "type": "boolean",
      "group": "IO related"
    },
    "code": {
      "short_desc": "EVC",
      "long_desc": "GPS Event code reported by 'EV' msg",
      "type": "number",
      "group": "Event identification"
    },
    "ecu_distance": {
      "short_desc": "VDist",
      "long_desc": "Vehicle dashboard odometer reported by ECUMon [m]",
      "type": "number",
      "group": "ECU monitor accessory related"
    },
    "ecu_idle_fuel": {
      "short_desc": "IFuel",
      "long_desc": "Fuel consumed in IDLE. Rep. by ECUMon [L x10]",
      "type": "number",
      "group": "ECU monitor accessory related"
    },
    "cf_rssi": {
      "short_desc": "rssi",
      "long_desc": "GSM Cell rssi (0-31)",
      "type": "number",
      "group": "Communication related"
    },
    "pid": {
      "short_desc": "Pegasus ID",
      "long_desc": "Pegasus site ID",
      "type": "number",
      "group": "Site identification"
    }
  }
}
```

The rawdata api allows you to select fields from the pegasus' events table.
Simply pass the fields parameter to the request with the fields you want.

To view a list of all the fields available to you, perform an empty GET request on the rawdata resource

<aside class="notice">
    You can also see the [sets](#sets) available towards the bottom.
</aside>

By default all rawdata requests come with

* system_time
* event_time
* id
* vid

To select fields, simply pass them to `fields` parameter

`/rawdata?fields=comdelay,speed,cf_rssi,hdop,dev_dist...`

You may also pass a JSON array to the fields parameter, this allows for higher flexibility when doing complex transformations.

`/rawdata?fields=["dev_ign","dev_idle","cf_rssi","hdop","dev_dist"]`

For a list of the rawdata fields please refer to the 

[Master Fields List](#master-fields-list)


For a more detailed list of the rawdata fields please refer to:

[Pegasus API Fields](https://docs.google.com/spreadsheets/d/1WBXq8NvWqPB6vRO2Tlye17JfrM5X3QZTlr9vs7uxVSk/edit#gid=1761829077)

### Sets
you can pass sets, which are a group of fields (ex. basic, counters, network, photo ...) as part of the `fields` parameter preceeded by `$`

`/rawdata?fields=$basic,$network`

### Exclude
If you want to bring some parts of a set or sets, it may be better to include the whole set, and then exclude what you dont want. 
In order to do this you can pass the `excludes` parameter.

`/rawdata?fields=$basic&excludes=speed,mph`

The excludes paramater also supports ***regular expressions***, this allows you to exclude multiple fields at once:
For example, bring all device counters and exclude all ecu counters

`/rawdata?fields=$counters&exclude=$ecu.*^`

Instead of:

`/rawdata?fields=dev_dist,dev_idle,dev_ign,dev_ospeed,dev_orpm...`

## Transformations

In addition to selecting fields, you can rename and create new fields.

> A rawdata transformation that renames rssi, latitude, and longitude<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&duration=P1D&fields=latitude:@lat,longitude:@lon,rssi:@cf_rssi,speed,head">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&duration=P1D&fields=latitude:@lat,longitude:@lon,rssi:@cf_rssi,speed,head</a>

```json 
{
  "head": 9,
  "vid": 197,
  "event_time": "2018-10-16T22:12:20",
  "system_time": "2018-10-16T22:12:24.412584",
  "longitude": -75.49467,
  "latitude": 10.35501,
  "rssi": 28,
  "speed": 0,
  "id": 5014678489
}
```

### Renaming Fields

To rename a field, simply pass the name you want, and the field it is renaming referenced with `@` and separated by a `:` 

**renamed_field:@field**

`odometer:@dev_dist`

You can do as many of these as you want.

`/rawdata?fields=rssi:@cf_rssi,satellites:@sv,gps_quality:@hdop`

###Building fields

> Rawdata request with a simple operator<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-05&to=2018-10-09&fields=ratio:@lat/@lon,lat,lon">https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-05&to=2018-10-09&fields=ratio:@lat/@lon,lat,lon</a>

```json
{
  "system_time": "2018-10-05T00:06:40.998478",
  "ratio": -0.3205509899,
  "vid": 1674,
  "event_time": "2018-10-05T00:06:21",
  "lon": -80.16553,
  "lat": 25.69714,
  "id": 5014187981
}
```

> Rawdata request that has more operators and manipulations<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-06&to=2018-10-07&fields=kph:@speed,mph,vehicle_battery_volts:@ecu_battery/1000,communication_delay:(@system_epoch-@event_epoch),centimeters_per_second:@mph*44.704,speed_in_knots:@mph*0.868976">https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-06&to=2018-10-07&fields=kph:@speed,mph,vehicle_battery_volts:@ecu_battery/1000,communication_delay:(@system_epoch-@event_epoch),centimeters_per_second:@mph*44.704,speed_in_knots:@mph*0.868976</a>

```json
{
  "system_time": "2018-10-06T15:51:02.014862",
  "vid": 1674,
  "event_time": "2018-10-06T15:50:58",
  "communication_delay": 4.0148599148,
  "mph": 7,
  "speed_in_knots": 6.082832,
  "centimeters_per_second": 312.928,
  "kph": 11.26538,
  "vehicle_battery_volts": null,
  "id": 5014260288
}
```

You can also use basic arithmetic operations to build new fields:

Divide the latitude by the longitude

`ratio:@lat/@lon`

or subtract epoch timestamps for the time the event was received by the time the event was generated.

`comdelay:@system_epoch-@event_epoch`

<aside class="notice">
<b>comdelay</b> is already provided for you, this is just an example of how to build it.
</aside>

Remember, you can also use numbers in the transformations, for example to get the km reported by dev_dist:

`km:@dev_dist/1000`

`vehicle_battery_volts:@ecu_battery/1000`

`centimeters_per_second:@mph*44.704`

`knots:@mph*0.868976`


### Casting Fields

You can also concatenate strings

`/rawdata?fields:["custom:'text_to_prepend'%2B@field__str"]`

Example:

`/rawdata?fields:["google:'https://google.com/maps?q=' %2B @lat__str %2B ',' %2B @lon__str"]`

Note that the `+` sign is URL encoded in the request as `%2B`, this is required in order for the parser to work.

> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=2600&duration=P1D&fields=%5B%22$basic%22,%22google:%27https://google.com/maps?q=%27%20%2B%20@lat__str%20%2B%20%27,%27%20%2B%20@lon__str%22%5D&distance=km&volume=liter&time=second&speed=kph&order=vid,event_time,system_time&tz=America/New_York">/rawdata?vehicles=2600&duration=P1D&fields=%5B%22$basic%22,%22google:%27https://google.com/maps?q=%27%20%2B%20@lat__str%20%2B%20%27,%27%20%2B%20@lon__str%22%5D&distance=km&volume=liter&time=second&speed=kph&order=vid,event_time,system_time&tz=America/New_York</a>


<aside class="notice">
Notice that we used JSON array notation, not csv, since our built string has a comma (<strong>,</strong>) in it.
Without the JSON array notation, we would have thrown off the parser.
</aside>

If you get a validation error, make sure the castings match! How?
Simply append one of

* __str
* __int
* __float

> Rawdata request that casts the speed as a string

> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-05&to=2018-10-09&fields=speed_as_string:@speed__str,speed&speed=kph">https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-05&to=2018-10-09&fields=speed_as_string:@speed__str,speed&speed=kph</a>

```json
{
  "system_time": "2018-10-06T16:36:54.534108",
  "vid": 1674,
  "event_time": "2018-10-06T16:36:45",
  "speed_as_string": "20.92142",
  "speed": 20.92142,
  "id": 5014261769
}
```

<aside class='warning'>
You may run into some issues with variable types mismatching, we provide a method for casting variables that is very simple.
</aside>

> Example combining everything above

> Results in the daily total hours with the engine on, distance traveled, last location, etc.

> [api/rawdata?vehicles=1673&fields=\["DATE_LAST_REPORTED...](https://cloud.pegasusgateway.com/api/rawdata?vehicles=1673&fields=%5B%22DATE_LAST_REPORTED:@event_time%22,%22LAST_LAT:@lat%22,%22LAST_LON:@lon%22,%22HOURS_OF_OPERATION:@dev_ign.round(1)%22,%22HOURS_IDLING:@dev_idle.round(1)%22,%22DISTANCE_TRAVELED_KM:@dev_dist.round(1)%22,%22LOCATION:%27https://google.com/maps?q=%27%20%2B%20@lat__str%20%2B%20%27,%27%20%2B%20@lon__str%22%5D&duration=P1D&tz=America/Santiago&resample=event_time&how=HOURS_OF_OPERATION:diff,HOURS_IDLING:diff,DISTANCE_TRAVELED_KM:diff,LAST_LAT:last,LAST_LON:last,DATE_LAST_REPORTED:last,LOCATION:last&group_by=vid&freq=1D&time=hour&distance=km&head=10)

```json
{
  "events": [
    {
      "vid": 1673,
      "event_time": "2019-01-09T00:00:00",
      "LAST_LAT": 25.74485,
      "HOURS_IDLING": 0,
      "LAST_LON": -80.22526,
      "LOCATION": "https://google.com/maps?q=25.74485,-80.22526",
      "HOURS_OF_OPERATION": 0.6,
      "DISTANCE_TRAVELED_KM": 22.2,
      "DATE_LAST_REPORTED": 1547075815000
    },
    {
      "vid": 1673,
      "event_time": "2019-01-10T00:00:00",
      "LAST_LAT": 25.78423,
      "HOURS_IDLING": 0,
      "LAST_LON": -80.29391,
      "LOCATION": "https://google.com/maps?q=25.78423,-80.29391",
      "HOURS_OF_OPERATION": 0.4,
      "DISTANCE_TRAVELED_KM": 11.1,
      "DATE_LAST_REPORTED": 1547116807000
    }
  ]
}
```

## Filtering

> Rawdata request filtered by events with valid positions<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-30T08:00:00&to=2018-10-30T23:59:59&fields=mph,valid_position&filter=valid_position">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-30T08:00:00&to=2018-10-30T23:59:59&fields=mph,valid_position&filter=valid_position</a>

```json
{
  "system_time": "2018-10-30T12:03:08.701770",
  "vid": 197,
  "event_time": "2018-10-30T12:03:05",
  "mph": 1,
  "valid_position": true,
  "id": 5002929550
}
```

> Rawdata request filtered by events with valid positions and mph > 30<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-30T08:00:00&to=2018-10-30T23:59:59&fields=mph,valid_position&filter=valid_position+and+mph+%3E+30">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-30T08:00:00&to=2018-10-30T23:59:59&fields=mph,valid_position&filter=valid_position+and+mph>30</a>

```json
{
  "system_time": "2018-10-30T12:03:08.701770",
  "vid": 197,
  "event_time": "2018-10-30T12:03:05",
  "mph": 1,
  "valid_position": true,
  "id": 5002929550
}
```

> Rawdata request with a filter that shows events after 8 with speeds greater than 20 and valid positions<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-10T00:00:00&to=2018-10-11T23:59:59&fields=mph,valid_position,event_hour&filter=event_hour%3E8+and+(speed+%3E+20+and+valid_position)">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2018-10-10T08:00:00&to=2018-10-11T23:59:59&fields=mph,valid_position,event_hour&filter=system_hour>8+and+(speed>20+and+valid_position)</a>

```json
{
  "system_time": "2018-10-10T09:14:59.506056",
  "vid": 197,
  "event_time": "2018-10-10T09:14:57",
  "mph": 28,
  "valid_position": true,
  "id": 5014414518,
  "event_hour": 9
}
```

The rawdata allows you to filter fields that meet a certain criteria. 
Using the `filter` field on the request, you can pass complex boolean logic that meets your criteria.

You can apply boolean logic to filter the desired results, available operators:

`>, < , >= , <=, ==, !=, and, or, not`

Some examples:

`valid_position and speed > 30`

`label == "spd" and mph < 50 `

`(lat < -4 and lat > -5) and ( lon < -74 and lon > -75 )`

`system_hour == 7 and (system_minute > 0 and system_minute < 30)`

`speed > 50`

A more complex example:

`(event_wkday == 3) and cf_rssi < 10 and ( event_hour > 7 and event_hour < 17)`

Which translates to:
get all events on wednesdays between 7AM and 5PM with an cf_rssi value less than 10 (low GSM signal)

<aside class="notice">
Whenever making analysis of data it's recommended that you use the following filters to remove any outliers due to bad gps conditions.

&filter=valid_position and hdop < 3
</aside>

### Codes

You can further filter by looking at individual event codes generated, event codes depend on the managed configuration of the device.

> Rawdata request of only event codes 4 and 47
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-09-30T08:00:00&to=2018-09-30T23:59:59&fields=$basic&filter=speed+%3E+10&codes=4,47">https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-09-30T08:00:00&to=2018-09-30T23:59:59&fields=$basic&filter=speed+%3E+10&codes=4,47</a>

```json
{
  "ecu_battery": null,
  "head": 7,
  "code": 47,
  "hdop": 1.16,
  "event_time": "2018-09-30T14:14:46",
  "ecu_distance": null,
  "vid": 1674,
  "lon": -80.16431,
  "sv": 7,
  "mph": 16,
  "label": "agglnchng",
  "valid_position": true,
  "type": 10,
  "lat": 25.69373,
  "system_time": "2018-09-30T14:14:49.760305",
  "speed": 16,
  "id": 5014039405,
  "device_id": 357042060068252
}
```

### Label

> GET labels
> [api/labels](https://cloud.pegasusgateway.com/api/labels)

```json
{
  ...
  "spd": {
    "edited_at": "2017-07-21T13:56:49.184650+00:00",
    "en": "Speeding:The vehicle has exceeded the speed limit continuously for more than the threshold configured",
    "created_at": "2017-07-21T13:56:49.184766+00:00",
    "label": "spd",
    "es": "Exceso de velocidad:El vehiculo ha excedido el limite de velocidad continuamente por mas del umbral configurado"
  },
  ...
}
```

Filtering by labels allows you ask for all the specific events across your fleet, no matter the configuration of the device. Labels are shared for every configuration, so you can ask for all the aggressive lane changes and speeding events for all your vehicles, regardless of the configurations of the devices.

For more information about labels please visit: [labels](#labels)

> Rawdata request from group 500 with speeding and aggressive lane change events 
> <a href="https://cloud.pegasusgateway.com/api/rawdata?groups=500&from=2018-09-30T08:00:00&to=2018-09-30T23:59:59&fields=$basic&filter=speed+%3E+10&labels=agglnchng,spd">https://cloud.pegasusgateway.com/api/rawdata?groups=500&from=2018-09-30T08:00:00&to=2018-09-30T23:59:59&fields=$basic&filter=speed>10&labels=agglnchng,spd</a>

```json
{
  "head": 191,
  "code": 47,
  "hdop": 0.75,
  "event_time": "2018-09-30T11:40:30",
  "type": 10,
  "vid": 1673,
  "lon": -80.20623,
  "sv": 12,
  "mph": 22,
  "label": "agglnchng",
  "valid_position": true,
  "lat": 25.82955,
  "system_time": "2018-09-30T11:40:36.382065",
  "speed": 22,
  "id": 5014035611,
  "device_id": 357042060068740
}
```

### <a name="data_types">Types</a>

> Rawdata request for maximum speed (metric==2) in driving metric events (types=12)<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-01T00:00:00&to=2018-10-10T23:59:59&fields=metric,metric_value&filter=metric==2&order=metric&types=12&speed=mph">https://cloud.pegasusgateway.com/api/rawdata?vehicles=1674&from=2018-10-01T00:00:00&to=2018-10-10T23:59:59&fields=metric,metric_value&filter=metric==2&order=metric&types=12&speed=mph</a>

```json
{
  "system_time": "2018-10-01T16:10:13.464688",
  "metric_value": 20,
  "vid": 1674,
  "metric": 2,
  "event_time": "2018-10-01T16:08:47",
  "id": 5014071812
}
```

Events reported by the devices are classified by types, the most common type is 10, which means it's a regular GPS event. 
There are other useful types for example 12, which gives you the driving metric values of the vehicles (Max Acceleration, Deceleration and Speed per trip)
In order to filter by types simply state:

`types=12`

Type |  Description |
--:|:----|
10 | EV TAIP regular events (GPS Type)
11 | PV TAIP position & velocity (GPS Type)
12 | RXAIT driving metrics (GPS Type)
13 | GPS BackLog (GPS Type)
20 | empty GPS Event (gps time = 0)
100 | Keep-alive (Connection Type)
110 | XART TAIP (Connection Type)
111 | XARS SIM Card Operator notice (Connection Type)
150 | Message
301 | Duplicate GPS Event (Other Type) 

## Ordering

> Rawdata request ordered by code (increasing), and mph (decreasing)<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?groups=500&fields=$basic&duration=PT4H&order=code,-mph">https://cloud.pegasusgateway.com/api/rawdata?groups=500&fields=$basic&duration=PT4H&order=code,-mph</a>

```json
{
  "head": 102,
  "code": 3,
  "hdop": 1.48,
  "event_time": "2018-10-17T23:24:02",
  "type": 10,
  "vid": 1483,
  "lon": -80.44694,
  "sv": 4,
  "mph": 0,
  "label": "ignoff",
  "valid_position": true,
  "lat": 25.66764,
  "system_time": "2018-10-17T23:24:05.677751",
  "speed": 0,
  "id": 5014716545,
  "device_id": 356612026170920
}
```

You can also order the resulting data set however you choose.
Simply pass the order field to the request

`order=mph`

This will order the events by mph in ascending order. but what about ascending?

Simple just append a `-`, to the field you want to order

`order=-mph`

What about multilevel ordering? Sure thing, no problem:

`order=vid,-mph`

This will return all events sorted by vehicle id (vid) and then by speed in descending order

## Limiting

> Rawdata request that limits the results to the first two and last 3 events<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?groups=500&fields=$basic&duration=PT4H&order=code,-mph&filter=code%3E1%20and%20code%3C10&head=2&tail=3">https://cloud.pegasusgateway.com/api/rawdata?groups=500&fields=$basic&duration=PT4H&order=code,-mph&filter=code%3E1%20and%20code%3C10&head=2&tail=3</a>

```json
{
  "head": 280,
  "code": 8,
  "hdop": 0.64,
  "event_time": "2018-10-17T22:43:00",
  "type": 10,
  "vid": 909,
  "lon": -83.85399,
  "sv": 13,
  "mph": 0,
  "label": "slwtrfc",
  "valid_position": true,
  "lat": 10.25766,
  "system_time": "2018-10-17T22:43:03.888619",
  "speed": 0,
  "id": 5014715284,
  "device_id": 356612026180218
}
```

By default the rawdata request will return all the events that match your criteria.

What if you want to see the first x events. Simply pass the `head` argument to the request

`head=100`

Will return the top 100 events from the set.

Want to see the last n events? pass `tail` as the argument.

`tail=100`

Want both? How about the first 100 and bottom 50 events?

`head=100&tail=50`

You can even pass negative values to hide the first or last rows, for example, `tail=-1` removes the first row, and `head=-1` removes the last row.

<aside class="notice">
If the resulting set is less than the values of `head` and `tail` combined, dont worry, we'll just return it as is.
</aside>

## Resampling

> Returns the difference in all counters (max-min), resampled at 1 day intervals and grouped by vids from November 1 . This gives you the total distance traveled, ignition on time, etc. per day per vehicle <br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=617,654&fields=$counters&from=2015-11-01T00:00:00&to=2015-11-10T00:00:00&filter=(valid_position+and+hdop+<+3)&how=$counters:diff&freq=1D&group_by=vid&resample=event_time">https://cloud.pegasusgateway.com/api/rawdata?vehicles=617,654&fields=$counters&from=2015-11-01T00:00:00&to=2015-11-10T00:00:00&filter=(valid_position+and+hdop+<+3)&how=$counters:diff&freq=1D&group_by=vid&resample=event_time</a>

```json
{
  "dev_dist": 72542,
  "vid": 909,
  "ecu_dist": 72421,
  "event_time": "2015-11-09T00:00:00",
  "dev_idle": 21069,
  "dev_orpm": 8151,
  "dev_ospeed": 0,
  "ecu_ifuel": 19.4,
  "ecu_tfuel": 212.4,
  "ecu_eusage": 36900,
  "ecu_eidle": 14400,
  "dev_ign": 40823
}
```


> Returns the mean value of all comdelays and the mean value of the RSSI (signal strength) sampled at 1 day intervals, grouped by vehicle IDs<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=617,654&fields=$basic,comdelay,rssi_avg:@cf_rssi&from=2015-11-1T00:00:00&to=2015-11-10T00:00:00&filter=(comdelay+<+3600)&how=comdelay:mean,rssi_avg:mean&freq=1D&group_by=vid&resample=event_time">https://cloud.pegasusgateway.com/api/rawdata?vehicles=617,654&fields=$basic,comdelay,rssi_avg:@cf_rssi&from=2015-11-1T00:00:00&to=2015-11-10T00:00:00&filter=(comdelay+<+3600)&how=comdelay:mean,rssi_avg:mean&freq=1D&group_by=vid&resample=event_time</a>

```json
{
  "comdelay": 155.2830875629,
  "rssi_avg": 15.835443038,
  "event_time": "2015-11-01T00:00:00",
  "vid": 617
}
```

Resampling is an important part of data analysis, timed data can be normalized to evenly timed chunks.

<!-- For more noticermation on how resampling can be used, and how it can benefit your analysis, see:  -->

<!-- [LINK TO SUPER DUPER BLOG]() -->

Events that pass through the Pegasus Gateway naturally contain time noticermation. 

These fields are:

* event_time : time reported by the device.
* system_time: time the event was processed by the gateway. 

The Rawdata API allows you to resample data based on either one of these.
<aside class="warning">
When resampling, you must make sure that the resample field (event_time or system_time) cannot be excluded via, the excludes paramter.
</aside>

There are 5 fields important to resampling.

* resample : timed field to use in resampling (event_time or system_time)
* freq : Frequency you want to resample to (ex. by day, by month, every 30 seconds, etc)
* how : Method to use for resampling, defaults to `mean` (ex. sum, diff, min, max ...)
* fill : How to fill gaps in the data set. ffill, pad, bfill
* group_by : group events by field(s) before performing resample. (useful for multiple vehicles)

<aside class="warning">
When resampling data. The resulting set of events will contain only the field you resampled by. 
That is, if you resampled by event_time, then system_time will be stripped, and vice-versa.

Resampling also strips the event id from the set.
</aside>

###resample

Used to activate resampling.
Can only be ***event_time*** or ***system_time***.
<aside class="warning">
When this field is provided, you must provide the `freq` parameter with which to resample the data.
</aside>

###freq
Establishes the frequency by which to resample.
Must meet the format:

* NF

where N is equal to a number greater than 0.
and F can be one of:

* M : Months
* D : Days
* H : Hours
* T : Minutes
* S : Seconds

###how
Determines how to resample the data, based on the frequency established.
This must be one of.

* sum
* mean : Average of values
* std : Standard deviation of values
* sem : MISSING
* max
* min
* median
* first
* last
* diff : range of values, max-min

You can also resample different fields by different methods.
Simply pass the fields as a csv, with the transformation you want separated by `:`

***field:method***

`how=dev_dist:diff,cf_rssi:mean,hdop:mean,hdop:max,vdop:min`

want to apply the same resample method to a set? Simply:

`how=$counters:diff`

<aside class="notice">
You can overwrite specific members of a set with different `how` methods.
</aside>

<aside class="warning">
When resampling specific fields, fields not included in the `how` parameter are dropped. <br>
To keep fields that you may feel are releveant, make sure to include them in the `how` parameter with an appropriate method, such as `max`,`first`,`last` or `min`. <br>
This will guarantee that each resampled entry contains data that you may need.
</aside>

<aside class="error">
Resampled data cannot be ordered as it is already broken down into chunks of time by the freq param. Thus don't use the parameter `order` when resampling data.
</aside>

###group_by
You can also group the events before performing a resample. this is useful if you want to keep events tied to a specific field, such as the vehicles id (vid).

`group_by:vid`

You can additionally group by more than one field:

`group_by:vid,label`

<aside class="notice">
If you want to perform fleet level metrics. Try grouping by something else, such as the event code or label, not vid.
</aside>

## Output Formats

> Export as HTML<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2015-10-30T08:00:00&to=2015-10-30T23:59:59&fields=vid,lat,lon&export=html">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2015-10-30T08:00:00&to=2015-10-30T23:59:59&fields=vid,lat,lon&export=html</a>


|     event_time      |     id     |  label  |   lat   |    lon    | vid
| ------------------: | ----------:| -------:| -------:| --------: | --: 
| 2015-10-30T12:03:05 | 5002929550 | trckpnt | 9.80091 | -74.76004 | 197
| 2015-10-30T12:08:06 | 5002929669 | trckpnt | 9.80670 | -74.73022 | 197


By default, events come in a JSON envelope under the `events` property.

The rawdata API also allows you to export in other formats:

* JSON (default)
* CSV
* TSV
* HTML table

Simply pass the `export` parameter on the request

`export=html`

Need another format? Send us a message on our [Gitter](https://gitter.im/dctdevelop/pegasus#) or create an issue on our [github](https://github.com/dctdevelop/pegasus)

## Asynchronous Requests

> An asynchronous requests that generates a job id to be queried later<br>
> <a href="https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2015-09-30T08:00:00&to=2015-09-30T23:59:59&fields=ecu_battery,ecu_distance&filter=speed+%3E+10&async=true">https://cloud.pegasusgateway.com/api/rawdata?vehicles=197&from=2015-09-30T08:00:00&to=2015-09-30T23:59:59&fields=ecu_battery,ecu_distance&filter=speed>10&async=true</a>

```json
{
  "message": "Job created",
  "job_id": 28813
}
```

> While the job is being processed in the background you can see the progress under /jobs/:job_id<br>
> <a href="https://cloud.pegasusgateway.com/api/jobs/28813">https://cloud.pegasusgateway.com/api/jobs/28813</a>

```json
{
  "control": "run",
  "stdout": "",
  "app": "pegasus2.0",
  "notice": null,
  "id": 28813,
  "user_id": 1,
  "state": "queued",
  "params": {
    "all": false,
    "event_groups": null,
    "labels": null,
    "raw": false,
    "export": null
  ...
}
```

> Once the job is finished you can see the data here<br>
> <a href="https://cloud.pegasusgateway.com/api/jobs/28813/data.json">https://cloud.pegasusgateway.com/api/jobs/28813/data.json</a>

```json
[
  {
    "code": 1,
    "device_id": 356612022409637,
    "ecu_battery": 13350,
    "ecu_distance": 269302153,
    "event_time": "2015-09-30T14:59:06",
    "hdop": 0.94,
    "head": 225,
    "id": 5002464983,
    "label": "",
    "lat": 4.61529,
    "lon": -74.10331,
    "mph": 11,
    "speed": 11,
    "sv": 8,
    "system_time": "2015-09-30T14:59:09.565434",
    "type": 10,
    "valid_position": true,
    "vid": 197
  }
]
```

Results returned by the rawdata API can get big, and we mean big.
This can bring up all sorts of issues when using via a web browser.

These include but are not limited to:

* Connection issues (timeouts, disconections)
* Lack of processing power on client machine (try parsing a 500MB JSON file on chrome)

In order to circumvent these issues, and guarantee that data is always retreived as requested. You can leave the query and subsequent processes (transformations, filtering and resampling) running in the background.

The `async` flag delegates the rawdata request to a Job to ensure the data is stored and can be safely used and reused

<aside class="notice">
We recommend you take a look at our Jobs section in order to get an understanding of a jobs progress, as well as how to handle possible errors.
</aside>

Simply set the `async` flag on the request

`async=1`

The request immediately returns a job ID
you can query the job ID to see the progress of the rawdata request
`/api/jobs/:jobid`

When the job finishes you can see the by accesing the jobs data
Data is stored depending on your export format.

For JSON (default)
`/api/jobs/:jobid/data.json`

For CSV, TSV, and HTML
`/api/jobs/:jobid/data.{export_format}`

<aside class="notice">
We recommend that you use a small time range to test the rawdata request before you make a large request.
</aside>
