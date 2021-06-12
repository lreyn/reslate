# Reports

## Job-Engines

Pegasus Gateways allow for external job-engines to create its reports.
The Pegasus Gateway API handles all user authorizations and scheduling, allowing the job-engines to
focus on the reporting logic, in whichever enviroment they choose.

The default job-engine for all gateways is located at:

[https://d1.pegasusgateway.com/jobs-engine](https://d1.pegasusgateway.com/jobs-engine)

You can extend your gateway with your own job-engine.

### Required endpoints

Job engines require 3 endpoints to work

*	1. GET /manifest
*	2. POST /jobs
*	3. GET /jobs/:site_id/:job_id/:file


### Manifest

> GET [job_engine_url]/manifest

```json
{
	"definitions": {},
	"scripts": []
}
```

The manifest is a [JSON Schema](https://spacetelescope.github.io/understanding-json-schema/reference/index.html) that represents the parameters required for your scripts to work.

The manifest allows UI applications (like Pegasus App) to build forms for creating the reports, as well as scheduling and running them with your gateways /job_manager API

You can use the default engines manifest as a guide

[https://d1.pegasusgateway.com/jobs-engine/manifest](https://d1.pegasusgateway.com/jobs-engine/manifest)

#### definitions

> definitions defined on schema

```json
{
	"definitions": {
		"from": {
			"description": "Events starting time",
			"type": "string",
			"format": "date",
			"peg_type": "$from"
		}
	}
}
```

> referencing definitions from the `scripts` object

```json
"scripts": [
	...
	"properties": {
		"from":
			"$ref: #/definitions/from",
	...
	}
]
```

You can establish shared definitions that will be used throughout the `scripts` object.
Definitions represent all the resources that you can work with on the [Pegasus API](#entities) (entities, geofences, triggers, etc), they cannot be modified.

use the JSON schema `$ref` construct to reference definitions within your `script` elements.

<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;
<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;
<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;
<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;
<br>&nbsp;<br>&nbsp;<br>&nbsp;


#### scripts

```json
{
	"script": "my_report",
	"title": "My Custom Report",
	"desc": "My custom report description",
	"icon": "https://imgur.com/link/to/image.svg",
	"app_url": null,
	"category": "trip_analysis",
	"schedule": true,
	"parameters_schema": {...}
}
```

The `scripts` object contains all of the available reports on your engine.

param | type | description
------|------|-------------
script | string | unique name for the API to identify each report, no spaces allowed
title | string | name of the report, spaces are allowed here, up to 50 characters
[desc] | string | short description of the report, up to 150 characters
[icon] | string | url with the icon's location (png or svg), url should be accessible via https
[app_url] | string | url for adding an embedded app to the Pegasus App UI, url should be accessible via https
[category] | string | allows you to group the reports on the Pegasus application UI (see [table](#category) for possible categories)
schedule | boolean | true to allow the report to be scheduled periodically
[parameters_schema] | object | defines the parameters the report will use

If you define an `app_url`, when clicking a report an iframe will be embedded with the `app_url` loaded, otherwise, pegasus loads a default template.

#### Example with `app_url` (custom template)
![alt text](https://cdn2.pegasusgateway.com/images/applications/yes_url.jpg "UI defined")


#### Example without `app_url` (default template)
![alt text](https://cdn2.pegasusgateway.com/images/applications/no_url.jpg "UI by default")

#### category

category | name
---------|------
live_reports | Live Reports
accessories | Accessories Reports
trip_analysis | Trip Analysis Reports
vehicle_metrics | Vehicle Metrics Reports
device_behavior | Device Behavior Reports
fleet_status | Fleet Status Reports

#### parameters_schema

> script's parameters_schema
> [job_engine_url]/manifest

```
	...
	"parameters_schema": {
		"type": "object",
		"required": ["from", "to"],
		"properties": {
			"from":
				"$ref: #/definitions/from",
			"to":
				"$ref: #/definitions/to",
			"bases":
				"$ref: #/definitions/bases"
			...
		}
	}
```

The parameters schema tells applications what parameters are required for the script to run.
You define your own schemas, or reference them from your root [definitions](#definitions) object.

param | type | description
------|------|-------------
type | string | object
required | array | which definitions are obligated to be filled in order for the report to execute
properties | object | reference the definitions from the manifest to ask the user for the parameters


You can see how pegasus reports section displays the engine's manifest here.

![alt text](https://cdn2.pegasusgateway.com/images/applications/custom_report.jpg "Custom Report")


### POST to job_engine

> Pegasus makes a POST request to your job engine
> POST [job_engine_url]/jobs

```json
{
	"configuration": {
		"site_id": "gateway id",
		"site_url": "gateway URL",
		"user_id": "user_id",
		"api_token": "token",
		"tz": "timezone",
		"job_id": 1,
		"job_name": "script_name",
		"job_created_at": timestamp,
		"job_expires": 86400
	},
	"parameters": {
		/* parameters specified by the parameters_schema for this report */
	}
}
```

```json
{
	"configuration": {
		"site_id": 1,
		"site_url": "https://pegasus1.pegasusgateway.com",
		"user_id": 86,
		"api_token": "9e34d311fc4ad45d726ab731917554ee279bf23cfa5b148df7c30e01",
		"tz": "UTC",
		"job_id": 22222,
		"job_name": "trips",
		"job_created_at": 1527613165.181
	},
	"parameters": {
		"from": "2018-01-01T00:00:00",
		"to": "2018-01-07T23:59:59",
		"vehicles": [5,6,7],
		"tz": "UTC"
	}
}
```

This is the endpoint where your magic comes through.
Pegasus Gateway provides your job engine the necessary parameters to construct a report by sending a POST to `[job_engine_url]/jobs` with the following info:


param | type | description
------|------|-------------
site_id | number | unique id for every gateway site
site_url | string | url for the gateway that made the request
user_id | string | user id that made the request
api_token | string | api token for the job
job_id | number | job id used for updating the job on the pegasus api
tz | string | originating timezone
job_name | string | name of the script
job_created_at | string | time the report was ran
job_expires | string | expiration time of the job


The `configuration` object contains a site_url and api_token, which enables you to interact with the Gateway's APIs
(/rawdata, /counters, etc) and generate your report.

The `job_expires` parameter refers to the duration of the jobs time on the gateway (in seconds), it is relative to the jobs `job_created_at` timestamp, `done_at` is marked once the job is updated with result `error` or `done`.

**Note** The *api_token* is restricted to the user's scopes. You must handle this in your reporting logic (user is asking for a geofence report but does not have access to geofences)

You must use these parameters to report the state of the job back to the gateway

PUT `[site_url]/api/job_manager/:job_id`



<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;
<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;



### Updating the Jobs

> Update a report's progress:

> PUT [site_url]/api/job_manager/15

```json
{
	"progress": {
		"percentage": 50
	}
}
```

> Update the parameters for a report if a vehicle id (7) was not found for example:

> PUT [site_url]/api/job_manager/15

```json
{
	"parameters": {
		"from": "2018-01-01T00:00:00",
		"to": "2018-01-07T23:59:59",
		"vehicles": [5, 6],
		"tz": "UTC"
}
```

> Update the state to done

> PUT [site_url]/api/job_manager/15

```json
{
	"state": "done"
}
```

> Update the job when report is `done`

> PUT [site_url]/api/job_manager/15

```json
{
	"parameters": {},
	"progress": {
		"percentage": 100
	},
	"state": "done",
	"result": {
		"files": ["custom.json", "custom.csv", "custom.xlsx", "custom.pdf"],
		"email": {
			"files": ["custom.xlsx", "custom.pdf"],
			"message": "<h2>Custom Report</h2><br><p>Attached detail with scheduled custom report</p>",
			"subject": "Custom Report"
		},
		"error": ""
	}
}
```

> Report returns an `error` message

> PUT [site_url]/api/job_manager/15

```json
{
	"parameters": { ... },
	"progress": {
		"percentage": 60
	},
	"state": "error",
	"result": {
		"error": "Failed to execute rawdata request"
	}
}

```

You need to inform the Gateway that started your Job of its progress and available data,
to do so you can make PUT requests with the `api_token` given in the `configuration` block.

**Note** `site_url` refers to the pegasus site url

PUT `[site_url]/api/job_manager/:job_id`


The payload can contain one or all of the following:

* parameters: updated parameters (i.e post-validation)
* progress: progress for the job.
* state: state of the job (`running`, `error` or `done`)
* result:
	* files: array with files generated in the report
	* email:
		* files: array with files to send in the email
		* subject: email subject
		* message: email subject message
	* error: error message when the script fails (optional)
	* summary:


<code>
{<br>
&nbsp;&nbsp;"parameters": { ... },<br>
&nbsp;&nbsp;"progress": {<br>
&nbsp;&nbsp;&nbsp;&nbsp;"percentage": 50<br>
&nbsp;&nbsp;},<br>
&nbsp;&nbsp;"state": "running"<br>
&nbsp;&nbsp;"result": {<br>
&nbsp;&nbsp;&nbsp;&nbsp;"files": [],<br>
&nbsp;&nbsp;&nbsp;&nbsp;"email": {<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"files": [],<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"message": "",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"subject": ""<br>
&nbsp;&nbsp;&nbsp;&nbsp;},<br>
&nbsp;&nbsp;&nbsp;&nbsp;"summary": {},<br>
&nbsp;&nbsp;&nbsp;&nbsp;"error": ""<br>
&nbsp;&nbsp;}<br>
}<br>
</code>

When the report is `done`, you must update the job with other parameters, in this case with the `result` field, this field has the files before explained.

Updating the job state to `error` or `done` closes the job to any further updates.
You won't be able to modify it further.

### Getting data from Jobs

> GET [site_url]/api/job_manager/15/script_name.json
> results in
> `[job_egine_url]/jobs/1/15/script.json`

```json
{
	"vid": 5,
	"name": "Test 1",
	"from": "2018-01-01T00:00:00",
	"to": "2018-01-07T23:59:59",
	"idleTime": 258929,
	"primary_group": null,
	"resolved": true,
	"percent_idle": 57
},
{
	"vid": 6,
	"name": "Test 2",
	"from": "2018-01-01T00:00:00",
	"to": "2018-07-07T23:59:59",
	"idleTime": 57911,
	"primary_group": null,
	"resolved": true,
	"percent_idle": 25
}
```

Once your report is finished, This is endpoint used by pegasus to fetch the files generated:

`GET [site_url]/jobs/:site_id/:job_id`

For example, 

GET `[site_url]/api/job_manager/15/script_name.json`

results in:

`[job_engine_url]/jobs/1/15/script.json`

## Pegasus Job Manager

> [site_url]/api/job_manager/engines

```json
[
	"https://d1.pegasusgateway.com/job-engines",
	"..."
]
```

Each gateway has a /job_manager API designed to interact with the job-engines registered on the gateway

You can view what job-engines are available on `[site_url]/api/job_manager/engines`


### Making the POST request

> Create a report
> POST `[site_url]/api/job_manager`

```json
{
    "script": "name",
    "name" : "report name",
    "expires": 0,
    "engine": "engine_name",
    "args" : { ... }
}
```

> example creating an idling report from the d1 [manifest](#manifest)

> POST `[site_url]/api/job_manager`

```json
{
  "script": "idling",
  "name": "Idling Usage",
  "expires": 432000,
  "engine": "http://d1.pegasusgateway.com/jobs-engine",
  "args": {
    "from": "2018-07-10T00:00:00",
    "to": "2018-07-10T23:59:59",
    "vehicles": [],
    "groups": [
      500
    ],
    "prefs": {
      "distance": "mile",
      "speed": "mph",
      "language": "es",
      "volume": "gallon"
    },
    "tz": "UTC"
  }
}
```

> RESPONSE

```json
{
    "engine": "http://d1.pegasusgateway.com/jobs-engine",
    "transaction": null,
    "name": "Idling Usage",
    "script": "idling",
    "created_at": 1532031389.04,
    "expires": 432000,
    "state": "created",
    "__v": 2,
    "params": {
        "tz": "UTC",
        "vehicles": [
            1673
        ],
        "to": "2018-07-10T23:59:59",
        "groups": [],
        "from": "2018-07-10T00:00:00",
        "prefs": {
            "volume": "gallon",
            "distance": "mile",
            "speed": "mph",
            "language": "es"
        }
    },
    "result": {},
    "done_at": null,
    "owner": 54,
    "progress": {},
    "cache_key": null,
    "id": 251336
}
```

To create a report pegasus sends a payload to the /job_manager resource `[site_url]/job_manager` with the following info:

param | type | description
------|------|-------------
script | string | name of the script defined in your manifest
name | string | name of the report
expires | number | expiration of job once it's finished (seconds)
engine | string | name of the job_engine
args | object | arguments for the report that was executed


## Scheduling

> Scheduling a report
> POST `[site_url]/tasks`

```json
{
  "id": "task_id",
  "name": "task_name",
  "processes": [
    [
      {
        "name": "report_name",
        "script": "script",
        "tz": "timezone",
        "engine": "job_engine_url"
      },
      {
        "vehicles": [
          5,
          6
        ],
        "recipients": [
          "example@example.com"
        ],
        "user": [
          11
        ],
        "groups": [],
        "back": {
          "days": 7,
          "hours": 0
        },
        "tz": "UTC"
      }
    ]
  ],
  "schedule": []
}
```

> Custom report task
> POST [site_url]/api/tasks

```json
{
	"name": "Custom Report Task",
	"processes": [
		{
			"name":"Custom Report",
			"script":"my_script",
			"tz": "UTC",
			"engine":"http://mycustomjobengine.com"
		},
		{
			"vehicles": [5,6],
			"recipients": ["example@example.com"],
			"users": [11],
			"groups":[],
			"back": {
				"days": 7,
				"hours": 0
			},
			"tz": "UTC"
		}
	],
	"schedule": ["0-15 9 * * 1"]
}
```


Jobs can additionally be scheduled, with the Pegasus /tasks API

Task parameters: 

param | type | description
------|------|-------------
name | string | how you want to name the scheduled report
processes | array | Contains the `configuration` and `parameters`
schedule | string | Cron format. Allows you to define time and frequency to run the task. For more information about Cron, go here: [Cron](https://crontab.guru/)

Configuration parameters: 

param | type | description
------|------|-------------
name | string | report name
script | string | script report
tz | string | originating timezone
engine | string | engine where the report will be running

Parameters: 

param | type | description
------|------|-------------
resource | varies | specifies the resources from your params object in the script 
recipients | string | array of emails that will receive the scheduled report
users | number | array of user ids that will receive the scheduled report
back | object | how many days / hours to run back the report
tz | string | originating [timezone](#timezone)

![alt text](https://cdn2.pegasusgateway.com/images/applications/add_task.jpg "Add Task")
