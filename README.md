# esphome-multi-presence
esphome yaml file for multi presence sensors using bluetooth and LD2410 sensor.

This project use esp32 with LD2410 sensor attached to enhance capabilities.
1. [LD2410 sensor](https://esphome.io/components/sensor/ld2410.html) 
2. [MQTT Room Presence](https://www.home-assistant.io/integrations/mqtt_room/) integartion using bluetooth to mqtt.
3. [Bluetooth Proxy](https://esphome.io/components/bluetooth_proxy.html)
4. [BLE gateway](https://github.com/myhomeiot/esphome-components) to use with [Passive BLE Monitor integration](https://github.com/custom-components/ble_monitor)



## How to install
1.  Add the file ```bluetooth-proxy-with-ld2410.yaml``` to your esphome folder.
you can rename the file and add a dot before the name to hide this file from esphome dashboard.
![esphome folder](/images/esphomefolder.png)


2. Create new device in esphome and add the following. (change the substitutions variables and api key)
```
substitutions:
  name: your device name
  devicename: your device name # (lower-case) hostname, ap
  friendlyname: your device friendlyname # mqtt, sensors.
  topic: your mqtt topic
  gateway_id: your gateway id
api:
  encryption:
     key: your key

<<: !include .bluetooth-proxy-with-ld2410.yaml
```
3. add the following to ESPHOME secrets. (change the values...)
```
wifi_ssid: "wifi_ssid"
wifi_password: "wifi_password"
mqtt_broker: mqtt_broker ip
mqtt_username: username
mqtt_password: password
```
4. Install
## Wiring
```
ESP32                 LD2410
5V ------------------ VCC
GND ----------------- GND
RX ------------------- Uart-Tx
TX ------------------- Uart-Rx 
```
In my file ESP32 TX pin is 17 and RX pin is 16, its depend on the borad, so maybe need to change it.


## Configuration
This project aim to replicate the abiltities of [espresence](https://espresense.com/) or [openmqttgateway](https://docs.openmqttgateway.com/) with the addittional LD2410 sensor and bluetooth proxy. 

After the installation you can configure it in Home Assistant.  

Config the LD2410 as described in [ESPHOME docs](https://esphome.io/components/sensor/ld2410.html). you can uncomment the end of file (g0-g8 sensors)and turn on engineering mode switch to calibrate the gates threshold/ 

Config the bluetooth presence settings using the ```mqtt_room_distance``` ```measured_power``` ```environmental_factor``` sensors.
- ```mqtt_room_distance```  for room size in meters.
- To calibrate  ```measured_power``` put your ble device 1 meter from the esp32 device and check the average rssi. 
-  To calibrate the  ```environmental_factor``` you need to play with it to get distance of 1 meter in the mqtt response. (thanks to  [this](https://stackoverflow.com/a/65124579))