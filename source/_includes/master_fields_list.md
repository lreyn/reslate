# Master Fields List

By default all fields marked in the `Base set` are included in [forwarders](#forwarders) unless you filter it in the general config. To add fields use the **keys** or **keys_include** options in the general field of the forwarders configuration

[json format](https://pegasus1.pegasusgateway.com/api/resources/rawdata/keys?auth=06e06ce41fea91c5cb75b471271e95cbe7ce77cd8cd2f45d4f097abe)

[sql format](https://github.com/dctdevelop/pegasus/blob/master/resources/eventKeysMySQL.sql)

[Spreadsheet with example values](https://docs.google.com/spreadsheets/d/1WBXq8NvWqPB6vRO2Tlye17JfrM5X3QZTlr9vs7uxVSk)


Field | Info | Type | Unit | Base set
-----:|------|------|------|---------
ac | GPS acceleration | number | mph/s | ✓
ad | ADC value | number | mV | ✓
age | GPS age | number | | ✓
aid | [Asset id](#assets) | number 
al | Altitude | number | meters | ✓
an_diff_in1_in2 | Analog diff. input 1, input 2 | number | mV | 
an_diff_in3_in4 | Analog diff. input 3, input 4 | number | mV | 
an_diff_in5_in6 | Analog diff. input 5, input 6 | number | mV | 
an_diff_in7_in8 | Analog diff. input 7, input 8 | number | mV | 
an_fdiff_in1_in2 | Analog filtered diff. input 1, input 2 | number | mV
an_fdiff_in3_in4 | Analog filtered diff. input 3, input 4 | number | mV
an_fdiff_in5_in6 | Analog filtered diff. input 5, input 6 | number | mV
an_fdiff_in7_in8 | Analog filtered diff. input 7, input 8 | number | mV
an_in1 | Analog input 1 | number | mV | 
an_in2 | Analog input 2 | number | mV | 
an_in3 | Analog input 3 | number | mV | 
an_in4 | Analog input 4 | number | mV | 
an_in5 | Analog input 5 | number | mV | 
an_in6 | Analog input 6 | number | mV | 
an_in7 | Analog input 7 | number | mV | 
an_in8 | Analog input 8 | number | mV | 
ap_cur_deg_x | Current angle in x-axis | number | degrees
ap_cur_deg_y | Current angle in y-axis | number | degrees
ap_cur_deg_z | Current angle in z-axis | number | degrees 
ap_cur_mag | Instant acceleration | number | milli-g
ap_mov | Movement by accelerometer | boolean | 
ap_ref_deg_x | Reference angle in x-axis | number | degrees 
ap_ref_deg_y | Reference angle in y-axis | number | degrees
ap_ref_deg_z | Reference angle in z-axis | number | degrees
axl_weight_trailer | Axle weight for trailer | number | tons
axl_weight_trailer_total | Total axle weight for trailer | number | tons
axl_weight_truck | Axle weight truck | number | tons
axl_weight_truck_total | Total axle weight for truck | number | tons
axl_weights | All axle weights | string | tons
axl_weights_01 | Axle 1 weight | number | tons
axl_weights_02 | Axle 2 weight | number | tons
axl_weights_03 | Axle 3 weight | number | tons
axl_weights_04 | Axle 4 weight | number | tons
axl_weights_05 | Axle 5 weight | number | tons
axl_weights_06 | Axle 6 weight | number | tons
axl_weights_07 | Axle 7 weight | number | tons
axl_weights_08 | Axle 8 weight | number | tons
bl | Device battery level | number | mV | ✓
btt_battery | Bluetooth tag battery | number | mV
btt_button | Bluetooth tag button | boolean | 
btt_driver | Bluetooth tag driver | number | 
btt_driver_set | Presence of bt tag driver | boolean | 
btt_freefall | Bluetooth tag freefall | boolean | 
btt_humidity | Bluetooth tag humidity | number | %
btt_impact | Bluetooth tag impact | boolean | 
btt_light | Bluetooth tag light | number | %
btt_mac | Bluetooth tag MAC | string | 
btt_motion | Bluetooth tag motion | boolean |  
btt_presence | Bluetooth tag presence | boolean | 
btt_reed | Bluetooth tag reed switch | boolean |
btt_temp | Bluetooh tag temperature | number | °C
btt_wreason | Bluetooth tag wake up reason | string | 
ce | Device ignition on timer | number | seconds
cf_cid | Cell ID | number | HEX | ✓
cf_lac | Local area code | number | HEX | ✓
cf_mcc | GSM Mobile country code | number | | |
cf_mnc | GSM Mobile network code | number | | |
cf_rssi | RSSI | number | dBm | ✓
cf_type | Radio link technology | string | | |
checkpoint | Checkpoint id | number |
cl | Device idle on timer | number | seconds 
climb_detection | Climb detected | boolean | 
code | Event Code | number | | ✓
comdelay | Communication delay | number | seconds
counter_ack_duration | Counter GSM | number | 
counter_bytes_mo | Counter GPRS | number | 
counter_bytes_mt | Counter GPRS bearer | number | 
counter_io_tamper | Total duration of tamper detection (case opened for Syrus 3/3G/CC devices only) | number | seconds
counter_io_in1 | Total duration of input 1 activation | number | seconds
counter_io_in2 | Total duration of input 2 activation | number | seconds
counter_io_in3 | Total duration of input 3 activation | number | seconds
counter_io_out2 | Total duration of output 1 activation | number | seconds
counter_io_out1 | Total duration of output 2 activation | number | seconds
counter_io_exp_in1 | Total duration of IO expander input 1 activation | number | seconds
counter_io_exp_in2 | Total duration of IO expander input 2 activation | number | seconds
counter_io_exp_in3 | Total duration of IO expander input 3 activation | number | seconds
counter_io_exp_in4 | Total duration of IO expander input 4 activation | number | seconds
counter_io_exp_out1 | Total duration of IO expander output 1 activation | number | seconds
counter_io_exp_out2 | Total duration of IO expander output 2 activation | number | seconds
counter_io_exp_out3 | Total duration of IO expander output 3 activation | number | seconds
counter_io_exp_out4 | Total duration of IO expander output 4 activation | number | seconds
counter_io_exp_out1_short | Total duration of IO expander output 1 short-circuit activation | number | seconds
counter_io_exp_out2_short | Total duration of IO expander output 2 short-circuit activation | number | seconds
counter_io_exp_out3_short | Total duration of IO expander output 3 short-circuit activation | number | seconds
counter_io_exp_out4_short | Total duration of IO expander output 4 short-circuit activation | number | seconds
counter_reset_gprs | Counter retransmissions | number | 
counter_reset_gprs_bearer | Counter bytes transmitted | number | 
counter_reset_gsm | Counter bytes received | number | 
counter_retransmissions | Counter transmissions | number | 
counter_transmissions | Counter acknowledge duration | number | 
cr | Device over rpm time | number | seconds
cs | Device over speed | number | seconds
cv<span style="color:#005596">00-49</span> | Counter value | number | | ✓
dev_dist | [Absolute total distance traveled](#counters) | number | meters |
dev_idle | [Absolute total idling count](#counters) | number | seconds |
dev_ign | [Absolute total ignition ON count](#counters) | number | seconds | 
dev_orpm | [Absolute total over RPM time](#counters) | number | seconds | 
dev_ospeed | [Absolute over speed time](#counters) | number | seconds | 
device_id | IMEI (unique identifier per device) | number | | ✓
dphoto_ptr | [Photo ID](#photocam) | number | seconds | 
ea_a | [Analog / temp sensor probe A](#temperature-sensor-analog-interface) | number | mV / °C | ✓
ea_b | [Analog / temp sensor probe B](#temperature-sensor-analog-interface) | number | mV / °C | ✓
ea_c | [Analog / temp sensor probe C](#temperature-sensor-analog-interface) | number | mV / °C | ✓
ecu_ac_high_pressure_fan | A/C high pressure fan switch | number | 
ecu_aftmt_doc_intk_tmp | Aftertreatment DOC intake temp | number | °C
ecu_aftmt_dpf_diff_psi | Aftertreatment DPF diff pressure | number | psi
ecu_aftmt_dpf_intake_tmp | Aftertreatment DPF intake temp | number | °C
ecu_aftmt_dpf_outlet_tmp | Aftertreatment DPF outlet temp | number | °C
ecu_aftmt_dpf_soot_load | Aftertreatment DPF soot regeneration | number | %
ecu_aftmt_intake_nox | Aftertreatment intake NOx | number | ppm
ecu_aftmt_outlet_nox | Aftertreatment outlet NOx | number | ppm
ecu_aftmt_purge_air_act | Aftertreatment purge air actuator | number |
ecu_aftmt_scr_intake_tmp | Aftertreatment SCR intake temp | number | °C
ecu_aftmt_scr_outlet_tmp | Aftertreatment SCR outlet temp | number | °C
ecu_aload | [Absolute load](#ecu-monitor) | number | % | 
ecu_ambient_air_tmp | [Ambient air temperature](#ecu-monitor) | number | °C | 
ecu_battery | [Vehicle battery level](#ecu-monitor) | number | mV | ✓
ecu_bms1_total_voltage | [BMS1 Total voltage](#ecu-monitor) | number | V | ✓
ecu_bms1_total_amps | [BMS1 Total current](#ecu-monitor) | number | A | ✓
ecu_bms1_soc | [BMS1 SOC - State of charge](#ecu-monitor) | number | % | ✓
ecu_bms1_positive_insulation_resistance | [BMS1 positive insulation resistance](#ecu-monitor) | number | kOhm | ✓
ecu_bms1_negative_insulation_resistance | [BMS1 negative insulation resistance](#ecu-monitor) | number | kOhm | ✓
ecu_bpressure | [Barometric pressure](#ecu-monitor) | number | kPa | 
ecu_brake_pedal | [Brake pedal pressed](#ecu-monitor) | number | % | 
ecu_ccontrol | [Cruise control state](#ecu-monitor) | number | | 
ecu_ccontrol_set_speed | Cruise control set speed | number | kph | 
ecu_clutch_pedal | [Clutch pedal](#ecu-monitor) | number | | 
ecu_cool_lvl | [Coolant level](#ecu-monitor) | number | % | ✓
ecu_cool_psi | [Coolant pressure](#ecu-monitor) | number | psi | ✓
ecu_cool_tmp | [Coolant temperature](#ecu-monitor) | number | °C | ✓
ecu_ddemand | [Driver's demand engine torque](#ecu-monitor) | number | % | 
ecu_def_consumed | [Diesel exhaust fluid consumed](#ecu-monitor) | number | liters | 
ecu_def_level | [Diesel exhaust fluid level](#ecu-monitor) | number | % | 
ecu_def_tmp | [Diesel exhaust fluid temperature](#ecu-monitor) | number | °C | 
ecu_dist | [Engine distance traveled](#counters) | number | meter
ecu_distance | [Total distance](#ecu-monitor) | number | meters | ✓
ecu_dpf_intake_psi | Diesel particulate filter intake pressure| number | psi
ecu_dpf_soot_load | Diesel particulate filter soot load | number | %
ecu_dpf_status | Diesel particulate filter active regeneration status | number | 
ecu_dtc_cleared | [Time since DTC cleared](#ecu-monitor) | number | minutes | 
ecu_eidle | [Engine engine idling time](#counters) | number | centihours
ecu_eload | [Engine load](#ecu-monitor) | number | % | 
ecu_eng_crank_psi | Engine crankcase pressure | number | psi
ecu_eng_egr_diff_psi | Engine exhaust gas recirculation differential pressure | number | psi
ecu_eng_egr_maf | Engine exhaust gas recirculation mass flow rate | number | kg/hr
ecu_eng_egr_tmp | Engine exhaust gas recirculation temperature | number | °C
ecu_eng_egr_valve_control | Engine exhaust gas recirculation valve control | number | 
ecu_eng_egr_valve_pos | Engine exhaust gas recirculation valve position | number |  
ecu_eng_exhaust_psi | Engine exhaust gas pressure | number | psi
ecu_eng_exhaust_tmp | Engine exhaust gas temperature | number | °C
ecu_eng_intake_manif_psi | Engine intake manifold pressure | number | psi
ecu_eng_load | Engine percent load at current speed | number | %
ecu_eng_maf | Engine intake air mass flow rate | number | kg/hr
ecu_eng_oil_lvl | [Oil level](#ecu-monitor) | number | % | ✓
ecu_eng_oil_psi | [Oil pressure](#ecu-monitor) | number | psi | ✓
ecu_eng_oil_tmp | [Oil temperature](#ecu-monitor) | number | °C | ✓
ecu_eng_pto_governor_enable | Engine PTO governor enable switch | number | 
ecu_eng_pto_pprog_speed_control | Engine remote pto governor preprogrammed speed control switch | number |
ecu_eng_ref_torque | Engine reference torque | number | N m
ecu_eng_turbo_intake_psi | Engine turbocharger compressor intake pressure | number | psi
ecu_eng_turbo_intake_tmp | Engine turbocharger compressor intake temperature | number | °C
ecu_eng_turbo_rpm | Engine turbocharger speed | number | rpm
ecu_eng_vgt_act | Engine turbocharger actuator | number | 
ecu_eng_vgt_control_mode | Engine variable geometry turbocharger control mode | number | 
ecu_eng_vgt_position | Engine turbocharger actuator position | number | %
ecu_eon | [Time since engine ON](#ecu-monitor) | number | seconds | ✓
ecu_error<span style="color:#005596">1-7</span> | [Engine error code](#ecu-monitor) | number | | ✓
ecu_eusage | [Engine ignition on count](#counters) | number | centihours
ecu_fan_state | [Fan state](#ecu-monitor) | number | | ✓
ecu_fpressure | [Fuel pressure](#ecu-monitor) | number | | 
ecu_fuel_iconsumption | [Instant fuel consumption](#ecu-monitor) | number | centi-liters/hours | ✓
ecu_fuel_level | [Analog fuel level](#ecu-monitor) | number | mV | ✓
ecu_fuel_level_real | [Fuel level](#ecu-monitor) | number | % | ✓
ecu_fuel_tmp | Engine fuel temperature | number | °C
ecu_hours | [Total engine usage](#ecu-monitor) | number | centihours | ✓
ecu_hours_idle | [Total engine usage while idling](#ecu-monitor) | number | centihours | ✓
ecu_hydr_oil_lvl | [Hydraulic oil level](#ecu-monitor) | number | % | ✓
ecu_hydr_oil_psi | [Hydraulic oil pressure](#ecu-monitor) | number | psi | ✓
ecu_hydr_oil_tmp | [Hydraulic oil temperature](#ecu-monitor) | number | °C  | ✓
ecu_idle_fuel | [Fuel consumed while idle](#ecu-monitor) | number | deciliters | ✓
ecu_ifuel | [Engine fuel consumed while Idling](#counters) | number | centiliters
ecu_ins_efficiency | [Fuel instant efficiency](#ecu-monitor) | number | deci-km/l | ✓
ecu_intake_air_tmp | [Intake air temperature](#ecu-monitor) | number | °C | 
ecu_intake_manif_tmp | [Intake manifold temperature](#ecu-monitor) | number | °C | ✓
ecu_maf | [Malfunction Indicator Lamp code](#ecu-monitor) | number | | ✓
ecu_mil_error_code | [Malfunction Indicator Lamp code](#ecu-monitor) | number | | ✓
ecu_mil_error_count | [Malfunction Indicator Lamp count](#ecu-monitor) | number | | ✓
ecu_mil_state | [Malfunction Indicator Lamp state](#ecu-monitor) | boolean | | ✓
ecu_nominal_friction_torque | Nominal friction percent torque | number | %
ecu_obd_auxios | [OBD auxiliary I/Os](#ecu-monitor) | number | | 
ecu_obd_ftype | [Fuel type](#ecu-monitor) | number | | 
ecu_oxygen | [Oxygen level](#ecu-monitor) | number | mV | ✓
ecu_pg<span style="color:#005596">1-4</span> | [Custom PGNs](#ecu-monitor) | number | | ✓
ecu_pto | [PTO status](#ecu-monitor) | number | |
ecu_rbatt | [Hybrid battery life](#ecu-monitor) | number | % |
ecu_remote_accel_enable | Remote accelerator enable switch | number | 
ecu_remote_accel_pedal | Remote accelerator pedal position | number | %
ecu_retarder_brake_assist | Retarder enable - brake assist switch | number | 
ecu_rpm | [Revolutions per minute](#ecu-monitor) | number | rpm | ✓
ecu_serv_distance | [Distance left to service vehicle](#ecu-monitor) | number | km | 
ecu_speed | [Engine speed](#ecu-monitor) | number | kph | 
ecu_tfuel | [Engine total fuel consumed](#counters) | number | deciliters 
ecu_throttle | [Throttle pedal position](#ecu-monitor) | number | % | ✓
ecu_tires_psi | [Tires pressure](#tpms) | string | psi |
ecu_tires_tmp | [Tires temperature](#tpms) | string | °C | ✓
ecu_torque | [Torque position](#ecu-monitor) | number | % | ✓
ecu_total_fuel | [Total fuel consumed](#ecu-monitor) | number | deciliters | ✓
ecu_total_run_time | Total ECU run time | number | hour | 
ecu_tpms_provision | [TPMS provision](#tpms) | string | | 
ecu_tpms_warnings | [TPMS warnings](#tpms) | string | | 
ecu_tpms_conditions | [TPMS conditions](#tpms) | string | | 
ecu_trans_lvl | [Transmission oil level](#ecu-monitor) | number | % | ✓
ecu_trans_psi | [Transmission oil pressure](#ecu-monitor) | number | psi | ✓
ecu_trans_tmp | [Transmission oil temperature](#ecu-monitor) | number | °C | ✓
ecu_trip_fuel | [Trip fuel](#ecu-monitor) | number | liters | ✓
ecu_trip_distance | [Trip distance](#ecu-monitor) | number | meter | ✓
ecu_vin | [VIN number](#ecu-monitor) | string | | 
ecu_water_in_fuel | Water in fuel indicator | number | 
ecu_weights | [Vehicle weights](#ecu-monitor) | string | kg | ✓
ecu_with_mil_distance | [Distance traveled with MIL on](#ecu-monitor) | number | meter | ✓
ecu_with_mil_time | [Time traveled with MIL on](#ecu-monitor) | number | minutes | 
ecu_ev_vehicle_speed | Electric vehicle speed | number | kph |
ecu_main_pos_relay_flt | Fault of vehicle main-positive contactor | number | |
ecu_insulation_detection_status | Work status: Insulation detection status | number | |
ecu_fault_level_of_insulation_detection | Fault level of insulation detection | number | |
ecu_positive_insulation_value | Positive insulation value | number | kohm |
ecu_negative_insulation_value | Negative insulation value | number | kohm |
ecu_rated_capacity_battery | Rated capacity of battery | number | Ah |
ecu_rated_voltage_battery | Rated voltage of battery | number | V |
ecu_rated_total_energy_battery | Total energy of battery | number | kWh |
ecu_max_available_voltage_batt | Maximum available voltage of battery pack | number | V |
ecu_min_available_voltage_batt | Minimum available voltage of battery pack | number | V |
ecu_max_available_temp_batt | Maximum available temperature of battery pack | number | C |
ecu_min_available_temp_batt | Minimum available temperature of battery pack | number | C |
ecu_state_of_charge | SOC. State of charge | number | % |
ecu_state_of_health | SOH. State of health | number | % |
ecu_b2v_pack_current | Battery pack current | number | A |
ecu_b2v_pack_in_high_voltage | Voltage inside the battery | number | V |
ecu_accumulated_charge | Accumulated charged electric quantity | number | kwh |
ecu_accumulated_discharge | Accumulated discharged electric quantity | number | kwh |
ecu_single_charge_electric | Single charged electric quantity | number | kwh |
event_epoch | Event time as epoch | number | |
event_time | Time the event was generated by the device (YYYY-MM-DDThh:mm:ss) | string | | ✓
flow_meter_state | Flow Meter State | boolean | |
flow_meter_display_state | Flow Meter Display State | boolean | |
flow_meter_current_flow | Flow Meter Current Flow | number | |
flow_meter_src_pwr_disc | Flow Meter Source Power Disconnected | number | |
flow_meter_sensor_disc | Flow Meter Sensor Disconnected | number | |
flow_meter_calibration_pattern | Flow Meter Calibration Pattern | number | |
flow_meter_volume_dispatched | Flow Meter Volume Dispatched | number | |
flow_meter_total_volume | Flow Meter Total Volume | number | |
fpr_code | Fingerprint code | number | | ✓
fpr_conn | Fingerprint connection | boolean | | ✓
fpr_id | Fingerprint ID | number | | ✓
groups | Group ids | array | | 
hdop | HDOP value | number | | ✓
head | Vehicle direction 0 = North, 180 = South | number | degrees | ✓
ib | [iButton ID](#ibutton) | string | | ✓ 
ib_set | [iButton present](#ibutton) | boolean | | 
id | Event unique ID (up to BIGINT) | number | | ✓
ii | Second IMEI | number | | ✓
io_exp_in1 | [Expanded input 1](#input-output-expander) | boolean | | ✓
io_exp_in2 | [Expanded input 2](#input-output-expander) | boolean | | ✓
io_exp_in3 | [Expanded input 3](#input-output-expander) | boolean | | ✓
io_exp_in4 | [Expanded input 4](#input-output-expander) | boolean | | ✓
io_exp_out1 | [Expanded output 1](#input-output-expander) | boolean | | ✓ 
io_exp_out1_short | [Expanded output 1 short circuit](#input-output-expander) | boolean | | ✓ 
io_exp_out2 | [Expanded output 1](#input-output-expander) | boolean | | ✓ 
io_exp_out2_short | [Expanded output 1 short circuit](#input-output-expander) | boolean | | ✓ 
io_exp_out3 | [Expanded output 1](#input-output-expander) | boolean | | ✓ 
io_exp_out3_short | [Expanded output 1 short circuit](#input-output-expander) | boolean | | ✓ 
io_exp_out4 | [Expanded output 1](#input-output-expander) | boolean | | ✓ 
io_exp_out4_short | [Expanded output 1 short circuit](#input-output-expander) | boolean | | ✓ 
io_exp_state | [IO expander state](#input-output-expander) | boolean | | ✓
io_ign | Ignition | boolean | | ✓
io_in1 | Input 1 | boolean | | ✓
io_in2 | Input 2 | boolean | | ✓
io_in3 | Input 3 | boolean | | ✓
io_out1 | Output 1/Safe immobilization | boolean | | ✓ 
io_out1_short | Output 1 short circuit | boolean | | ✓
io_out2 | Output 2 | boolean | | ✓
io_out2_short | Output 2 short circuit | boolean | | ✓
io_pwr | Device power | boolean | | ✓
io_tamper | Tamper detected (case opened) | boolean | 
ip | IP address | number | | ✓
jamm_detected | Communication jamming detected | boolean | | ✓
kph | GPS speed | number | kph |
ky | [Device configuration key](#managed-legacy-configurations) | string | 
label | Label | string | | ✓ 
lat | GPS Latitude | number | | ✓ 
light_sensor | Light sensor | boolean | | ✓ 
lon | GPS Longitude | number | | ✓
message | Device event message | string | | 
moving | Moving | boolean | | 
mph | Speed | number | mph | ✓
pc | Pulse counter | number | | ✓
pdop | PDOP value | number | | ✓
pid | Pegasus site ID | number | | ✓
port | Port | number | | ✓
primary_name | Entity name | string | | 
re_index | Region index | number | | ✓
re_sense | Region sense | boolean | | ✓
re_type | Region type | number | | ✓
rfi_fac | RFID facility code | number | | 
rfi_full_id | Full rfid code | string | | 
rfi_id | RFID | number | | 
sa_seqn | Sequential ack | number | | 
sa_type | Sequential acknowledge type | number | | 
source | GPS source | number | | ✓
sv | Satellites in view | number | | ✓
system_epoch | Epoch time event was received on the server | number | |
system_time | Time event was received on the server (YYYY-MM-DDThh:mm:ss.sssss) | string | | ✓
timestamp | Epoch timestamp | number | |
server.timestamp | Epoch timestamp of the server | number | |
tc<span style="color:#005596">XX</span> | Time counter values | number | | ✓
tec_1_ff | Technoton 1 frequency | number | Hz
tec_1_fn | Technoton 1 level | number | liters 
tec_1_ft | Technoton 1 temperature | number | °C
tec_1_st | Technoton 1 state | boolean | | 
tec_2_ff | Technoton 2 frequency | number | Hz
tec_2_fn | Technoton 2 level | number | liters 
tec_2_ft | Technoton 2 temperature | number | °C
tec_2_st | Technoton 2 state | boolean | | 
tec_3_ff | Technoton 3 frequency | number | Hz
tec_3_fn | Technoton 3 level | number | liters 
tec_3_ft | Technoton 3 temperature | number | °C
tec_3_st | Technoton 3 state | boolean | | 
tec_ff | Technoton fuel frequency | number | Hz  
tec_fn | Technoton fuel level | number | liters  
tec_ft | Technoton fuel temperature | number | °C 
tec_st | Technoton state | boolean | | 
temp | General purpose temperature | number | °C | 
temp1 | General purpose temperature 1 | number | °C | 
temp2 | General purpose temperature 2 | number | °C | 
temp3 | General purpose temperature 3 | number | °C | 
temp4 | General purpose temperature 4 | number | °C | 
temp5 | General purpose temperature 5 | number | °C | 
temp6 | General purpose temperature 6 | number | °C | 
ti_sense | Signal sense | boolean | |
ti_signal | Signal | string |   
ti_thr | Signal threshold | number | |
ti_val | Signal value | number | | 
trip_id | Trip ID | number | | 
tx | RS-232 MDT data | string | | ✓
type | Event type | number | | ✓
usense_hour | Ultrasonic fuel sensor hour | number | hour
usense_minute | Ultrasonic fuel sensor minute | number | minute
usense_filtered_value | Ultrasonic fuel sensor filtered value | number | 
usense_signal_strength | Ultrasonic fuel sensor signal strength | number | 
usense_software_code | Ultrasonic fuel sensor software code | number | 
usense_hardware_code | Ultrasonic fuel sensor hardware code | number | 
usense_raw_value | Ultrasonic fuel sensor raw unfiltered value | number | 
usense_median_value | Ultrasonic fuel sensor median value | number | 
usense_valid_signal | Ultrasonic fuel sensor valid signal | number | 
usense_tilt_angle | Ultrasonic fuel sensor tilt angle | number | 
valid_position | Valid position | boolean | | ✓
vdop | VDOP value | boolean | | ✓
vehicle_dev_dist | User editable device distance | number | meters | 
vehicle_dev_dist__km | User editable device distance | number | km | 
vehicle_dev_dist__mi | User editable device distance | number | miles | 
vehicle_dev_idle | User editable total idle time | number | seconds | 
vehicle_dev_ign | User editable total engine on time | number | seconds | 
vehicle_dev_orpm | User editable over rpm time | number | seconds | 
vehicle_dev_ospeed | User editable over speed time | number | seconds | 
vehicle_ecu_dist | User editable engine odometer | number | meters | 
vehicle_ecu_dist__km | User editable engine odometer | number | km | 
vehicle_ecu_dist__mi | User editable engine odometer | number | miles | 
vehicle_ecu_eidle | User editable engine idle time | number | seconds | 
vehicle_ecu_eusage | User editable engine on time | number | seconds | 
vehicle_ecu_ifuel | User editable fuel consumed while idling | number | liters | 
vehicle_ecu_tfuel | User editable total fuel consumed | number | liters | 
vid | Vehicle id | number | |
vinfo | Vehicle info | object | |  
vo | *Deprecated, use counters api ([dev_dist](#counters))* 