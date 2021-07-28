# Counters

The rawdata API allows for extreme flexibility when browsing events.

Perhaps the most frequent use case for the rawdata API, is a resampling of a vehicle(s) counters over a period of time.

For example, to view how much the vehicle's counters changed over a large period, such as a month for example, you would make a request such as :

`GET api/rawdata?vehicles=2600&duration=P1M&fields=$counters&resample=event_time&how=$counters:diff&freq=1M&group_by=vid`

This would give you values such as the total distance covered, total time driving, total amount of fuel used for that month.
For a more in depth view, such as what happened each day of that month, you would adjust the freq parameter to your liking.
such as `1D`.

Given this frequent use case, we provide a separate, specialized API for browsing counters which automatically does the resampling for you.

The counters API has nearly all of the functionality of the rawdata API, along with some added fields.

Here is a list of the paramateres accepted by the `counters` API.

Parameter | Description 
---------:| -------
from |  Date from YYYY-MM-DD[Thh:mm:ss]
to |  Date to YYYY-MM-DD[Thh:mm:ss]
duration |  ISO_8601 Duration format (PnYnMnDTnHnMnS, PnW, PT) (ex: P1D = Past 1 Day)
vehicles |  Vehicles to get data from
distance | unit to return distance values in (default: meters)
volume | unit to return volume values in (default: liters)
time | unit to return time values in (defalut: second)
speed | unit to return speed values in (default: distance_unit/time_unit)
groups | Groups to get data from
filter | Create a custom filter for the results
delta | fragment size.
order | Order the results by a particular field
head | Get top **n** events from result
tail | Get bottom **n** events from result
export | Export the data to json (default), html, csv, or tsv
round | Round the results to 'x' number of decimal places (1-10) 
async | Run the report in the background (returns a job id)

You may override the speed values to either `kph`, `mph`, or `user`.

A speed preference of 'user' establishes either `kph` or `mph` based on the users distance preference (`km` or `mile` respectively)

<aside class="warning">
Be sure to check out the <a href="#units" class="">units</a> section, as it provides in depth detail on the accepted values.
</aside>

<aside class="notice">
Notice that most fields present serve the same purpose as in the `rawdata` API. <br>
We encapsulate the resample and field selection parameters since we are focused on counters only.
</aside>

## Introduction

> [api/counters](https://cloud.pegasusgateway.com/api/counters)

```json
{
    ...
    "base": {
        "dev_dist": "Distance reported by Syrus",
        "ecu_ifuel": "Fuel spent while idling, reported by ECU monitor",
        "ecu_eidle": "Idle time reported by ECU monitor",
        "ecu_dist": "Distance reported by ECU monitor",
        "dev_ign": "Engine time reported by Syrus",
        "dev_idle": "Idle time reported by Syrus",
        "dev_orpm": "Time with engine in high RPM's, reported by Syrus",
        "ecu_eusage": "Engine usage time reported by ECU Monitor",
        "dev_ospeed": "Time overspeeding, reported by Syrus",
        "ecu_tfuel": "Total fuel speng, reported by ECU monitor"
    },
    "derived": {
        "dev_tot_avg_speed": "Total average speed, Syrus. dev_dist/dev_ign",
        "ecu_tra_avg_speed": "Traveling average speed, ECU. ecu_dist/(ecu_eusage-ecu_eidle)",
        "ecu_tot_fuel_efcy": "ECU calculated Fuel efficiency. ecu_dist/ecu_tfuel",
        "ecu_tra_fuel_efcy": "ECU calculated Traveling fuel efficiency. ecu_dist/(ecu_tfuel-ecu_ifuel)",
        "dev_tra_fuel_efcy": "Syrus calculated Traveling fuel efficiency. dev_dist/(ecu_tfuel-ecu_ifuel)",
        "dev_tra_avg_speed": "Traveling average speed, Syrus. dev_dist/(dev_ign-dev_idle)",
        "ecu_fuelp_idling": "Fuel percentage used while idling. ecu_ifuel*100/ecu_tfuel",
        "ecu_tot_avg_speed": "Total average speed, ECU. ecu_dist/ecu_eusage",
        "dev_tot_fuel_efcy": "Syrus calculated Fuel efficiency. dev_dist/ecu_tfuel"
    }
}
```

Pegasus keeps track of various internal counters for each vehicle. The counters are an absolute value that always increase over time.

The counters api is used for knowing time, distance, and volume information for any vehicle within a given time range.

The default units for distance, time, and volume are meters, seconds, and liters respectively.

__Sources for the counters__

+ Syrus \[dev_*\]
+ ECU Monitor \[ecu_*\] ([Accessory](#ecu-monitor))

The source of the data is controlled by the managed configuration on the device.
The ECU Monitor provides data from the computer onboard the vehicle, whether OBDII or J1939/1708.

<aside class="notice">
Similar to the rawdata API, and empty GET request to `/api/counters` resources will provide you with information regarding the resource.
</aside>

Counter field | Description
---- | ----
dev_dist | Traveled distance 
dev_ign | Ignition time 
dev_idle | Time in idle 
dev_ospeed | Time spent speeding 
dev_orpm | Time spent over RPM threshold 
ecu_dist | Engine Distance 
ecu_eusage | Engine Ignition 
ecu_eidle | Engine Idling 
ecu_tfuel | Fuel consumption 
ecu_ifuel | Idle fuel consumption 

<aside class="notice">
You will notice these are the same as the <strong>$counters</strong> field set from the rawdata API.
</aside>

Considering these as base counters. The Pegasus API provides derived calculated counters:

Derived field | Description
---- | ----
dev_tot_avg_speed | Total average speed, Syrus.
dev_tra_avg_speed | Traveling average speed, Syrus.
dev_tot_fuel_efcy | Syrus calculated Fuel efficiency.
dev_tra_fuel_efcy | Syrus calculated Traveling fuel efficiency.
ecu_tot_avg_speed | Total average speed, ECU.
ecu_tra_avg_speed | Traveling average speed, ECU.
ecu_tot_fuel_efcy | ECU calculated Fuel efficiency.
ecu_tra_fuel_efcy | ECU calculated Traveling fuel efficiency.
ecu_fuelp_idling | Fuel percentage used while idling.

## Browser

The counters API encapsulates some of the complexities that the rawdata provides, in order to streamline the counters information.

The counters API supports two main use cases. 

+ Retrieving the absolute difference between two dates for each vehicle passed. (ex. total distance traveled in one month)
+ Calculating the difference in smaller intervals given a larger time frame (ex. distance traveled per hour for a week. or per day for a month)

## Absolute Changes

> Total counter values for month of November

> [api/counters?vehicles=2600&from=2018-11-01T00:00:00&to=2018-11-30T23:59:59](https://cloud.pegasusgateway.com/api/counters?vehicles=197&from=2018-11-01T00:00:00&to=2018-11-30T23:59:59)

```json
{
    "units": {
        "volume": "liter",
        "distance": "meter",
        "speed": "meter/second",
        "time": "second"
    },
    "counters": [
        {
            "ecu_dist": 7285823,
            "vid": 2600,
            "ecu_ifuel": 49.6,
            "dev_tot_avg_speed": 9.7753377857,
            "from": "2018-10-31T23:59:49",
            "dev_idle": 188131,
            "ecu_eidle": 75960,
            "ecu_fuelp_idling": 1.3752564742,
            "to": "2018-11-30T23:00:00",
            "ecu_eusage": 662760,
            "ecu_tot_avg_speed": 10.9931543847,
            "ecu_tfuel": 3606.6,
            "ecu_tra_avg_speed": 12.4161946149,
            "dev_orpm": 54301,
            "dev_tra_fuel_efcy": 2049.6412707338,
            "ecu_tot_fuel_efcy": 2020.1361393002,
            "ecu_tra_fuel_efcy": 2048.3055946022,
            "dev_dist": 7290574,
            "dev_ign": 745813,
            "dev_tra_avg_speed": 13.0729950043,
            "dev_tot_fuel_efcy": 2021.4534464593,
            "dev_ospeed": 23452
        }
    ]
}
```

To view a vehicle's metrics over a period of time, simply pass the vehicles or groups parameter, along with a from and to date or a duration.

Like in the `rawdata` api, the <b>to</b> parameter is defaulted to the time the request is made

The resources return an envelope with the units, and counters for the given time range.

Additionally, each counter object within the counters array, contains a `from` and `to` property. These properties are the times of the start event and end event respectively.

<aside class="noticer">
Notice that the <strong>from</strong> and <strong>to</strong> times are the closest approximation to the dates requested. The counters API accounts for gaps in the vehicles reporting times.
</aside>


## Fragmented changes

> Daily counters requests for the past 30 Days<br>
> [api/counters?vehicles=2600&duration=P30D&delta=1D](https://cloud.pegasusgateway.com/api/counters?vehicles=2600&duration=P30D&delta=1D)

```json
{
    "units": {...},
    "counters": [
        {
            "ecu_dist": 179603,
            "vid": 2600,
            "ecu_ifuel": 1.4,
            "dev_tot_avg_speed": 5.8565823033,
            "from": "2018-11-01T00:00:54",
            "dev_idle": 10879,
            "event_time": "2018-11-01T00:00:00",
            "dev_ospeed": 269,
            "ecu_fuelp_idling": 0.7769145394,
            "to": "2018-11-01T22:30:10",
            "ecu_eusage": 22860,
            "ecu_tot_avg_speed": 7.8566491689,
            "ecu_tfuel": 180.2,
            "ecu_tra_avg_speed": 8.67647343,
            "dev_orpm": 0,
            "dev_tra_fuel_efcy": 1001.7114093959,
            "ecu_tot_fuel_efcy": 996.6870144283,
            "ecu_tra_fuel_efcy": 1004.4910514541,
            "dev_dist": 179106,
            "dev_ign": 30582,
            "dev_tra_avg_speed": 9.0902908187,
            "dev_tot_fuel_efcy": 993.9289678135,
            "ecu_eidle": 2160
        },
        {
            "ecu_dist": 8047,
            "vid": 2600,
            "ecu_ifuel": 0.9,
            "dev_tot_avg_speed": 1.8951807229,
            "from": "2018-11-02T00:30:10",
            "dev_idle": 2906,
            "event_time": "2018-11-02T00:00:00",
            "dev_ospeed": 0,
            "ecu_fuelp_idling": 12.676056338,
            "to": "2018-11-02T22:18:52",
            "ecu_eusage": 4320,
            "ecu_tot_avg_speed": 1.8627314815,
            "ecu_tfuel": 7.1,
            "ecu_tra_avg_speed": 2.6297385621,
            "dev_orpm": 0,
            "dev_tra_fuel_efcy": 1268.5483870956,
            "ecu_tot_fuel_efcy": 1133.3802816892,
            "ecu_tra_fuel_efcy": 1297.9032258053,
            "dev_dist": 7865,
            "dev_ign": 4150,
            "dev_tra_avg_speed": 6.3223472669,
            "dev_tot_fuel_efcy": 1107.7464788723,
            "ecu_eidle": 1260
        },
        ...
    ...
}
```

You can also query the counters API for subranges, similar to the resampling frequency in the __Rawdata API__ (`freq` paramater).
This allows you to get multiple data points per request.

Say for example, you want to view the distance traveled by a vehicle per day, for the past 30 days. 

You could, make 30 requests, to the counters API with varying start and end days representing each day in your desired range.

This however, is not an optimal request. Since it requires multiple requests to complete,and may also trigger the rate limiter.

In order to circumvent this, you could instead request the counters API for a fragmented snapshot of the counters.

Simply pass the `from` and `to` dates that encapsulate the desired range, along with the `delta` parameter representing your desired fragment size (1D for Daily for example)

Instead of 30 one day requests, you instead have 1 requests, spannig 30 days, with 1 day fragments.

<aside class="noticer">
Useful in fleet performance reports, if you want to compare idling times, excess in speed times between a large number of vehicles.
</aside>

## Vehicle Counters

The counters mantained by pegasus are absolute, meaning they always increase. In order to facilitate the upkeep of virtual odommeters. Pegasus allows you to see counters per vehicle, globally and shared per vehicle.

### Global vehicle counters

> Global vehicle counters for vehicle 1716 <br>
> [api/vehicles/1716/counters](https://cloud.pegasusgateway.com/api/vehicles/1716/counters)

```json
{
  "user": [],
  "vehicle": {
    "plain": {
      "counter_io_exp_in1": 0,
      "counter_io_exp_in2": 0,
      "counter_io_exp_in3": 0,
      "counter_io_exp_in4": 0,
      "counter_io_exp_out1": 0,
      "counter_io_exp_out1_short": 0,
      "counter_io_exp_out2": 0,
      "counter_io_exp_out2_short": 0,
      "counter_io_exp_out3": 0,
      "counter_io_exp_out3_short": 0,
      "counter_io_exp_out4": 0,
      "counter_io_exp_out4_short": 0,
      "counter_io_in1": 0,
      "counter_io_in2": 0,
      "counter_io_in3": 0,
      "counter_io_out1": 29129.0,
      "counter_io_out2": 0,
      "dev_dist": 123889,
      "dev_idle": 2665418,
      "dev_ign": 2692667,
      "dev_orpm": 0,
      "dev_ospeed": 3286
    },
    "calculated": {},
    "counters": {}
  }
}
```

> Updating the global vehicle dev_dist (odometer) counter for vehicle 1716

> `PUT` [api/vehicles/197/counters](https://cloud.pegasusgateway.com/api/vehicles/197/counters)

```json
{
  "counter": "dev_dist",
  "value": 1203942
}
```

You can view a vehicles counters by inspecting the vehicle

`GET /api/vehicles/:vid/counters`

### Updating Vehicle Counters

Let's say you want to edit the vehicle's distance counter to match the odometer on your car, simply make a 

`PUT /api/vehicles/:vid/counters`

where the body of the request is the name of the `counter` and `value`

<code>
{<br>
&nbsp;&nbsp;"counter": "dev_dist",<br>
&nbsp;&nbsp;"value": 123456789<br>
}
</code>

Note that once you make this change the device will have to report another event in order for the change to be reflected in the /counters API. 

This change will only update the rawdata `vehicle_`'s counters (in this case `vehicle_dev_dist` to  123,456,789 meters), the raw `dev_`, `ecu_`, `counter_io` counters (`dev_dist`, `dev_idle`, `counter_io_out1`, etc.) are not changed and these will remain the same.

So to summarize when you edit a vehicle's counter using the /counters api you will trigger an update on the equivalent `vehicle_`'s counter in the rawdata api, and it will start incrementing from that point forward.

counter | value units (these units are fixed)
-------:|-------
`dev_dist` | meters
`ecu_dist` | meters
`dev_ign` | seconds
`dev_idle` | seconds
`dev_ospeed` | seconds
`ecu_ifuel` | liters
`ecu_eusage` | seconds
`ecu_eidle` | seconds
`ecu_tfuel` | liters
`counter_tamper` | seconds
`counter_io_in1` | seconds
`counter_io_in2` | seconds
`counter_io_in3` | seconds
`counter_io_out2` | seconds
`counter_io_out1` | seconds
`counter_io_exp_in1` | seconds
`counter_io_exp_in2` | seconds
`counter_io_exp_in3` | seconds
`counter_io_exp_in4` | seconds
`counter_io_exp_out1` | seconds
`counter_io_exp_out2` | seconds
`counter_io_exp_out3` | seconds
`counter_io_exp_out4` | seconds
`counter_io_exp_out1_short` | seconds
`counter_io_exp_out2_short` | seconds
`counter_io_exp_out3_short` | seconds
`counter_io_exp_out4_short` | seconds

<aside class="notice">
Only gateway administrators and those with "w" scopes on "vehicles.counters" can modify the shared global counters.
</aside>

## Shared User Counters

> Get the shared user counters for a particular vehicle
> [api/vehicles/197/counters](https://cloud.pegasusgateway.com/api/vehicles/197/counters)

```json
{
  "user": [
    {
      "description": "Time since new alternator",
      "set_at": 1474660977.65856,
      "counter": "dev_ign",
      "value": 19529142,
      "offset": 9690591,
      "id": 0
    }
  ],
  "vehicle": {...}
}
```

Shared user counters are counters created by users that anyone with visibility to the vehicle can see.

You can create as many of these counters are you'd like for the same vehicle.

These counters are only persisted in the /counters api, and are not available in the /rawdata api.

To see the counters you have defined you can use:

`GET /api/vehicles/:id/counters`

These counters exist only in this API, the idea here is that once you create them you can query this API again at any point to see the new values for these shared user counters.

### Create 

> Create a shared counter that starts counting distance starting at 123 from the moment it's created<br>
> `POST` [api/vehicles/197/counters](https://cloud.pegasusgateway.com/api/vehicles/197/counters)

```json
{
  "counter": "dev_dist",
  "initial_value": "123",
  "description": "Some description"
}
```

Now let's create a shared user counter for those new tires you just changed

### Updating Shared User Counters

In order to update a shared user counter you create, you can make a PUT request to the specific counter id:
`PUT /api/vehicles/:vid/counters/:id`

the body of the request is the name of the `counter`, `description`, `initial_value`

<code>
{<br>
&nbsp;&nbsp;"counter": "dev_dist",<br>
&nbsp;&nbsp;"description": "front tires",<br>
&nbsp;&nbsp;"initial_value": 0<br>
}
</code>

These changes apply to that particular vehicle for the user that changed.
