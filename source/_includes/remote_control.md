# Device Interaction

Device interaction or the remote control API is used for editing the parameters of [managed configurations](#managed-legacy-configurations), managed configurations are limited in the amount of editable parameters that can be edited, whereas the [device control](#device-control) configurations are much more flexible in the definition and execution of commands.

## Configurations

There are two ways you can configure your Syrus device.  One is with [*managed*](#managed-legacy-configurations) or *legacy configurations*, and the other is with advanced [*device control*](#device-control) configurations.

Managed configurations are predefined scripts with instructions for devices that make them behave in specific ways, these are found under the `/configurations` API. These configurations are used to define basic reporting, and their behavior is more restricted in the sense that they have predefined thresholds and limited parameters that can be edited.

Device control configurations are completely flexible and allow you to create your own configurations as well as edit parts of it using your own parameters.  Essentially, with device control you can build your own API to configure the unit.

### Managed (Legacy) Configurations

> Get a list of the managed/legacy configurations

> GET [api/configurations](https://cloud.pegasusgateway.com/api/configurations)

```json
{
	"s219": {
		"name": {
			"en": "Syrus Standard low data consumption",
			"es": "Estándar de Syrus bajo consumo de datos"
		},
		"warnings": [],
		"limit_remote_console": "20/hour",
		"limit_remote_qpv": "1/hour",
		"restricted_fw": {
			"max_number": 9999999,
			"allow_only_flavor_string": null,
			"min_number": 2001005
		},
		"ky": "s219"
	}
}
```

To get a list the current managed configurations you have access to, go to:

GET `api/configurations`

Response

 Field | Description 
--------: | -------
ky | Unique key that identifies each configuration
name | Name of the configuration
restricted_device |  Configuration is restricted to specific device types
restricted_fw |  Min and max device firmwares allowed to upload configurations (applies to Syrus devices only)
warnings | internal use
limit_remote_console | Limits the amount of commands able to be sent per device via the console
limit_remote_qpv | Limits the amount of position queries able to be sent per device

* `ky` refers to a unique key that identifies each configuration, when you're looking at the parameters for a particular configuration you use the `api/configurations/ky` to reach it.

* `restricted_device` is a restriction for a specific device model, for example a custom third party device can be reporting to Pegasus and you can create a configuration for that specific device

* `restricted_fw` has 3 fields, two for the min & max firmware, these have the format 

* **ABBBCCC**
	* where **A** is the first digit of the firmware
	* **BBB** is the second digit
	* **CCC** is the third digit
	* Using the example on the right for `"min_number": 2001005`
	
	* It's 2 001 005, removing the 0 in front, we end up with a minimum device firmware required to upload the configuration of 2.1.5, compare this to the `version` field in the /devices api to know if your device supports that configuration

* `limit_remote_console` a limit in the amount of [console commands](#send-a-device-command) able to be sent per device

* `limit_remote_qpv` limit on the amount of position queries that can be triggered per device

### View

> View a particular managed configuration

> GET [api/configurations/:ky](https://cloud.pegasusgateway.com/api/configurations/p988)

```json
{
	"ev_labels": {
		"0": "prdtst",
		"1": "trckpnt",
		"2": "ignon"
	},
	"tracking_resolutions": {
		"1": [
			null,
			1800
		],
		"2": [
			null,
			720
		]
	},
	"allowed_cmds": [
		">SRT;ECU<",
		">SXAQQ",
		">SXAICAA1",
		">SXAIC"
	]
}
```

> Example third party device configuration

```json
{
    "restricted_device": "custom",
    "ev_labels": {
        "in1on": "dooropn",
        "in2on": "buzzeron",
        "spd": "excess"
    },
    "ios_names": {
        "io_in1": "SOS",
        "io_in2": "Input #2"
    },
    "params": {
        "safeimmo": false
    }
}
```

To view a managed configuration's contents

GET `api/configurations/:ky`

Response

 Field | Description 
-----: | -------
name | Name of configuration
allowed_cmds | Commands that are permitted to be sent via the [`/remote/console`](#send-a-device-command) command
ev_labels | Event labels
gps_codes_names | Names of the events *deprecated* - use [labels](#labels) instead
ignition_off_event_code | *deprecated*
ignition_on_event_code | *deprecated*
ky | Unique id for the configuration
params | Editable parameters in this config (used in [/remote](#remote-control))
restricted_fw | Minimum firmware needed to load the configuration
sms_alias_actions | SMS alias actions
tracking_resolutions | Tracking resolutions

<div id="evlabels"></div>

### Event Labels

> Update the event labels of a configuration

> PUT [`api/configurations/:ky`](https://cloud.pegasusgateway.com/api/configurations/r021)

```json
{
	"ev_labels": {
		"in1on": "dooropn",
        "in2on": "buzzeron",
        "spd": "excess"
    }
}
```

> Map the numeric codes reported by a device to labels

> PUT [`api/configurations/:ky`](https://cloud.pegasusgateway.com/api/configurations/r021)

```json
{
    "ev_labels": {
        "0": "prdtst",
        "2": "ignon",
        "4": "spd"
    }	
}
```

The event labels can map to codes reported by the Syrus and other devices, or convert one label to another (not both).


### Configuring a device

> Configure a device with a managed configuration

> POST [api/device-config/:imei](https://cloud.pegasusgateway.com/api/device-config/350000000000001)

> Body

```json
{
		"ky": "s219"
}
```

> Response

```json
{
	"msg": "instruction set to device. ",
	"device_is_online": false,
	"oids": [
		543757
	]
}
```

> View progress of configuration on device

> GET [api/devices/:imei/configuration](https://cloud.pegasusgateway.com/api/devices/350000000000001/configuration)

> Response shows us the state of this configuration is pending (state: 1)

```json
{
	"values_rpmlimit_rpm": null,
	"values_satcom_wait_time": null,
	"values_slowtraffic_distance": null,
	"values_satcomevents": null,
	"values_speedlimit_secs": 10,
	"values_satcom_networktest_time": null,
	"values_tracking_resolution": 5,
	"values_idle_secs": null,
	"values_slowtraffic_secs": null,
	"values_tt_alarm_from_deattach": 0,
	"values_photocamevents": "",
	"id": 7765,
	"state": 1,
	"values_timeonlyreport_mins": null,
	"values_speedlimit_mph": 50
}
```

> View pending commands waiting to be sent to the device

> GET [api/devices/:imei/messages/pending](https://cloud.pegasusgateway.com/api/devices/357042062922274/messages/pending)

```json
{
	"queue": [
		{
			"useky": false,
			"uid": 1115,
			"last_try": 1553530605.6320829,
			"_tries": 1,
			"ctype": "consolecmd",
			"time": 1553530605.0752239,
			"msg": "QVR",
			"id": 461
		}
	],
	"outbox": {
		"_epoch": 1553530605.075285,
		"last_id": 462
	}
}
```

In order to change the managed configuration of the device you need to make a POST request to
`api/device-config/:imei` passing `ky` in the body of the request, which corresponds to a particular configuration.

Once the configuration has been set, you can view the status of the configuration with:
GET `api/devices/:imei/configuration`

the state field has the following values

state | description
----: | -----------
-2 | Configuration error
-1 | Configuration mismatch, invalid configuration
0 | Inital state, nothing done since configuration assigment
1 | Device has pending configuration messages
3 | Configuration ready (synchronized)

<aside class="error">Changing a configuration is not reversible and overwrites any previous commands sent.</aside>

<aside class="warning">This API is compatible with any non Syrus devices, however only Syrus devices will receive instructions to change their configuration, other 3rd party devices will not receive any commands</aside>

### Allowing a command 

> allow command `>SRIA` (add RFID tags) on a managed configuration

> PUT [api/configurations/:ky](https://cloud.pegasusgateway.com/api/configurations/s219)

```json
{
	"allowed_cmds": [
		">SXAWA",
		">SXAV",
		">SXAQQ",
		">SRT;SFBUFF",
		">SRT;ECU<",
		">SSSU",
		">SXAGH",
		">SXATT",
		">SXASP",
		">SXAIL",
		">SRIA"
	]
}
```

Lets say that you have an existing configuration and you want to add some RFID tags using the `>SRIA` Syrus command to a device, but the [`remote/console`](#send-a-device-command) command is returning `>RER04` in the response of the command message sent. 

What you need to do is update the managed configuration to allow the `>SRIA` command to be sent via the /remote/console method. 

To do this simply send a PUT request to the managed configurations/ky
with the `allowed_cmds` array with a new entry for the command you want to allow.




## Remote control

> View a list of remote commands to interact with a vehicle's device

> GET [api/devices/:imei/remote](https://cloud.pegasusgateway.com/api/devices/357042062922274/remote)

```json
{
	"fwupdate": {
		"POST": []
	},
	"configuration": {
		"POST": [
			"ky"
		]
	},
	"console": {
		"POST": [
			"cmd"
		],
		"GET": [
			"cid"
		]
	}
}
```

Once a device has a [managed configuration](#managed-legacy-configurations) you are able to interact with other commands.

GET `/api/devices/:imei/remote`

You can also use the vehicle's API to get the remote commands for that particular vehicle's device:

GET `/api/vehicles/:id/remote`

Commands | Method | Params | Description
--------:| -------- | -------- | -------- | 
call | POST | `index` | Call an authorized number
configuration | POST | `ky` | Send a managed configuration* *deprecation warning* - [alternative]()
console | GET | `cid` | View the response of a command sent
console | POST | `cmd` | Send a command to a unit
diagnostic | GET | | Quick connect the unit to the diagnostic server
ecu_state | GET | | View the status of the ECU Monitor parameters
fwupdate | POST | | Firmware update to the latest stable version
gps_status | POST | | Get the device's GPS diagnostic information 
mdt | POST | `message` | Send a message via the MDT 
output | POST | `otype`, `out`, `state` | Set the device's output
outputsetlog | GET | | View a list of the output commands sent
phones | DELETE | `phone` | Delete an authorized number
phones | GET | | View  a list of the authorized phone numbers
phones | POST | `phone` | Authorize a phone number on the device
rpm | GET | | Get the current rpm limit configuration
rpm | POST | `rpm`, `persistencesecs` | Set the RPM threshold that generates an 'over rpm' event
safe_immo | POST | `action` | Send a Safe Immobilization to the device
sms_alias | GET | | View the SMS alias that are configured
sms_alias | POST | `smsaliases` | Create a new SMS alias
sms_alias | PUT | `alias`, `action` | Update an SMS alias
speed | GET | | View the current speed limit threshold
speed | POST | `mph`, `persistencesecs` | Set the speed limit threshold
state | GET | | Get the status of the device's inputs and outputs
tracking_resolution | GET | | View the current configuration of the tracking resolution
tracking_resolution | POST | `resolution` | Set a new tracking resolution for the device
trigger_position_event | POST | | Query the location and generate an event

<aside class="info"> 
In order to perform `GET` remote requests you need 'read' permission on `remote` scope, and `POST` methods require `write` permission on `remote` scope.
</aside>

### Send a device command

> Add a new RFID tag to the device

> POST [/api/devices/:imei/remote/console](https://cloud.pegasusgateway.com/api/devices/357042062922274/remote/console)

```json
{
		"cmd": ">SRIA123456<"
}
```

> Response

```json
{
		"msg": "instruction set to device. ",
		"imei": 357042062922274,
		"via": "outbox",
		"cid": 981,
		"oids": [
				981
		]
}
```

> See the response of the message (command) you just sent

> GET [api/devices/:imei/remote/console?cid=:cid](https://cloud.pegasusgateway.com/api/devices/357042062922274/remote/console?cid=460)

> This response `>RER04` means that the command is not permitted, add it to [`allowed_cmds`](#allowing-a-command) in the managed configuration

```json
{
		"message": ">RER04:SRIA123456;SI=1-1CC;ID=357042062922274"
}
```

> See messages (commands) pending to be sent to the device

> [api/devices/:imei/messages/pending](https://cloud.pegasusgateway.com/api/devices/357042062922183/messages/pending)

```json
{
		"queue": [
				{
						"useky": true,
						"_last_try": 1505466920.614913,
						"_tries": 183,
						"state": false,
						"ctype": "setoutput",
						"otype": "e",
						"time": 1505426401.141027,
						"msg": "SSSXE40",
						"id": 980,
						"out": 4
				},
				{
						"msg": "SRT",
						"id": 981,
						"ctype": "consolecmd",
						"time": 1505712766.7529349
				}
		],
		"outbox": {
				"_epoch": 1505712766.752985,
				"last_id": 982
		}
}
```

> List all commands sent to the device

> [api/devices/:imei/messages/sent](https://cloud.pegasusgateway.com/api/devices/357042062922183/messages/sent)

```json
[
	{
		"outb": {
			"useky": false,
			"uid": 1115,
			"last_try": 1553530605.6320829,
			"_tries": 1,
			"ctype": "consolecmd",
			"time": 1553530605.0752239,
			"msg": "QVR",
			"id": 461
		},
		"epoch": 1553530605.638777
	},
	{
		"outb": {
			"useky": false,
			"uid": 1115,
			"last_try": 1553521312.5816419,
			"_tries": 1,
			"ctype": "consolecmd",
			"time": 1553521312.0280581,
			"msg": "SRIA123456",
			"id": 460
		},
		"epoch": 1553521312.588011
	}
]
```

Sending a [device command](https://syrus.pegasusgateway.com/syrdocsindex/) (also referred to as device messages) can be done with the `api/devices/:imei/remote/console` api, with the `cmd` parameter where you put the command you want to send.

<aside class="notice">
If using the POST <code>console</code> method Make sure the command you want to send to the unit is in the list of <a href="#allowing-a-command">allowed_cmds</a> for that managed configuration or else it will return an Error 04.
</aside>

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>

### Immobilize a vehicle

> Safely immobilize a vehicle

> POST [api/vehicles/:vid/remote/safe_immo](https://cloud.pegasusgateway.com/api/vehicles/123/remote/safe_immo)

> or via the device api

> POST [api/devices/:imei/remote/safe_immo](https://cloud.pegasusgateway.com/api/devices/356612021234567/remote/safe_immo)

```json
{
	"action": true
}
```

> response

```json
{
		"msg": "instruction set to device. ",
		"imei": 356612022409637,
		"response": null,
		"oids": [
				166
		]
}
```

> you can monitor the state of the immobilization via API or Websockets

> GET [api/vehicles/:vid/remote/state](https://cloud.pegasusgateway.com/api/devices/356612022409637/remote/state)

> GET [api/devices?imeis=:imei&select=outbox,ios_state,safeimmo_state](https://cloud.pegasusgateway.com/api/devices?imeis=357042062897906&select=outbox,ios_state,safeimmo_state)

```json
		{
			"safeimmo_state": {
				"instruction": true,
				"_epoch": 1618318377.628331,
				"uid": 1,
				"set_at": 1618318372.517008,
				"cid": 8994
			},
			"imei": 357042062897906,
			"ios_state": {
				"io_pwr": true,
				"io_out2_short": false,
				"_epoch": 1618318402.044902,
				"io_tamper": false,
				"io_ign": false,
				"io_exp_state": false,
				"io_out1_short": false,
				"io_out1": true,             <- output1 indicates when the immobilization is activated
				"io_out2": false,
				"io_in1": false,
				"io_in2": false,
				"io_in3": false
			},
			"outbox": [
				
			]
		}
```

Syrus devices have an immobilization mechanism that waits for certain conditions to meet before activating, in this way the immobilization is _safe_. We'll refer to immobilization as 'safeimmo' from now on. Safeimmo of course requires that the device is installed by a certified person that knows how to install a relay on a vehicle's ignition cable or starter cable with the device's output 1 (blue/red) cable.  Note that output 1 is always used for safeimmo.

The first step in knowing whether or not safeimmo is supported by a particular device is to look for the key `safeimmo_support` in the device's configuration using the device api. 

[api/devices/:imei?select=config](https://cloud.pegasusgateway.com/api/devices/356612022409637?select=config)

> Safe immobilization supported

```json
{
		"config": {
				"kydef": {
						"lumeway": false,
						"secugen": false,
						"garminmode": false,
						"safeimmo": true,
						"satcom": false,
						"_read_at": null,
						"photocam": false,
						"ios_names": {}
				},
				"kymod": {},
				"_config_state": 1,
				"_epoch": 1617041571.785082,
				"safeimmo_support": true,
				"values": {
						"values_rpmlimit_secs": null,
						"values_rpmlimit_rpm": null,
						"values_speedlimit_secs": 10,
						"values_speedlimit_mph": 50
				},
				"ky": "q740"
		}
}
```

**Important Notes**
If safeimmo is active and the device's configuration is changed from one that supports safeimmo and to one that does not support it, then the output 1 will go to false, and you can use the `/remote/output` to activate/deactivate the device's outputs.

Once a configuration is sent to the device, it takes some time for the instructions to reach the device, you can look at the `_config_state` and if it's = 1 it's pending, 3 = it's done.

We recommend waiting for the configuration to be in synched state before sending any instruction to safely immobilize a vehicle.

After you send the command you can monitor the response via api or via websockets, the `api/devices/:imei?select=outbox,ios_state,safeimmo_state,config` will give you the state of everything relevant about the safe immobilization. 

`outbox`: whether or not there is an instruction pending to be sent to the device. Here you'll see an array of objects with instructions of `"ctype": "seco"` if the `"msg": "SXASES1"` it indicates it's an activation of safeimmo.  If you see `"msg": "SXASES0"` it's a deactivation of safeimmo.

`safeimmo_state`: indicates the pending instruction and the time of it's last activation

`ios_state`: indicates the status of the device's output, `io_out1` is always used for the safeimmo support.

`config`: indicates whether or not the device supports safeimmo using the `safeimmo_support` key.

> Safeimmo Walkthrough

> Step 1: Safeimmo supported by the device's configuration 

> [api/devices/:imei?select=config](https://cloud.pegasusgateway.com/api/devices/356612022409637?select=config)

```json
 				"safeimmo_support": true,
```

> Step 2: Command to immobilize is sent

> POST [api/devices/:imei/remote/safe_immo](https://cloud.pegasusgateway.com/api/devices/356612021234567/remote/safe_immo)

```json
{
	"action": true
}
```

> Step 3: Waiting for device to receive instruction

> GET [api/devices/:imei?select=outbox,ios_state,safeimmo_state,config](https://cloud.pegasusgateway.com/api/devices/356612021234567?select=outbox,ios_state,safeimmo_state,config)


```json
{
		"outbox": [
				{
						"useky": true,
						"uid": 1,
						"last_try": 1618318372.491439,
						"_tries": 1,
						"ctype": "seco",
						"time": 1618318372.419283,
						"msg": "SXASES1",
						"id": 8994
				}
		]
}
```

> Step 4: Instruction sent

> GET [api/devices/:imei?select=outbox,ios_state,safeimmo_state,config](https://cloud.pegasusgateway.com/api/devices/356612021234567?select=outbox,ios_state,safeimmo_state,config)

> outbox is empty, but the io_out1 is still not active, waiting for the instruction to be executed on the device

```json
	{
		"safeimmo_state": {
				"instruction": true,
				"_epoch": 1618318377.628331,
				"uid": 1,
				"set_at": 1618318372.517008,
				"cid": 8994
		},
		"imei": 357042062897906,
		"ios_state": {
				"io_pwr": true,
				"io_out2_short": false,
				"_epoch": 1618316545.294416,
				"io_tamper": false,
				"io_ign": false,
				"io_exp_state": false,
				"io_out1_short": false,
				"io_out1": false,
				"io_out2": false,
				"io_in1": false,
				"io_in2": false,
				"io_in3": false
		},
		"outbox": []
}
```

> Step 5: Action executed confirming that the safe immobilization is complete

> GET [api/devices/:imei?select=outbox,ios_state,safeimmo_state,config](https://cloud.pegasusgateway.com/api/devices/356612021234567?select=outbox,ios_state,safeimmo_state,config)

```json
{
		"safeimmo_state": {
				"instruction": true,
				"_epoch": 1618318377.628331,
				"uid": 1,
				"set_at": 1618318372.517008,
				"cid": 8994
		},
		"imei": 357042062897906,
		"ios_state": {
				"io_pwr": true,
				"io_out2_short": false,
				"_epoch": 1618318402.044902,
				"io_tamper": false,
				"io_ign": false,
				"io_exp_state": false,
				"io_out1_short": false,
				"io_out1": true,
				"io_out2": false,
				"io_in1": false,
				"io_in2": false,
				"io_in3": false
		},
		"outbox": []
}
```

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

### Activate output 2

> Activating embedded output 2 on vehicle 2063

> POST [api/vehicles/2063/remote/output](https://cloud.pegasusgateway.com/api/vehicles/2063/remote/output)

```json
{
		"otype": "n",
		"out": 2,
		"state": true
}
```

> Deactivating embedded output 1 on vehicle 2063

> POST [api/vehicles/2063/remote/output](https://cloud.pegasusgateway.com/api/vehicles/2063/remote/output)

```json
{
		"otype": "n",
		"out": 1,
		"state": false
}
```

> State of output 

> GET [api/vehicles/:vid/remote/state](https://cloud.pegasusgateway.com/api/devices/356612022409637/remote/state)

```json
{
		"ionames": {...},
		"ios": {
				"io_pwr": true,
				"evid": 19711112435180,
				"io_in1": false,
				"evtime": 1505711767,
				"io_exp_state": false,
				"io_out1": false,
				"io_out2": false,        <<<<<<<<<<<<<<<<<<<
				"systime": 1505711770.180109,
				"trip_id": 19711108701981,
				"io_ign": true,
				"io_in2": false,
				"io_in3": false
		}
}
```


Activating output 2 is done with the `/api/vehicles/:vid/remote/output` api, making a POST request with `otype`, `out`, `state` as parameters.

If using an IO expander accessory check out [this section](https://docs.pegasusgateway.com/#input-output-expander).

Param | Type | Description
-----:|------|------------
otype | String | Use device embedded output: "n", or external IO Expander accessory: "e"
out | Number | Output number
state | Boolean | True to activate output (when an output is activated it's grounded)

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>



### Make a Call

> Authorize phone number +13058675309

> POST [api/devices/:imei/remote/phones](https://cloud.pegasusgateway.com/api/devices/357042062920955/remote/phones)

```json
{
	"phone": "+13058675309"
}
```

> Response

```json
{
	"msg": "instruction set to device. ",
	"imei": 357042062920955,
	"oids": [
		48
	]
}
```

> View the phone number we just configured

> GET [api/devices/:imei/remote/phones](https://cloud.pegasusgateway.com/api/devices/357042062920955/remote/phones)

```json
{
	"phones": [
		{
			"action": "1",
			"access": "0",
			"phone": "+13058675309"
		}
	],
	"errors": [],
	"pending": true
}
```

> Make a call to the number +13058675309

> POST https://cloud.pegasusgateway.com/api/vehicles/1956/remote/call

```json
{
		"index": 0
}
```

This section describes how to generate calls directly from the device (this is unlike the trigger's phone call generation because this is directly from the device, triggers uses a third-party api in order to make the call). Note that in order for your device to be able to make a phone call the SIM card has to be provisioned with voice services.

Start by authorizing a phone number on one of the 5 available slots for every device. (index 10-14)

`POST /api/vehicles/:vid/remote/phones`

or

`POST /api/devices/:imei/remote/phones`

<code>
{<br>
&nbsp;&nbsp;"phone":"+13058675309"<br>
}<br>
</code>

then make a call to that number with the remote method `call`

`POST /api/vehicles/:vid/remote/call`

<code>
{<br>
&nbsp;&nbsp;"index": 0<br>
}<br>
</code>

Note that the indexes must be between 0 and 4.

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>


## Device actions 

### Silencing events

The `api/device-mute/:imei` API enables muting, un-muting Syrus' event codes generation.
Muting means that the events are processed internally on the device and never reach the server. This is useful in cases where the device is generating a lot of false events like inputs activation, and you'd like to stop the events from generating because it's consuming lots of data. Simply `mute` the event, then if the problem is fixed `un-mute` the event.

When a mute action is POSTed for an imei the following occurs:

* If `code` is already-muted, nothing is done.
* else `QEDcc` is pushed to outbox.
* The response `REDccABCD[...]` is transformed to `SEDccABCDE[...]` and stored
* `SEDccSBCDE[...]` is pushed to outbox.

When an un-mute action is POSTed:

* If device is already in `un-muted/un-listed`, nothing is done
* else, the previously stored `SEDccABCDE[...]` is pushed to outbox

### GET

> GET api/device-mute/356612024151062

```json
{
	"evc-1": {
		"state": "querying",
		"oid": 161,
		"ev_code": 1,
		"uid": null,
		"time": 1520865531.0303299
	}
}
```

> As device object too (live package and /devices API):

```json
{
	"imei": 356612024151062,
	"muted_evs": {
		"_epoch": 1520881684.852546,
		"events": {
			"evc-1": {
				"uid": 1,
				"oid": 123,
				"qed_response": "RED01NV0;C00+",
				"state": "muted",
				"restore_command": "SED01NV0;C00+",
				"ev_code": 1,
				"time": 1520881682
			}
		}
	}
}
```

An object containing muted-events is returned. Its keys follow the format evc-{evcode}

### Note
Only possibly muted event codes are returned. Check state to confirm event is actually muted/un-muted

### Muted event object

 Param | Description 
--------- | -------
ev_code |  Event code
state |  Muted-state, see below
oid |  OutBox ID of last OutBox push
uid |  User that gave the last instrucion (mute/un-mute)
time |  Object update epoch


### state

 State         | Description
-------------- | ----------
querying | QED sent, waiting response. Code is in process of being muted
muting | Got QED response, SEDxxS sent. Code is in process of being muted
muted | Got confirmation of SEDzzS. Code is muted.
un-muting | SEDxx-restore sent. Code is in process of being un-muted
un-muted | Got response to SEDxx-restore. Code is un-muted. This state is is equivalent as not having the muted-event returned on the muted-events object. This is state is temporal, yields to object being eliminated.
ev-undefined | Tried silencing an undefined event. This is state is temporal, yields to object being eliminated.

### POST

> POST api/device-mute/460001331053605 

```json
{
	"code": 1,
	"action": "mute"
}
```

```json
{
	"action": "mute",
	"imei": 350000000000001,
	"code": 1,
	"events": {
		"evc-1": {
			"state": "querying",
			"oid": 42,
			"ev_code": 1,
			"uid": 1,
			"time": 1520868125.9707451
		}
	}
}
```

**POST Params**

Params | type | description
------ | ---- | -----------
action | string | `mute` or `un-mute`
code   | number | numeric event code

**Returns**

 Param | Description 
--------- | -------
action | action
code | code
imei | device imei
events | the same object returned by GET /device-mute/<imei>

<aside class="info"> 
	Note that if you send a new <a href="#configuring-a-device">configuration</a> to a device the device mute events are cleared
</aside>
