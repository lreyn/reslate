# Remote

# Realtime

```node
//Using the request module
var request = require("request");

var options = { method: 'POST',
  url: 'http://yourlistener.com/path/to/receiver',
  headers: 
   { 
     'content-type': 'application/json' 
   },
  body: 
   [ { io_pwr: true,
       ac: 2,
       code: 1,
       type: 10,
       vid: 1392,
       cf_lac: 103,
       ip: '208.131.186.63',
       port: 26052,
       pid: 110,
       al: 521,
       vo: 6356212,
       io_ign: true,
       event_time: '2016-01-12T19:40:31+00:00',
       source: 3,
       lon: -77.32573,
       io_exp_state: false,
       id: 4348333385,
       cv10: 580,
       cv11: 0,
       cv12: 268,
       cv13: 0,
       head: 315,
       vdop: 83,
       hdop: 97,
       bl: 4394,
       mph: 28,
       valid_position: true,
       lat: 18.08918,
       pdop: 128,
       device_id: 357666050758579,
       system_time: '2016-01-12T19:40:32.799671+00:00',
       cf_rssi: '10',
       age: 2,
       sv: 9,
       io_out1: false,
       io_out2: false,
       io_in1: false,
       io_in2: false,
       io_in3: false } ],
  json: true };

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

<script language="JavaScript">
  function autoResize(id){     
    var newheight;     
    var newwidth;     
      if(document.getElementById){         
        newheight = document.getElementById(id).contentWindow.document .body.scrollHeight;         
        newwidth = document.getElementById(id).contentWindow.document .body.scrollWidth;     
      }     
      document.getElementById(id).height = (newheight) + "px";     
      document.getElementById(id).width = (newwidth) + "px"; 
    }
</script>

<iframe id="rawdatatutorial" style="max-width: 60%; height: 1290px; border: none;" src="https://docs.google.com/document/d/1WiLfTHyOofOb3CQkz2xHLqpl_kV4cuNRYdt6no7XeR0/pub" width="100%" height="1290px" frameborder="0" marginheight="0"></iframe>

# Data Upload API

Data can be uploaded to Pegasus via an HTTP POST request. 

The data enclosed in the body of the HTTP POST should be an array of JSON objects where each object corresponds to an entity/telemetry's event.

For example, position or temperature related information from other telemetry devices for analysis in the API & visualization in the Pegasus application can be sent in the HTTP POST.

<aside class="info">Only arrays of length up to 50 or less are accepted.
  <br> 
  <code> 
  [{"event_0": "value_0"},{"event_1": "value_1"},...{"event_49": "value_49"}] 
  </code>
</aside>

The only keys required in the JSON object are:

key | info | type 
-----:|------|------
device_id | IMEI of the device (15 digit) | number
event_time* | ISOFORMAT UTC timestamp (YYYY-MM-DDThh:mm:ss) | string

*If the <b>event_time</b> is not provided we'll use an epoch timestamp from the server the moment the event was received.*

If you are uploading telemetry data to the gateway from other tracking devices, it's extremely useful to use a [label](#labels) for the events sent.
Labels are short strings that represent the action that took place on the device, for example if the Ignition of the device was detected ON, send `"label": "ignon"`

For a small list of common labels please see: [common labels](#common-labels)

For a list of all the keys that can be forwarded see: [master field list](#master-fields-list)

Note that the device that you're uploading data for needs to exist on the gateway prior to the data upload.

Devices can only be added by [DCT Support](mailto:support@digitalcomtech.com).

### Uploading data

> POST https://api.pegasusgateway.com/receiver

> Uploading temperature data

> note there's no timestamp on this message, thus an epoch timestamp from the server will be used once the event is received

```json
[
  {
    "device_id": 357000000000009,
    "temp": 23.4
  }
]
```

> Uploading telemetry data 

```json
[
  {
    "device_id": 357000000000009,
    "event_time": "2019-01-07T12:00:00",
    "lat": 20.993852,
    "lon": -89.710796,
    "mph": 10,
    "head": 342,
    "al": 10,
    "label": "ignon"
  }
]
```

Data is uploaded to the same endpoint: [https://api.pegasusgateway.com/receiver](https://api.pegasusgateway.com/receiver) 

Simply add a [token](#authentication) from a valid session as an HTTP header, along with `Content-Type`

header | value
------|------------
Content-Type | application/json
Authenticate | INSERT_VALID_TOKEN

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

### Visualizing the data

> POST [api/vehicles](https://cloud.pegasusgateway.com/api/vehicles)

> make sure prior to creating the vehicle the device exists on the gateway [/devices](https://cloud.pegasusgateway.com/api/devices/357000000000009) api

```json
{
  "name": "Temperature sensor", 
  "device": 357000000000009
}
```

> Response

```json
{
    "info": {},
    "associations": [],
    "__updated": 1546889912.079602,
    "associated_at": 1546889911.9000001,
    "name": "Temperature sensor",
    "primary": null,
    "properties": {},
    "images": {},
    "token": null,
    "groups": [],
    "device": 357000000000009,
    "configuration": "0000",
    "type": "vehicle",
    "id": 3263
}
```

> Requesting data for vehicle ID: 3263

> GET [/api/rawdata?vehicles=3263&fields=$basic&duration=P1D](https://cloud.pegasusgateway.com/api/rawdata?vehicles=3263&fields=$basic&duration=P1D)

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
      "device_id": 357000000000009,
      "event_time": "2019-01-07T12:00:00",
      "system_time": "2019-01-07T19:38:31.91234",
      "lat": 20.993852,
      "lon": -89.710796,
      "mph": 10,
      "head": 342,
      "al": 10,
      "label": "ignon"
    }
  ]
}
```


In order to visualize the data that was uploaded, you'll need to associate the device to a vehicle ID. 

To do this [create a vehicle](https://cloud.pegasusgateway.com/api-static/docs/#api-Vehicle-CreateVehicle) with the correct `device_id`.


Once the data is uploaded it can be explored with the corresponding vehicle ID that's associated to the device.


&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>





# Wireless

> Get wireless sims
> GET [api/m2m/wireless/v1/Sims](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims)

* `/api/m2m/wireless/v1/Sims` which is exclusively for the management of DCT's wireless SIMs

* `sid` - Wireless sim unique id, example: DE5787d29324d0fa1a6b4748046f16c38d

### Wireless SIMs

With the wireless SIMs API you are able to activate and suspend, view the consumption, and send SMS commands to the devices.

Each wireless SIM features:
* Connectivity: 2G/3G/LTE
* Services: Data
* Roaming: US and International
* Data limits: managed per 30 days by the rate plan

#### View Wireless SIMs

> Get particular sim 
> `sid` Wireless Sim ID
> GET [/api/m2m/wireless/v1/Sims/:sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d)

```json
{
    "unique_name": "cloud Syrus OBDII Demo 9",
    "date_updated": "2020-03-28T17:03:37Z",
    "voice_url": null,
    "iccid": "8901260852291475879",
    "reset_status": null,
    "voice_fallback_method": "POST",
    "sid": "DE5787d29324d0fa1a6b4748046f16c38d",
    "voice_fallback_url": null,
    "status": "active",
    "e_id": null,
    "commands_callback_method": "POST",
    "rate_plan_sid": "WPc655a43a634490ad84d3df4e3ff30edd",
    "sms_url": null,
    "ip_address": null,
    "voice_method": "POST",
    "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d",
    "friendly_name": null,
    "sms_fallback_url": null,
    "account_sid": "AC5e3bed6b465b3f2a60b2cf4f21474e5a",
    "sms_method": "POST",
    "sms_fallback_method": "POST",
    "date_created": "2018-04-23T22:01:34Z",
    "commands_callback_url": null,
    "links": {
        "data_sessions": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/DataSessions",
        "usage_records": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/UsageRecords",
        "rate_plan": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/RatePlans/WPc655a43a634490ad84d3df4e3ff30edd"
    }
}
```

> Get a wireless sim via the Iccid
> `iccid` 20 digit SIM ID number
> GET [/api/m2m/wireless/v1/Sims?Iccid=:iccid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims?Iccid=8901260852291475879)

```json
{
    "sims": [
        {
            "unique_name": "cloud Syrus OBDII Demo 9",
            "date_updated": "2020-03-28T17:03:37Z",
            "voice_url": null,
            "iccid": "8901260852291475879",
            "reset_status": null,
            "voice_fallback_method": "POST",
            "sid": "DE5787d29324d0fa1a6b4748046f16c38d",
            "voice_fallback_url": null,
            "status": "active",
            "e_id": null,
            "commands_callback_method": "POST",
            "rate_plan_sid": "WPc655a43a634490ad84d3df4e3ff30edd",
            "sms_url": null,
            "ip_address": null,
            "voice_method": "POST",
            "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d",
            "friendly_name": null,
            "sms_fallback_url": null,
            "account_sid": "AC5e3bed6b465b3f2a60b2cf4f21474e5a",
            "sms_method": "POST",
            "sms_fallback_method": "POST",
            "date_created": "2018-04-23T22:01:34Z",
            "commands_callback_url": null,
            "links": {
                "data_sessions": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/DataSessions",
                "usage_records": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/UsageRecords",
                "rate_plan": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/RatePlans/WPc655a43a634490ad84d3df4e3ff30edd"
            }
        }
    ],
    "meta": {
        "page": 0,
        "page_size": 50,
        "first_page_url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims?Iccid=8901260852291475879&PageSize=50&Page=0",
        "previous_page_url": null,
        "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims?Iccid=8901260852291475879&PageSize=50&Page=0",
        "next_page_url": null,
        "key": "sims"
    }
}
```

To view a particular wireless SIM you can use the `/api/m2m/wireless/v1/Sims/:sim_sid` or the SIMs ICCID `/api/m2m/wireless/v1/Sims?Iccid=:iccid`


#### Rate plans

> View rate plans
> [api/m2m/wireless/v1/RatePlans](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/RatePlans)

> View particular rate plan
> [/api/m2m/wireless/v1/RatePlans/:rate_plan_sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/RatePlans/WP1d1e35ae4daa3f2fd6c57d2117fb6979)
> notice how it has a `data_limit` of 10MB

```json
{
    "rate_plans": [
        {
            "voice_enabled": false,
            "unique_name": "10MB PAYG US/INTL-ROAMING + SMS",
            "international_roaming_data_limit": 10,
            "data_limit": 10,
            "data_metering": "payg",
            "date_updated": "2019-11-06T19:15:21Z",
            "friendly_name": null,
            "account_sid": "AC5e3bed6b465b3f2a60b2cf4f21474e5a",
            "date_created": "2019-11-06T19:15:21Z",
            "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/RatePlans/WP1d1e35ae4daa3f2fd6c57d2117fb6979",
            "messaging_enabled": true,
            "data_enabled": true,
            "sid": "WP1d1e35ae4daa3f2fd6c57d2117fb6979",
            "international_roaming": [
                "data",
                "messaging"
            ],
            "national_roaming_data_limit": 10,
            "national_roaming_enabled": true
        }
    ]
}
```

> Change the rate plan/data limit of a SIM
> POST [/api/m2m/wireless/v1/Sims/:sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d)

```json
{
  "RatePlan": "WP8c317c6831cf8cbc311d776b2e1ace2f"
}
```

The data limits is controlled by the *Rate plan* that the SIM belongs to. 
A default rate plan of 10MB is set on the SIMs, but they can moved to a different rate plan at will. 

If you need additional rate plans please email [m2m@digitalcomtech.com](mailto:m2m@digitalcomtech.com) requesting the new rate plan limit.

Once a SIM is associated to a rate plan it begins 30 day cycles to measure the limit consumption.
If the SIM does not hit the limit on the rate plan in 30 days, it will reset and start at 0 again.
If the SIM does reach it's rate plan limit in 30 days, the state of the SIM will still stay active but the SIM itself will not be able to transmit any data until the 30 day period is over. 

<aside class="notice">Note that while the SIM is not able to transmit data, the device is accumulating it internally on it's memory and could download a large amount of data when the SIM comes back online.</aside>

#### Activate/Suspend

> Suspend a SIM
> POST [/api/m2m/wireless/v1/Sims/:sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d)

```json
{
  "Status": "suspended"
}
```

> Reactivate a SIM
> POST [/api/m2m/wireless/v1/Sims/:sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d)

```json
{
  "Status": "active"
}
```

You can change the state of the SIM to any of the following values: `ready`, `active`, `suspended`, or `deactivated`.

The following table describes the possible statuses of the SIM, some of these are temporary while the SIM changes states:

Status | Name | Description |
-------|-------|-------------|
new	 | Inventory | Default state, think of it as it's stored in inventory, no charge is made to this SIM, once it's moved to *ready* or *active* it cannot be new 
ready	| Ready to Activate | The SIM can connect to the network and is capable of consuming network resources in accordance with its Rate Plan, but no monthly fee will be charged. Once the SIM has consumed 250KB of data, or three months have passed, the SIM will transition automatically to active status. On the fifth Command sent to_sim or from_sim, the device will automatically become active. 
active | Active |	The SIM can connect to the network and is capable of consuming network resources in accordance with its Rate Plan.
suspended | Suspended |	The SIM is blocked from connecting to the network. 
deactivated | Deactivated |	The SIM is blocked from connecting to the network. After 72 hours, the SIM will transition automatically to the canceled status.
canceled | Retired | Terminal status. The SIM is blocked from connecting to the network and can never be reactivated.
scheduled	| Scheduled to Change | The network operator is waiting to receive the instruction to change it's status, the SIM may remain in this state for some time, it depends on the network operator's availability. The status will be automatically updated to the requested status when the network operator resumes accepting transactions.
updating | Updating |	The SIM is in the process of being updated.


#### Send SMS

> Send >QVR< to Wireless SIM
> POST [/api/m2m/wireless/v1/Commands](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Commands)

```json
{
    "Sim": "DE5787d29324d0fa1a6b4748046f16c38d",
    "Command": ">QVR<"
}
```

> View SMS commands sent to a SIM
> GET [/api/m2m/wireless/v1/Commands?Sim=:Sim_sid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Commands?Sim=DE5787d29324d0fa1a6b4748046f16c38d)
> GET [/api/m2m/wireless/v1/Commands?Iccid=:iccid](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Commands?Iccid=8901260852291475879)

```json
{
    "commands": [
        {
            "status": "received",
            "direction": "from_sim",
            "date_updated": "2020-02-28T16:48:52Z",
            "account_sid": "AC5e3bed6b465b3f2a60b2cf4f21474e5a",
            "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Commands/DCdfe81b30b8fc720e07aa3e799ea7617a",
            "command": "dns success!",
            "delivery_receipt_requested": true,
            "sid": "DCdfe81b30b8fc720e07aa3e799ea7617a",
            "date_created": "2020-02-28T15:36:23Z",
            "sim_sid": "DE0fc1f8187e01fdfe9589eed898ee9b9b",
            "transport": "sms",
            "command_mode": "text"
        }
    ]
}
```


To send commands you can use the `/api/m2m/wireless/v1/Commmands` api, just pass the Wireless Sim ID `sid` from `/api/m2m/wireless/v1/Sims` and the `Command` you want to send.

#### View consumption

> View SIMs Usage
> GET [/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/UsageRecords](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE41f2289340c8abc104150758549d8f22/UsageRecords)

```json
{
    "usage_records": [
        {
            "commands": {
                "billing_units": "USD",
                "from_sim": null,
                "to_sim": null,
                "national_roaming": {
                    "billing_units": "USD",
                    "billed": 0,
                    "total": 0,
                    "from_sim": 0,
                    "to_sim": 0
                },
                "home": {
                    "billing_units": "USD",
                    "billed": 0,
                    "total": 0,
                    "from_sim": 0,
                    "to_sim": 0
                },
                "international_roaming": [],
                "billed": 0,
                "total": null
            },
            "sim_sid": "DE41f2289340c8abc104150758549d8f22",
            "data": {
                "billing_units": "USD",
                "upload": 2268417,
                "download": 579158,
                "national_roaming": {
                    "billing_units": "USD",
                    "upload": 0,
                    "download": 0,
                    "units": "bytes",
                    "billed": 0,
                    "total": 0
                },
                "home": {
                    "billing_units": "USD",
                    "upload": 0,
                    "download": 0,
                    "units": "bytes",
                    "billed": 0,
                    "total": 0
                },
                "units": "bytes",
                "international_roaming": [
                    {
                        "billing_units": "USD",
                        "upload": 2268417,
                        "download": 579158,
                        "country_code": "GTM",
                        "units": "bytes",
                        "billed": 0,
                        "total": 2847575
                    }
                ],
                "billed": 0.0975,
                "total": 2847575
            },
            "period": {
                "start": "2020-03-02T00:00:00Z",
                "end": "2020-04-03T00:00:00Z"
            },
            "account_sid": "AC5e3bed6b465b3f2a60b2cf4f21474e5a"
        }
    ],
    "meta": {
        "page": 0,
        "page_size": 50,
        "first_page_url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE41f2289340c8abc104150758549d8f22/UsageRecords?PageSize=50&Page=0",
        "previous_page_url": null,
        "url": "https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE41f2289340c8abc104150758549d8f22/UsageRecords?PageSize=50&Page=0",
        "next_page_url": null,
        "key": "usage_records"
    }
}
```

> View SIMs Usage for particular time period
> GET [/api/m2m/wireless/v1/Sims/DE5787d29324d0fa1a6b4748046f16c38d/UsageRecords?Start=2020-04-01T04:00:00Z&End=2020-04-02T23:59:59Z&Granularity=daily](https://cloud.pegasusgateway.com/api/m2m/wireless/v1/Sims/DE41f2289340c8abc104150758549d8f22/UsageRecords)

To view the consumption of the SIM you can use the `UsageRecords` api