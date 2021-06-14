# Accessories

Syrus has a lot of [accessories](https://drive.google.com/open?id=17Ux1G-qs_Qe9mRIOSm7tC0x0GwTzTFng) that can interface and work simultaneously with one another, below you will find the accessories integration with the API.
If you are looking for installation guides and more general information about the accessories please visit our [support site](https://support.digitalcomtech.com).


## ADAS

> Daily event count for Speeding, Left Turn Signal, Right Turn Signal and Headway Warning

> [api/rawdata?vehicles=1477&from=2016-10-15&to=2016-10-18T23:59:59&fields=$basic,count:1&labels=mblyspd,mblylftsig,mblyrghsig,mblyhdwrn,mblypddng&resample=event_time&freq=1D&how=count:sum&group_by=label](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=1477&from=2016-10-15&to=2016-10-18T23:59:59&fields=$basic,count:1&labels=mblyspd,mblylftsig,mblyrghsig,mblyhdwrn,mblypddng&resample=event_time&freq=1D&how=count:sum&group_by=label)

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
      "count": 5,
      "event_time": "2016-10-16T00:00:00",
      "label": "mblyhdwrn"
    },
    {
      "count": 11,
      "event_time": "2016-10-16T00:00:00",
      "label": "mblylftsig"
    },
    {
      "count": 4,
      "event_time": "2016-10-16T00:00:00",
      "label": "mblypddng"
    },
    {
      "count": 11,
      "event_time": "2016-10-16T00:00:00",
      "label": "mblyrghsig"
    },
    {
      "count": 146,
      "event_time": "2016-10-16T00:00:00",
      "label": "mblyspd"
    }
  ]
}
```

Syrus is compatible with the following Advanced Driver Assistance Systems (ADAS) accessories:

* Movon, which integrates via the devices RS-232 cables (RX/TX)
* Mobileye™, which integrates via the ECU Monitor accessory

The following table describes the available signals per accessory, these signals generate a label which is found in the standard configuration of each accessory.

* ky: q712 Movon Standard Configuration
* ky: p657 Mobileye Standard Configuration

Syrus signal | Label | Short Description | Mobileye™ | Movon |
------------:|------:|:------------------|:---------:|:-----:|
L01 | mblyerr | Error | ✔️ | ✔️ |
L02 | mblylldw | Left Lane Departure Warning | ✔️ | ✔️ |
L03 | mblyrldw | Right Lane Departure Warning | ✔️ | ✔️ |
L04 | mblyfcw | Forward Collision Warning | ✔️ | ✔️ |
L05 | mblymaint | Maintenance Flag | ✔️ |   |
L06 | mblyflsf | Failsafe | ✔️ |   |
L07 | mblypdfcw | Pedestrian Forward Collision Warning | ✔️ | ✔️ |
L08 | mblypddng | Pedestrian in Danger Zone | ✔️ |   |
L09 | mblytmpr | Tamper alert | ✔️ |   | 
L10 | mblyspd | Speeding | ✔️ |   |
L11 | mblyhdwrn | Headway Warning | ✔️ | ✔️ |
L12 | mblybrkon | Brakes on | ✔️ | ✔️ |
L13 | mblylftsig | Left turn signal | ✔️ | ✔️ |
L14 | mblyrghsig | Right turn signal | ✔️ | ✔️ |
L15 | mblywprs | Wipers on | ✔️ |   |
L16 | mblylwbm | Low Beams | ✔️ |   |
L17 | mblyhibm | High Beams | ✔️ |   |
L18 | mblytime | Time Signal | ✔️ |   |
L19 | mdasfvsa | Front vehicle start alarm |   | ✔️ |
L20 | mdasfpw | Forward Proximity Warning |   | ✔️ |

Name | Description
----:|:-----------
Error | Error detected in accessory 
Left Lane Departure Warning | Vehicle departed the left lane
Right Lane Departure Warning | Vehicle departed the right lane
Forward Collision Warning (FCW) | Vehicle imminent collision
Maintenance Flag | maintenance required
Failsafe | Indicate one of the following: blurred image, saturated image, low sun, partial blockage or partial transparent
Pedestrian Forward Collision Warning | Collision warning with pedestrian detected
Pedestrian in Danger Zone | Vehicle close to pedestrians
Tamper | tampering detected
Speeding | Speed of vehicle above speed limit sign detected on road
Headway Warning | Vehicle driving too close to vehicle in front
Brakes on | Brakes applied
Left turn signal | Left turn signal activated
Right turn signal | Right turn signal activated
Wipers on | Wipers activated
Low Beams | Vehicle lights turned on
High Beams | Vehicle high beams turned on
Time Signal | Change in time of day
Front vehicle start alarm | Vehicle in front has started moving, but host vehicle has remained standing still for 2 seconds 
Forward Proximity Warning | Notifies the driver when there is a vehicle existing in the detection range 

## Bluetooth Tags

### Associating to asset

> PUT `api/:bt_tags_mac`

> PUT [api/bt_tags/00:07:80:EA:B9:8C](https://pegasus1.pegasusgateway.com/api/bt_tags/00:07:80:EA:B9:8C)

```json
{
  "asset_id": 123
}
```

In order to associate a Bluetooth tag to an asset you must make a PUT request to the Tag's MAC address /api/:bttag_mac with the `asset_id` you want associated

<aside class="warning">Note that the asset ID has to be unique when associating the tag to it. In other words there can only be 1 tag associated per asset</aside>

<aside class="warning">
    Note that if a peripheral is associated to an asset, and that asset is later deleted, the asset_id that shows up as associated to that peripheral is 0.
</aside>

### Getting Bluetooth Tag Data

> GET Bluetooth tag data

> GET [api/rawdata?vehicles=2050&fields=$bttag&duration=P1D&filter=btt_battery>0](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=2050&fields=$bttag&duration=P1D&filter=btt_battery>0)

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
      "btt_impact": null,
      "system_time": "2017-06-23T16:12:19.462809",
      "btt_battery": 100.0,
      "vid": 2050,
      "event_time": "2017-06-23T16:12:19.462809",
      "btt_reed": false,
      "btt_motion": false,
      "btt_button": false,
      "btt_light": 0.0,
      "id": 5023175567.0,
      "btt_freefall": null,
      "btt_temp": 212,
      "btt_mac": "00:07:80:EA:B8:C0",
      "btt_humidity": 61.0
    }
  ]
}
```

You can use the set called: $bttag on the [rawdata](#rawdata) API to request all the fields for the bluetooth tag.

Field | Type | Description | Range |  
-----:| ---- | ----------- | ----- | 
btt_battery | Number | Battery % | 0, 100
btt_button | Boolean | True if button is detected | 
btt_driver | Number | Asset ID associated event | 
btt_freefall | Boolean | True if freefall detected | 
btt_humidity | Number | Humidity % | 0, 100
btt_impact | Boolean | True if impact detected | 
btt_light | Number | Light % | 0, 100
btt_mac | String | Tag's unique MAC address | 
btt_motion | Boolean | True if motion is detected | 
btt_presence | Boolean | True if tag presence is detected | 
btt_reed | Boolean | True if reed switch is detected | 
btt_temp | Number | °C * 10  (divide by 10 to get the temperature in °C) | -100, 450 (-10°C to 45°C)
btt_wreason | String | Reason for waking up |

Bluetooth tag wake up reasons

btt_wreason | Description
-----------:|------------
R | Reed switch.
C | Time.
B | Button.
M | Motion.
L | Light.
W | Low batt.
X | Beacon mode.
i | First Time.
u | Unknown.




## Camera

Photocam is a camera that can be connected to the Syrus and it can take photos by request or with a programmable trigger.

The photocam can be accessed under the plugins namespace, the interaction happens per device or vehicle, so we need to specify either a vehicle ID `api/vehicles/:vid` or an `api/devices/:imei`

> All photocam methods

> [`/api/vehicles/:vid/plugins/photocam`](https://pegasus1.pegasusgateway.com/api/vehicles/1956/plugins/photocam)

> [`/api/devices/:imei/plugins/photocam`](https://pegasus1.pegasusgateway.com/api/devices/357042062920955/plugins/photocam)

### Capturing a photo

> Capture a photo from the camera connected to vehicle id: 1956<br>

> [api/vehicles/1956/plugins/photocam/capture](https://pegasus1.pegasusgateway.com/api/vehicles/1956/plugins/photocam/capture)

```json
{
    "msg": "instruction sent to device. ",
    "device_is_online": true
}
```

To take a photo simply send: 
GET `/api/vehicles/:vid/plugins/photocam/capture`

or 

GET `/api/devices/:imei/plugins/photocam/capture`

When the photo is taken it will take some time to upload it, at this point you can poll the api's `last` method while the photo uploads, or subscribe to the vehicle's events via [live communication](?json#live-communications) to get the status updates

&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>

### Getting the last photo

> Requesting the last photo taken from a vehicle with 1 camera connected<br>
> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last">https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last</a>

```json
{
    "status": "Photo taken, device is uploading it.",
    "source": 0,
    "status_no": 3
}
```

> after some seconds you will see the following response<br>
> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last">https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last</a>

```json
{
  "photos": [
    {
      "status": "jpeg photo found",
      "debug": "2016-10-17 15:40:09-05:00",
      "source": 0,
      "epoch": 1476736809,
      "imei": 356612024116677,
      "b64data": "/9j/2wCEABQODxIPDR__BASE_64__DvqS9BAKgPU1YhKKYBRQAUUAFJQB//Z",
      "status_no": 4,
      "size": 8586
    }
  ],
  "event": {
    "dphoto_ptr": 46742,
    "system_time": "2016-10-17T15:40:21",
    "event_time": "2016-10-17T15:40:09",
    "lon": -80.29358,
    "lat": 25.78397,
    "id": 5014711973,
    "device_id": 356612024116677
  }
}
```

> Requesting the last photo taken from a vehicle with 3 cameras connected<br>
> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/last">https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/last</a>

```json
{
  "photos": [
    {
      "status": "jpeg photo found",
      "debug": "2016-09-07 14:04:12-05:00",
      "source": 1,
      "epoch": 1473275052,
      "imei": 356612024116677,
      "b64data": "/9j/2wCEABQODxIPDRQS__BASE_64_DATA__EBIXFRQYHjIhHhwcHj0sLiQySUBM"
    },
    {...},
    {...}
  ],
  "event": {
    "dphoto_ptr": 44379,
    "system_time": "2016-09-07T14:04:15",
    "event_time": "2016-09-07T14:04:12",
    "lon": -80.29348,
    "lat": 25.78357,
    "id": 5013175609,
    "device_id": 356612024116677
  }
}
```

In order to obtain the last photo you can perform a 
GET `/api/vehicles/:vid/plugins/photocam/last`

The photo itself mime type: jpeg and is base64 encoded, to convert this into an image you can the HTML `img` tag and modify the source of the image with the following format:

`<img src='data:image/jpeg;base64,__INSERT__BASE__64__' />`

you can also use a website such as:
<a href="http://freeonlinetools24.com/base64-image" target="_blank">http://freeonlinetools24.com/base64-image</a>

![decoded image](slate/img/image.png "decoded image")

&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>
&nbsp;<br><br><br>

### Browsing photos

> Browse photos<br>
> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/browse?_from=2016-05-11&_to=2016-05-12">https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/browse?_from=2016-05-11&_to=2016-05-12</a>

```json
[
  {
    "dphoto_ptr": 40656,
    "system_time": "2016-05-11T21:16:43",
    "code": 48,
    "event_time": "2016-05-11T21:16:40",
    "vid": 469,
    "lon": -80.29351,
    "label": null,
    "lat": 25.78392,
    "id": 5008241019
  }
]
```

You can use the `browse` method to see a list of events that have a photo associated to them.

GET `/api/vehicles/:vid/plugins/photocam/browse`

This method requires two parameters `_from` and `_to`
which is the dates from and to search for photos. 

GET `/api/vehicles/:vid/plugins/photocam/browse?_from=YYYY-MM-DD[Thh:mm:ss]&_to=YYYY-MM-DD[Thh:mm:ss]`

The time [Thh:mm:ss] is optional

The result gives us an object with different `id` that correspond to an event with photos

<code>
[<br>
&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"dphoto_ptr": 40602,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"system_time": "2016-05-10T20:41:00",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"code": 48,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"event_time": "2016-05-10T20:40:57",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"vid": 469,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"lon": -80.29355,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"label": null,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"lat": 25.78403,<br>
&nbsp;&nbsp;&nbsp;&nbsp;<code style="background:yellow">"id": 5008197387</code><br>
&nbsp;&nbsp;}<br>
]
</code>

With this ID we can use the `from_event` method to obtain the photo from this event.

### Getting an event's photo(s)

> Requesting the photos associated to the event id: 5008241019

> [api/vehicles/469/plugins/photocam/from_event?event=5008241019&time=2016-05-11](https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/from_event?event=5008241019&time=2016-05-11)

```json
{
  "photos": [
    {
      "status": "jpeg photo found",
      "photo_id": 10000321,
      "debug": "2016-05-11 16:16:40-05:00",
      "source": 1,
      "epoch": 1463001400,
      "imei": 356612024116677,
      "b64data": "/9j/2wCEABQODxIPDRQSE__BASE_64_Data__HhwcHj0sLiQySUBMS0dARkV"
    },
    {...},
    {...}
  ],
  "event": {
    "dphoto_ptr": 40656,
    "system_time": "2016-05-11T16:16:43",
    "event_time": "2016-05-11T16:16:40",
    "lon": -80.29351,
    "photo_status": null,
    "photo_status_time": null,
    "lat": 25.78392,
    "pi": null,
    "id": 5008241019,
    "device_id": 356612024116677
  }
}
```

> Requesting a photo from a camera that has an error <br>
> [`/api/vehicles/617/plugins/photocam/from_event?event=5013893009&time=2016-09-26`](https://pegasus1.pegasusgateway.com/api/vehicles/617/plugins/photocam/from_event?event=5013893009&time=2016-09-26)

```json
{
  "photos": [
    {
      "status": "Photo could not be taken. Device can not communicate with the camera.",
      "source": 0,
      "status_no": 5
    }
  ],
  "event": {
    "dphoto_ptr": 45535,
    "system_time": "2016-09-26T10:56:09",
    "event_time": "2016-09-26T10:56:01",
    "lon": -80.29352,
    "photo_status": null,
    "photo_status_time": null,
    "lat": 25.78409,
    "pi": null,
    "id": 5013893009,
    "device_id": 356612023728084
  }
}
```


In order to get a particular photo we can use the method
GET `/api/vehicles/:vid/plugins/photocam/from_event`

this method requires two parameters `event` and `time`
where `event` is the id from the event that has a photo associated to it, and `time` is the timestamp that the event was received on the server (system_time)

GET `/api/vehicles/:vid/plugins/photocam/from_event?event=####&time=YYYY-MM-DDThh:mm:ss`

<aside class="notice">
It is recommended to use the system time since sometimes the event_time can have a wrong date due to GPS 0 date.
</aside>

If there's a problem taking the photo, you will get a `status_no` which is a status number that corresponds to one of the following errors.

__Photo Camera Status Numbers__

status | description
------:|-------------
0 | Received photo request 
1 | Waiting for the camera's capture acknowledge 
2 | Device is receiving photo from the camera 
3 | Photo is being uploaded 
4 | Photo ready, loading... 
5 | Serial camera is not responding 
6 | Capture command could not be sent to the camera 
7 | Camera did not recognize the capture command, try again 
8 | The information of the captured photo could not be read 
9 | Comms. error between camera and device 
10 | Device could not create photo file 
11 | Device could not send photo to server 
12 | Device could not connect to server, will retry 
13 | Another photo is being captured, photo in queue 
14 | Device memory full, could not capture 
15 | Error capturing photo, photo removed from queue 
16 | Device could not capture, ID max index reached 
99 | Camera inactive or initializing 


For more info please visit our [Support Site](https://support.digitalcomtech.com/accessories/serial-camera/)

## Driver ID

> See a list of all iButtons and their associations
> [`api/ibuttons`](https://pegasus1.pegasusgateway.com/api/ibuttons)

```json
{
    "id": "0A000017B5E64F01",
    "entity": 1037
}
```

The iButton is an accessory that can be used to identify physical entities or assets such as drivers.  iButtons have a unique 16 character ID that can be associated to an asset.

### Associating an iButton

To associate an iButton to an entity or asset simply make a `POST` with the __ID__ of the iButton (16 alpha-numerical characters) and the asset ID, to the fields id and entity respectively.

You can see the iButton ID associated to any asset by going into that particular asset's ID:
`GET api/assets/:id`


> Associating an iButton ID to an asset
> POST [api/ibuttons](https://pegasus1.pegasusgateway.com/api/ibuttons)

> Payload of request, this will associate ibutton ID: `1234567890123456` to asset id: `1032`

```json
{
    "id":"1234567890123456",
    "entity": 1032
}
```

> See a particular asset's information (including iButtons associated)<br>
> [api/assets/1032](https://pegasus1.pegasusgateway.com/api/assets/1032)

```json
{
  "info": {
    "first_name": "Test",
    "last_name": "User",
    "type": "driver",
    "email": "user@email.com"
  },
  "name": "Test User",
  "ibuttons": [
    "1234567890123456"
  ],
  "groups": [
    1,
    479,
    566
  ],
  "device": 356612021234567,
  "type": "driver",
  "id": 1032,
  "counters": null
}
```

### Setting Up the iButton

In order to setup the iButton effectively on the API and associate the assets correctly you need to update the device configuration to use the **;IS** extended tag. To confirm that this is done for the managed configuration that your device is using check the device's [configuration commands](?json#view). 
Look for commands that start with >RXAEF and it has to contain ;IS, this will tell us that the iButton is kept associated to the device until a new one is introduced.


### Retrieving an Asset's events

Once the user places that iButton on the device, it will update a field called: `aid` (asset ID) in the [rawdata](#rawdata) API and the [trips](#trips) API
This field is kept associated to the device it was introduced until a new iButton is placed.
Allowing you to associate the events by an asset ID.


> This rawdata request will group the resampled events by aid (asset id)<br>
> [api/rawdata?vehicles=617,654&fields=$basic,$counters,aid&duration=P1D&filter=(valid_position and mph > 5 and comdelay < 3600 and hdop < 3)&how=$counters:diff,mph:mean&freq=1D&group_by=aid&resample=event_time](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=617,654&fields=$basic,$counters,aid&duration=P1D&filter=%28valid_position+and+mph+%3E+5+and+comdelay+%3C+3600+and+hdop+%3C+3%29&how=$counters%3Adiff,mph%3Amean&freq=1D&group_by=aid&resample=event_time)

> The result is the daily values of the counters, and the average speed grouped by the asset ID<br>

For more info please visit our [iButton Documentation on our Support Site](https://support.digitalcomtech.com/accessories/ibutton/)




## ECU Monitor

The ECU monitor can be used to connect the Syrus directly to the vehicle's onboard computer to obtain CAN data.
The possible protocols that the ECU Monitor can detect are:

+ J1939 CAN
+ J1708/J1587 
+ FMS Interface
+ ISO 14230 KWP2000 (OBDII)
+ ISO 9141 (OBDII)
+ ISO 15765 CAN (OBDII)

For more information about protocols and vehicles which are compatible visit our [support site](https://support.digitalcomtech.com/accessories/ecu-monitor/)

The connection to the onboard computer automatically gives you access to precise vehicle metrics.  The [master fields list](#master-fields-list) gives you an idea of all the possible values obtained with the ECU monitor.

> Counters API request with ecu parameters
> [api/counters?vehicles=1716&duration=P1D&round=2&time=hour&speed=mph&distance=mile&volume=gallon](https://pegasus1.pegasusgateway.com/api/counters?vehicles=1716&duration=P1D&round=2&time=hour&speed=mph&distance=mile&volume=gallon)

```json
{
  "units": {
    "volume": "gallon",
    "distance": "mile",
    "speed": "mph",
    "time": "hour"
  },
  "counters": [
    {
      "ecu_dist": 232.7,
      "vid": 1716,
      "ecu_ifuel": 0.5,
      "dev_tot_avg_speed": 34.12,
      "tra_avg_speed": 36.36,
      "from": "2016-10-17T13:30:51",
      "dev_idle": 0.41,
      "ecu_eidle": 1.35,
      "ecu_fuelp_idling": 1.39,
      "to": "2016-10-18T13:30:13",
      "ecu_eusage": 7.75,
      "ecu_tot_avg_speed": 30.03,
      "ecu_tfuel": 36.22,
      "ecu_tra_avg_speed": 36.36,
      "dev_orpm": 0,
      "dev_tra_fuel_efcy": 7.43,
      "ecu_tot_fuel_efcy": 6.42,
      "tot_avg_speed": 30.03,
      "ecu_tra_fuel_efcy": 6.52,
      "dev_dist": 265.44,
      "distance": 232.7,
      "dev_ign": 7.78,
      "dev_tra_avg_speed": 36.01,
      "dev_tot_fuel_efcy": 7.33,
      "idle": 1.35,
      "ignition": 7.75,
      "dev_ospeed": 0.56
    }
  ]
}
```

When using the [counters](#counters) API, you will notice that the following fields will be available, they give us precise engine data 

Counter | Description | Calculation
-----:|:------------------|:--------
dev_tot_fuel_efcy | Syrus calculated Fuel efficiency | <code>dev_dist/ecu_tfuel</code>
dev_tra_fuel_efcy | Syrus calculated Traveling fuel efficiency | <code>dev_dist/(ecu_tfuel-ecu_ifuel)</code>
ecu_dist | Distance reported by ECU monitor |
ecu_eidle | Idle time reported by ECU monitor |
ecu_eusage | Engine usage time reported by ECU Monitor |
ecu_fuelp_idling | Fuel percentage used while idling | <code>ecu_ifuel*100/ecu_tfuel</code>
ecu_ifuel | Fuel consumed while idling, reported by ECU monitor 
ecu_tfuel | Total fuel consumed, reported by ECU monitor 
ecu_tot_avg_speed | Total average speed, ECU | <code>ecu_dist/ecu_eusage</code>
ecu_tot_fuel_efcy | ECU calculated Fuel efficiency | <code>ecu_dist/ecu_tfuel</code>
ecu_tra_avg_speed | Traveling average speed, ECU | <code>ecu_dist/(ecu_eusage-ecu_eidle)</code>
ecu_tra_fuel_efcy | ECU calculated Traveling fuel efficiency | <code>ecu_dist/(ecu_tfuel-ecu_ifuel)</code>

When using the [rawdata](#rawdata) API you'll notice that you have access to all the fields in the [master fields list](#master-fields-list) that start with <b>ecu_</b>

> Rawdata request with the ECU RPMs and the flag

> [/api/rawdata?vehicles=1716&duration=P1D&fields=ecu_rpm,ecu_rpm_flag](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=1716&duration=P1D&fields=ecu_rpm,ecu_rpm_flag)

```json
{
  "ecu_rpm_flag": "F",
  "system_time": "2016-10-17T16:58:39.711674",
  "vid": 1716,
  "event_time": "2016-10-17T16:58:37",
  "ecu_rpm": 0,
  "id": 5014706111
}
```

> Filtered by rpm_flag = "T" (show's valid values)

> [api/rawdata?vehicles=1716&duration=P1D&fields=ecu_rpm,ecu_rpm_flag&filter=(ecu_rpm_flag==%22T%22)](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=1716&duration=P1D&fields=ecu_rpm,ecu_rpm_flag&filter=(ecu_rpm_flag==%22T%22))

```json
{
  "ecu_rpm_flag": "T",
  "system_time": "2016-10-17T13:39:34.614058",
  "vid": 1716,
  "event_time": "2016-10-17T13:39:31",
  "ecu_rpm": 692,
  "id": 5014700250
}
```

Use [`/api/resources/rawdata/keys`](https://pegasus1.pegasusgateway.com/api/resources/rawdata/keys) to get the sets of rawdata fields that are available, for example:

- $ecu
- $ecu_advanced
- $ecu_aftertreatment
- $ecu_common
- $ecu_custom
- $ecu_distances
- $ecu_dpf
- $ecu_durations
- $ecu_error_codes
- $ecu_exhaust
- $ecu_fluids
- $ecu_fuel
- $ecu_levels
- $ecu_obdii
- $ecu_pedal
- $ecu_pressures
- $ecu_states
- $ecu_temp
- $ecu_turbo
- $ecu_weights

All the ECU Monitor parameters have the following flags associated
<br>
ecu_***_flag  (where *** corresponds to any of the ECU parameters found in the [Master fields list](/#master-fields-list))  

Flag Status | Description 
-----:|:------------------
F | ECU Monitor accessory detected and vehicle's engine turned off  
O | ECU Monitor accessory has not received new data from this parameter in the last 2 minutes  
T | ECU Monitor accessory detected and vehicle's engine turned on. ECU VALID DATA  

As seen above, only when the flag is "T" will the ECU parameters be valid and should be used for calculations, thus when making the rawdata request you could add this as a filter

`&filter=(ecu_rpm_flag==%22T%22)`

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

### Tips 

> Get the ECU Monitor information for a vehicle

> GET <a href="https://pegasus1.pegasusgateway.com/api/vehicles/1944/remote/ecu_state">api/vehicles/:vid/remote/ecu_state</a>

```json
{
  "protocol_name": "J1708",
  "protocol": null,
  "upgrading": false,
  "ecu_fw": 40,
  "_epoch": 1508232214.0765109,
  "detected": true,
  "ecu_parameters_1": 0,
  "ecu_parameters_0": 468383157349548003,
  "params_numbers": [],
  "ecu_fw_full": "4.3.0",
  "params": [
    [
      "Vehicle's speed",
      "Vehicle's speed in km/h."
    ],
    [
      "Engine's oil pressure",
      "Engine's oil level pressure in psi."
    ],
    [
      "Total traveled distance",
      "Total traveled distance reported by the vehicle in meters."
    ],
    [
      "Total fuel consumption",
      "Total fuel consumption reported by the vehicle in liters. This value must divided by 10 to obtain the actual value."
    ],
    [
      "Vehicle's throttle position",
      "Vehicle's throttle position value as a percentage. 100% means that the throttle is fully depressed."
    ],
    [
      "Engine RPM",
      "Engine RPM value."
    ],
    [
      "Instant Fuel Consumption",
      "Vehicle's instant fuel consumption in liters/hour. This value must be divided by 100 to obtain the actual value."
    ],
    [
      "Diagnostic message",
      "ECU Monitor diagnostic messages"
    ],
    [
      "Cruise control and/or PTO state availability",
      "0 indicates that neither parameter is available. 1 indicates that either or both parameters are available."
    ],
    [
      "Parking break status.",
      "Parking break status."
    ],
    [
      "Vehicle's battery",
      "Vehicle's battery level value in millivolts."
    ],
    [
      "Fuel Percentage (read by vehicle's computer)",
      "Fuel level read by vehicle's computer. Tank's Percentage"
    ],
    [
      "Engine's coolant temperature",
      "Engine's coolant temperature in ºC"
    ],
    [
      "Engine usage",
      "Total engine usage time reported by the vehicle in hours. This value must divided by 100 to obtain the actual value."
    ],
    [
      "Total time while engine in idle",
      "Total time while engine is in idle reported by the vehicle in hours. This value must divided by 100 to obtain the actual value."
    ],
    [
      "Tires temperature",
      "Indicates if the protocol used by the vehicle supports the Tires' temperature parameter"
    ],
    [
      "ECUMon communication quality indicator",
      "ECUMon communication quality indicator"
    ],
    [
      "Ambient air temperature",
      "Ambient air temperature"
    ],
    [
      "Engine's oil temperature",
      "Engine's oil temperature"
    ]
  ],
  "ecu_updateepoch": 1508232214.076124,
  "engineison": false
}

```

You can get a list of the parameters that the ECU supports with the following api
GET `/api/vehicles/:vid/remote/ecu_state`

check the response on the right hand side (JSON)

## Fatigue Sensor
  
The Lumeway accessory is a fatigue alert sensor that works essentially the same as the Photocam accessory, the API requests are very similar, with a few exceptions to some commands you can send to configure the Lumeway.

The lumeway can be accessed under the plugins namespace, this interaction happens per vehicle, so we need to specify a vehicle ID first `:vid`

sending this request will give us all the possibilities with the lumeway accessory.

`/api/vehicles/:vid/plugins/lumeway`

### Capturing a photo

> Capturing a photo for a Lumeway accessory

> [api/vehicles/1/plugins/photocam/capture](https://pegasus1.pegasusgateway.com/api/vehicles/1/plugins/photocam/capture)

```json
{
  "msg": "Capture commands set",
  "device_is_online": true,
  "oids": [
    436085
  ]
}
```

When the photo is taken it will take some time to upload it, 
at this point you can poll the api's `last` method while the photo uploads.


### Getting the last photo

In order to obtain the last photo you can perform a 
GET `/api/vehicles/:vid/plugins/photocam/last`

The photo itself is mime type jpeg and base64 encoded, to convert this into an image you can the HTML `img` tag and modify the source of the image with the following format:

`<img src='data:image/jpeg;base64,__INSERT__BASE__64__' />`


<aside class="notice">
Notice how the source of the image is 99, this is always the case when the photos are generated from the Lumeway accessory
</aside>


> Getting the last photo <br>
> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/1/plugins/photocam/last">https://pegasus1.pegasusgateway.com/api/vehicles/1/plugins/photocam/last</a>

> Notice how the source is 99, this is always the case when the photos are generated from the Lumeway accessory

```json
{
  "photos": [
    {
      "status": "Photo taken, device is uploading it.",
      "source": 99,
      "status_no": 3
    }
  ],
  "event": {
    "dphoto_ptr": 8308,
    "system_time": "2016-10-12T18:11:10",
    "event_time": "2016-10-12T18:11:08",
    "lon": -69.09872,
    "lat": -24.25018,
    "id": 4357500360,
    "device_id": 356612024713036
  }
}
```

> after some seconds you will see the following response

> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last">https://pegasus1.pegasusgateway.com/api/vehicles/397/plugins/photocam/last</a>

```json

{
  "photos": [
    {
      "status": "jpeg photo found",
      "debug": "2016-10-12 17:47:11-05:00",
      "source": 99,
      "epoch": 1476312431,
      "imei": 356612024713036,
      "b64data": "/9j/4AAQSkZJRgABAQAA....5H5gVt3I=",
      "status_no": 4,
      "size": 3269
    }
  ],
  "event": {
    "dphoto_ptr": 8306,
    "system_time": "2016-10-12T17:47:13",
    "event_time": "2016-10-12T17:47:11",
    "lon": -69.12449,
    "lat": -24.25723,
    "id": 4357497035,
    "device_id": 356612024713036
  }
}
```

### Browsing photos

> Browse photos from any date to any date <br>
> https://pegasus1.pegasusgateway.com/api/vehicles/1/plugins/photocam/browse?_from=2016-10-01&_to=2016-10-01T10:39:40

You can use the `browse` method to see a list of events that have a photo associated to them.

GET `/api/vehicles/:vid/plugins/photocam/browse`

This method requires two parameters `_from` and `_to`
which is the dates from and to search for photos. 

GET `/api/vehicles/:vid/plugins/photocam/browse?_from=YYYY-MM-DD[Thh:mm:ss]&_to=YYYY-MM-DD[Thh:mm:ss]`

The time [Thh:mm:ss] is optional

The result gives us an object with different `id` that correspond to an event with photos

<code>
[<br>
&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;"dphoto_ptr": 8033,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"system_time": "2016-10-01T10:39:40",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"code": 84,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"event_time": "2016-10-01T10:39:39",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"vid": 790,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"lon": -69.12207,<br>
&nbsp;&nbsp;&nbsp;&nbsp;"label": "lumenodrv",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"lat": -24.25782,<br>
&nbsp;&nbsp;&nbsp;&nbsp;<code style="background:yellow">"id": 4356063141</code><br>
&nbsp;&nbsp;}<br>
]
</code>

With the ID of the photos we just browsed, we can use the `from_event` method to carry obtain that photo

> <a href="https://pegasus1.pegasusgateway.com/api/vehicles/790/plugins/photocam/browse?_from=2016-10-01&_to=2016-10-01T10:39:40">https://pegasus1.pegasusgateway.com/api/vehicles/790/plugins/photocam/browse?_from=2016-10-01&_to=2016-10-01T10:39:40</a>

```json
[
  {
    "dphoto_ptr": 8033,
    "system_time": "2016-10-01T10:39:40",
    "code": 84,
    "event_time": "2016-10-01T10:39:39",
    "vid": 790,
    "lon": -69.12207,
    "label": "lumenodrv",
    "lat": -24.25782,
    "id": 4356063141
  }
]
```

> using the id <b>4356063141</b> - we can get that particular photo with the <b>from_event</b> method

### Getting an event's photo(s)

In order to get a particular photo we can use the method
GET `/api/vehicles/:vid/plugins/photocam/from_event`

this method requires two parameters `event` and `time`
where event is the id from the event that has a photo associated to it, and time is the time that the event was generated

GET `/api/vehicles/:vid/plugins/photocam/from_event?event=####&time=YYYY-MM-DDThh:mm:ss`

<aside class="notice">
Note that the time doesn't have to be exact to the hh:mm:ss, passing the day that the photo was taken is sufficient.
</aside>

If there's a problem taking the photo, you will get a `status_no` which is a status number that corresponds to one of the following errors. 

<aside class="notice">
Remember that lumeway photos come with source 99 so you can identify whether you need to look up the photocam or lumeway status no's.
</aside>

__Lumeway Status Status Numbers__

Status Code | Description |
-----------:| ----------- |
00 | Lumeway ready. 
03 | Sending photo to the FTP server.
04 | Photo was successfully sent to the FTP server.
05 | Lumeway is not responding.
06 | Capture command could not be sent to the camera.
08 | The information of the captured photo could not be read.
09 | Wrong checksum.
10 | Could not create photo file on the Syrus
11 | Could not send photo to the FTP server, because the Syrus's FTP service is busy.
12 | Could not connect to the FTP server.
13 | Another photo is being captured. Photo in queue.
14 | Error capturing photo. Queue full or camera disconnected.
15 | Error capturing photo. Photo removed from queue.
16 | Error max photo id reached. "999999999".
17 | Error limit of requests reached
99 | Inactive mode or initializing.  

> Requesting the photos associated to the event id: 4356063141 <br>
> [api/vehicles/790/plugins/photocam/from_event?event=4356063141&time=2016-10-01](https://pegasus1.pegasusgateway.com/api/vehicles/790/plugins/photocam/from_event?event=4356063141&time=2016-10-01)

```json
{
  "photos": [
    {
      "status": "jpeg photo found",
      "photo_id": 990003450,
      "debug": "2016-10-01 05:39:39-05:00",
      "source": 99,
      "epoch": 1475318379,
      "imei": 356612024713036,
      "b64data": "/9j/4AAQSkZJRg...+18kcnsehrWguYorYEkfSv/Z",
      "status_no": 4,
      "size": 3261
    }
  ],
  "event": {
    "dphoto_ptr": 8033,
    "system_time": "2016-10-01T05:39:40",
    "event_time": "2016-10-01T05:39:39",
    "lon": -69.12207,
    "photo_status": null,
    "photo_status_time": null,
    "lat": -24.25782,
    "pi": null,
    "id": 4356063141,
    "device_id": 356612024713036
  }
}
```

For more info please visit our [Support Site](https://support.digitalcomtech.com/accessories/lumeway/)



## Garmin™

The Garmin™ accessory has the following methods exposed

- `message`
- `job`
- `state`

you can send a message to a Garmin™ device and have that message appear in the inbox of the Garmin™ screen, you can also send a job or a location you want the Garmin™ to navigate you to.

These methods are also found in 
GET `/api/vehicles/:vid/plugins/garmin`

> Get the methods available for the Garmin™

> [api/vehicles/254/plugins/garmin](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin)

```json
{
  "job": {
    "POST": [
      "message",
      "flat",
      "flon"
    ]
  },
  "message": {
    "POST": [
      "message",
      "mtype"
    ],
    "GET": [
      "_from"
    ], 
    "DELETE": []
  },
  "state": {
    "POST": [
      "new_mode"
    ],
    "GET": []
  }
}
```

> Get the current state of the Garmin™

> [api/vehicles/254/plugins/garmin/state](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin/state)

```json
{
  "state": 1,
  "pending": 0
}
```

> Enable the Garmin™ mode

> POST [/api/vehicles/254/plugins/garmin/state](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin/state)

```json
payload 
{
  "new_mode": true
}
```

To start interacting with the Garmin™, make sure the state is `online`
in order to check the state of the Garmin™ you can send

GET `/api/vehicles/:vid/plugins/garmin/state`



### Sending Messages

Messages are sent with the `garmin/message` method under the plugins API

> POST `/api/vehicles/:vid/plugins/garmin/message`

This method accepts two parameters: `mtype` and `message`

Parameter | Description
-----:|:------------------
mtype | `inbox` means the message goes to the inbox of the garmin, `screen` means the message shows up on the garmin screen immediately
message | text to be displayed on the garmin, up to 40 characters

> Send a message to the inbox on the Garmin™ device

> POST [api/vehicles/254/plugins/garmin/message](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin/message)

```json
payload 
{
  "message":"testing",
  "mtype":"inbox"
}
```

> a successful response means that the instruction / message was sent to the Garmin™ device

```json
{
  "msg": "instruction set to device. ",
  "ts_message_id": 48,
  "device_is_online": true,
  "oids": [
    124272
  ]
}
```

### Getting Messages

Messages are retrieved with the GET method on `api/vehicles/:vid/plugins/garmin/message` by passing the `_from` parameter.  This will give you all the messages received and sent to the Garmin device from that particular date.


**Request Parameters**

Parameter | Description | Required
-----:|:------------------|:------
upto | Retrieve up to this many messages (Number) | No
skip | Skip this many messages (Number) | No
_from | Retrieve messages from this date (YYYY-MM-DD) | Yes

> GET messages from a Garmin

> GET [`api/vehicles/254/plugins/garmin/message?_from=2017-08-03`](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin/message?_from=2017-08-03)

```json
{
  "skip": 0,
  "set": 1,
  "total": 1,
  "data": [
    {
      "update_time": null,
      "deleted": false,
      "msg": "Hello World",
      "isread": false,
      "user": "user@email.com",
      "time": 1502064044116,
      "imei": 450000169939621,
      "type": "TS",
      "id": 1284
    }
  ]
}
```

<!-- 

Once the message is sent to the Garmin you'll notice there's a particular ID for that particular message `ts_message_id`, we'll use this to delete the message later if we need to.

### Deleting Messages

In order to delete a particular Garmin™ message we need to make a DELETE request with the message id.

DELETE `/api/vehicles/:vid/plugins/garmin/message/:ts_message_id`

-->



### Assigning a Job

A job is a geographical destination that can be sent along with a message to the Garmin™.  Once this job is created it will appear on the screen of the Garmin™ device where the driver can click on it and give step by step directions to the destination.

An example of this would be if you wanted to send a driver the location of where to pick up a package.

In order to assign a job we need to use the `garmin/job` method under the plugins API

POST `api/vehicles/:vid/plugins/garmin/job`

This method accepts three parameters `message`, `flat`, `flon`

Parameter | Description
-----:|:------------------
flat | Latitude 
flon | Longitude
message | Text to be displayed on the way to the job (Up to 40 characters - ASCII)

> POST [`api/vehicles/254/plugins/garmin/job`](https://pegasus1.pegasusgateway.com/api/vehicles/254/plugins/garmin/job)

```json
payload 
{
  "message": "package #3242 pick up @ 1:30pm",
  "flat" : 25.78393,
  "flon" : -80.29353
}
```

> successful response means that the instruction was sent to the Garmin™

```json
{
  "msg": "instruction set to device. ",
  "job_message_id": 25,
  "device_is_online": true,
  "oids": [
    124279
  ]
}
```

## Input/Output Expander

> Rawdata request with $io_exp set<br>
> [api/rawdata?vehicles=56&fields=$io_exp&duration=P1D&filter=io_exp_state%3E0](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=56&fields=$io_exp&duration=P1D&filter=io_exp_state%3E0)

```json
{
  "io_exp_in1": false,
  "io_exp_in2": true,
  "io_exp_in3": true,
  "io_exp_in4": true,
  "vid": 56,
  "event_time": "2016-10-17T17:21:09",
  "io_exp_out1": true,
  "io_exp_out2": false,
  "io_exp_out2_short": null,
  "io_exp_out4": false,
  "system_time": "2016-10-17T17:21:51.625232",
  "io_exp_state": true,
  "io_exp_out3_short": null,
  "io_exp_out1_short": null,
  "io_exp_out4_short": null,
  "id": 4384626550,
  "io_exp_out3": false
}
```

The input/output expander allows your device to control an additional 4 more inputs and 4 more outputs.

You will have an additional 13 fields available in the rawdata api, you may use the `$io_exp` set to easily access the fields

* `io_exp_inX` (True if Input 'X' is detected ON)
* `io_exp_outX` (True if Output 'X' is ON)
* `io_exp_outX_short` (True if Output 'X' is in short circuit)

### Interacting with Outputs

> Activating extended output 3 <br>
> POST [`api/vehicles/:vid/remote/output`](https://pegasus1.pegasusgateway.com/api/vehicles/617/remote/output)

```json
{
  "otype": "e",
  "out": 3,
  "state": true
}
```

> Response

```json
{
  "msg": "instruction set to device. ",
  "device_is_online": false,
  "oids": [
    449500
  ]
}
```

> View the status of the extended inputs/outputs

> [api/devices/:imei?select=ios_state](https://pegasus1.pegasusgateway.com/api/devices/357042062920955?select=ios_state)

```json
{
  "imei": 357042062920955,
  "ios_state": {
    "io_pwr": true,
    "io_exp_in1": false,
    "io_exp_in2": false,
    "io_exp_in3": false,
    "io_exp_in4": false,
    "_epoch": 1538175893.721557,
    "io_ign": true,
    "io_out2_short": false,
    "io_exp_out1": false,
    "io_exp_out2": false,
    "io_exp_out3": false,
    "io_exp_out4": false,
    "io_exp_state": true,
    "io_out1_short": false,
    "io_exp_out2_short": false,
    "io_in2": false,
    "io_out1": false,
    "io_out2": true,
    "io_exp_out1_short": false,
    "io_exp_out4_short": false,
    "io_in1": true,
    "io_exp_out3_short": false,
    "io_in3": false
  }
}
```

> View a log of the output changes<br>

> [api/devices/357042062920955/remote/outputsetlog](https://pegasus1.pegasusgateway.com/api/devices/357042062920955/remote/outputsetlog)

```json
[
  {
    "state": true,
    "otype": "e",
    "out": 3,
    "user": "developer@digitalcomtech.com",
    "time": 1452542329.846
  }
]
```

In order to activate/deactivate the outputs of the IO Expander, you can use the `output` remote method, with the `otype` as ***e***<br>
POST `/api/vehicles/:vid/remote/output`

- `otype` ('e' for extended outputs)
- `out` (1-4 for the 4 outputs)
- `state` (true if you want to activate, false to deactivate)

Payload for activating the output 3 of the expander

<code>
{<br>
&nbsp;&nbsp;"otype": "e",<br>
&nbsp;&nbsp;"out": 3,<br>
&nbsp;&nbsp;"state": true<br>
}
</code>

You can view the changes done to the device's outputs with the 

GET `api/vehicles/617/remote/outputsetlog`

[More info](https://pegasus1.pegasusgateway.com/api-static/docs/#api-remote-RemoteOutput)


## Multicamera/expander

> Take a photo with a device that has 3 cameras connected to it

> [api/vehicles/469/plugins/photocam/capture](https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/capture)

```json
{
  "msg": "instruction set to device. ",
  "device_is_online": true,
  "oids": [
    334966
  ]
}
```

> Get the last photos for vehicle id: 469

> [api/vehicles/469/plugins/photocam/last](https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/last)

```json
{
  "photos": [
    {
      "status": "Device is taking/uploading the photo.",
      "source": 1,
      "status_no": 0
    },
    {
      "status": "Device is taking/uploading the photo.",
      "source": 2,
      "status_no": 0
    },
    {
      "status": "Device is taking/uploading the photo.",
      "source": 3,
      "status_no": 0
    }
  ]
}
```

> After some seconds

> [api/vehicles/469/plugins/photocam/last](https://pegasus1.pegasusgateway.com/api/vehicles/469/plugins/photocam/last)

```json
{
  "photos": [
    {
      "status": "jpeg photo found",
      "photo_id": 10000122,
      "debug": "2016-11-13 15:04:23-05:00",
      "source": 1,
      "epoch": 1447445063,
      "imei": 356612024116677,
      "b64data": "/9j/2wCQOxI__BASE64_DATA__bOTGSTSa6q9z/2Q==",
      "status_no": 4,
      "size": 9232
    },
    {
      "status": "jpeg photo found",
      "photo_id": 20000122,
      "debug": "2016-11-13 15:04:23-05:00",
      "source": 2,
      "epoch": 1447445063,
      "imei": 356612024116677,
      "b64data": "/9j/2wCEODxy+vHO__BASE64__DATA__KFFMQ0gP//Z",
      "status_no": 4,
      "size": 5532
    },
    {
      "status": "jpeg photo found",
      "photo_id": 30000122,
      "debug": "2016-11-13 15:04:23-05:00",
      "source": 3,
      "epoch": 1447445063,
      "imei": 356612024116677,
      "b64data": "/9j/2wCEApoZWM__BASE64_DATA__eKkCsfselAH//Z",
      "status_no": 4,
      "size": 6894
    }
  ],
  "event": {
    "dphoto_ptr": 30798,
    "system_time": "2016-11-13T15:04:27",
    "event_time": "2016-11-13T15:04:23",
    "lon": -80.29337,
    "photo_status": null,
    "photo_status_time": null,
    "lat": 25.78379,
    "pi": null,
    "id": 5003163587,
    "device_id": 356612024116677
  }
}
```

The serial expander can be used to connect multiple serial communication (RS-232) accessories simultaneously.
This allows you to take up to 3 photos simultaneously with the camera.

Depending on the Syrus configuration you use you can have the option to associate an event to take a photo with 1, 2 or 3 of the cameras connected.

<aside class="notice">
The capture command for the single camera, and mutiple camera is the same, the difference is in the managed configuration.
Note that the managed configuration can be configured to take 1, 2, or 3 photos per any event.
</aside>


<img src='data:image/jpeg;base64,/9j/2wCEABQODxIPDRQSEBIXFRQYHjIhHhwcHj0sLiQySUBMS0dARkVQWnNiUFVtVkVGZIhlbXd7gYKBTmCNl4x9lnN+gXwBFRcXHhoeOyEhO3xTRlN8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fHx8fP/AABEIAPABQAMBIQACEQEDEQH/3QAEAAr/xAGiAAABBQEBAQEBAQAAAAAAAAAAAQIDBAUGBwgJCgsQAAIBAwMCBAMFBQQEAAABfQECAwAEEQUSITFBBhNRYQcicRQygZGhCCNCscEVUtHwJDNicoIJChYXGBkaJSYnKCkqNDU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6g4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2drh4uPk5ebn6Onq8fLz9PX29/j5+gEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoLEQACAQIEBAMEBwUEBAABAncAAQIDEQQFITEGEkFRB2FxEyIygQgUQpGhscEJIzNS8BVictEKFiQ04SXxFxgZGiYnKCkqNTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqCg4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2dri4+Tl5ufo6ery8/T19vf4+fr/2gAMAwEAAhEDEQA/ANZQAAB0AxTqyAUU6gQop1ACbBg4AGfShVZWPJKnn6UAMZVZwWDJjPKnHJHP6ClWFFf5BwMHG5hggcfXoOtTYdxoLxeVGF2H5B1zkk4I/ClSVTHumAbId9wH8IbAoGWYQgDKjbtpwfY9P6VJTEKKXFABijbQAm2o5hiJyewzSA//0JgOBS4qRBijFABijFAC4pCtIBcUbD6UAIRjrx9aYZIh1ljH1YUBuN86DtNGfowNKGQ/d3N9FJ/pRcdmGCfuxzH6Quf6UeXJ2t7k/SB/8KAsNMU3/Ppd/wDfhv8ACo3WdRk2V3j/AK4mgVitLMY0LvbXKL/eaIgfnTY2Eq+aoO1umaAZ/9HYFLisgFAp1AhQKa5ZcFVJ9cUAKJRnDAg5x/hUgORmgB2ARg8g9qaY1Yk9z196QxY1ZDjcStQALKVV4yjHA+XjOcnoe3y0bghzQ+YqkMWHzHIAyNxznn0qWaQpKhGdqglgO+eB+tLYe46Gfcqh/vnrgHGckf0qYMCAQQQehBoEOpcUAGKgvOLWXHp/WmtxM//SsEYGTwPeo2uLdThp4gfTeM1Ih6sX5jinkHqsLkfnjFTrbXb/AHLKb6sVX+ZoHYlGm3zdIYk/35f8AakTR7tvvzQR/RGf+oosBKuiN/Hdt/wCMD+eakGiQ/xXFw3/AAJR/ICiwDxo1mPvCVvrM/8AjTxpNiOtsjf73zfzp2HceunWS/dtLcf9sx/hUywRJ9yJF+igUWQXH0tMQUUAFIRnrSYHP+MZPL0lEHG6Qce2D/8AWrChTy7eNe4FITP/09gU4VkA6lFAhRTHD5JQj7vA96AFLMeGU4zwaUlJAMN29aQD0VlPJyMelOVs5HpQMcpDdDmnUANMQJ44pCZAxyodc5HHTv8A0oQmM8uMkYLJ0JX1wSR+pphhljjITIwvVT6Jj+dIolW52nDfMvQEdT93+e6rCSKxwGGeeM88HFArElMlhSeNopASjcHBI/UU1uB//9TpI9G06PkWcLH1ddx/WrkcMcQxHGiD/ZUCgB9FABRQAUUAFFABRQAUUAFFABTS2D90mk3YDlPGUvmTWdt0zkkfUgD+Rqm3QD2qb3Ez/9XYFOFZAOpRQA6oyELEg4Ykd/SgQo8xR8vIHT/ClAUthlwQeCP50AOVAqlIzgj1/wA/SnqHJRevA3H+dICY2itg7yD9KBbuvSUEZ7r2/OnoNi7cHGR+tOEbHpj86QA0JPVc1GY2UfLlceootYBhAJzKik8fMOvBBFBgIYsjYJOee3JP9aTGPtmJjOSxwcfMcnoP65qYtjH1FCEf/9bsqKVwFopgFFABRQAUmRSbSAM0UXAKKV2AUmT3x+dDbAQtjuo+pqIynOA8R/Gpd2Bx+uu1x4iRHIJiQDjp0yP1ah+WNMl7n//X2RSishDhTqBi1GrLn7vOe3Pb/wCvSELyV+Rx65JxTiXB5UsASO1ACnYWBYEHH1/CrFshVMhg2QMfSgCfPHzDFNXaW4JGO1IBVB6MART+BSGgOe1GemepoGL2ppRfSrbEN2Kg+XgemKaxxj6ipA//0OypDkdBmpfkAueOcCjPvTuAZpM0rgGaM0rgGaATRdiDmg5pajE5xyaazqn3nUfU0tWIja7tl+9cRD6uKhe/sd2WvLf/AL+j/GizDchfUtOx/wAfsH/fYqD+1NNRs/bIz9Mn+lArHONILnXLu4U7o84RvUdv5CpiaoD/0dhN2PnKk57DFPFZCFFOFAxTnBx17VHudeXUEA4OT7dfzoEK0ca/LkjgjrTgjZO1wfXB70mxkkYLMu9e4yfw5q7hcADjHFAhDuHTmk2g/eXmkA8LtHWg9OaGrDQgHPcUoBoSGBHocUxiyLu6gD8TTsIYHL/MQR7GmTkCPJyBkdKOoj//0uvOFOTn86SRRIuPyrK/YCq15aREhp1yDggc4NNOrWg+6zt9EP8AWmkyRh1iL+GGY/gP8aYdYb+C2J/3nx/SnYBh1a4P3YI1+rE/4Uw6neH/AJ4r9FP+NFkFxhvr0/8ALcL/ALqD+opjXN0eWuZD9AB/IUANMk563Mx+jkf1pjBm+9LK31kJoAYYIz1UH68037PEOkSf98igA8pB0QD8KNoHQUANIphpARHlz7CmGmB//9PZFOrIQ4UooAGBIwOuRSASIABjAz1z+FIBDIMEuhztz0x3xT0KsGCE8ENz6cH8qduoXLVpGyJ8xyOx9qmYgdqAGdSMEjHqKkGakEBNGfSk3coXOOooz6VV9BCZ9aqtIZfmRyo6AEd8UXAeM4GcZ9qiuv8AU/iKFuI//9Tpmuj08sf99VWN1OJwjMArZA2jrWSEVruPLeZ1P8RPeq1WtUJi0tMQYpaQwo4pAC4x9OKWgBDTSRQAwsvqPzphkX+8PzoAYZF9RUTSL60CITKq7ieMn0ppkHrQB//V14ZY5k3wusi+qnNSVkIcKcKAEZdwGDjBzSASKOCTgfWkMcZMNtK8ZABJ606Mo7BRkFuMfWgC7s2qQlNJkB7EZpAPx6ilzRcBMnPTilzSGBPpSFsdeB602xFeSQucq+3A4B+tI5bjchceooAev3RgYqK5/wBUPqKEJn//1t6XICnP8QpDjdzyM5+hrAQSAMpB6Gs512MR27VpETEpaskM01iexxRYBuX/ALxpMN/eP50hhtPqfzpNv1oATYPSkKD0pAN2D0pCo9KYDCtMK0gIZlzG30zVPdSY0f/X5yGaSFw8MjIw7qcVr2niO4jwtyizL6/daoEbdprFldEBZRG/92T5f16VoVICinUgHCpbeMZLAYxwMUDJ6KAFpCAakYfSmljn7vFNCYZwCTxUO/echwU7UARt/tx4+lAwV3JJhffiiwXJRnAz1qG6OIh/vCgD/9Decbhg1GxIb0P86xAAOOuarXPBX3zVx3JZDRWhIlFAC4oxUjDFGKQCYppFACEU0igBhFMIoAjZcgj14rH3cUmNH//R5kUtQAvXrVy11G7s/wDUTMF/uHlfyNIDatPEyHAu4Sh/vx8j8jWzbXdvdrm3mST2B5H4daTQiwKnjdQoGcGkAqTxucK65zjGealpMFqLRQUFJQBGx3fSo/LUKVXIB9D0oEIqup++WHPGKYehDpxuzwTRcViVeFAHTFV704hX/fH9aa3Bn//S3qa65/z0rECPkf1qG7GUX61cdxMrA4ODS5rQzDNCnLAUwJdtLsrMoNtJtoANtIVoAaVppWgBhWoytADCMGsGVdkjr/dYj9aQ0f/T5mlqAHClpAKKVSVYMpIYdCOCKANS01+9t8B2E6DtJ1/Otu08Q2c+BKWt3/2+V/MUrCNBYLeVNyJGyHoydP0qWKNYf9WCuQAeT0HApXAYGvI8/Ojrk4G05/n9an85xHuYchc4A70adAIxfx4y7lO3I4z9am37h1yKLDDNGaQBSUAFVr7/AFSf9dB/I1UdxPY//9TeFITg1iAhUfhVe8XEQ/3v6GqjuSyieetLWxmJTohmVB6sP50AaHkml8k1lcsPJo8mi4B5NNMIouMQxCmGIUAMaMelRtGPSgCJk9qwL5dl5MP9rP58/wBaAR//1eapagAp1IAozQAuaM0ATW91Pavut5XjP+yeD9RW1aeKJVwt5CJB/fT5T+XT+VJ6gbtlqtle4EU6hz/A/wArVf2UthDTGpOSKbJHvTaGK/SkMYiSoQGkDJjuuD+lDNMhJCh168HBH+NNiHeb+73urKPTBJ/KkWeJ/uyLn0JwfyNKwD6rX3+qT/roP5GqjuD2P//W6BR8o+gpCvNYiFAA64qG+UfZwR/eH8jVR3E9jMPWnIhdsAqPdiAK2IJhaO3SSD/v5n+WakisnWRGLqdrA/KrHv8ASp5kVy9zQAX/AKaf9+W/wpQvokp/4Bj+ZrOxVhfLb/njL/47/jS+S/aF/wAXUU7BYTyJT0hX8Zf/ALGj7NKf+WUX/f1v8KOUdkJ9kl/uwj8WP9aT7HMerW4/4Ax/9mofmAn2CTvJD/35/wATSf2c3eZR9IU/wqbiZRntzHKy7i2O+AO3tXNaumzUJAe4U/oP8KaEf//X5qlrMApaACloAKKAFooAWtCz1q+ssCOYug/gk+YUAb1n4qt5cLdxtA395fmX/GtqGaK5TfBIki+qnNS1YQ7FFIYlNKg9RQAtV9QGIoveQfyNVHcT2P/Q6aGFmjU5UZA96kEH+1z7AVgA7yR/ff8AA4qrqUYWzYhnPzDqxPeqiDMXvVnT+L6H/eP8jW7M1udA8iRjLsqj1Jx71HLdQRB/MlQGNdzDPIHripNCNNQtpEjdJNyyHCkDrzt/nUk91DbxNJM+1Fxk46ZOB+tJuwC+evmBO5Gf8/lTJbtY4hJ5crAnGFXJ/Kp5tbDsKZ2DOvlt8uMHnB6UyW5dGiAhZxI20lcnb7njpTuFhpvJSjMlrKSCBgjFPd5wHKoDhCVX1bsM1LsLqVml1EyWmyBAjf6/kZXkdOfrUlu9401wLqKNIg37plbJYZPX8MUtAaI7hcvn2rk/Eq7L9D6wj/0JqEI//9Hm6KzAKWgAooAWigBaKAFopAFPhlkgffDI8b/3kODTA2rTxRdxYW6RbhfX7rf4fpW3aa9p90P9eIX/ALkw2/r0qbAaOcnAKn6GggjqCKQCVV1MkpBzj98On0NVHcT2P//S6uD/AFKf7o/lURGdpec5weVHB4rACRtuSPMYE9h+NR3cRuLZ0iYFiQQP1oW6AxpYJImIdCCKfZf8fkP+9XRe5nazNy7lSKEtJG0ingqq5zSPHFmRjbhiyfMdoO4envUGhEW8p4EissoSRuVQoj5649+tPElztkJgHDYQKwywz154FDGrdQWS586JTCNhUF23Dg4OR/L86YhvvsxMiR+eT0DYUDHrj1oVxDvKuRcSv5+Y2XCR7R8p47/n+dPAuCIslRyd4x/Kod7j0IjFdiJ9sqmQtkbjgYyfb6U/Eu3Y0qhivJB5zj/HmlqGhAYGKQ5uuUPzHJ+Y5Hv/AJzSWZhFzdLHdCZwfnTP3OT/AI/pSsx30tYlkXJrkvF42T2rjujD8j/9emtyD//T5ulrMAooAKWgApcUALijFIBaMUAFFACUUAWba/urQYgmZV/uHlfyPFalt4hI4niZP9qBiP8Ax0/40Aa1vqgn/wBRcxzH+6ygN+XBou70ERieE8SA5VtuD+INJOzDc//U6GG/gMajDgbQORn+VThwY9yxrjHBPHf3rnYxZJJAF8qNZCWx16ChpxBGZJozGBjJHzD8hzTXcErj3ZJYtzL8pGRuUjH9RWelridJVKttbc2w54+laXsHLcvTmaRQLZlQd3J5HtgioLm6nRQts9vLKASyk8ke3NJtpFJLqV01G9jvIorqGNQ7AfL1Genc/wCQa1HztIDbT64zQpN7ikktiu1u73cM32ohUGDGBw5556+9V9KS2t4Zliu2nG4FieqnAGP0p9CSHxFPcRQwLbM6l3IOzOT6Cudmlvldkkkudw6jcTit6fLbUzle+gltLdRXcEhacASqCWzjryP51105jF8gKAsygZ3c857Y9vWoqpXVioeZnwPF9iGyzfCSYCsznouR1HqAKu20EMN7O0VsY3kGXk+Yhj+PHc9K52zZqytcnkOK5TxmBixb3kB/8dprcyP/1ebpazAWigApaQC4pcUAGKXFAC4oxQAmKMUAJTaADNGaYC7jVyPU7pSm+VpVQghXY9voent0oA//1rtl4mt3jWK4RoGAA3j5l/xrWSe1mXes4kBIPyn09awaAgubnbKPIwVU7uvU1XuNVwjNJG+3uIgS557cik7lxt1M+TxBAQcS6hGe2U/+uaonxNOFKi7lwR3Rc/nioXte52RdC3vr7iK48RTXMAhluGKD/ZwT9cdang125dX8gxKWGGdYwGP40/fWrGvYS92JrWf2q+ms5PLbYm3c+OPlPPP5/rW4NnmzbYtjjq5A+arW2pyVEr2REl0pufJEWTkY6ZAIBzUMM2yOUrpvl4cAhU+9yRnpzxj8609TNeRU8QSkWljO0bKfMDbM7SDtzj9KoDVUdFJl8g5+ZRETnLBjyD0zkfQn1rWMW46EOVmVLzUPP8tVZSolWQ4UjBAAA5J967N2kEihEBU/eOelTVVkgi7spMupNE4dokbf8pRsZXHuD3qZFuRclpHQw7eFB5zx7fX86xdrF6ahccYrl/Fy7ra1JOAHYfmB/hTW5J//1+bpazAWigBaUUgFpcUALilxSAXFGKAENIaAGmmmmAlFABS5pgf/0MFTVu3kZCGRmVh3U4rJgaMd9I2PNAk9+jfnVhpVnhaNJ3hYjh14dfxpAULm31K3XcNVvXXsVZ2+meeKyxfXxIH9p3HPdpGx/OhTb3Rv7KD2kPa6vgcf2o5/7ak0kcxyWnlaSQ/xE1Ld1saU6ShK90dnpD3SaLbNbRJIXLN8zY+Ukn+tabGTL7h8vY8VbvZWOeVrspi0uBqK3AKYON3PPQA9qtyC7KOI2iV8/IWBI69/wqrpohFXVdPk1GyiiaZEmRg27HyscEHj8axR4dZwSL+3wpwcLnB/OtIVOVWsTKN9SSPw3GJlEt+jY5ZFTBI/Pit6UrJJG6ygKpORjOf8KirJzHFJFKW0s44rgzXGB5od2zjaSeBToGskvIvJkdpJYgU4JUrjg+2Qv6VlrsU77k90cBfxrmfFhB0+H2m/9lNNEn//0ebpRWYC0UAOFLSAWloAWlpALRSAQ000wGmmmmAlFMBKAaAP/9Ln1NWIjxWQFpDViM0gLMUkiHKEg+xqlf6VHeDfEqQTZyWAwrfUDp9RQnYDEudNurQEyxEp/fXkVFbxvPMkUXLuwVfrV7gehpp80mlx2trqQTy8ASomflCYx198/l9atXh/0mBTIRhWIHZjUSegMkgVFl4kLH0xjuf/AK9OuDG0E2ZjGq8sydVxzQhlcQ273FtN9plZlQbVzw2c4J461Wt/7Phgn8t5iolw2ezcjA9qdmDlzaCs1iZ2bZKWaLkb8Dbt9M+lLZGxe1QwxOiKxCgyHPUE9+ecflTlorh5EjywSfaV+yeZ8w3jH3yDx/LNOVYhPCUto12oArbeUGDwP896mwm2JftiNP8Ae/pXNeJznTY/+uw/9BamhH//0+bpRWYC0UAOFLSAUUooAWlpALRQAhppoAQ000wG0hpgJRQB/9TnQasRnFZMCdHqzE4NJgXEGR1qQL70gJAmR940+0t4bW4E8UUYlH8WwZoAvefEtqyPCzKBnAfsBTrW7tdXtI7tUdNhYJuPPv0p2Q9y5F5KksoUMOpH+fc0hFrtcYXEnL8daASuOjS3bb5aJ8gAX5eg7Yp4giAIESAMckbRyaV7hsQzXUEMhVuWA5AHIo+1R7FKg4bkAYp2dhdRJLjZj5JCSOw6UglLkfIQCASSenWgCtfn90v+9/Q1zniT/kHJ/wBdl/8AQWpoR//V5qlrMBaWgBaUUgFpaAFpaQC5ozQAlJQAhpppgNptACUUwP/W5wVMhrICVWqeJ+aTAurKoHJpwuE9TSAX7ZED/F+VSpdx4yA1MCWW7AjkUKc+Wx6+1O8NSJH4ehJQsWmMfXGMnrR0C9iy2rNHbGVNPkkbgiNSSW569PqakTU0OrwWBtApkiDlt33flJxjHtVJaAGnazHPq1zYGFYmjLBGDZ37Tg/41IuqyN4gfTvLXy1j37889Af60rAU7fV7+8j1AW0ELTW8ipGvTcNxBzk+gqmmra7PeS2kcFsJ4hl1I6Djvu9xTsgsh97f6r/akNjbSRJI0IY7lGN2CTzg+lW7CPVkuN2oXMMkW0/Ig5z2/hFJ2Bk9837pf97+hrnfETD+z0z/AM9h/wCgtQhH/9fmQaWswFpc0ALS5pALmlzQAuaXNABmjNIAzSZoASmmmAhptACUUwP/0ObFSqayAkBqWM80gLgZQORSBgW4FICRsZHFSSHpQBHcyhFmz3iI/Orug8eHbQ/3roH/AMe/+tT6AXVujbvassTSHy4lwvvvGf61XmP/ABXEP/XH/wBlaqEjHkjnGo6nfWx/eWVyZMeoLtn+XPtWnY3Md34tNxFnZJbhhn6LT6DE8NzRrfalEXHmPKWC+wY5P/jwp9gf+Kr1M/8ATP8A+IpAVtT+0t4oj+wlBP5I2l+nRs/pWppw1FfNOpSxPnGwRjGOuc8D2oYmO1Fv3Sf739DXO+IDmwjH/TUfyakB/9HmKXNQAoNLmkAoNLmgAzS5pALmlzQAZozQAZozQA0mkoAQ0lMBKKAP/9LmxUi1kA8VNGORQBb34HSgNluBUgSclhxT5DjGSAPc0ARTQSXAlMaO58s7QoJycVpaaht9EsreQbZkuAXTPK5LHmm9gNGzXE8J9IkH6P8A41kajcGz8Ui6dcRrGF3vkLkqe+KpErQrwana2s93LGyMbtiZA2WXqTgcDj5jVWzvbPTpvOtWYSbduSpbj8aWpQkGpW1rcNcW6MszZywU85OTwWIpw1xUnedFkEsn33VUBb8cGizEOlvJF8rV41maUgpl1UoBkjnBz+grQ0/VLpgLjUJYI7VgVXapB3dv0zQ2Uo8z0Ll7IstvE6NuVyGU+oIrA18/6FH/ANdP6Ghbkn//0+WzS1AC5ozSAXNLmgBc0ZoAXNGaQC5ozQAZozQAmaM0AIaSmAYooA//1ObAzUoHFZASKamRsUmBYDnHSl80gcKPzpAQ3E7GPjg+1UXkJxkk1SQHW6TKiRhG++enHYAf41feTyjtjhz5gYsygDBHTPrmkAGcRYZnQKQDnPTPAH51zOv6xDqESQQyZiVt2fLOSQCPy5pxBoz49Ju5kVoYiwYZBJ6ipF0K/Y4Ea59MnP8AKhysC1I/7KnHLPEqjvuzT20WdXZGlRSq7ju4wKdxXLdjptuu/wA3y59qkEgAhM85JB+tJMEuoxC9w8/kjC4dEwSPU9anVjTa1RqySIYYoQAhjAXb6cdKxPEB/wBHgHq7fyFNbiP/1eVoqAFzRmgBc0ZpALmjNAC5ozQAZozQAuaM0AGaTNABmjNABmjNAH//1ubBqQGswHrUy0gJgDjpmhhn+A0CILgEAA8VWUEyqBzzTGdNpbgsryAx43D5+P7tXLuZnx9ldXIR9wVxx8vH61KsDKF1FHfQz2tu8catg7cZOV7HnI6D86zxoaxPH5puHyoc+XBuGf7vB/wpph6mhB51lHp5nDRxK0mQxwduPlB9TS2t9FZ2CwMXlkUNySoBJJPdvehsSOejjlt93lrakt1MpjbH0ya1b26t5Zbsi4gYSRoikMBnByeKaYbGW6xy5aa5hWQn5VjHyj9KkVYri7CuCwByNpPPzcH8sUndD3NRgfPMkrAdCMZI798cdaztYdZBAFIO0sT+mKlSux8r3Wx//9flKWoAKKAClzQAZozQAZpc0AGaM0gDNFABRmgAzRQAUtMD/9DmhUgrMCRamSkBYBwKdvPpSEVLtiSM8VBbk+epAyR2qlsM6bTjG8Lm4RRz0YZ4wKtsYFjfyFQfLzsUCpVgIg4kjdSHJ2nBQ4YHHb3qhBrZgs40uYnkmT5SQT8w7N07iqViTB1a7F9eSTBGVSRgMOQMAf0qpGivIqllQMQCx6D3NUUXTpsYUkXauR/CkLkn8wB+tQ3dqLbZtd23E/fiKdMe/NArkMUZllVFxk9AQTn8q2tMaS3jjDllWc+XvUjgjO0d+D+HSpb1sXy3jc0Lm2Z+HbK7fmGfvHGOvXpWPqcMcLRLEu3KknknP50ktSVJpWP/0eUoqACigAooAWigAooAKXNABmjNABRQAlLQAUopAf/S5kVIKzAkWpUNICYH3o/4FQIq3B565pLTiXPpVAzodICzRSLKofJPX04q7JGkSbYkVQQcj14qO4GPq97cWtoNkhjeUgAr12jk/wAx+dV9K0jzoobmWTMbDIQMRnkg5/Lt+dV0BlLVrgC8uLaOGJESQjKqcnH4+1Z9O1hiliepJ/GkpgKkjRNvU4I6GujtIIpBHPHdvJKNu/5cYyOR70mrhdrYvTspkcnO4p+FYmrHNyg9Ix/M0luI/9Pk6WoAKKACigAooAKWgAooAKKAFopDsFJQAtLTEf/U5kU+swJFqZeKQEgb2o3D0piKk5y1S2m1W+YdfSmM6HSMCN9gIXdzn6CrN2BhSxwAG/lUbhsc7rTC41K1tSSFXaGOOhbH9MVrlrOCG3Z5ikcYyvPH/wBeq6WEYd6ItUuGbTrWTdndJIxwMn2/z9KZdaUlpbeZLdjeVyqCI/MfTNO/QdmSJo8PkrJLe7ScAqsW7DYzjrzT30e0i3b71/lGf9WBn6fNQCuWLKytIZVaGC4vGYZBCZCfgO9XLeSLMqw2+xg+XGQeeepBpXBxfURpjKZMKpXoGVgc/lWRqZzeN7AD9M/1oEf/1eToqACigApaACigApaACigAooGLRSGgooGxKM0yD//W5cGn5rMCVTT8mkAu8+tJvb1pgV3JLVPbyvHkrg/WgDb0qZ5Ii54xIRgd+BVq8k8mGeaRCRszhv5VFwOelukn1S1mX5m2xiTI43jg49ulWrqX7ZfQWzIohhXeyrxn6/p+dW9xFsarBE4hCRK+R8ixkDP4VVVGFuwljgkkfO39yu4985xu/WolKyNaUOZ2IvsUmd7zxo3r5mcf98gikje6tCVE80KseWDHB9Dms1UV9zulQ5lsQSXU8kqx3U7SKR9+V2b/ABp8twYdHAibiR+Bjj34/CtvM4JbWE0pyYZ2OPmYDgY6fT60l1zJQ9yD/9fkqKkAopALRQAtFABS0AFFIYtJQAUUDDNFAXEpaCT/0OWFPFZgSqafmkAUYwOaAID1p6dQKYG7pSZtz/vn+QqTWJM6bctyeAv05FR1A5yyUG5VywAQ7jwTnv2qyW/05ZH3xCRTkg88DB+nSqvqFhHkto8zxLz0XJJ3H15p1tegPuJy/dj1+lZzi5I7sK4xfvGilxHICeh/3uP5H+dON3DsKNhkIweBn9K4nB9D1GrmTqBhMREQwFwRTZrdpLC1AZRtGcepYiu+m3yq54uJio1GkXECRx7IwAFbHHfHeqtwcuaZzn//0eSoqACimAUUgFooAKWgYUZoAXNJmkAUUwCigAooEf/S5YU8VmA9afSAXdikaQ4oAgaQLyantwD853ZPQGmwNrTJ9kci7TtJBBB7/wCcU3VpF/sudVUjlepz3FQtwMXT2iDSNPwMdvTB/wDrUbjMh+ZYxuzyeRxz781fUb2RCXMjRqwLhV6A06Iz27FlRhwRyvrxRextFXRMs6eUC5IbPpTDcFztRST+ZrLkOyVbRJCPDPKD+7PTmrshOy3XbsxIo+YjoPxzVJrZHLVi5O7J0gYDHJ5JzimvbQj5nkOfSp5iVR17n//Z' />


<img src='data:image/jpeg;base64,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' />

<img src='data:image/jpeg;base64,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' />


For more info please visit our [Support Site](https://support.digitalcomtech.com/accessories/serial-expander/)

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>



## Temperature/Analog Sensor

The analog interface or temperature sensor has 3 sensors that can detect temperature at different points

### Getting Temperature

The temperature comes in 3 fields 
`ea_a` 
`ea_b`
`ea_c`

These values can be queried with `rawdata`

the results of these values correspond to a temperature value:

here's a graph that has the Temperature vs mV relation
[Graph](https://docs.google.com/spreadsheets/d/1WBXq8NvWqPB6vRO2Tlye17JfrM5X3QZTlr9vs7uxVSk/pubchart?oid=1014869331&format=interactive)

You'll see there's a 12th degree polynomial function that's used to calculate the temperature value. 

<code style="background:#333;color:#fff">f(x) = -2.0368055E-35*x^11 + 6.2029607E-31*x^10 – 8.2710455E-27*x^9 + 6.3466737E-23*x^8 – 3.0989103E-19*x^7 + 1.0055822E-15*x^6 – 2.2005923E-12*x^5 + 3.2320968E-09*x^4 – 3.1180082E-06*x^3 + 0.0019020671*x^2 – 0.70336715*x + 141.46249</code>

**Javascript**

<code>
function degC(mV) { <br> 
&nbsp;&nbsp;&nbsp;&nbsp;return parseFloat((-2.0368055E-35 * Math.pow(mV,11) + 6.2029607E-31 * Math.pow(mV,10) + -8.2710455E-27 * Math.pow(mV,9) + 6.3466737E-23 * Math.pow(mV,8) + -3.0989103E-19 * Math.pow(mV,7) + 1.0055822E-15 * Math.pow(mV,6) + -2.2005923E-12 * Math.pow(mV,5) + 3.2320968E-09 * Math.pow(mV,4) + -3.1180082E-06 * Math.pow(mV,3) + 0.0019020671 * Math.pow(mV,2) + -0.70336715*mV + 141.46249).toFixed(2))<br>
}
</code>


**Python**

<code>
import math
<br><br>
def degC(mV):<br>
&nbsp;&nbsp;&nbsp;&nbsp;return -2.0368055E-35 * math.pow(mV,11) + 6.2029607E-31 * math.pow(mV,10) + -8.2710455E-27 * math.pow(mV,9) + 6.3466737E-23 * math.pow(mV,8) + -3.0989103E-19 * math.pow(mV,7) + 1.0055822E-15 * math.pow(mV,6) + -2.2005923E-12 * math.pow(mV,5) + 3.2320968E-09 * math.pow(mV,4) + -3.1180082E-06 * math.pow(mV,3) + 0.0019020671 * math.pow(mV,2) + -0.70336715*mV + 141.46249
</code>


Temp °C |  mV | Temp °C |  mV | Temp °C |  mV  | Temp °C |  mV  |  Temp °C |  mV |
--:|:----|---:|:----|---:|:-----|---:|:-----|----:|:----|
60 | 189 | 40 | 291 | 20 | 548  |  0 | 1271 | -20 | 3583
59 | 192 | 39 | 299 | 19 | 569  | -1 | 1333 | -21 | 3791
58 | 195 | 38 | 307 | 18 | 590  | -2 | 1398 | -22 | 4014
57 | 199 | 37 | 315 | 17 | 613  | -3 | 1467 | -23 | 4252
56 | 203 | 36 | 324 | 16 | 638  | -4 | 1540 |
55 | 207 | 35 | 334 | 15 | 663  | -5 | 1618 |
54 | 211 | 34 | 344 | 14 | 690  | -6 | 1700 |
53 | 215 | 33 | 354 | 13 | 718  | -7 | 1788 |
52 | 219 | 32 | 365 | 12 | 749  | -8 | 1881 |
51 | 224 | 31 | 377 | 11 | 780  | -9 | 1980 |
50 | 229 | 30 | 389 | 10 | 814  | -10| 2085 |
49 | 234 | 29 | 401 | 9  | 849  | -11| 2196 |
48 | 239 | 28 | 414 | 8  | 886  | -12| 2314 |
47 | 245 | 27 | 428 | 7  | 925  | -13| 2440 |
46 | 250 | 26 | 443 | 6  | 967  | -14| 2574 |
45 | 256 | 25 | 458 | 5  | 1010 | -15| 2716 |
44 | 263 | 24 | 474 | 4  | 1057 | -16| 2868 |
43 | 269 | 23 | 491 | 3  | 1106 | -17| 3030 |
42 | 276 | 22 | 509 | 2  | 1158 | -18| 3202 |
41 | 283 | 21 | 528 | 1  | 1213 | -19| 3386 |


For more information about the temperature sensor please visit our [support site](https://support.digitalcomtech.com/accessories/temperature-sensor/)


## TPMS

> Request tire data (doran example)

> [api/rawdata?vehicles=1113&fields=ecu_tires_psi,ecu_tires_tmp,ecu_tpms_conditions,ecu_tpms_provision&duration=P1D&head=1](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=1113&fields=ecu_tires_psi,ecu_tires_tmp,ecu_tpms_warnings,ecu_tpms_provision&duration=P1D&head=1)

```json
[
    {
        "ecu_tires_psi": "019107,018103,017102,016107,035106,03472,033104,032111,050110,049135,002113,001113",
        "ecu_tires_tmp": "01920,01819,01721,01623,03531,03433,03338,03229,05028,04931,00234,00136",
        "ecu_tpms_provision": "100412,01919,01818,01717,01616,03535,03434,03333,03232,05050,04949,0022,0011",
        "event_time": "2020-11-22T18:40:39",
        "ecu_tpms_conditions": "0341030141",
        "system_time": "2020-11-22T18:40:42.747017",
        "vid": 9,
        "id": 916863107747
    }
]
```

> Request tire data (continental example)

> [api/rawdata?vehicles=1113&fields=ecu_tires_psi,ecu_tires_tmp,ecu_tpms_warnings,ecu_tpms_provision&duration=P1D&head=1](https://pegasus1.pegasusgateway.com/api/rawdata?vehicles=1113&fields=ecu_tires_psi,ecu_tires_tmp,ecu_tpms_warnings,ecu_tpms_provision&duration=P1D&head=1)

```json
[
    {
        "ecu_tpms_warnings": "0320030,0330030,0340030,0350030,0000030,0160030,0170030,0180030,0190030",
        "system_time": "2018-10-01T16:51:07.881648",
        "vid": 1113,
        "event_time": "2018-10-01T16:51:05",
        "ecu_tpms_provision": "010310,0191825501010,0181835292341,0171834981980,0161834979294,0351831256396,0341831255008,0331835297967,0321835293060,0011834981950,0001835292410",
        "ecu_tires_psi": "019107,018107,017109,016106,035109,034107,033109,032109,001110,000111",
        "id": 111312277332881,
        "ecu_tires_tmp": "01923,01820,01720,01623,03527,03424,03324,03227,00129,00028"
    }
]
```

Syrus is capable of reading data from different Tire pressure monitoring system sensors, specifically:

* [Doran 360™ TPMS](https://doranmfg.com/the-doran-360-tire-pressure-monitoring-systems-components/) from Doran™
* [ContiPressureCheck](https://www.continental-tires.com/transport/products/overview-product-lines/contipressurecheck) from Continental™

and report the following fields associated with this accessory:

field | description 
-----:|:------------------
ecu_tires_psi | Tire pressure
ecu_tires_tmp | Tire temperature
ecu_tpms_warnings | Tire warnings
ecu_tpms_provision | Tire provisioning
ecu_tpms_conditions | Tire conditions (exclusively for Doran hardware)

All the fields report in a csv format with the corresponding Tire and it's value. 


### Tire location

The tire location can be calculated by taking the low order 4 bits, which represent a position number, counting left to right when facing in the direction of normal vehicle travel (forward). The high order 4 bits represent a position number, counting front to back on the vehicle

You can use the following code to calculate the tire_axle & tire_position (assuming that tire_location is converted from string to integer)

`tire_axle = (tire_location >> 4) & 0x0F`

`tire_position = tire_location & 0x0F`

Example: tire location `019`, corresponds to `tire_axle: 1` & `tire_position: 3`

<aside class="notice">Be aware that the first axle (Axle #0) will report position's 1 & 2, where 2 is the right most tire in that case.</aside>

### Explanation of data

**Tire pressure**

`ecu_tires_psi: 019107,018107,017109,016106,035109,034107,033109,032109,001110,000111`

tire: `019` has a pressure of `107` psi, etc.

> <code style="display: contents">"ecu_tires_psi": "</code><code style="font-weight: bolder; color: red; display: contents;">AAA</code><code style="font-weight: bolder; color: #FBC02D; display: contents;">BBB</code><code style="font-weight: bolder; display: contents">,AAABBB,AAABBB,AAABBB..."</code>

ecu_tires_psi | Description
-------------:|-------------
| <pre style="font-weight: bolder; display: contents; color: red">AAA</pre> | 000 - 255. Tire Location.
| <pre style="font-weight: bolder; display: contents; color: #F9A825">BBB</pre> | Pressure (psi)

--- 

> 

> <code style="display: contents">"ecu_tires_tmp": "</code><code style="font-weight: bolder; color: red; display: contents;">AAA</code><code style="font-weight: bolder; color: #9575CD; display: contents;">BBB</code><code style="font-weight: bolder; display: contents">,AAABBB,AAABBB,AAABBB..."</code>

**Tire temperature**

`ecu_tires_tmp: 01923,01820,01720,01623,03527,03424,03324,03227,00129,00028`

tire: `019` has a temperature of `23` °C, etc.

ecu_tires_tmp | Description
-------------:|-------------
| <pre style="font-weight: bolder; display: contents; color: red">AAA</pre> | 000 - 255. Tire Location.
| <pre style="font-weight: bolder; display: contents; color: #7E57C2">BBB</pre> | Temperature (°C)


--- 

> 

> <code style="display: contents">"ecu_tires_warnings": "</code><code style="font-weight: bolder; color: red; display: contents;">AAA</code><code style="font-weight: bolder; color: #42A5F5; display: contents;">B</code><code style="font-weight: bolder; color: #D4E157; display: contents">C</code><code style="font-weight: bolder; color: #26A69A; display: contents;">D</code><code style="font-weight: bolder; color: #FF5722; display: contents">E</code><code style="font-weight: bolder; display: contents">,AAABCDE,AAABC..."</code>

**Tire Alerts (Continental)**
*for Doran's hardware check out the tire conditions below*

`ecu_tpms_warnings: 0320030,0330030,0340030,0350030,0000030,0160030,0170030,0180030,0190030`

tire `032` has no `0` Alarm warning, `0` TTM not defective, `3` TTM not supported and `0` no battery warnings

ecu_tpms_warnings | Description
-----------------:|------------
| <pre style="font-weight: bolder; display: contents; color: red">AAA</pre> | 000 - 255. Tire Location.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">B</pre> | <pre style="font-weight: bolder; display: contents; color: #42A5F5">Alarm warning</pre>
| <pre style="font-weight: bolder; display: contents; color: #C0CA33">C</pre> | TTM defective (1 if TTM is defective)
| <pre style="font-weight: bolder; display: contents; color: #26A69A">D</pre> | <pre style="font-weight: bolder; display: contents; color: #26A69A">Loose TTM detection</pre>
| <pre style="font-weight: bolder; display: contents; color: #FF5722">E</pre> | Battery warning.

| <pre style="font-weight: bolder; display: contents; color: #42A5F5">Alarm warning</pre> | Description
| -------------:|-------------
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">0</pre> | Ok.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">1</pre> | Under-inflation warning.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">2</pre> | Under inflation alarm.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">3</pre> | Tire leak alarm.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">4</pre> | TTM (Truck tire module) mute.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">5</pre> | Temperature warning.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">8</pre> | TTM over temperature warning.

| <pre style="font-weight: bolder; display: contents; color: #26A69A">Loose TTM detection</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #26A69A">0</pre> | No problem
| <pre style="font-weight: bolder; display: contents; color: #26A69A">1</pre> | TTM loose
| <pre style="font-weight: bolder; display: contents; color: #26A69A">2</pre> | TTM turned
| <pre style="font-weight: bolder; display: contents; color: #26A69A">3</pre> | Not supported


> 

> <code style="display: contents">"ecu_tpms_conditions": "</code><code style="font-weight: bolder; color: red; display: contents;">AAA</code><code style="font-weight: bolder; color: #42A5F5; display: contents;">B</code><code style="font-weight: bolder; color: #D4E157; display: contents">C</code><code style="font-weight: bolder; color: #26A69A; display: contents;">D</code><code style="font-weight: bolder; color: #FF5722; display: contents">E</code><code style="font-weight: bolder; color: #800080; display: contents">F</code><code style="font-weight: bolder; color: #6F4242; display: contents">G</code><code style="font-weight: bolder; color: #F81814; display: contents">H</code><code style="font-weight: bolder; display: contents">,AAABCDEFGH,AAABC..."</code>


**Tire Conditions (Doran)**

`ecu_tpms_conditions: 0341030141`

tire `034` has the sensor status OK (`1`), no airleak (`0`), `3` which is always present, the tire temperature is OK (`0`), system ID is a trailer (`1`), pressure threshold is extreme under pressure (`4`) and the data is valid (`1`)

ecu_tpms_conditions | Description
-----------------:|------------
| <pre style="font-weight: bolder; display: contents; color: red">AAA</pre> | 000 - 255. Tire Location.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">B</pre> | <pre style="font-weight: bolder; display: contents; color: #42A5F5">Sensor Status</pre>
| <pre style="font-weight: bolder; display: contents; color: #D4E157">C</pre> | <pre style="font-weight: bolder; display: contents; color: #D4E157">Air Leak</pre>
| <pre style="font-weight: bolder; display: contents; color: #26A69A">D</pre> | Electrical status (not supported, always 3)</pre>
| <pre style="font-weight: bolder; display: contents; color: #FF5722">E</pre> | <pre style="font-weight: bolder; display: contents; color: #FF5722">Tire Temperature</pre>
| <pre style="font-weight: bolder; display: contents; color: #800080">F</pre> | <pre style="font-weight: bolder; display: contents; color: #800080">System ID</pre>
| <pre style="font-weight: bolder; display: contents; color: #6F4242">G</pre> | <pre style="font-weight: bolder; display: contents; color: #6F4242">Threshold detection</pre>
| <pre style="font-weight: bolder; display: contents; color: #F81814">H</pre> | <pre style="font-weight: bolder; display: contents; color: #F81814">Validity</pre>

| <pre style="font-weight: bolder; display: contents; color: #42A5F5">Sensor Status</pre> | Description
| -------------:|-------------
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">0</pre> | Mute.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">1</pre> | Signal OK.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">2</pre> | Not defined.
| <pre style="font-weight: bolder; display: contents; color: #42A5F5">3</pre> | Defective.

| <pre style="font-weight: bolder; display: contents; color: #D4E157">Air Leak</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #D4E157">0</pre> | No fault
| <pre style="font-weight: bolder; display: contents; color: #D4E157">1</pre> | Fast leak
| <pre style="font-weight: bolder; display: contents; color: #D4E157">2</pre> | Error
| <pre style="font-weight: bolder; display: contents; color: #D4E157">3</pre> | Not supported

| <pre style="font-weight: bolder; display: contents; color: #FF5722">Tire Temperature</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #FF5722">0</pre> | Temperature OK
| <pre style="font-weight: bolder; display: contents; color: #FF5722">1</pre> | Over temperature
| <pre style="font-weight: bolder; display: contents; color: #FF5722">2</pre> | Not used
| <pre style="font-weight: bolder; display: contents; color: #FF5722">3</pre> | Not supported

| <pre style="font-weight: bolder; display: contents; color: #800080">System ID</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #800080">0</pre> | Truck
| <pre style="font-weight: bolder; display: contents; color: #800080">1</pre> | Trailer

| <pre style="font-weight: bolder; display: contents; color: #6F4242">Threshold Detection</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #6F4242">0</pre> | Extreme over pressure
| <pre style="font-weight: bolder; display: contents; color: #6F4242">1</pre> | Over pressure; 25% over baseline
| <pre style="font-weight: bolder; display: contents; color: #6F4242">2</pre> | Pressure good
| <pre style="font-weight: bolder; display: contents; color: #6F4242">3</pre> | Under pressure
| <pre style="font-weight: bolder; display: contents; color: #6F4242">4</pre> | Extreme under pressure; 25% under baseline
| <pre style="font-weight: bolder; display: contents; color: #6F4242">5</pre> | Not defined
| <pre style="font-weight: bolder; display: contents; color: #6F4242">6</pre> | Error
| <pre style="font-weight: bolder; display: contents; color: #6F4242">7</pre> | Unknown
  
| <pre style="font-weight: bolder; display: contents; color: #F81814">Validity</pre> | Description 
| -------------------:|------------
| <pre style="font-weight: bolder; display: contents; color: #F81814">0</pre> | Not valid
| <pre style="font-weight: bolder; display: contents; color: #F81814">1</pre> | Valid

--- 

> 

> <code style="display: contents; overflow-x:scroll;">"ecu_tpms_provision": "</code><code style="font-weight: bolder; color: red; display: contents;">A</code><code style="font-weight: bolder; color: #42A5F5; display: contents;">B</code><code style="font-weight: bolder; color: #D4E157; display: contents">CC</code><code style="font-weight: bolder; color: #26A69A; display: contents;">DD</code><code style="font-weight: bolder; display: contents">,</code><code style="font-weight: bolder; color: #FF5722; display: contents">ZZZ</code><code style="font-weight: bolder; color: #4FC3F7; display: contents">XXXXXXXXXX</code><code style="font-weight: bolder; display: contents">,ZZZXXXX..."</code>

**Tire provisioning**

*continental*
`ecu_tpms_provision: 010310,0191825501010,0181835292341,0171834981...`

*doran*
`ecu_tpms_provision: 100518,03535,03434,03333,03232,06767,01919,...`

system `0` is a truck, whose state is `0` OK, the number of axles is `03`, and the number of TTMs or Tire sensors is `10`

the tire ID `019` has a Sensor ID of `1825501010` dec, or `6CCEEF52` in HEX.

on Doran's hardware you'll notice that the Sensor ID and the tire ID are the same.

ecu_tpms_provision | Description
------------------:|-------------
| <pre style="font-weight: bolder; color: red; display: contents;">A</pre> | System ID (0: Truck, 1: Trailer)
| <pre style="font-weight: bolder; color: #78909C; display: contents;">B</pre> | <pre style="font-weight: bolder; display: contents; color: #78909C">System state.</pre>
| <pre style="font-weight: bolder; color: #D4E157; display: contents">CC</pre> | Number of axles.
| <pre style="font-weight: bolder; color: #26A69A; display: contents;">DD</pre> | Number of tire sensors
| <pre style="font-weight: bolder; color: #FF5722; display: contents">ZZZ</pre> | 000 - 255. Tire Location. 
| <pre style="font-weight: bolder; color: #4FC3F7; display: contents">XXXXXXXXXX</pre> | Sensor ID (decimal) 

| <pre style="font-weight: bolder; display: contents; color: #78909C">System state</pre> | Description 
| ------------:|-------------
| <pre style="font-weight: bolder; display: contents; color: #78909C">0</pre> | Ok.
| <pre style="font-weight: bolder; display: contents; color: #78909C">1</pre> | System malfunction (could indicate DTC errors or bad configuration of the CCU)
| <pre style="font-weight: bolder; display: contents; color: #78909C">2</pre> | System deactivated.

It is recommended to use the tire provisioning field always when working with the TPMS data, for example when determining which sensor was the one that reported the mute alert, it's not enough to just read the TPMS warnings field, rather you have to compare it with the tpms provisioning field at that time.

<aside class-"info">Note that on rare ocassions the TPMS accessory can generate 'null' or empty values for a particular tire, it just means that the sensor inside the tire could not be picked up by the accessory at that time. Before analyzing tire pressure and temperature data it's recommended to filter rows with missing tire information, or fill it in with the previous value.</aside>

<aside class="warning">The system malfunction state can occur on the CPC accessory because the Central Control Unit (CCU) was programmed as a truck system, to have an Additional Receiver (Add. Rx), but physically  you don't have one connected to the test system, this will generate an SPN 611 (FMI 2 or 14, depending on your physical setup) leading to System malfunction.</aside>

### Pushing data to server 

> TPMS Temperature sent along with event data to external resource

> POST http://yourserver.com/webserver/receiver

```json
{
  ...
  "lat": -33.51001,
  "lon": -70.70053,
  "event_time": "2018-09-25T21:19:24+00:00",
  "tpms_tmp": [
    {
      "position": 0,
      "axle": 0,
      "value": 24
    },
    {
      "position": 1,
      "axle": 0,
      "value": 22
    },
    {
      "position": 0,
      "axle": 1,
      "value": 24
    },
    {
      "position": 1,
      "axle": 1,
      "value": 22
    },
    {
      "position": 2,
      "axle": 1,
      "value": 21
    },
    {
      "position": 3,
      "axle": 1,
      "value": 22
    },
    {
      "position": 1,
      "axle": 2,
      "value": 16
    },
    {
      "position": 2,
      "axle": 2,
      "value": 16
    }
  ]
}
```

The TPMS data can be sent to an external server via a [forwarder](#forwarders) in separate fields.
