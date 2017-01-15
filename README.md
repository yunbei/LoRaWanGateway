# LoRaWanGateway
A LoRaWan Gateway in LUA
## Goals
+ Port the OneChannelGateway to NodeMCU/LUA (https://github.com/things4u/ESP-1ch-Gateway) - Done!
+ Add support for sending messages to nodes - Done!
+ Add support for OTAA - Done!
+ Add support for receiving multiple SF's on a channel - Done!
+ Add support for more than one channel

## Why LUA?
Lua is an event driven language, which is a good match for a gateway: a gateway has to respond to incoming messages from nodes and routers.
The LoraWanGateway runs on a ESP8266 chip, with the NodeMCU firmware it runs a full WiFi stack and a lua runtime. The NodeMCU contains a 
lot of libraries that are usefull for a gateway: WiFi, NTP support, SPI, 6 timers, json support etc.

## Hardware
In order to run a LoRaWanGateway you need
+ A ESP8266 module
+ A SX1278 module
+ A way to flash your ESP8266

Connections
<table>
<tr><th>ESP PIN</th><th>SX1276 PIN</th></tr>
<tr><td>D1</td><td>DIO0</td></tr>
<tr><td>D2</td><td>DIO1</td></tr>
<tr><td>D5</td><td>SCK</td></tr>
<tr><td>D6</td><td>MISO</td></tr>
<tr><td>D7</td><td>MOSI</td></tr>
<tr><td>D0</td><td>NSS</td></tr>
<tr><td>GND</td><td>GND</td></tr>
<tr><td>3.3V</td><td>VCC</td></tr>
</table>

## How to run

The LoRaWanGateway needs quite some RAM and processing power, so it it necessary to flash firmware that uses as little resources as possible. 
Therefore you have to build NodeMCU firmware containing only the modules needed. The build I'm using can be found in the firmware directory, along with the nodemcu flasher application.

+ Use the firmware in the firmware directory

OR

+ Get the latest NodeMCU firmware on http://nodemcu-build.com/index.php 
	+ select the dev branch
	+ select the following modules: bit,cjson,encoder,file,gpio,net,node,rtctime,sntp,spi,tmr,uart,wifi

+ Flash the integer version on your ESP8266
	+ connect your ESP8266 to a serial port
	+ start ESP8266Flasher.exe in the firmware directory
	+ choose the correct serial port
	+ push the Flash button
	
+ Format ESP8266 filesystem
+ Register your wifi network
	+ in the lua shell run 
		+ wifi.setmode(wifi.STATION)
		+ wifi.sta.config("your SSID","your key")
		+ wifi.sta.autoconnect(1)
		+ wifi.sta.connect()
	+ Your ESP8266 will remember your wifi settings!
+ Upload all files in the src directory to your ESP8266
+ Restart your ESP8266
+ The LoRaWanGateway will start after first compiling all your sources


The LoRaWanGateway is configured to listen on EU channel 0.

Changing the configuration can be done in init.lua (channel, location of gateway, pin mappings for DIO0 and DIO1)

It will listen to all spreading factors but only on one specific channel. It will send on whatever channel or datarate the router seems fit...

## Revisions

* 2017-01-15 add firmware directory containing flasher and nodemcu firmware
+ 2017-01-08 change UDP send because latest firmware changed udpsocket.send method.
+ 2016-09-21 add GW_ALT and GW_NSS parameters to init.lua, fix stat message
+ 2016-08-12 measure RSSI in Lora mode, speed up SPI bus, cpufreq 160Mhz
+ 2016-08-08 receive messages on all spreading factors
+ 2016-07-04 changed SX1278 NSS pin to D0
+ 2016-07-03 refactor to use integer version of firmware in order to have more free resources
+ 2016-07-02 initial revision


 



