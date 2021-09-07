# Data Receivers

Pegasus allows you to upload custom device data via an HTTP POST method. For example, position related information from other telemetry devices for analysis in the Pegasus API & visualization in the Pegasus Application.

Data can be uploaded and associated to an entity type [vehicle](#vehicles) or to an entity type: [asset](#assets).

## Vehicles

Read the following instructionss on how to get started uploading data to your Pegasus Gateway from third party platforms/devices - [documentation for Gateway administrators](https://drive.google.com/open?id=1C5VIos3IKzb30iGwXy8dn1OukIPePL5E) / [documentation for end user/developer](https://drive.google.com/open?id=1Vz65R6U6J9ZfvB9j_EJeMnAJ7YusQd-U)

### Intro

In order to start receiving data from third party devices via an HTTP POST, you'll need to:

* Step 1: Activate the JSON receiver for your Pegasus Gateway
* Step 2: Add the device IMEI that you'll be sending data from to the API
* Step 3: Create an application token

For the first 2 steps you can contact your [account representative](mailto:sales@digitalcomtech.com). 

For the last step you can use the Webservices section of Pegasus Gateway or see the [tokens api](#data-receiver-tokens) section to create data receiver tokens.

## Assets

### Authentication

> POST [/api/assets](https://cloud.pegasusgateway.com/api/assets)

> creating an asset

```json
{
  "first_name": "name of asset",
  "type": "tracking_device"
}
```

> 200 OK (id: 3315)

```json
{
  "info": {},
  "__updated": 1553211859.1255651,
  "name": "name of asset ",
  "ibuttons": [],
  "__created": 1553211859.1255569,
  "fingerprints": [],
  "primary": null,
  "properties": {},
  "images": {},
  "token": null,
  "tracker": {
    "active": false,
    "asset_id": 3315,
    "token": null
  },
  "groups": [],
  "device": null,
  "rfids": [],
  "type": "asset",
  "id": 3315,
  "counters": null
}
```

> enabling the Taurus tracker code

> PUT [/api/assets/3315](https://cloud.pegasusgateway.com/api/assets/3315)

```json
{
  "tracker_mode": true
}
```

> 200 OK

```json
{
  ...
  "tracker": {
    "asset_id": 3315,
    "site_id": 1,
    "site_url": "cloud.pegasusgateway.com",
    "active": true,
    "token": "UTNZEHBL",
    "device": 450000141115142
  },
  ...
}
```

> setting the configuration to the device

> POST [/api/device-config/450000141115142](https://cloud.pegasusgateway.com/api/device-config/450000159088405)

```json
{
  "ky": "s000"
}
```

> 200 OK

```json
{
  "kydef": {},
  "kymod": {},
  "total_count": 19,
  "_config_state": 1,
  "_epoch": 1553253405.348958,
  "ky": "s000",
  "devconfig_id": 9188,
  "pending": true
}
```

To get started we'll need to generate a token that we'll use to reference the asset that we want to populate data with on Pegasus Gateway.
In order to create this token you'll have to create a new asset via the POST [/assets](https://cloud.pegasusgateway.com/api-static/docs/#api-Assets-CreateAsset)
here you can populate the asset with the included parameters such as `first_name`, `last_name`, `email`, `type` (type is required), etc.

After you create the asset you'll need to activate a token for it, to do this make a PUT [/assets](https://cloud.pegasusgateway.com/api-static/docs/#api-Assets-UpdateAsset) with `tracker_mode: true`.

This will generate a unique `token` for that particular asset that we can use to POST data to. Example token: UTNZEHBL

To prepare the asset to receive data we have to assign the device associated to this asset a configuration. This can be done with POST `/api/device-config/:imei` sending a key `ky` with the value `s000` 

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
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

### Upload data

> POST https://api.pegasusgateway.com/assets/event?tracker=UTNZEHBL

> note that the POST is always made to 'api.pegasusgateway.com'

> using server time

```json
{
  "latitude": 20.993852,
  "longitude": -89.710796,
  "mph": 10,
  "heading": 342,
  "altitude": 10,
  "label": "ignon",
  "use_server_time": true
}
```

> 200 OK

```json
{
  "message": "event processed successfully"
}
```

> using an epoch

```json
{
  "latitude": 21.993852,
  "longitude": -90.710796,
  "mph": 99,
  "heading": 193,
  "altitude": 200,
  "label": "ignoff",
  "use_server_time": false,
  "epoch": 1553187370
}
```


In order to publish telemetry data to Pegasus server, send POST request to the following URL:
`https://api.pegasusgateway.com/assets/event?tracker=:token`

replacing `:token` with the token generated previously.

The only keys the upload API is accepting at the moment are:

param | type | description
------|------|------------
latitude | number | WGS84 latitude
longitude | number | WGS84 longitude
mph | number | speed in mph
heading | number | heading in degrees 0-359 0=north, 90=east, etc.
altitude | number | device altitude in meters
[label] | string | event label (if not specified a `trckpnt` is automatically assigned) see list of labels [below](#common-labels)
use_server_time | boolean | true to use a server timestamp for the incoming data

### Common labels

label | description
-----:|------------
prdtst| Periodic report with ignition OFF
trckpnt| Periodic report with Ignition ON (default if no label is set)
ignon | Ignition was turned ON
ignoff| Ignition was turned OFF
panic | report when a panic button is pressed
pwrloss| Main power was lost
pwrrstd| Main power was restored
lwbatt | Low internal device battery
spd | Speeding
idl | Idling
stt | Device motion
stp | Device stopped moving
in1on | Input 1 ON
in1off | Input 1 OFF
in2on | Input 2 ON
in2off | Input 2 OFF
in3on | Input 3 ON
in3off | Input 3 OFF

For a full list of the core labels please check out the core labels api: [`api/core_labels`](https://cloud.pegasusgateway.com/api/core_labels?auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

Please note that if the `use_server_time` is not set, the server-side timestamp will be assigned to uploaded data!

In case your device is able to get the client-side timestamp, you can use following format:

{"epoch":1532452713}

In the example above, we assume that “1532452713” is a unix timestamp with seconds precision. ‘1532452713’ corresponds to ‘Tuesday, July 24, 2018 5:18:33 PM GMT’
