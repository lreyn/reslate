# Bluetooth

##Overview
Communication over bluetooth allow send and receive data to the syrus using the `TAIP` protocol in a security mode. With this other devices can ask,configure, and query the syrus in a nearby area.

<aside class="notice">
	Bluetooth Communication is a unique feature of <b>Syrus 3GBT and Syrus Tags</b>
</aside>


##Requisites
1. Syrus Bluetooth or Syrus Tag
2. Device with Bluetooth Peripheral
 - Bluetooth  Low Energy 3.0 or superior
3. Pegasus Gateway User

##Explanation
![alt text](slate/img/bluetooth-diagram.png "Bluetooth Diagram Explanation")


La comunicacion bluetooth agrega una capa adicional y paralela de comunicacion con syrus independiente del gateway, a su vez Syrus puede comunicarse con los tags.

Los comandos son enviados en formato **TAIP** de manera similar a la comunicacion via cable serial, permitiendo asi el envio de  casi cualquier comando disponible en syrus.

Se utiliza **Low Energy Bluetooth** no confirmada para la comunicacion. Enviados en paquetes a traves de caracteristicas. los paquetes **no** deben superar los 63 bytes de informacion.

Asi mismo la recepcion de paquetes mayores a 63 bytes no esta soportada. Para datos mayores a 63 bytes la data sera dividida y enviada en chunks debe ser reemsamblada e interpretada en el _endpoint_


La data esta protegida por un keyphrase  manejada a traves de pegasus, syrus no respondera a ningun comando **unless** the keyphrase sea proporcionada

los comandos enviados y sus respectivas respuestas son asincronas, y, aunque syrus responde del primero al ultimo enviado, el tiempo entre pregunta<->respuesta es variable. comandos de proceso pesado como >QXAIM< pueden tardar hasta 3 segundos en llegar. 


##Services and Characteristics
###Syrus
Syrus exposes only one (1) service for the communication:

| Service       | UUID                                   | Description                                          |
|---------------|:-------------------------------------- |:---------------------------------------------------- |
| Communication | 00000000-dc70-0080-dc70-a07ba85ee4d6   | All the communication Protocol is going here         |


and 2 characteristics:

| Characteristic        | UUID                                   | Description                                  |
|-----------------------|:---------------------------------------|:---------------------------------------------|
| Communication         | 00000000-dc70-0180-dc70-a07ba85ee4d6   | Allow send and receive data in TAIP Protocol |
| Events                | **TODO:** In develop                   | Allow to receive Events from the syrus       |


all the data is sent **without confirmation** to improve the performance.

###Tags
Tags exposes only one (1) service for the communication:

| Service       | UUID                                   | Description                                          |
|---------------|:-------------------------------------- |:---------------------------------------------------- |
| Communication | 00000000dc700070dc70a07ba85ee4d6       | All the communication Protocol is going here         |


and 3 characteristics:8100

| Characteristic        | UUID                                   | Description                                  |
|-----------------------|:---------------------------------------|:---------------------------------------------|
| Query tag Data        | 00000000-dc70-1070-dc70-a07ba85ee4d6   | Allow read data from the tag                 |
| Configuration         | 00000000-dc70-2070-dc70-a07ba85ee4d6   | Allow configurate the tag                    |
| Commands              | 00000000-dc70-3070-dc70-a07ba85ee4d6   | Allow send commands to  the tag              |

<aside class="notice">
	This documentation doesn't cover the tag communication protocol. For more information please read the  
	<a href="#!"> Tag Documentation</a>
</aside>


## Syrus Communication
### Establishing connection
Syrus emit the name like **Syrus 3GBT ABCDE** where:

`ABCDE` is the last five digits of the syrus IMEI



```js
//********* SENDING DATA TO SYRUS **********///
// get uuid from the syrus 
var device_id = getDeviceId();
// Data must be array of bytes
var data = stringToBytes(">QPV<");
// send data to syrus without response
ble.writeWithoutResponse(device_id,
	"00000000-dc70-0080-dc70-a07ba85ee4d6",
	"00000000-dc70-0180-dc70-a07ba85ee4d6", 
	data, 
	response_callback(){}, error_callback(){}
	);
function stringToBytes(string){
	var array = new Uint8Array(string.length);
	for(var i = 0, l = string.length; i < l; i++){
		array[i] = string.charCodeAt(i);
}
return array.buffer;
}
```

### Sending Data

To send data to  syrus must be send over Communication Service and Communication Characteristic, the example show how to send a command using javascript and cordova library BLE plugin central. you can read more about 
8100
[BLE PLUGIN CENTRAL](https://github.com/don/cordova-plugin-ble-central)


<aside class="warning">
	<b>WARNING</b> <br>
 	<i>
 		Data sent to the syrus must be on array of bytes  see the example above to convert the string  to bytes, see example.
 	</i>
</aside>

###Receiving data
```js
//******* RECEIVING DATA FROM SYRUS *************///
// get uuid from the syrus 
var device_id = getDeviceId();
ble.startNotification(device_id, 
"00000000-dc70-0080-dc70-a07ba85ee4d6",
"00000000-dc70-0180-dc70-8100a07ba85ee4d6"
, success(data), error(err));

function success(data)
{
	var command = bytesToString(data);
	console.log(command) // show th command in console
}
function error(err){
	console.error(err); // output the error on console
}
function bytesToString(buffer) {
	return String.fromCharCode.apply(null, new Uint8Array(buffer));
} 

```

To receive data from syrus you need to subscribe to the communication characteristic and expect changes, each package of data contains at max of 63 bytes, if the command received contains more than the limit the string will be spitted in n packages, and the user need to join the packages.

the example show how to send a command using javascript and cordova library BLE plugin central.
<aside class="warning">
	<b>WARNING</b> <br>
 	<i>
 		Data received from the syrus gonna be on array of bytes  see the example above to convert the bytes  to string, see example.
 	</i>
</aside>

###Keeping alive the connection
Syrus implements a protocol of disconnection, to prevent undesired connection over time. if a device connected to the syrus it's not send commands in a lapse of time of 2 minutes, will be disconnected.

To keep alive the connection you can send a command in a interval of time. Every 45 seconds it's a good interval to ask syrus a simple query like Query Position (>QPV<) 


###Limitations
1. Bluetooth Protocol is limited to send/receive 63 bytes by package of data, this is a hardware limitation
2. Syrus can connect at max of one Android device and one IOs device simultaneous, sometimes is possible connect to multiples android devices if the hardware bluetooth is different in each devices.
3. Most of personal devices (windows,Linux,android,ios,mac,etc...) can connect  at max of 6 syrus simultaneous, but it's not recommended because  can generate los8100t packages and other security issues.
4. Sometimes some packages could be loss if the syrus is occupied with another process, bluetooth layer if below in priority 
that Core process.
5. Send multiple commands when syrus is busy could produce glitches on the communication, so it's recommended send commands only when syrus have returned the response.


##Authorization

 ```js
//********* SENDING PIN TO SYRUS **********///
// get uuid from the syrus 
var device_id = getDeviceId();
var device_imei = getDeviceImei().substring(-5);
// Data must be array of bytes
var data = stringToBytes(">SBIK"+ device_imei + "<");
// send data to syrus without response
ble.writeWithoutResponse(device_id,
	"00000000-dc70-0080-dc70-a07ba85ee4d6",
	"00000000-dc70-0180-dc70-a07ba85ee4d6", 
	data, 
	response_callback(), error_callback()
	);
function stringToBytes(string){
	var array = new Uint8Array(string.length);
	for(var i = 0, l = string.length; i < l; i++){
		array[i] = string.charCodeAt(i);
}
return array.buffer;
}
```

Syrus implements a protocol of security authentication. A `Keycode` based on the IMEI of the device by default, this command must be the first thing send over bluetooth.

if Syrus don't receive the keycode in **lapse time from 10 seconds**, it will gonna close the connection. This code can be get in the Pegasus Gateway 

By default the keycode will be the last five digits of the syrus IMEI, its recommended changes the code after installation.
you can change the command sending `>CBIK{old_pin};{new_pin}<`.


To send the pin to syrus after you get the code from pegaus, you must send the code >SBIK{pin_code}<. if syrus respond to the next command the authentication was successfully.


