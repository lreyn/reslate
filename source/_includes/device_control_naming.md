#### Naming Conventions

| Name Starts With  | Compatibility | 
| ----------------- | ------------- | 
| `syrus.core` | All |
| `syrus.list` | All |
| `syrus.`*device_model* | *device_model* |
| `syrus.acc.`*accessory_name* | *accessory_name* | 


### Core 

Core commands are those that can be applied to any Syrus regardless of the model version.

| Command | Shortcode | Description | 
| ------- | --------- | ----------- |
| `syrus.core.accessory.management.model` | XAWA | One wire accessory management
| `syrus.core.auto_restart.model` | AR | Automatic restart based on communication
| `syrus.core.backlog.accelerometer.model` | XADL | Last 7 seconds of data in accelerations
| `syrus.core.backlog.gps.model` | XAKL | Second by second gps historic data 
| `syrus.core.baud_rate.model` | XABR | Device baud rate
| `syrus.core.call_authorization.model` | XAVI | Allow calls authorized or non-authorized numbers
| `syrus.core.diagnostic.model` | XADD | Connect immediately to the diagnostic server
| `syrus.core.gps.filter.model` | XAEC::gps | Adjust the gps anti-drift filter.
| `syrus.core.keep_alive.model` | XAKA | Keep alive
| `syrus.core.leds.model` | XALO | LEDs silent mode
| `syrus.core.outputs.extended.model` | XE | Extended Outputs using accessory
| `syrus.core.outputs.model` | XP | Device output(s)
| `syrus.core.power_save.end.model` | XAPS | Power Save End
| `syrus.core.power_save.immediate.model` | XAPS | Power Save Start Immediately
| `syrus.core.reset_buffer.model` | RT | Clear all messages in the buffer
| `syrus.core.restart.gprs.model` | XAGP | Restart the GPRS communication
| `syrus.core.restart.interface.model` | XAEC::i2c | Restart interface
| `syrus.core.restart.model` | RT | Restart device
| `syrus.core.restart.gps.model` | RT | Restart GPS module
| `syrus.core.safe_immobilization.model` | XASE | Execute safe engine immobilization
| `syrus.core.signal_status.model` | SS | Set Signal Status

### Lists

Syrus has a set of commands which are actually lists you can define, for example a list of iButtons, tags or geofences.
These lists have indices between 1-n numbers and have model type: `ulist` 

| Model | Shortcode | Description | 
| ----- | --------- | ----------- |
| `syrus.list.signal_persistence` | XASP | Signal Persistences |
| `syrus.list.circular_regions` | XAGR | Circular Regions |
| `syrus.list.gps_accelerations` | XAGN | Configure GPS acceleration |
| `syrus.list.phones` | XADP | Telephone DPs |
| `syrus.list.sms_alias` | XATA | SMS Aliases |
| `syrus.list.speed_limits` | GS | Speed Limits |
| `syrus.list.battery_levels` | XABS | Device Battery Levels |
| `syrus.list.adc_levels` | XAGA | ADC Levels |
| `syrus.list.heading_deltas` | XAGH | Heading Deltas |
| `syrus.list.pulse_counters` | XAPCT | Pulse Counter Thresholds |
| `syrus.list.time_windows` | GT | Time Windows |

### Device Specific

These commands are specific to a particular device model.

| Name Starts With  | Compatibility | 
| ----------------- | ------------- |
| `syrus.s2` | Syrus 2 |
| `syrus.scc` | Syrus CC |
| `syrus.scc2` | Syrus CC+ |
| `syrus.s3` | Syrus 3 US/EU |
| `syrus.s3bt` | Syrus 3BT |


#### Commands

| Command | Compatibility | 
| ------- | ------------- |
| `syrus.scc.accelerometer.value` | Syrus CC |
| `syrus.scc2.accelerometer.value` | Syrus CC |
| `syrus.s3.accelerometer.value` | Syrus CC |
| `syrus.s3bt.accelerometer.value` | Syrus CC |

> Instant Acceleration value

```json
{
  "dryRun": false,
  "definition": {
    "name": "Restart Device"
    "type": "simple",
    "cmds": [
      ">SXAEC::i2c 6<",
      ">SXAEC::i2c 12<"
    ]
  }
}
```



### Accessories

These models are specific to an accessory. 

| Name Starts With  | Accessory | Description |
| ------------------------ | --------- | ----------- |
| `syrus.acc.temperature` | Temperature Sensor | Set thresholds |
| `syrus.acc.ecu` | ECU Monitor | Set thresholds | 
| `syrus.acc.fingerprint` | Fingerprint Reader | Interact |
| `syrus.acc.garmin` | Garmin | Interact |
| `syrus.acc.ioexp` | IO Expander | Interact |
| `syrus.acc.ibutton` | iButtons | Authorize |
| `syrus.acc.lumeway` | Lumeway | Interact | 
| `syrus.acc.satcom` | Satcom | Set Events | 
| `syrus.acc.photocam` | Photocam | Set Events |
| `syrus.acc.serial_exp` | Serial Expander | Configure | 
| `syrus.acc.bttag` | Bluetooth Tag | Configure & Interact |
| `syrus.acc.technoton` | Technoton | Set thresholds |
| `syrus.acc.audio ` | Audio Kit | Interact & Set Params |
| `syrus.acc.mdt ` | MDT | Send messages |


#### Legacy

We refer to the original remote API as the Legacy Remote API.
This API was found under the [API Documentation Reference section](https://cloud.pegasusgateway.com/api-static/docs/#api-Remote). 
We use this section to map the legacy API to it's equivalent in the new device Control / remote2.0 API

| Legacy Remote API | Replaced With  | Description |
| ----------------- | -------------- | ----------- |
| `/fwupdate` | `syrus.`*device_model*`.fwupdate` | FW for Specific Models |
| `/console` | `syrus.core.console` | Interact with device console | 
| `/phones` | `syrus.list.phones` | Set Telephone DPs | 
| `/ecu_state` | `syrus.acc.ecu.state` | Query ECU State | 
| `/diagnostic` | `syrus.core.diagnostic` | >SXADDUP< |
| `/safe_immo` | `syrus.core.safe_immo` | Safely Immobilize |
| `/state` | `syrus.core.state` | Query state of all inputs/outputs |
| `/output` | `syrus.core.output` & `syrus.acc.ioexp` | Set Outputs |
| `/call` | `syrus.acc.audio.call` | Make Phone Call |
| `/rpm` | `syrus.acc.ecu.rpm` | Set threshold | 
| `/speed` | _set on configuration_ | XACS & Persistence |
| `/mdt` | `syrus.acc.mdt` | Send messages |
| `/sms_alias` | `syrus.list.sms_alias` | Set SMS Aliases |
| `/tracking_resolution` | _set on configuration_ | Heading, Min & Max Time |
| `/gps_status` | `syrus.core.gps_status` | GPS Information | 
| `/configuration` | **PUT** `/dcontrol/device/:imei/config` | `configName`, `overwriteConfig` |

#### Pending 

| Legacy Remote API | Pending | 
| ----------------- | ------- |
| `/outputsetlog` | Remote 2.0 logs with details, timestamp and user_id |
| `/location2` & `/location` | Better way to query location |
| `/trigger_position_event` | Better way to get the location |

### Plugins

The Plugins API gave you the possibility to interact with the device's accessories. We can also find this plugins API in the [API Reference Section](https://cloud.pegasusgateway.com/api-static/docs/#api-Plugins)

| Legacy Plugins | Replaced With  | Description |
| ----------------- | -------------- | ----------- |
| `/photocam` | `syrus.acc.photocam` | Interact |
| `/garmin` | `syrus.acc.garmin` | Interact | 
| `/lumeway` | `syrus.acc.lumeway` | Interact | 



### Naming conventions

#### Device Control Configurations

| Name Starts With  | Compatibility | Min Fw |
| ----------------- | ------------- | ------ |
| `pegasus.one`  | Syrus 1, Syrus 2, Syrus 3, Syrus 3BT, Syrus CC+ | 1.3.61 |
| `pegasus.tt` | Syrus TT, Syrus 2, Syrus 3, Syrus CC+ | 2.1.41 |
| `pegasus.s2` | Syrus 2, Syrus 3, Syrus CC+| 2.1.47 |
| `pegasus.s3` | Syrus 3  | 3.3.35 |
| `pegasus.cc` | Syrus 2, CC, CC+, | 3.3.35 | 
| `pegasus.bt` | Syrus 3BT | 3.3.35 | 


#### Standard Configs

| Name | Description |
| ---- | ----------- |
| `pegasus.one.standard` | Syrus 1 Standard Configuration |
| `pegasus.standard` | Syrus 2 & 3 Standard Configuration |
| `pegasus.standard.lowdata` | Low Data Consumption | 
| `pegasus.standard.ecu` | Includes ECU Parameters | 
| `pegasus.standard.photocam` | Includes Photocam |
| `pegasus.standard.satcom` | Includes Satcom | 
| `pegasus.standard.lumeway` | Includes Lumeway |
| `pegasus.standard.mobileye` | Includes Mobileye |
| `pegasus.standard.temperature` | Includes Temperature |
| `pegasus.standard.technoton` | Includes Technoton |
| `pegasus.standard.ibutton` | Includes iButton |
| `pegasus.standard.fingerprint` | Includes Fingerprint | 
| `pegasus.standard.io_exp` | Includes IO Expander | 
| `pegasus.standard.serial_exp` | Includes Serial Expander |
| `pegasus.standard.audio` | Supports Audio 
| `pegasus.tt` | Trailer Tracker |
| `pegasus.cc` | Closed/Controlled CloudConnect |
| `pegasus.cc.open` | Open CloudConnect Plans | 
| `pegasus.cc.motorcycle` | Optimized for Motorcycles | 
| `pegasus.bt.standard` | Includes Bluetooth Tag |

#### Site Specific Configurations

| Name | Description |
| ---- | ----------- |
|`pegasus.`**:pid**`.name` | Syrus configuration specific for site id: **pid**
|`p129.standard.custom_name`| Syrus 2 Standard Configuration |
