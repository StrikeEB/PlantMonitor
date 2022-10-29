# Plant Health Monitoring Project

A simple and fun project to monitor plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). 

The solution will provide _insights at a glance_ into plant health using time-series data on temperature, soil humidity and moisture.

**insert picture of dashboard**

This project focuses on _sensing and insights visualisation_, which could potentially call for an action. _Control systems_ to automatically water plants when the need is identified by the insights provided could be the next potential iteration of the project. 

The project is based around a single plant, but the methodology and solution architecture introduced are easily scalable. You can use this project as a basis to then add sensors for further insights, or scale by increasing the number of data collection nodes to inform about performance of multiple plants – even a whole farm!

_Disclaimer: this project is set up as part of the coursework for MSc Connected Environments at UCL module CASA0014_


## Project architecture

The project uses Feather Huzzah ESP8266 as a data collection node to which all sensors are added. Feather Huzzah ESP8266 is programmed using Arduino IDE to send time series data to the MQTT protocol which is then collected and stored in InfluxDB database and visualised in Grafana. Raspberry Pi is used as a gateway for the data.

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/network%20diagram.jpg)

_Credits to Prof Duncan Wilson_

Although not imperative for a small scale project with only a single data collection node, the architecture set up with an MQTT protocol as a broker will be beneficial if you'd like to scale up your monitoring ecosystem. In other words, you should be able to apply the methodology proposed here to build, for example, a plant monitoring system for a commercial size vertical farm.

There are only two types of people who are good at orientation aka "not getting lost": the ones that have an innate sense of direction and the ones that can follow a map. Since I am the latter, I have sketched a high level project map for fellow map lovers:

![Alt text](https://github.com/StrikeEB/PlantMonitor/blob/24e39f4671f419e6fca6abf293e37a72f6e9d009/Project%20map.jpg)

Workstreams 1 and 2 require you to set up your hardware: data collector node for Workstream 1 and data gateway for Workstream 2, connect it to a network and MQTT broker. Workstream 3 is about automating data feed from the broker to the database where it can be stored, and from the database to the platform where data can be analysed and insights visualised. To make the process worthwhile in cases where data is not gathered for research, it might be a good idea to present insights in a way that would call for an action. In this instance,  Grafana dashboard will suggest when the observed plant might need to be watered. 

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

-Arduino IDE
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

Prerequisites:

**Hardware**
-	RPi 4
-	USB-C power supply
-	A microSD card
-	A keyboard & mouse & TV screen or a laptop

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
3.	Install InfluxDB
4.	Install Telegraf
5.	Install Grafana


## Workstream 3 – setting up a database, analysing and visualising data

You should have all prerequisites from Workstreams 1 and 2

Steps: 
1.	Transform data and set up your variables in InfluxDB
2.	Visualise insights in Grafana
