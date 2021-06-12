# Information

General information about the API

## HTTP
### Verbs

verb | meaning
----:|:-------
GET | Used for retrieving resources.
POST | Used for creating resources, sending-info, actions
PUT | Used for replacing/updating resources.
DELETE | Used for deleting resources.

### Status

code | description | meaning
----:| ---------- | ----------
200 | OK | General success status code.
201 | CREATED | Entity has been created (POST request successful). Response body content may or may not be present.
204 | NO CONTENT | Status when wrapped responses are not used and nothing is in the body (e.g. DELETE).
304 | NOT MODIFIED | Used in response to conditional GET calls to reduce bandwidth usage.
400 | BAD REQUEST | General error when fulfilling the request would cause an invalid state. Domain validation errors, missing data, etc.
401| UNAUTHORIZED | User is not authenticated with Gateway, does not have the required scopes, or does not share groups with requested entity
403| FORBIDDEN | User is not allowed to perform operation, regardless of scopes or admin level.
404 | NOT FOUND | Used when the requested resource is not found, whether it doesn't exist or if there was a 401 or 403 that, for security reasons, the service wants to mask.
409| CONFLICT | Whenever a resource conflict would be caused by fulfilling the request. Duplicate entries, deleting root objects when cascade-delete not supported are a couple of examples.
429 | TOO MANY REQUESTS | Too many requests in a given amount of time, both global limits as well as per-resource and per-user limits apply. See [rates](#rates).
500 | INTERNAL SERVER ERROR | Catch-all error when the server-side throws an exception.

## Rates

The API handles limits to the different resources on a per user and global rate for the whole site:

[Rawdata](#rawdata) & [Counters](#counters):

* global:  `60/minute; 800/hour`
* per user: `3/second; 30/minute; 500/hour`

[Trips](#trips): 

* global: `120/minute, 1600/hour`
* per user: `6/second, 30/minute, 500/hour`

[Trips](#trips): 

* global: `120/minute, 1600/hour`
* per user: `6/second, 30/minute, 500/hour`

[Geofences](#geofences):

* global: `1000/minute`
* per user: `100/minute`

[Geofence Types](#geofence-types)

* global: `240/minute`
* per user: `80/minute`

[Reversegeo](#reverse-geocoding)

* global: `1200/minute, 15000/hour`
* per user: `200/minute, 4200/hour`

Other resources such as vehicles, groups, assets, etc. apply an anti DDOS (Distibuted Denial of Service) limit, which would only be reached if various machines spam the API.

Whenever you hit the limit the API returns a [429 Status Code](#status). At which point it is important that your app suspends more subsequent requests to the API until the limit is reset.

<aside class="error"> If your app continues to make requests after it hits a 429 the API will eventually block the originating IP address.</aside>

```json
{
  "reason": "vehicles",
  "message": "Rate exceeded",
  "limit": 10,
  "id": "ec2ec6e51a42f40d12e7009787d833b8",
  "value": 11.0455
}
```

```shell
> HTTP/1.1 429 TOO MANY REQUESTS
> Connection: keep-alive
> Content-Length: 135
> Content-Type: application/json
> Date: Sun, 30 Sep 2018 16:45:13 GMT
> Server: nginx
> X-Peg-Id: 1
> X-Peg-Server: F
> X-Peg-User-Id: 54
> X-RateLimit-Limit: 10
> X-RateLimit-Remaining: 0
> X-RateLimit-Reset: 1538325923


{reason: "vehicles", message: "Rate exceeded", limit: 10, id: "ec2ec6e51a42f40d12e7009787d833b8",…}
```

The API returns some response headers which include `X-RateLimit-*` keys so you know when it's safe to make requests again.

The response on the right hand side tells us there's a limit of 10 requests, and the client has 0 remaining, the limit resets back to 10 at the following epoch timestamp: 1445098790 - Sun Sep 30 2018 14:45:23 GMT

field | description
-----:|------------
reason | resource that reached limit
message | description
limit | amount of request allowed
id | internal use
value | rate of requests

header | description
------:|-------------
X-RateLimit-Limit | resource limit
X-RateLimit-Remaining | amount of requests that remain
X-RateLimit-Reset | when it's safe to make more requests

<aside class="notice">If your application is on the verge or is constantly hitting the limits you can checkout the <a href="#live-communications">Live Communication</a> section which allows you to subscribe to the vehicle's data in realtime or <a href="#forwarders">Forwarders</a> which allows you to receive all the vehicle's data in a JSON format to any external endpoint.</aside>

## Units

Unless otherwise specified, the API handles all units for keys in the following manner by default.

Metric | Unit
---- | ----
Distance | __meter__
Volume | __liter__
Time | __second__
Temperature | __Celsius__

Speed and acceleration units vary per resource, but will normally take the form of 

- distance_unit / time_unit
- distance_unit / time_unit / second

The API's of [Rawdata](#rawdata), [Counters](#counters) and [Trips](#trips) support unit paramaters, which can modify the output to the desired units.

### Allowed values

The Gateway allows you to specify multiple values for each unit of measurement.

Metric | Allowed values
---- | ----
Distance | meter, km, mile
Volume | liter, gallon
Time | second, minute, hour
Temperature | C, F

You may also pass `user` for either the `distance or volume` parameters, where applicable. The API will return the values in accordance with the users preference.

<aside class="warning">
Be sure to read each API's unit handling.
</aside>

### Speed

Where applicable, the speed is determined by the distance and time units (default: meter/second)

The Counters and Trips API's allows you to override this unit, by specifying one of `kph , mph or user`, where `user` falls back to either `kph or mph` based on the users distance preference (mile or km)

### Acceleration

The acceleration field is always the speed units, per second

- meter/second/second
- mile/hour/second.

## Timezone

> List of available time zones <br>
> [api/tz](https://pegasus1.pegasusgateway.com/api/tz)

```shell
curl https://pegasus1.pegasusgateway.com/api/tz? \
    --header 'Authenticate: 99ff984fbe2c90603da515cdb193fda3146c9c4c9a347bcf062b0760'

{"server_utc_epoch":1538478876.558243,"allowed":["Africa/Abidjan","Africa/Accra","Africa/Addis_Ababa","Africa/Algiers","Africa/Asmara","Africa/Bamako","Africa/Bangui","Africa/Banjul",...]}
```

The API by default returns all timestamps in UTC (GMT+0) 

You can change the timestamp by passing a URL param `tz` or an HTTP header value with: 
`"X-Time-Zone" : "time_zone_value"`

`&tz=America/New_York`

`"X-Time-Zone" : "America/New_York"`

> URL param 

> [api/rawdata?vehicles=2600&fields=$basic&duration=P1D&tz=America/New_York](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=2600&fields=$basic&duration=P1D&tz=America/New_York)

```json
{
  "units": {
    "volume": "liter",
    "distance": "meter",
    "speed": "mph",
    "time": "second"
  },
  "events": [
    {
      "head": 291,
      "code": 0,
      "hdop": 0.82,
      "event_time": "2018-10-02T09:20:47",
      "type": 10,
      "vid": 2600,
      "lon": -72.50346,
      "sv": 9,
      "mph": 0,
      "label": "prdtst",
      "valid_position": true,
      "lat": 7.94937,
      "system_time": "2018-10-02T09:20:50.505475",
      "speed": 0,
      "id": 260012351115505,
      "device_id": 356612022409637
    }
  ]
}
```    

> HTTP Header

```shell
curl 'https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=2600&fields=$basic&duration=PT1H' \
    --header 'X-Time-Zone: America/New_York' \
    --header 'Authenticate: 99ff984fbe2c90603da515cdb193fda3146c9c4c9a347bcf062b0760'

{"units": {"volume": "liter", "distance": "meter", "speed": "mph", "time": "second"}, "events": [{"head": 291, "code": 0, "hdop": 0.81999999999999995, "event_time": "2018-10-02T09:20:47", "type": 10, "vid": 2600, "lon": -72.503460000000004, "sv": 9, "mph": 0, "label": "prdtst", "valid_position": true, "lat": 7.94937, "system_time": "2018-10-02T09:20:50.505475", "speed": 0, "id": 260012351115505.0, "device_id": 356612022409637}]}
```
