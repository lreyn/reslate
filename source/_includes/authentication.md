# Authentication

> Normal login

```shell
curl -X POST https://pegasus1.pegasusgateway.com/api/login \
    --header 'Content-Type: application/json' \
    -d '{"username":"developer@digitalcomtech.com",
        "password":"deV3lopErs"}'

{"message":"User successfully authenticated","app":null,"auth":"7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0"}
```

> Infinite token login

```shell
curl -X POST https://pegasus1.pegasusgateway.com/api/login \
    --header 'Content-Type: application/json' \
    -d '{"username":"developer@digitalcomtech.com",
        "password":"deV3lopErs", "scheme": "infinite"}'

{"message":"User successfully authenticated","app":null,"auth":"7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0"}
```

> **POST** [api/login](https://pegasus1.pegasusgateway.com/api/login)

> Body 

```json
{
  "username": "developer@digitalcomtech.com",
  "password": "deV3lopErs"
}
```

> Response

```json
{
  "message": "User successfully authenticated",
  "app": null,
  "auth": "7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0"
}
```

To authenticate use the `/login` resource.

param | description
------:|-------------
username | your username (email address)
password | your password
scheme | set to infinite to generate an infinite token
app | custom name to manage tokens
expires | expiration of token in seconds (default: 3600)

A successful login returns a token that <b>has to be used on every subsequent request</b>.  Tokens expire after 60 minutes of no requests made with the token, unless you set the scheme to `infinite`. 

The token that's generated has to be sent on a header called <b style="color:#005596;">Authenticate</b> in order to retrieve data from the API.<br>
`Authenticate: 7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0`


When sending JSON make sure you use the following <b style="color:#005596">Content-Type</b> header<br>
`Content-Type: application/json`

## Sessions

> View tokens you have created

> [`api/user/sessions`](https://pegasus1.pegasusgateway.com/api/user/sessions)

```json
{
  "tokens": [
    {
      "origin": "--",
      "scopes": "groups=285&write=remote.output,tasks",
      "app": "myApp",
      "expires": 7183,
      "token": "9c56c37b52ff3b96ed0a501ac63e574fcb42c32a28c251c2a2033724",
      "app_scheme": "",
      "scheme": "finite"
    }
  ],
  "session": {
    "origin": "--",
    "scopes": "",
    "app": "None",
    "expires": 3600,
    "token": "7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0",
    "app_scheme": "",
    "scheme": "normal"
  }
}
```

You can manage tokens and the current session with the `/user/sessions` resource.  You can also create your own tokens with a **POST** to the same resource

param | description
------:|-------------
scheme | infinite or finite token
limit | limit of token in seconds
app | custom app name
scopes | specify the group(s) id that the token has access to, and any [scope](#scopes)
app_scheme | pegasus application use only

<aside class="warning">Note that only 50 infinite tokens are allowed per account. To remove a token use the logout method.</aside>

<!-- TODO: document user/sessions -->

> Create a scoped API token

```shell
curl -X POST https://pegasus1.pegasusgateway.com/api/user/sessions \
    --header 'Content-Type: application/json' \
    --header 'Authenticate: 7ebbd199ecb10a156bdc189d3e0cd8ad9f9b676979deaaadca3612c0' \
    -d '{"scheme":"infinite","app":"myApp","scopes":"groups=285&write=remote.output,tasks"}'
```

> Remove a token

```shell
curl https://pegasus1.pegasusgateway.com/api/logout?auth=640c59b1df21aed608fe9cb775d56faaea9e33dd74730e31a94456e2 \
    --header 'Content-Type: application/json'

{"message": "Session terminated"}
```


## Scopes

> [api/user](https://pegasus1.pegasusgateway.com/api/user)

```json
{
  "username": "developer@digitalcomtech.com",
  "scopes": {
    "sims": "r",
    "tasks": "w",
    "remote.phones": "w",
    "pindrops": "w",
    "remote.tracking_resolution": "w",
    "remote.gps_status": "w",
    "remote.diagnostic": "w",
    "users": "r",
    "vehicles.counters": "r",
    "geofences:visibility.all": "r",
    "groups.vehicles": "r",
    "remote.trigger_position_event": "w",
    "remote.safe_immo": "w",
    "remote.call": "w",
    "geofences": "w",
    "remote.output": "w",
    "remote.speed": "w",
    "remote.sms_alias": "w",
    "rawdata": "w",
    "remote.state": "w",
    "remote.ecu_console": "w",
    "remote.rpm": "w",
    "remote.outputsetlog": "w",
    "groups": "r",
    "remote.configuration": "w",
    "plugins.photocam": "w",
    "remote.fwupdate": "w",
    "remote": "w",
    "assets": "r",
    "geofences:visibility.groups": "r",
    "triggers": "w",
    "vehicles": "r",
    "devices": "r",
    "groups.assets": "r",
    "groups.users": "r",
    "remote.console": "w",
    "remote.mdt": "w",
    "plugins.garmin": "w",
    "plugins.lumeway": "w"
  }
}
```

A user's access to the API resources is controlled by their what groups they belong to and the `scopes` they have assigned.

When a user belongs to a particular group they can see all the entities (vehicles, sims, assets, devices, etc) that group has associated.

Scopes are identified by the name of the resource and then the letter's: `r` or `w` corresponding to their access level for that resource

**r** : read-only permissions : GET

**w** : read and write permissions : POST + PUT + DELETE

For example, a user with scopes: `{ "triggers": "r", "vehicles": "w" }` has *read* permission for `triggers` and *write* permissions for `vehicles` allowing the user to only read triggers as well as create, edit, and delete vehicles

<aside class="notice">
If you don't have access to a particular resource due to scope restrictions a <a class="link" href="#status">401 is returned</a>
</aside>

List of available scopes - `resources/users/scopes`

resource | permission | description | API
--------:|------------|-------------|-----
assets | read | see the assets (drivers, things, etc) | [assets](?json#assets)
assets | write | create and edit assets (drivers, things, etc) | [assets](?json#assets)
configurations | read | read the managed configurations | [configurations](?json#managed-configurations)
device.mute | read | view event mute status | [devices](?json#devices)
device.mute | write | mute device events | [devices](?json#devices)
devices | read | view devices | [devices](?json#devices)
devices | write | create devices is not available, contact [support](mailto:support@digitalcomtech.com) | 
entities.link | write | associate or deassociate two entities | [entities](?json#entities)
forwarders | read | view forwarder status | [forwarders](?json#forwarders)
forwarders | write | contact [support](mailto:support@digitalcomtech.com) to create forwarders |
geofences | read | view own geofences | [geofences](?json#geofences)
geofences | write | create and edit your own geofences | [geofences](?json#create100)
geofences:visibility | write | view public geofences | [geofences](?json#geofences)
geofences:visibility.all | write | create public geofences | [geofences](?json#geofences)
geofences:visibility.groups | write | create geofences for your group | [geofences](?json#geofences)
groups | read | read the groups and its' info | [groups](?json#groups)
groups | write | create and edit groups | [groups](?json#groups)
groups.assets | write | manage assets within your group | [assets](?json#assets)
groups.users | write | manage users within your group | [users](?json#users)
groups.vehicles | write | manage vehicles within your group | [vehicles](?json#vehicles)
pindrops | write | create a pindrop (safe zone) around an entity | [pindrops](#)
plugins.garmin | read | see garmin jobs & messages | [plugins.garmin](?json#garmin)
plugins.garmin | write | create garmin jobs and send garmin messages | [plugins.garmin](?json#garmin)
plugins.lumeway | read | read the lumeway data | [plugins.lumeway](?json#lumeway)
plugins.lumeway | write | take a photo with the lumeway accessory | [plugins.lumeway](?json#lumeway)
plugins.photocam | read | see the photos | [photos](?json#getting-the-last-photo)
plugins.photocam | write | take photos | [photos](?json#capturing-a-photo)
plugins.satcom | read | see satcom parameters | [satcom](#)
plugins.satcom | write | set satcom parameters | [satcom](#)
rawdata | read | get the vehicle's rawdata | [rawdata](?json#rawdata)
remote | read | see the remote GET methods | [remote](?json#remote-control)
remote | write | ability to execute all remote commands | [remote](?json#remote-control)
remote.call | write | call an authorized number | [remote](?json#methods)
remote.configuration | write | set a managed configuration | [remote](?json#methods)
remote.console | write | send console (freestyle) commands | [remote](?json#methods)
remote.fwupdate | write | execute a firmware update | [remote](?json#methods)
remote.mdt | write | send mdt (serial port) messages | [remote](?json#methods)
remote.output | write | set an output | [remote](?json#methods)
remote.phones | write | authorize a number | [remote](?json#methods)
remote.rpm | write | set rpm threshold | [remote](?json#methods)
remote.safe_immo | write | execute a safe engine immobilization | [remote](?json#methods)
remote.sms_alias | write | set an sms_alias for actions | [remote](?json#methods)
remote.speed | write | set a speed limit threshold | [remote](?json#methods)
remote.tracking_resolution | write | set tracking resolution | [remote](?json#methods)
remote.trigger_position_event | write | query device's live location | [remote](?json#methods)
routes | read | see entity routes | [routes](#routes)
routes | write | create routes | [routes](#routes)
sims | read | see the SIMs info | [sims](?json#sims)
tasks | read | view a task | [tasks](?json#tasks)
tasks | write | create/edit a task | [tasks](?json#tasks)
triggers | read | see the triggers assigned to your user | [triggers](?json#triggers)
triggers | write | create and edit triggers | [triggers](?json#triggers)
users.application_data | read | view permissions for other users | [users](?json#users)
users.application_data | write | assign permissions for other users | [users](?json#users)
users.password_reset | write | set or reset the password of other users | [users](?json#users)
users | read | read the user's info | [users](?json#users)
users | write | create and edit the users | [users](?json#users)
vehicles | read | read the vehicle list and it's information | [vehicles](?json#vehicles)
vehicles | write | create and edit a vehicle | [vehicles](?json#vehicles)
vehicles.counters | read | read the vehicle's counters | [vehicle-counters](?json#global-vehicle-counters)
vehicles.counters | write | edit the global vehicle counters | [vehicle-counters](?json#global-vehicle-counters)

