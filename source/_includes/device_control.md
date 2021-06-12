# Device Control

The Device Control API gives you the ability to send commands to a device and build your own scripts, it is a more flexible alternative to using a [managed configuration](#managed-legacy-configurations) which has predefined behavior and parameters.

Everything starts with creating your own API definitions.

## Intro 

> Viewing model & configuration definitions

> Model with an instruction that tells the device to restart itself

> Model: `syrus.core.restart.model`

> GET [api/dcontrol/definition/syrus.core.restart.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.restart.model)

```json
{
  "tag": "syrus.core.restart.model",
  "type": "simple",
  "cmds": [
    ">SRT<"
  ]
}
```

> Model with multiple instructions (commands) for the device

> Model: `syrus.standard.hour_report.model`

> GET [api/dcontrol/definition/syrus.standard.hour_report.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.standard.hour_report.model)

```json
{
  "tag": "syrus.standard.hour_report.model",
  "type": "simple",
  "cmds": [
    ">SGC00TR03600<",
    ">SED00NV0;C00+<"
  ]
}
```

> Configuration definition with the two models above

> Configuration: `syrus.standard.testing.config`

> GET [api/dcontrol/definition/syrus.standard.testing.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.standard.testing.config)

```json
{
  "name": "Sample Config",
  "allowedConsoleCmds": [],
  "models": [
    "syrus.core.restart.model",
    "syrus.standard.custom_periodic_reporting.model"
  ],
  "evLabels": {},
  "tag": "syrus.standard.testing.config",
  "ky": "a1kt",
  "description": "Syrus Sample Config for use in docs"
}
``` 

The Device Control API is made up of *definitions*.  
A definition is an API, which you can define, that holds the device commands used to configure the unit.  It's also used to reference which commands can be sent to the device.

There are two types of definitions, **Models** and **Configurations**. 

*Models* are definitions that have the actual device commands, while *Configurations* are a collection of models.

A device is configured with one *Configuration* at a time, this configuration can have any number of models defined.

To simplify this, imagine you're working with the standard device configuration, you can think of models as the commands needed to execute an event.  For example, almost all configurations have a tracking resolution, which is an event that's used to define how often the device reports when the ignition is ON, you can have a model called `standard_tracking_resolution` with editable parameters that define the time & heading between reports, or you can make your own model `standard_tracking_resolution_with_distance` that includes a new parameter for defining a distance between reports.

Note that **all Models definitions API end with `.model` while Configurations definitions API ends with `.config`**.
So when we defined the earlier models, they should actually be named: `standard_tracking_resolution.model` and `standard_tracking_resolution_with_distance.model`.  

Finally, remember that in order to send a command to the unit it **MUST** exist in that device's configuration.  
In other words, if you want to send a command that calibrates the accelerometers movement (>SXAICAR1<) for example, the model that accomplish this must be included in the device's configuration `.config`.


#### Applications and Uses

The Device API has many applications and can be used a number of ways, while there's no right or wrong way to use it we do encourage you to stick with the following.

Models

 - Make them concise (3-4 events at most)
 - Use parameters when possible 
 - Make them generalized, so they can apply in many applications
 - Use labels to describe the functionality

Configurations

 - Separate them by device models and applications
 - Add a description 
 - Clone from the base 
 - Use versions

#### Things to Consider

The Device Control API has lots of benefits and features, some of the more notable benefits are:

- Simple / Easy to understand API commands rather than complex protocol commands
- Allows synchronization of a configuration across multiple devices simultaneously
- Access to a graphical user interface to use the Device control commands

however there are some things that you need to keep in mind:

- In many cases you will need to refer to the device's hardware programming language
- There's no guarentee that the models you use to program the device are compatible with each other

### Permissions 

> GET [api/dcontrol/access](https://pegasus1.pegasusgateway.com/api/dcontrol/access)

```json
{
  "edit": [
    "pegasus.*",
    "syrus.*"
  ],
  "create": [
    "pegasus.*",
    "syrus.*"
  ],
  "delete": null,
  "view": [
    "pegasus.*",
    "syrus.*"
  ]
}
```

The last thing you need to know before you get started creating your first definitions is that you need to have access to create them.  By default you're only allowed access to create models whose name start with the list found here: `api/dcontrol/access`

The response to this API will tell you what the name of the definitions you're able to edit, create, view, and delete start with.

For example,

<code>
&nbsp;&nbsp;"edit": [<br>
&nbsp;&nbsp;&nbsp;&nbsp;"pegasus.\*",<br>
&nbsp;&nbsp;&nbsp;&nbsp;"syrus.*"<br>
&nbsp;&nbsp;],<br>
</code>

means you can edit definitions that start with the word `pegasus.` and `syrus.` 


<aside class="success">As a general rule, we use definitions that start with the name `pegasus` for the actual configurations, and `syrus` for remote commands.</aside>

## Definitions

### View

> View all definitions

> GET [api/dcontrol/definitions](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions)

```json
[
  "syrus.core.restart.model",
  "pegasus.test.all.config",
  "pegasus.lr.standard.config",
  "syrus.list.geofences.circular.model"
]
```

> View definitions that start with 'syrus.core'

> GET [api/dcontrol/definitions?name=syrus.core.\*](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions?name=syrus.core.*)

```json
[
  "syrus.core.restart.model",
  "syrus.core.baud_rate.model"
]
```

To view the definitions you can make a `GET` request to `api/dcontrol/definitions`
The definitions resource accepts a URL param called `name` which lets you filter the definitions using a regular expression.

<aside class="success">In regular expressions you can use an asterisk (*) which essentially works as a wildcard to match anything.</aside>


### Create

> Creating a new definition

```json
{
  "dryRun": false,
  "definition": {
  	...
  	insert model or configuration params here
  	...
  }
}
```

In order to create a model or configuration definition, we need to make a POST request to `api/dcontrol/definition` with the following two parameters **dryRun** and **definition**.

| Parameter | Description | Type |
| --------- | ----------- | ---- |
| dryRun | true if you want to test the request | Boolean |
| definition | The actual definition, model or configuration | Object |


## Models 

### View

> View all models

> [api/dcontrol/definitions?name=*.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions?name=*.model)

```json
[
  "syrus.core.gps.filter.model",
  "syrus.acc.ecu.vin.model",
  "pegasus.standard.harsh_accel_braking.model",
  "pegasus.standard.position_request.model",
  ...
]
```

> View all models that contain the word 'gps'

> [api/dcontrol/definitions?name=.*gps*.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions?name=.*gps*.model)

```json
[
  "syrus.core.gps.filter.model",
  "pegasus.standard.no_gps.model",
  "syrus.core.gps.filter.copy.model",
  "pegasus.standard.no_gps.copy.model"
]
```

To view all the models you have access to, you can make a GET request to the `api/dcontrol/definitions` resource, with a URL param filter called: `name`, where you can pass it a regexp that matches the definition you're looking for, in this case we can use `name=.*.config` to refer to all definitions 

#### Particular Model

> View a particular model

> [api/dcontrol/definition/syrus.acc.ecu.vin.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.acc.ecu.vin.model)

```json
{
  "name": "VIN Number",
  "description": "Get the Vehicle's VIN",
  "tag": "syrus.acc.ecu.vin.model",
  "type": "simple",
  "cmds": [
    ">QXAZV<"
  ]
}
```

> Get tracking with Ignition ON model 

> [api/dcontrol/definition/pegasus.standard.tracking.ignition.on.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard.tracking.ignition.on.model)

```json
{
  "name": "Tracking Resolution",
  "description": "Periodic reports while the ignition is ON."
  "tag": "pegasus.standard.tracking.ignition.on.model",
  "paramsDefs": {
    "min_time": {
      "units": "seconds",
      "range": [
        5,
        86400
      ],
      "type": "number",
      "label": "Minimum time between events"
    },
    "heading": {
      "units": "degrees",
      "range": [
        5,
        120
      ],
      "type": "number",
      "label": "Heading in degrees"
    },
    "max_time": {
      "units": "seconds",
      "range": [
        5,
        86400
      ],
      "type": "number",
      "label": "Maximum time between events"
    }
  },
  "initialValues": {
    "min_time": 10,
    "heading": 45,
    "max_time": 180
  },
  "type": "simple",
  "cmds": [
    ">SXAGH001{{params.heading|fix 3}}<",
    ">SED97SV0;F00+;ACT=SGC01TR{{params.max_time}};ACT=SGC19TC{{params.min_time |fix 5}}<",
    ">SED95SV0;J00F00&+;ACT=SGC01TR{{params.max_time |fix 5}}<",
    ">SED01NA0;C01J00|C19&+;XCT=SGC19TC{{params.min_time |fix 5}}<",
    ">SED82SV0;F00!+;ACT=SGC01U<"
  ]
}
```

To see a particular model you can make a request to the `api/dcontrol/definition` resource.

**Response**

| Param | Description | Type |
| ----- | ----------- | ---- |
| name | Name of the Model | String |
| description | Descriptive text for the model | String |
| tag | Short name of the model | String | 
| paramDefs | Parameters for the model | Object |
| initialValues | Initial values using the parameters of the model | Object |
| cmds | Programming commands for the Syrus | Array |
| evLabels | Event Labels for the model | Object |
| keywords | Short codes that can be used to group similar models | Array |


<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br>

### Create 

> Create a simple model for restarting the device 

> POST [api/dcontrol/definition/syrus.core.restart.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.restart.model)

```json
{
  "dryRun": false,
  "definition": {
  	"name": "Restart",
    "description": "Restart device",
    "type": "simple",
    "cmds": [
      ">SRT<"
    ],
    "keywords": [
      "core"
    ]
  }
}
```

> Create a ulist model for creating circular regions

> POST [api/dcontrol/definition/syrus.list.regions.circular.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.list.regions.circular.model)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Circular Regions",
    "description": "Create circular regions on the device",
    "type": "ulist",
    "initialValues": {
      "0": {
        "radius": 50
      }
    },
    "ulistRange": [
      0,
      99
    ],
    "paramsDefs": {
      "radius": {
        "type": "number",
        "range": [
          50,
          65000
        ]
      }
    },
    "cmds": [
      ">SXAIR{{uindex|fix 2}}{{uparams.radius|fix 6}}<"
    ],
    "keywords": [
      "list"
    ]
  }
}
```

In order to create a model you have to make a request to the <br>
`api/dcontrol/definition/`*modelname*`.model` <br>
where *modelname* is the unique name you want to give to your model. <br>

<aside class="notice">
Remember that it has to start with the name that's defined in the <a href="#permissions">permission</a> section, and it must end with <code>.model</code></aside>

#### POST Body

**Simple Model**

| Parameter | Description | Type | Required |
| --------- | ----------- | ---- | -------- |
| name | Name of the model | String | |
| type | Type of model (**simple**) | String | yes |
| cmds | Syrus commands | Array | yes |
| paramsDefs | Parameters for model | Object | |
| initialValues | Initial values for model | Object | |
| evLabels | Labels for the event generated | Object | |
| keywords | Shortcodes that can be used to group similar models | Array | |

**UList Model**

| Parameter | Description | Type | Required |
| --------- | ----------- | ---- | -------- |
| name | Name of the model | String | |
| type | Type of model (**ulist**) | String | yes |
| cmds | Syrus commands | Array | yes |
| paramsDefs | Parameter definitions for the model | Object | |
| initialValues | Initial values for model | Object | |
| ulistRange | List range | Array | yes |
| ulistUnsetCmdUIndex | Index location to Undefine command | Integer | yes |
| keywords | Short codes that can be used to group similar models | Array | |


### Parameter Definitions

There are 3 main `types` of paramDefs in a simple model:

- Number
- Text
- Boolean (Numberbool)

When making these paramDefs you're able to define the following additional parameters

| Parameter | Description | Type | 
| --------- | ----------- | ---- |
| type | Parameter type: number, text, boolean | String |
| range | Range of numbers | Array |
| list | List of items (number & text types) | Array |
| labels | Short description | String |
| units | Units for parameter | String |



#### Number

> Create a model for setting the camera resolution 

> POST [api/dcontrol/definition/syrus.acc.photocam.resolution.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.acc.photocam.resolution.model)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Camera Resolution",
    "description": "Set the camera resolution",
    "type": "simple",
    "initialValues": {
		"resolution": 2
    },
    "paramsDefs": {
      "resolution": {
        "type": "number",
        "range": [
        	1,
        	3
        ]
      }
    },
    "cmds": [
      ">SXASCM20{{params.resolution}}<"
    ]
  }
}
```

> Create a model that defines a periodic report on the device.
> Notice how the `cmds` have a filter called '`fix`' that adds a padding to the number

> POST [api/dcontrol/definition/pegasus.standard.periodic_report.ign_off.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard.periodic_report.ign_off.model)

```json
{
  "dryRun": false,
  "definition": {
    "keywords": [
      "events",
      "common",
      "ios"
    ],
    "paramsDefs": {
      "time": {
        "units": "seconds",
        "range": [
          5,
          86400
        ],
        "type": "number",
        "label": "Periodic report with ignition off"
      }
    },
    "name": "Events - Periodic Report with Ignition Off",
    "initialValues": {
      "time": 7200
    },
    "cmds": [
      ">SGC00TR{{params.time|fix 5}}<",
      ">SED00NA0;C00+<",
      ">SED98SV0;F00-;ACT=SGC00TR{{params.time|fix 5}}<",
      ">SED99SV0;F00+;ACT=SGC00U<"
    ],
    "type": "simple",
    "description": "Reports with the ignition Off"
  }
}
```

> Create a model for setting the device baud rate 

> POST [api/dcontrol/definition/syrus.core.baud_rate.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.baud_rate.model)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Baud Rate",
    "description": "Set the baud rate",
    "type": "simple",
    "initialValues": {
		"baud_rate": 9600
    },
    "paramsDefs": {
      "baud_rate": {
        "type": "number",
        "list": [
        	9600,
        	115200
        ]
      }
    },
    "cmds": [
      ">SXABR{{params.baud_rate}}<"
    ]
  }
}
```

##### Range of Numbers

| Parameter | Description | Type | 
| --------- | ----------- | ---- |
| type | number | String |
| range | Range of numbers (min, max) | Array |
| labels | Short description | String |
| units | Units for parameter | String |

When working with a range you can specify a filter on the `cmds` that will fill the values according to how many digits the device accepts for that parameter.

##### List of Numbers 

| Parameter | Description | Type | 
| --------- | ----------- | ---- |
| type | number | String |
| list | List of numbers | Array |
| labels | Short description | String |
| units | Units for parameter | String |

The **number** type parameter definition can be used for creating a variable on the Syrus command that accepts a numeric value.

For example, the command to change the photocam accessory resolution is: `>SXASCM20#<`
where # refers to a number between 1 - 3 which corresponds to the photo resolution

- 1 = Low Resolution
- 2 = Medium Resolution
- 3 = High Resolution

we can make a model that can accept a variable called: `resolution` that accepts a number between 1 - 3 that corresponds to this resolution.
We can pass the parameter a range or list of numbers with the `range` and `list` parameters.

#### Text

> Create a model for setting the device extended tags

> POST [api/dcontrol/definition/pegasus.standard.extended_tags.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard.extended_tags.model)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Extended Tags",
    "description": "Parameters reported on standard configuration",
    "type": "simple",
    "initialValues": {
      "signals": "AC;AL;BL;CF;DOP;IX;JO;SV;VO;CL;CE;CS;CR;TI"
    },
    "paramsDefs": {
      "signals": {
        "type": "text"
      }
    },
    "cmds": [
      ">SXAEFA;{{params.signals}}<"
    ]
  }
}
```

The **text** type parameter definition can be used for creating a variable that accepts any string value.  This is useful for commands where you have to write something such as the extended tags or parameters you want reported in the events.

#### Numberbool

> Create a model for controlling the 1-wire accessories

> POST [api/dcontrol/definition/syrus.core.one_wire_management.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.one_wire_management.model)

```json
{
  "dryRun": false,
  "definition": {
    "name": "One Wire Management",
    "description": "One wire accessory management",
    "paramsDefs": {
      "ibutton": {
        "type": "numberbool",
        "label": "Enable iButton"
      },
      "io_expander": {
        "type": "numberbool",
        "label": "Enable IO Expander"
      },
      "ecu_monitor": {
        "type": "numberbool",
        "label": "Enable ECU Monitor"
      },
      "temperature": {
        "type": "numberbool",
        "label": "Enable Temperature Sensor"
      }
    },
    "initialValues": {
      "io_expander": true,
      "ecu_monitor": true,
      "ibutton": true,
      "temperature": false
    },
    "type": "simple",
    "cmds": [
      ">SXAWA{{params.ibutton}}{{params.io_expander}}{{params.ecu_monitor}}{{params.temperature}}0000<"
    ]
  }
}
```

The **numberbool** type is used for setting 1 and 0 values using `true` and `false`.  Note that the 1 and 0 values can be obtained with the `number` type but as you will see in the example it makes more sense (and it's more legible) to assign a boolean value (true/false) rather than 1 or 0.

<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br>

### Update

> Updating the tracking with Ignition ON model to include distance

> PUT [api/dcontrol/definition/pegasus.standard.tracking.ignition.on.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard.tracking.ignition.on.model)

```json
{
  "name": "Tracking Resolution",
  "description": "Periodic reports while the ignition is ON with Distance parameter."
  "tag": "pegasus.standard.tracking.ignition.on.model",
  "paramsDefs": {
    "min_time": {
      "units": "seconds",
      "range": [
        5,
        86400
      ],
      "type": "number",
      "label": "Minimum time between events"
    },
    "heading": {
      "units": "degrees",
      "range": [
        5,
        120
      ],
      "type": "number",
      "label": "Heading in degrees"
    },
    "max_time": {
      "units": "seconds",
      "range": [
        5,
        86400
      ],
      "type": "number",
      "label": "Maximum time between events"
    },
    "distance": {
      "type": "number",
      "range": [
        100,
        99999
      ],
      "units": "meters",
      "label": "Distance between reports"
    }
  },
  "initialValues": {
    "min_time": 10,
    "heading": 45,
    "max_time": 180,
    "distance": 1000
  },
  "type": "simple",
  "cmds": [
    ">SXAGH001{{params.heading|fix 3}}<",
    ">SED97SV0;F00+;ACT=SGC01TR{{params.max_time |fix 5}};ACT=SGC19TC{{params.min_time |fix 5}};ACT=SGC18DR{{params.distance |fix 5}}<",
    ">SED95SV0;J00F00&+;ACT=SGC01TR{{params.max_time |fix 5}}<",
    ">SED01NA0;C01J00|C18|C19&+;XCT=SGC19TC{{params.min_time |fix 5}}<",
    ">SED82SV0;F00!+;ACT=SGC01U<"
  ]
}
```

To update a model you simply make a `PUT` request to the API endpoint for that model passing all the parameters of the definition in the request.

<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br>

### Execute

> Execute the commands in a model for a device (Single command or multiple command example)

> PUT [api/dcontrol/device/:imei/model/:model_with_commands](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.restart.model)

> `no payload`

> This will execute the command >SRT< for the device

<br>&nbsp;<br>

> Execute the commands in a model with a single parameter for a device

> PUT [api/dcontrol/device/:imei/model/:model_with_parameters](https://pegasus1.pegasusgateway.com/api/dcontrol/device/356612021234567/model/syrus.acc.photocam.resolution.model)

```json
{
  "execValues": {
    "resolution": 3
  }
}
```

> Execute the commands in a model with multiple parameters

> PUT [api/dcontrol/device/:imei/model/:model_with_mult_parameters](https://pegasus1.pegasusgateway.com/api/dcontrol/device/356612021234567/model/syrus.tracking.turn_by_turn.model)

```json
{
  "execValues": {
    "heading": 45,
    "time": 60
  }
}
```

> Another example of executing commands for a model

> POST [api/dcontrol/definition/syrus.core.one_wire_management.model](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.core.one_wire_management.model)

```json
{
  "execValues": {
    "io_expander": false,
    "ecu_monitor": false,
    "ibutton": false,
    "temperature": true
  }
}
```

To execute the commands in a model you make a `PUT` request to the [Model API endpoint](#view104).  If the model has no parameters you can simply make a `PUT` request with no payload to execute it.

If the model has parameters look at it's definition to see how they are executed, your best bet is to look at the initialValues object and send this.

Finally, to execute the parameters you have to send it in an object called `execValues`.

<code>{&nbsp;<br>
&nbsp;&nbsp;"execValues": {<br>
&nbsp;&nbsp;&nbsp;&nbsp;"command": value<br>
&nbsp;&nbsp;}<br>
}
</code>




### Delete

In order to delete a model definition you can make a `DELETE` request to the definitions URI.

<aside class="warning">Deleting is not possible at the moment, in a future release it will be available</aside>


## <a name="dcontrol_configs">Configurations</a>

Remember that a Configuration is a collection of models, and models are the instructions that the device uses to program itself to report and behave how you want it to.  In this section we look at how to interact with API definitions that end with `.config`

### View all

> View all configurations

> [api/dcontrol/definitions?name=*.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions?name=*.config)

```json
[
  "pegasus.standard.config",
  "pegasus.standard.photocam.config",
  "pegasus.standard.high_reporting.config",
  "pegasus.test.all.config"
]
```

> View all configurations that start with 'pegasus.standard'

> [api/dcontrol/definitions?name=pegasus.standard.*.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definitions?name=pegasus.standard*.config)

```json
[
  "pegasus.standard.config",
  "pegasus.standard.photocam.config",
  "pegasus.standard.high_reporting.config"
]
```

To view all the configurations you have access to, you can make a GET request to the `api/dcontrol/definitions` resource, with a URL param filter called: `name`, where you can pass it a regexp that matches the definition you're looking for, in this case we can use `name=*.config` to refer to all definitions 

### View one

> View a particular configuration

> [api/dcontrol/definition/pegasus.standard.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard.config)

```json
{
  "name": "Pegasus Standard Test All Models Config",
  "description": "Configuration to test all possible models",
  "allowedConsoleCmds": [],
  "evLabels": {},
  "models": [ ... ],
  "tag": "pegasus.test.all.config",
  "ky": "a3m8"
}
```

To see a particular configuration you can make a request to the `api/dcontrol/config_definition.config` resource.



**Response**

| Param | Description | Type |
| ----- | ----------- | ---- |
| name | Name of the Configuration | String |
| description | Descriptive text for configuration | String |
| allowedConsoleCmds | Which commands are allowed via the device console | Array |
| evLabels | Required, for now you can use an empty object {} | Object |
| models | Names of models the configuration uses | Array |
| tag | Short name of the configuration | String | 
| ky | Unique shortcode that allows synchronization with the device | String |


<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br>

### Create

> Create a standard custom configuration

> POST [api/dcontrol/definition/pegasus.standard_custom.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/pegasus.standard_custom.config)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Sample Config",
    "description": "An example of creating a configuration",
    "allowedConsoleCmds": [],
    "models": [
      "syrus.core.restart.model",
      "pegasus.standard.ignition_events.model",
      "pegasus.standard.periodic_report.model"
    ],
    "evLabels": {}
  }
}
```

In order to create a configuration you need to make a POST request with the name of the configuration as part of the URI, the body of the request will use the parameters mentioned above.

<aside class="warning">The name of the configuration must start with the access permissions you have available in /dcontrol/access API, also, all configurations must end with `.config`, consequently, you cannot include the word config anywhere else in the name of the configuration.</aside>

| Param | Description | Type |
| ----- | ----------- | ---- |
| name | Name of the Configuration | String |
| description | Descriptive text for configuration | String |
| allowedConsoleCmds | Which commands are allowed via the device console | Array |
| evLabels | Required, for now you can use an empty object {} | Object |
| models | Names of models the configuration uses | Array |
| keywords | Short codes that can be used to group similar configurations | Array |

<br>&nbsp;<br><br>&nbsp;<br>

### Update

> Add another model to a configuration

> PUT [api/dcontrol/definition/syrus.standard_custom.config](https://pegasus1.pegasusgateway.com/api/dcontrol/definition/syrus.standard_custom.config)

```json
{
  "dryRun": false,
  "definition": {
    "name": "Sample Config",
    "description": "An example of creating a configuration",
    "allowedConsoleCmds": [],
    "models": [
      "syrus.core.restart.model",
      "pegasus.standard.ignition_events.model",
      "pegasus.standard.periodic_report.model",
      "syrus.list.geofences.circular.model"
    ],
    "evLabels": {}
  }
}
```

Updating a configuration requires that you make a `PUT` request to the `api/dcontrol/definition/`*configuration_name*`.config`
When doing this it will update the configuration across all devices that share this configuration.

<aside class="notice">The PUT request must have all the parameters of the configuration.</aside>


<br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br>
<br>&nbsp;<br><br>&nbsp;<br>

### Execute

> Send a configuration to a device

> PUT [api/dcontrol/device/:imei/config](https://pegasus1.pegasusgateway.com/api/dcontrol/device/356612021234567/config)

```json
{
  "configName" : "pegasus.syrus_standard.config",
  "overwriteConfig" : true
}
```

> This will set the configuration named: pegasus.syrus_standard.config to the device

<br>&nbsp;<br>

To set the configuration for a device you make a `PUT` request to the `api/dcontrol/device/:imei/config`.
This method accepts 2 parameters, `configName` and `overwriteConfig`.

parameter | type | description
----------|------|------------
configName | string | config api endpoint `pegasus.syrus_standard.config`
overwriteConfig | boolean | true if you want to overwrite the device's current configuration (default)


### Delete 

In order to delete a configuration definition you can make a `DELETE` request to the definitions URI.

<aside class="warning">Deleting is not possible at the moment, in a future release it will be available</aside>



