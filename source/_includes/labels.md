# Labels

> [/api/core_labels](https://cloud.pegasusgateway.com/api/core_labels)

```json
{
  ...
  "ignon": {
    "edited_at": "2017-09-28T14:58:06.220521+00:00",
    "en": "Vehicle ON",
    "created_at": "2017-07-19T21:47:45.858997+00:00",
    "label": "ignon",
    "es": "Vehículo encendido"
  },
  "spd": {
    "edited_at": "2017-10-05T01:51:17.662842+00:00",
    "en": "Speeding:Speeding",
    "created_at": "2017-07-21T13:56:49.184766+00:00",
    "label": "spd",
    "es": "Exceso de velocidad:Exceso de velocidad"
  }
  ...
}

```

> [/api/labels](https://cloud.pegasusgateway.com/api/labels)

```json
{
  "spdingeo": {
    "edited_at": "2017-08-07T02:40:36.570574+00:00",
    "en": "Speeding in Geofence:Speeding inside of geofence",
    "created_at": "2017-07-21T13:57:35.002875+00:00",
    "label": "spdingeo",
    "es": "Exceso en geocerca:Exceso de velocidad en geocerca"
  },
  "ip3onspd10": {
    "edited_at": "2017-11-08T19:55:50.029681+00:00",
    "en": "Input 3 activated and speed between 0-10kph for 10s:Input 3 activated and speed between 0-10kph for 10s",
    "created_at": "2017-11-08T19:55:50.029775+00:00",
    "label": "ip3onspd10",
    "es": "IP3 activo y velocidad entre 0 y 10 kph durante 10 seg:IP3 activo y velocidad entre 0 y 10 kph durante 10 seg"
  }
}
```

> Example where the telemetry device reported an idling event

```json
{
  "events": [
    {
      "head": 263,
      "code": 0,
      "hdop": 0.79,
      "event_time": "2019-01-08T16:31:14",
      "type": 10,
      "vid": 1673,
      "lon": -80.29328,
      "sv": 10,
      "mph": 0,
      "label": "idl",
      "valid_position": true,
      "lat": 25.78386,
      "system_time": "2019-01-08T16:31:17.193250",
      "speed": 0,
      "id": 167320829742193,
      "device_id": 357042062897906
    }
  ]
}
```

Labels are short strings that represent the action that took place on any device, they are meant to give meaning/significance to the messages a telemetry device sends. For example if the Ignition cable on the device was detected ON, the device will associate the label `ignon` to the event that was generated. If the device was going over the speed limit, it will use the label `spd`, and so on. 

For Syrus devices, labels are assigned on the [managed configuration](#managed-legacy-configurations) of a device. The managed configuration has an ev_labels object which relates the code reported by a device (0-99) with a label.

There are two types of labels: core, and site labels.

* `Core labels` cannot be deleted, they are for standard gateway configurations

* `Site labels` are labels that gateway administrators can create, edit and delete.

Core labels can be accessed via [api/core_labels](https://cloud.pegasusgateway.com/api/core_labels). The description of these labels can be updated from the `/labels` api, but core labels cannot be deleted.

Site labels along with core labels can be accessed via [api/labels](https://cloud.pegasusgateway.com/api/labels). 

### Methods

The methods to modify the labels can be found here:

* [Create](https://cloud.pegasusgateway.com/api-static/docs/#api-Labels-CreateLabels)
* [Update](https://cloud.pegasusgateway.com/api-static/docs/#api-Labels-UpdateLabels)
* [Delete](https://cloud.pegasusgateway.com/api-static/docs/#api-Labels-DeleteLabels)

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