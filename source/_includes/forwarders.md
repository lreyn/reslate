# Forwarders

`/forwarders`

Forwarders are realtime redirections of data to any endpoint. They are useful when you want to obtain all the data from all your entities and store it in a database.
Forwarders have an active queue and require a confirmation that a message was received by the endpoint, thus with forwarders you're able to achieve database replication.

Pegasus supports forwarding data in an array of JSON objects where each object corresponds to a entity event which includes any location information or data reported by the entity's device.


## Intro

The way it works is that you define a URL (endpoint) that's going to be used as a receiver for the HTTP POST requests sent from Pegasus.

`Pegasus forwards`

\>>>> `[{"lat": 25.23432, "lon": -80.24912, "id":10001},{"lat": 25.23435, "lon": -80.24913, "id":10002}]`

The forwarder service expects the endpoint to 'confirm' the reception of the message Pegasus sends.

In order to confirm the reception of all events successfully, you can respond with an empty array `[]`

`Response from client`

<<<< `[]`

<aside class="warning">
Note that we expect a response within 20 seconds of sending the request, if we don't receive any response we will resend the message.
</aside>

Should there be any events that we sent in the forward that you did not process correctly in your database you can reply with the ID of the event that was not processed

<<<< `[10002]`

and Pegasus forwards that event again

\>>>> `[{"lat": 25.23435, "lon": -80.24913, "id":10002}]`

In order for Pegasus to forward data to your endpoint you must allow HTTP traffic to a path in your server where the code will reside.  
Check to make sure there are no firewall restrictions and the site is reachable by an external application.

By default the fields that are forwarded are the one's found in the [Master Fields List](#master-fields-list).

Depending on what [accessories](#accessories) the device has connected, the fields will be sent accordingly.  In other words only a device with an [ECU Monitor](#ecu-monitor) accessory connected will report ecu_ related fields, otherwise these fields will not be forwarded in the POST request.

At the moment, forwarders can only be created by DCT, once created however, if you are an administrator you'll be able to edit it.

## Parameters 

> Don't forget to send the entire payload

> dates_format

> PUT [/api/forwarders/:id](https://pegasus1.pegasusgateway.com/api/forwarders/109)

```json
{
    "dates_format": "%Y-%m-%d %H:%M:%S"
}
```

> this gives you dates with: `YYYY-MM-DD hh:mm:ss`, example: `2018-12-25 23:59:59`

> ---

> time_zone

> PUT [/api/forwarders/:id](https://pegasus1.pegasusgateway.com/api/forwarders/109)

```json
{
    "time_zone": "America/New_York"
}
```

> ---

> keys

> PUT [/api/forwarders/:id](https://pegasus1.pegasusgateway.com/api/forwarders/109)

```json
{
    "general": {
        "keys": [
            "lat",
            "lon",
            "speed",
            "primary_name",
            "event_time"
        ]
    }
}
```

```json
[
    {
        "lat": -34.88844,
        "lon": -56.11606,
        "speed": 33,
        "primary_name": "Blue Toyota",
        "event_time": "2018-12-17T14:22:09+00:00",
        "system_time": "2018-12-17T14:51:37.969868+00:00"
    }
]
```

> ---

> extra_headers

> PUT [/api/forwarders/:id](https://pegasus1.pegasusgateway.com/api/forwarders/109)

```json
{
    "extra_headers": {
        "Authorization": "Bearer YWRqZmFsZGpmbGFqc2RmbGFhZGxmam"
    }
}
```

> ---

> primary

> PUT [/api/forwarders/:id](https://pegasus1.pegasusgateway.com/api/forwarders/109)

```json
{
    "include_primary": true,
    "keys_include": [
        "primary"
    ]
}
```

```json
[
    {
        "vid": 2292,
        "lon": -80.19266,
        "primary": {
            "info": {
                "range_unit": "mile",
                "description": "Sample vehicle",
                "tank_volume": null,
                "color": "Black",
                "make": "Peterbilt",
                "vin": null,
                "license_plate": "ABC123",
                "alias": "foxtrot",
                "range": 400,
                "year": "2020",
                "model": "567",
                "tank_unit": "gallon"
            },
            "associations": [
                {
                    "device": 862831032123456,
                    "time": 1592847039,
                    "id": 86,
                    "association": true,
                    "vehicle": 2292
                }
            ],
            "__updated": 1596475523.951629,
            "associated_at": 1592847871,
            "name": "My special car",
            "primary": 9,
            "__created": 1592607412.826321,
            "images": {
                "photo": false,
                "on_icon": false,
                "idle_icon": false,
                "off_icon": false,
                "icon": false
            },
            "token": null,
            "properties": {
                "custom_properties": "yes",
                "network": "4G",
                "city": "FLORIDA",
                "sensor": "v1.2",
                "ID": "10073367",
                "region": "SOUTH",
                "my_info": {
                    "code": "11223344",
                    "state": "UPDATING"
                },
                "country": "US"
            },
            "groups": [
                6
            ],
            "__version": "5.3.2",
            "device": 862831032123456,
            "configuration": "0000",
            "type": "vehicle",
            "id": 2292
        },
        "label": "new",
        "lat": 26.68205,
        "device_id": 862831032123456
    }
]
```

param | type | description
-----:|------|-------------
dates_format | string | default: ISOFORMAT (YYYY-MM-DDTHH:MM:SS+HH:MM), python [dateformat](https://docs.python.org/2/library/datetime.html) compatible
time_zone | string | default: UTC (recommended), [timezone](#timezone)
extra_headers | object | default: `Content-Type: application/json`
include_vinfo | boolean | if true, sends an object with the entities information
general | object | update the general config (see below)

To update a forwarder, you can use a `PUT` request on the forwarder ID to update the general configuration. 
When sending the request, make sure to use the entire JSON object as the body of the `PUT` request, with the modifications.

general params | type | description
-----:|------|------------
discard | boolean | discards old messages in queue & any new messages 
keys | array | use it to only forward certain keys
keys_all | boolean | use it to forward all keys
keys_include | array | use it to include keys that are not defined in the base set (has priority over keys)
keys_exclude | array | use it to exclude any keys from the forwarder
stop_send | boolean | when true it stops the forwarder and queues events
stop_queue | boolean | when true it stops queuing events, discarding any new events
labels_in | array | only allows certain [labels](#common-labels) to forward data
labels_out | array | exclude the following [labels](#common-labels) from forwarding data
default_values | object | key and default value, ex: `{"ib": "000000000000"}`
include_primary | boolean | when true along with the field `primary` in `keys` or `keys_include` it includes a key called *primary* which contains the vehicle's information and custom properties

legacy general params | type | description
-------------------:|------|-------------
legacy | boolean | when true it sends the lat/lon as whole integers values
protocol | string |  when set to "rpc", it wraps the json array of events in the following envelope `{"params":[2,[{EVENT_1},{EVENT_2}]],"method":"pushevents","id":1}`
codes_map | json | object with key-value pairs, the keys should be the original event_code, the value represents the value to which it will be switched
filter_in | array | only forwards events from these event codes
filter_out | array | removes any unwanted event codes from forwarding data

## Tutorials 

### PHP & phpMyAdmin

The following tutorial demonstrates how to handle the messages forwarded by the Pegasus Sites through PHP (version 5.3 or later).

To start download or clone the [receiver code from our repo](https://github.com/dctdevelop/pegasus/tree/master/examples/receiver_php_7%2B)

The root file must be allowed access from a url, such as `http[s]://[YOURDOMAIN]/[PATH TO TEMPLATE]/` this will be your endpoint.


In the **config.php**, modify the constants to suit your PHP installation and Database access credentials.

```php
date_default_timezone_set("UTC");

define("DB_SERVER", "YOURDATABASE_SERVER");
define("DB_USER", "DATABSE_USER");
define("DB_PASS", "DATABASEP_PW");
define("DB_NAME", "DATABASE_NAME");
define('DB_TABLE_NAME', 'UNIT_EVENTS_RPC');
```

Once you have verified your Database credentials, remove/comment the first exit() statement within the ‘index.php’ file.

```php
// in the index.php comment or remove the following line
//exit()
```

```php
// make sure to have the PEGASUS_PROTOCOL as JSON
define('PEGASUS_PROTOCOL', JSON);
```

By default, when you test the forwarder it will create a table with the following keys (assuming it's not already created)
[eventKeysMySQL.sql](https://github.com/dctdevelop/pegasus/blob/master/resources/eventKeysMySQL.sql) 

If you do not want to store the data to a database, you can disable it by setting the variable ‘$db_storage’ to false within the ‘config.php’ file. This will prevent the table from being created, and will prevent all new events from being inserted into the database.

```php
// set to true to to store the data in the db
$db_store = true;
```

Note: This does not delete the table if it has been previously created, this only prevents the sql statement that creates the table from executing.

**Events Object data structure**

In most cases, it may be may be suitable for the events object to be handled differently or in addition to the database storage mechanism discussed in the previous section.

In this instance you may add your logic directly into the `peg_insert_array` function of the `config.php` file.
You can use the `$data` object at your discretion, and modify it to your will. The `$data` object is a PHP associative array which contains all the events sent from the pegasus gateway. You may add any logic here, which can include sending emails, forwarding the data to another script, or storing to a file. You must make sure that any errors are handled appropriately.

NOTE: It is important that you make sure your logic returns an empty array `[]`. As this lets pegasus know that the events have been handled accordingly and the queue can be cleared.

Once done you can test the forward here:
[Testing the Forward](#testing-the-forward)

<iframe width="560" height="315" src="https://www.youtube.com/embed/UdRudtYmf9Y" frameborder="0" allowfullscreen></iframe>

### Windows, .NET, Microsoft SQL 

Download the following zip [ms_receiver.zip](https://drive.google.com/open?id=0BzkN5ZFBA7BeejZEWmZoLUN3d2M)

In the receiver.ashx
there’s 3 places where you must input data

Line 50: Replace `[dbo].[UNIT_EVENTS_RPC]` with your Database Table Name

Line 105: Replace `SERVERNAME` with the name of your server

Line 106: Replace `Pegasus_db` with the name of your database

Please note that this solution uses:
[Windows Authentication Mode](https://msdn.microsoft.com/en-us/library/vstudio/bb669066(v=vs.100).aspx)

Run the following in Microsoft SQL Server to create the DB.
[eventKeysMSSQL.sql](https://github.com/dctdevelop/pegasus/blob/master/resources/eventKeysMSSQL.sql)

Make sure your Microsoft server has IIS, Active Directory services running and port binding correctly done. 
DNS/DHCP services are optional but recommended.

## Testing the forward

In order to simulate what Pegasus would send to your server/database you may use an application that sends HTTP requests, like POSTman, or cURL.
Just use the URL with the path to your JSON listener and follow one of the examples on the right.

[Full example with all keys](https://github.com/dctdevelop/pegasus/blob/master/resources/all_keys.json)

```shell
curl -X POST "http://yourlistener.com/path/to/receiver" \
-H "Content-Type: application/json" \
-d '[{"io_pwr": true, "ac": 2, "code": 1, "type": 10, "vid": 1392, "cf_lac": 103, "ip": "208.131.186.63", "port": 26052, "pid": 110, "al": 521, "vo": 6356212, "io_ign": true, "event_time": "2016-01-12T19:40:31+00:00", "source": 3, "lon": -77.32573, "io_exp_state": false, "id": 4348333385, "cv10": 580, "cv11": 0, "cv12": 268, "cv13": 0, "head": 315, "vdop": 83, "hdop": 97, "bl": 4394, "mph": 28, "valid_position": true, "lat": 18.08918, "pdop": 128, "device_id": 357666050758579, "system_time": "2016-01-12T19:40:32.799671+00:00", "cf_rssi": "10", "age": 2, "sv": 9, "io_out1": false, "io_out2": false, "io_in1": false, "io_in2": false, "io_in3": false}]'
```

## Tips & Recommendations

**Receiving data from multiple forwarders**

If you are looking to receive data in your database from multiple forwarders, it is recommended that you create tables based on the PID field.  This field corresponds to the Pegasus site ID, every forwarder received from different servers will have a unique PID per site.

**Working with large data**

When working with large data it is important to separate the database into manageable tables.  For example, you could have a table for every 1 or 2 vehicles. 

You may partition the vehicle tables per week or trimester. Every partition has a constraint over the Date column that way you only look over the table with the dates you're looking for.  This is especially important when you have to drop tables in the future.

**Enabling a Forwarder on your site**

In order to enable to create the forwarder please make sure you [test](?shell#testing-the-forward) with cURL or POSTman.
After successfully testing you may request enabling your forwarder on your site using the following form:
[Submit Application](http://digitalcomtech.com/syrusmart/submit/)

It takes approximately 8-24 hours to enable the application on your Gateway.

**Source IP addresses**

The source IP address for forwarded data are any of the following addresses:

* 104.238.146.47 main
* 159.203.80.12  backup
* 169.46.109.164 backup to the backup
* 169.46.109.172 backup to the backup's backup