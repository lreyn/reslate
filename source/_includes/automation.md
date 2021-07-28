# Automation

> Trigger categories

> **GET** [/api/resources/triggers/structure](https://cloud.pegasusgateway.com/api/resources/triggers/structure)

```json
{
      "structure": {
        "site": {
              "status": {
                "rule_keys": [],
                "key": "pid"
            },
            "notification": {
                  "rule_keys": [],
                "key": "pid"
            },
            "$default": "pid"
        },
        "user": {
              "access": {
                    "rule_keys": [],
                "key": "id"
            },
            "$default": "uid"
        },
        "vehicle": {
              "device-diagnostic": {
                    "rule_keys": [],
                "key": "vehicle"
            },
            "$default": "vid",
            "event": {
                  "rule_keys": [
                        "ac",
                    "ad",
                    ...                    
                    "vid",
                    "vo"
                ],
                "key": "vid"
            },
            "counter": {
                  "rule_keys": [
                        "dev_dist",
                    "dev_idle",
                    "dev_ign",
                    "dev_orpm",
                    "dev_ospeed",
                    "ecu_dist",
                    "ecu_eidle",
                    "ecu_eusage",
                    "ecu_ifuel",
                    "ecu_tfuel"
                ],
                "key": "vehicle"
            }
        }
    }
}
```


Automation allows you to create specific conditions (rules), using boolean logic that apply to specific categories within the api.

The categories exposed so far are **vehicle_events** and **counters**, they can be found in `/api/resources/triggers/structure`.

The combination of one or more rules and any subsequent action performed is called a **trigger**. 

## Triggers

> Trigger operators

> **GET** [/api/resources/triggers/rules](https://cloud.pegasusgateway.com/api/resources/triggers/rules)

```json
{
    "rules": {
        "postfix": {},
        "special": {
            "inside_fence": [...],
            "within": [...],
            "exited_fence": [...],
            "entered_fence": [...],
            "delta": [...],
            "delta+": [...],
            "delta-": [...],
            "outside_fence": [...]
        },
        "standard": [
            ">=",
            "==",
            "<=",
            "!=",
            "<",
            ">"
        ]
    }
}
```

To create a **vehicle_events** trigger the structure shows that we can use any of the `rule_keys` which are the [fields](/#master-fields-list) that a device reports to the Gateway and apply logic to the values received, these fields operate over the key defined in the structure, which is the vehicle id: `vid`.

__rules__ is an array of objects that represent the conditions for firing the trigger, they consist of:

* **key** (from [rule_keys](https://cloud.pegasusgateway.com/api/resources/triggers/structure))
* **operator** (from [standard and special](https://cloud.pegasusgateway.com/api/resources/triggers/rules))
* **value** (value for the rule)
* **id** (name of the rule)

Example that's true when the [label](#labels) reported by the device is `idl` (Idling)

`{`<br>
&nbsp;&nbsp;`"id": "idling",`<br>
&nbsp;&nbsp;`"key": "label",`<br>
&nbsp;&nbsp;`"operator": "==",`<br>
&nbsp;&nbsp;`"value": "idl"`<br>
`}`

### Create a trigger

> __Step 1__: Create an empty trigger with: name, type, and category

> **POST** [/api/triggers](https://cloud.pegasusgateway.com/api/triggers)

```json
{
      "name": "My special trigger",
    "description": "Simple trigger template",
    "type": "event",
    "category": "vehicle",
    "rules": [],
    "postfix": [],
    "vehicles": [],
    "options": {}
}
```

> __Step 2__: Create the rule(s) for activating the trigger
> Triggers can contain multiple rules, although in most cases a few will suffice.

> **PUT** [/api/triggers/:id](https://cloud.pegasusgateway.com/api/triggers/123)

```json
{
      "name": "My special trigger",
    "description": "Trigger template with rules",
    "type": "event",
    "category": "vehicle",
    "timezone": "America/New_York",
    "rules": [
          {
                "id": "moving",
            "key": "mph",
            "operator": ">=",
            "value": 20
        },
        {
              "id": "input1_press",
            "key": "io_in1",
            "operator": "==",
            "value": true
        }
    ],
    "postfix": [],
    "vehicles": [],
    "options": {}
}
```

> __Step 3__: Give the rules some logic in the postfix

> The logic is combined in a post fixed notation syntax using "and", "or", "not". Post-fixed notation means that the rule's IDs are placed first, and the operator is at the end of the rule ids to be evaluated, the rule's id and the operators are separated by a comma.

> When only 1 rule is used, the postfix just needs the name of the ID used on the trigger rule.

> **PUT** [/api/triggers/:id](https://cloud.pegasusgateway.com/api/triggers/123)

```json
{
      "name": "My special trigger",
    "description": "Trigger for moving and input 1 pressed",
    "type": "event",
    "category": "vehicle",
    "timezone": "America/New_York",
    "rules": [
          {
                "id": "moving",
            "key": "mph",
            "operator": ">=",
            "value": 20
        },
        {
              "id": "input1_press",
            "key": "io_in1",
            "operator": "==",
            "value": true
        }
    ],
    "postfix": ["moving","input1_press","and"],
    "vehicles": [],
    "options": {}
}
```

> __Step 4__: Assign the vehicles or groups that will be applied to the trigger

> **PUT** [/api/triggers/:id](https://cloud.pegasusgateway.com/api/triggers/123)

```json
{
      "name": "My special trigger",
    "description": "Trigger for moving and input 1 pressed on group 123 & vehicle id: 1673",
    "type": "event",
    "category": "vehicle",
    "timezone": "America/New_York",
    "rules": [
          {
                "id": "moving",
            "key": "mph",
            "operator": ">=",
            "value": 20
        },
        {
              "id": "input1_press",
            "key": "io_in1",
            "operator": "==",
            "value": true
        }
    ],
    "postfix": ["moving","input1_press","and"],
    "groups": 123,
    "vehicles": [1673],
    "options": {}
}
```

> __Step 5__: Verify that the trigger generated reaches the [trigger logs](https://cloud.pegasusgateway.com/api-static/docs/#api-Triggers-GetTriggerLogs)

> Make sure the conditions are met and check the trigger logs api for results, you can use the `full` param to see all the details of the trigger generated

> **GET** [/api/trigger_logs?from=2020-06-01&triggers=123&full=1](https://cloud.pegasusgateway.com/api/trigger_logs?from=2020-12-25T00:00:00&triggers=123)

```json
{
      "category": "vehicle",
    "trigger_snapshot": {},
    "level": null,
    "tuuid": "71df2610-ae6a-11ea-8fe8-062dc07e7ac3",
    "fields": {},
    "object_snapshot": {},
    "pid": 1,
    "object_id": 1673,
    "trigger": 123,
    "time": 1592158278.843752,
    "type": "event",
    "day": 20200614
}
```

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
&nbsp;<br>&nbsp;<br>&nbsp;<br>
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
&nbsp;<br>&nbsp;<br>&nbsp;<br>


#### Special operator

> Special operators

```json
{
    "rules": {
        "special": {
            "inside_fence": [
                "fences",
                "types"
            ],
            "within": [
                "collection"
            ],
            "exited_fence": [
                "fences",
                "types"
            ],
            "entered_fence": [
                "fences",
                "types"
            ],
            "delta": [
                "value",
                "start",
                "end",
                "pre",
                "post",
                "freq"
            ],
            "delta+": [
                "value",
                "start",
                "end",
                "pre",
                "post",
                "freq"
            ],
            "delta-": [
                "value",
                "start",
                "end",
                "pre",
                "post",
                "freq"
            ],
            "outside_fence": [
                "fences",
                "types"
            ]
        }
    }
}
```

The special operators are: 

operator | description
---------|-------------
`within` | True for a value found within an array of values
`inside_fence` | True while the event generated is inside a geofence, or geofence_type
`outside_fence` | True while the event generated is outside a geofence, or geofence_type
`exited_fence` | True when the event generated goes from inside to outside of the geofence or geofence_type
`entered_fence` | True when the event generated goes from outside to inside of the geofence or geofence_type
`delta` | True when a positive or negative incremental value matches 
`delta+` | True when a positive incremental value matches
`delta-` | True when a negative incremental value matches

&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>
&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>

> Rules with special operators:

> **within**

```json
{
    "id": "out1_or_out2",
    "key": "label",
    "operator": "within",
    "value": [
        "out1on",
        "out2on"
    ]
}
```

> **inside_fence**

> Note: when using the fence related operators you must define a key, in this case we can set it to `"lat"` (latitude) always

```json
{
    "operator": "inside_fence",
    "id": "in_geo",
    "value": {
        "fences": [
            164
        ],
        "types": [
            123
        ]
    },
    "key": "lat"
}
```

> **delta+**

```json
{
    "id": "hourmeter_1hr",
    "operator": "delta+",
    "key": "vehicle_dev_ign",
    "value": {
        "value": "3600",
        "start": "10000",
        "end": "0",
        "pre": "1200",
        "post": "1800",
        "freq": "600"
    }
}
```

**delta+**

The delta+ operator can be used to notify when a device reports in increments of a particular value. 
The only field required is the _value_ which is the value that it's going to be true every.

The delta+ operator can be customized with the following fields:

field | description
------|-------------
`value` | Value that it's true every
`start` | Start at a particular value
`end` | End at a particular value
`pre` | True this much before the `value`
`post` | True this much after the `value`
`freq` | True every this much between `pre` and `post` values

For example, assuming we're using the key `dev_dist`.

Notify every 10,000 km

* `"value": 10,000,000` (meters)

Notify every 10,000 km from 40,000 to 60,000 km

* `"value": 10,000,000`
* `"start": 40,000,000`
* `"end": 60,000,000`

Notify every 10,000 km from 40,000 to 60,000 km and notify every 1,000 km, 5,000 km before and 6,000 km after the 10,000 km.

* `"value": 10,000,000`
* `"start": 40,000,000`
* `"end": 60,000,000`
* `"pre": 5,000,000`
* `"post": 6,000,000`
* `"freq": 1,000,000`

Since the odometer won't hit exactly 10,000 km the trigger will fire the first time the value passes 10,000 km.

<aside class="notice">Remember that triggers operate over events that come into the system. Thus, if the event has a delay they will be evaluated in the order received, unless that delay is several weeks old in which case the trigger will not generate actions.</aside>


### Trigger processes (actions)

Trigger processes are the actions performed once a trigger's posfix rules have been met. 
The process can be sending an email, generating a report, consuming the API, making a voice call, text message, send a POST to any resource, etc.


> Send an email

```json
{
    "processes": [
        [
            "email_event",
            {
                "destinations": [
                    "john@email.com",
                    "luke@email.com"
                ]
            }
        ]
    ]
}
```

> Consume resource

```json
{
    "processes": [
        [
            "consume_resource",
            {
                "headers": {
                    "Content-Type": "application/json"
                },
                "resource": "http://50.232.21.70/api/2d1c0bf32ecc934f26c5ceeb2558dac7/lights/1/state",
                "method": "put",
                "template": "{\"on\":true, \"sat\":254, \"bri\":254,\"hue\":65535}"
            }
        ]
    ]
}
```
