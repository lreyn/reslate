
# Entities

> returns the vehicle or asset for that id

> [`api/entities/:id`]

> returns vehicle's name & device info

> [`api/vehicles?select=name,device:*`](https://pegasus1.pegasusgateway.com/api/vehicles?select=name,device:*)

<br> 

> return particular vehicle's name & device's network info

> [`api/vehicles?select=name,device:network&ids=2600`](https://pegasus1.pegasusgateway.com/api/vehicles?select=name,device:network&ids=2600)

Entities are collections of vehicles and assets. Both these resources are interchangeable with respect to the data they collect and can report, in other words both vehicle's and assets can have rawdata, trips, counters etc.  Also, vehicle's and assets can be associated to one another. 

The API manages a concept of *primary* & *secondary* IDs when associating these entities.

A **primary** entity is the main device that's reporting telemetry data, while a **secondary** entity is any peripheral or accompanying accessory that is related/associated to a primary entity, a classic example is a *vehicle* as a primary entity and a *driver* as a secondary entity.

Other resources which include collections of things are: [devices](/#devices), [geofences](/#geofences), [users](/#users), [groups](/#groups), [sims](/#sims).

All of the collections mentioned above support [pagination](/#pagination), `select`, and `search` parameter.

**select**

The `select` parameter is used to filter the data that you are looking for from the resources.

For example if you want to select just the vehicle's name
[api/vehicles?select=name](https://pegasus1.pegasusgateway.com/api/vehicles?select=name)

or the vehicle's name and the device's latest location
[api/vehicles?select=name,device:latest.loc](https://pegasus1.pegasusgateway.com/api/vehicles?select=name,device:latest.loc)

or the vehicle's name, device location, online state, and sim information
[api/vehicles?select=name,device:latest.loc network connection](https://pegasus1.pegasusgateway.com/api/vehicles?select=name,device:latest.loc%20network%20connection)

**search**

> Search url param: 

> [`/api/vehicles?search.name="My car"`](https://pegasus1.pegasusgateway.com/api/vehicles?search.name=My%20car)

> [`/api/vehicles?search.info.license_plate=ABC123`](https://pegasus1.pegasusgateway.com/api/vehicles?search.info.license_plate=ABC123)

```json
{
    "name": "My car",
    "info": {
      "license_plate": "ABC123",
      "year": "2014",
      "make": "Honda",
      "model": "CRV",
      ...
    },
    ...
}
```

The `search` parameter can be used to find specific key values within a set of resources. For example if you have custom properties per vehicle or geofences you can filter by that property. It works for vehicles, assets, groups, sims, geofences, tasks, triggers, geofence_types.

The way it works is that you define the key that you're filtering for with a url param that begins with `search`. For finding specific keys within a json you can reach the key by separating the keys with a period `.` example:

the vehicle's result with a json similar to this:

you can search for any vehicle that matches the name _My car_ with
[api/vehicles?search.name="My car"](https://pegasus1.pegasusgateway.com/api/vehicles?search.name=My%20car)

you can search for any vehicle that is of make _Honda_ with 
[api/vehicles?search.info.make=Honda](https://pegasus1.pegasusgateway.com/api/vehicles?search.info.make=Honda)

you can also combine search, so search for _red_ _Hondas_
[api/vehicles?search.info.make=Honda&search.info.color=red](https://pegasus1.pegasusgateway.com/api/vehicles?search.info.make=Honda&search.info.color=red)

<aside class="warning">Note that the search is case sensitive</aside>

you can even search for multiple items within an array and combine that with the select to return the location information for the entities found
[api/vehicles?search.groups=[1,2,3]&select=name,device:latest.loc](https://pegasus1.pegasusgateway.com/api/vehicles?search.groups=[1,2,3]&select=name,device:latest.loc)

or search by the license_plate
[api/vehicles?search.info.license_plate=ABC123&select=name,device:latest.loc](https://pegasus1.pegasusgateway.com/api/vehicles?search.info.license_plate=ABC123&select=name,device:latest.loc)

an example with a custom property on geofence could be
[api/geofences?search.properties.custom.random_key=value](https://pegasus1.pegasusgateway.com/api/geofences?search.properties.custom.random_key=value)


## Users

> All users
> [`api/users`](https://pegasus1.pegasusgateway.com/api/users)

> Particular user
> [`api/users/:id`](https://pegasus1.pegasusgateway.com/api/users/632)

> Usernames & preferences only
> [`api/users?select=username,prefs`](https://pegasus1.pegasusgateway.com/api/users?select=username,prefs)

Users are identified by email.
Usernames give access to both the Pegasus Platform GUI and to the API.

A User's API access is controlled by [scopes](/#scopes)
Their entities visibility is determinated by its groups' membership.

[More info](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Users)

### Own User

You can look at your own user with the following resource: [`api/user`](https://pegasus1.pegasusgateway.com/api/user)

> [api/user](https://pegasus1.pegasusgateway.com/api/user)

this is helpful in determining what [permissions/scopes](#scopes) you have access to as well as the groups of vehicles you are assigned.
Note that administrators have `"is_staff": true` and will have access to all scopes.

## Groups

> All groups

> [`api/groups`](https://pegasus1.pegasusgateway.com/api/groups)

<br>

> Particular group

> [`api/groups/:id`](https://pegasus1.pegasusgateway.com/api/groups/500)


Groups are identified by a unique ID and a user-defined name. 
Groups relate users, vehicles & assets.
The most common usage is "a group per client", but it's flexible to serve other purposes:

* Sub-setting for Vehicle selections
* Sub-setting for Triggers-tasks
* Sub-setting for Forwarders

[More info](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Groups)

## Vehicles

> All vehicles

> [`api/vehicles`](https://pegasus1.pegasusgateway.com/api/vehicles)

<br>

> Particular vehicle

> [`api/vehicles/:id`](https://pegasus1.pegasusgateway.com/api/vehicles/2600)

<br>

> All vehicle's info & last device communication

> [`api/vehicles?select=info,device:last*`](https://pegasus1.pegasusgateway.com/api/vehicles?select=info,device:last*)

Vehicles are the main entity type.  Remote interaction with a Syrus is done to its associated vehicle.  90% Of Automation triggers are triggered by vehicle events.

[More info](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Vehicles)


## Assets

> Rawdata request that groups by asset id `aid`, shows the max speed daily per driver <br>

> [api/rawdata?groups=500&duration=P3D&fields=$basic,aid&resample=event_time&how=mph:max,vid:first&freq=1D&group_by=aid](https://pegasus1.pegasusgateway.com/api/rawdata?groups=500&duration=P3D&fields=$basic,aid&resample=event_time&how=mph:max,vid:first&freq=1D&group_by=aid)

```json
{
  "events": [
    {
      "aid": 672,
      "mph": 47,
      "vid": 598,
      "event_time": "2016-10-04T12:00:00"
    },
    {
      "aid": 791,
      "mph": 54,
      "vid": 759,
      "event_time": "2016-10-04T12:00:00"
    },
    {
      "aid": 791,
      "mph": 47,
      "vid": 759,
      "event_time": "2016-10-05T00:00:00"
    }
  ]
}
```

> Assign a vehicle to an asset manually

```shell
curl -X POST https://pegasus1.pegasusgateway.com/api/entities/link \
  --header 'Content-Type: application/json' \
  --header 'Authenticate: 99ff984fbe2c90603da515cdb193fda3146c9c4c9a347bcf062b0760' \
  -d '{"primary_id": 2600, "secondary_id": 691}'
```

Assets are entities that can provide a relationship between a device and an accessory, this makes them very valuable when you want to assign a driver to a vehicle for example.

The accessories that are compatible with assets are [iButton](#ibutton), Fingerprint readers, Bluetooth tags, RFIDs and Taurus.  An example use case is that the asset is assigned a unique iButton id. Whenever that unique iButton id is read by any device, that device will instantly be associated to that asset.

The typical application for an asset is for assigning a driver to a vehicle.

There are two ways to associate an asset & a vehicle together, the first is manually with the `api/entities/link`

The second method is with the device automatically reporting an [iButton](#ibutton) ID, Fingerprint ID, Bluetooth Tag MAC address, RFID or Taurus token and associating it to an existing asset. To do this: 

First, you create an asset POST [`/assets`](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Assets-CreateAsset)

Second, you associate the unique iButton ID, Bluetooth Tag MAC, Fingerprint ID, RFID or enable the Taurus Token

Third, you present the particular iButton ID, RFID or MAC address near the device

And that's it, afterwards the vehicle's device will be assigned that `asset` until another iButton or something else takes it place.

This opens up the possibilities of making requests with asset informations. The following fields would be available:

`aid` or asset ID 

Thus when you want to create a rawdata request, and you'd like to resample and group_by a field, you can now group_by the asset ID, thereby allowing you to have rawdata requests per asset or driver.

**Associations**

* [iButton](?json#ibutton)
* [Bluetooth Tags](?json#bluetooth-tags)


## Devices

> All devices

> [`api/devices`](https://pegasus1.pegasusgateway.com/api/devices)

<br>

> Particular device

> [`api/devices/:imei`](https://pegasus1.pegasusgateway.com/api/devices/356612022409637)

<br>

> All device's inputs & outputs state

> [`api/devices?select=ios_state`](https://pegasus1.pegasusgateway.com/api/devices?select=ios_state)


> Device's latest data reported

> [`api/devices/:imei?select=latest`](https://pegasus1.pegasusgateway.com/api/devices/356612022409637?select=latest)

> Device's latest location data only

> [`api/devices?select=latest.loc`](https://pegasus1.pegasusgateway.com/api/devices?select=latest.loc)

Devices refer to any of the device's in the [DCT ecosystem](https://www.digitalcomtech.com/devices-2018), a vehicle can only have one device assigned at any one time.
This vehicle-device association may change over time (unit replacement) but it will always be a one to one relationship. 
You may see an `association` history of the vehicle's / IMEIs when you make an `api/vehicles/:id` request

The device's API is very powerful and full of information related to the current status of the device, it always represents the last information that the device reported. 

[More info](https://pegasus1.pegasusgateway.com/api-static/docs/#api-Devices)


## SIMs

> Get all sims
> GET [api/sims](https://pegasus1.pegasusgateway.com/api/sims)

* `/api/sims` which is for any SIM

The SIMs resource is self populated, there is no method for adding SIMs to the API, the action of reading and adding a SIM to the API is performed automatically by the devices as soon as they connect to the server.

There are some IDs to keep in mind when working with SIMs resources
* `id` or `resource_id` - refers to the API's ID for that sim, starts at 1 and increments per sim
* `iccid` - refers to the unique Integrated Circuit Card Identifier or 20 digit SIM's id, example: 8901260852291475879

### /api/sims 

> Get particular sim 
> `iccid` 20 digit SIM ID
> `resource_id` API ID
> GET [api/sims/[:iccid]](https://pegasus1.pegasusgateway.com/api/sims/8957123310512805589)
> GET [api/sims/[:resource_id]](https://pegasus1.pegasusgateway.com/api/sims/3)

> Configure the line number
> PUT [api/sims/:resource_id](https://pegasus1.pegasusgateway.com/api/sims/3)

```json
{
  "line_number": "+1234567890"
}
```

The `/api/sims` resource has methods for placing a line number to a particular sim, this method works over the resource's id, not the actual 20 digit SIM ID
