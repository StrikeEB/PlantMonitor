## Table of Contents  

- [Plant Health Monitoring Project](#plant-health-monitoring-project)
  * [Project architecture](#project-architecture)
  * [Workstream 1 – how to sense and share data](#workstream-1---how-to-sense-and-share-data)
  * [Workstream 2 – building a gateway for obtaining data](#workstream-2---building-a-gateway-for-obtaining-data)
  * [Workstream 3 – setting up a database, analysing and visualising data](#workstream-3---setting-up-a-database--analysing-and-visualising-data)

# Plant Health Monitoring Project

A simple and fun project to monitor plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). 

The solution will provide _insights at a glance_ into plant health using time-series data on temperature, soil humidity and moisture. 

An example dashboard visualising plant monitor outputs:

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/Example%20Grafana%20Plant%20Monitor%20Dashboard_Credits%20to%20Ivana%20Huckova.jpg)
_[Credits to Ivana Huckova]( https://grafana.com/blog/2021/03/08/how-i-built-a-monitoring-system-for-my-avocado-plant-with-arduino-and-grafana-cloud/)_

This project focuses on _sensing and insights visualisation_. The project is based around a single plant, but the methodology and solution architecture introduced are easily scalable. You can use this project as a basis to then add sensors for further insights, or scale by increasing the number of data collection nodes to inform about performance of multiple plants – even a whole farm! At the end of this READ ME file I also provide some commentary how this simple prototype can be improved further by scaling the system horizontally and vertically.

_Disclaimer: this project is set up as part of the coursework for MSc Connected Environments at UCL module CASA0014_


## Project architecture

The plant monitor uses Feather Huzzah ESP8266 as a data collection node to which sensors are added. It has an inbuilt Wi-Fi microcontroller and is programmed using Arduino IDE to send time series data to the MQTT protocol, which then acts as a broker and sends data to its permitted subscribers. We used Raspberry PI (RPI) as our servers to subscribe to the data and installed Telegraf which is a server-based agent that helps to request data from MQTT and push it to the time-series InfluxDB database. The final step was to set up Grafana, which would retrieve data from InfluxDB and enable us to build automated dashboards with insights into a plant’s health.

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/network%20diagram.jpg)

_Credits to Prof Duncan Wilson_

The benefit of this set-up is that we could leave RPI on for extended periods of time and the data would continue flowing from the data collection node to MQTT to then InfluxDB database via Telegraf and the visuals in Grafana dashboard would automatically refresh providing up-to-date insights. Although not imperative for a small scale project with only a single data collection node, the architecture set up with an MQTT protocol as a broker will be beneficial if you'd like to scale up your monitoring ecosystem. In other words, you should be able to apply the methodology proposed here to build, for example, a plant monitoring system for a commercial size farm (assuming you size your server capacity correctly).

There are three workstreams to complete the basic project. Workstreams 1 and 2 require you to set up your hardware: data collector node for Workstream 1 and a computer for Workstream 2, then connect them both to a network and an MQTT broker. Workstream 3 is about automating data feed from the broker to the database where it can be stored, and from the database to the platform where data can be analysed, and insights visualised.

There are only two types of people who are good at orientation aka "not getting lost": the ones that have an innate sense of direction and the ones that can follow a map. Since I am the latter, I have sketched a high level project map for fellow map lovers:

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/A%20prototype%20architecture%20for%20sensing%20data.jpg)


## Workstream 1 – how to sense and share data

What you'll need to set up a basic data collector node?

**Hardware**

- Feather HUZZAH ESP8266
- Humidity and temperature sensor. This project uses DHT22
- 4 resistors – **how many watts each?**
- Two nails
- Two ~20cm 1core cables
- Type C to USB cable 

**Software**


- Arduino IDE
- MQTT Server
- MQTT Explorer
- CP2104 driver

Steps:

1.	First connect to Wi-Fi
2.	Set up time as the microchip does not know it – every time it’s resent it will not know the time again so we need to tell the chip the time
3.	Set up moisture value readings
4.	Set up temperature value readings 
5.	Publish data to MQTT server


## Workstream 2 – building a gateway for obtaining data 

What you’ll need?

**Hardware**
-	RPi 4
-	USB-C power supply
-	A microSD card
-	A laptop / desktop (for setting up the microSD card)
-	USB microSD adapter
-	Optional: a keyboard & mouse & TV screen 

**Software**
-	Raspberry Pi OS 64 bit
-	Terminal for a Mac user and Putty for a Windows user
-	WinSCP
-	InfluxDB
-	Telegraf
-	Grafana

Steps:

1.	Set up your microSD card
2.	Connect and start up RPi 4

Host in putty is stud-pi-ucqbutk
Login is pi
Password is 52201


3.	Install InfluxDB
4.	Install Telegraf
5.	Install Grafana


## Workstream 3 – setting up a database, analysing and visualising data

You should have all prerequisites from Workstreams 1 and 2

Steps: 
1.	Transform data and set up your variables in InfluxDB
2.	Visualise insights in Grafana


/* Explicitly set the ESP8266 to be a WiFi-client, otherwise, it by default,
     would try to act as both a client and an access-point and could cause
     network-issues with your other WiFi-devices on your WiFi-network. */

TCP allows for transmission of information in both directions. This means that computer systems that communicate over TCP can send and receive data at the same time, similar to a telephone conversation. The protocol uses segments (packets) as the basic units of data 



```PlantMonitor/Code/Arduino Code.ino

```

And, you can refer specific lines as
```PlantMonitor/Code/Arduino Code.ino [4-5]
 
```
