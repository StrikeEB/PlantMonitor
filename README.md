### Table of Contents

- [Plant Health Monitoring Project](#plant-health-monitoring-project)
  * [Project architecture](#project-architecture)
  * [Workstream 1 – how to sense and share data](#workstream-1---how-to-sense-and-share-data)
  * [Workstream 2 – building a gateway for obtaining data](#workstream-2---building-a-gateway-for-obtaining-data)
  * [Workstream 3 – setting up a database, analysing and visualising data](#workstream-3---setting-up-a-database--analysing-and-visualising-data)
  * [How this prototype could be improved further?](#how-this-prototype-could-be-improved-further-)

# Plant Health Monitoring Project

A simple and fun project to monitor plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). 

The solution will provide _insights at a glance_ into plant health using time-series data on temperature, soil humidity and moisture. 

An example dashboard visualising plant monitor outputs:

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/Example%20Grafana%20Plant%20Monitor%20Dashboard_Credits%20to%20Ivana%20Huckova.jpg)
_[Credits to Ivana Huckova]( https://grafana.com/blog/2021/03/08/how-i-built-a-monitoring-system-for-my-avocado-plant-with-arduino-and-grafana-cloud/)_

This project focuses on _sensing and insights visualisation_. It is based around a single plant, but the methodology and solution architecture introduced are easily scalable. You can use this project as a basis to then add sensors for further insights, or scale by increasing the number of data collection nodes to inform about performance of multiple plants – even a whole farm! 

At the end of this READ ME file I also provide some commentary how this simple prototype can be improved further by scaling the system horizontally and vertically.

_Disclaimer: this project is set up as part of the coursework for MSc Connected Environments module CASA0014 at UCL. A workshop followed can be found following this [link]( https://workshops.cetools.org/codelabs/CASA0014-2-Plant-Monitor/index.html?index=..%2F..casa0014#0)_


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
- 4 resistors
- Two nails
- Two ~20cm 1core cables
- Type C to USB cable 

**Software**


- Arduino IDE
- MQTT Server
- MQTT Explorer
- CP2104 driver

**Wiring up the data collection node**

Connect all parts using the schematic below:

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/Plant%20Monitor%20Schematic.jpg)
_Credits to Prof Duncan Wilson_

I would highly recommend following the Adafruit guide to know more about the pinouts and how to solder. 

It’s not required but good practice to add a box to protect the hardware from any tampering with a box. Mine was printed with a 3-D printer in the class. 

Final product:

![Alt text]( https://github.com/StrikeEB/PlantMonitor/blob/main/Images/Sensor%202.png)



**Steps to programme the data collection node for variable we want to monitor and automate data delivery to the MQTT protocol**

0.	Prep your Arduino IDE and plug in Huzzah to a USB port
-	Open Arduino IDE and ensure that you have selected the correct board and port
-	If you cannot find board in the board manager you will have to add it. Follow the steps shown on the [Adafruit site](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide)
-	Test Arduino IDE by running the _blink_ example code on Huzzah
-	If the light is not flashing on the Huzzah microcontroller, you might need to install the CP2104 driver on your computer
1.	Code starts here. First, install all relevant libraries:
-	https://github.com/StrikeEB/PlantMonitor/blob/910312e4d023f8044c1ccd05bdab170153dfe1b3/Code/PlantMonitorMQTT.ino#L4-L9
2.	Define your sensors
-	Link 
3.	Create an additional but secret library to store your WiFi and MQTT login details. You can add and store multiple network SSIDs and passwords there. 
-	Open a new Arduino file and use Arduino_Secrets library code as a basis (saved in the code foler)
-	Set your Wifi SSID, password and also MQTT login details:
https://github.com/StrikeEB/PlantMonitor/blob/d00fbbdacc55f5e55aba53cf0002f41a3ab723ab/Code/arduino_secrets%20library.ino#L16-L19
-	Convert Arduino_secrets file from .ino format to .h format – I simply did it by renaming the file name.
-	Move Arduino_secrets.h file to the same folder as your main code!
4.	Include password library you just created in the code
-	link
5.	Connect to Wi-Fi and MQTT
-	link
6.	Set up time as the microchip does not know it – every time it’s resent it will not know the time again so we need to tell it the time. However, it's likely that a date time series ready MQTT protocol will know the date of each data point sent from the data collection node
-	link
7.	Set your void setup() function to set up your inputs, outputs, start Wi-Fi, MQTT connection, etc.
-	link
8.	Set up value readings in this case moisture, temperature and humidity
-	link
9.	Read the data and store it in variable format
-	Link
10.	Push float and int variables to the HTML page in MQTT
-	link


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
3.	Install InfluxDB
4.	Install Telegraf
5.	Install Grafana


## Workstream 3 – setting up a database, analysing and visualising data

You should have all prerequisites from Workstreams 1 and 2. This workstream is about data transformation and visualisation.

Steps: 
1.	Transform data and set up your variables in InfluxDB
2.	Visualise insights in Grafana


## How this prototype could be improved further?

Think about the plant monitor as a modular and scalable system:
•	Horizontally: by adding more sensors to the data nodes or adding number of data notes and/or increase space and RAM or the server, and
•	Vertically: by adding further functionalities such as control systems, business/financial analytics and ML models to detect patterns and predict outcomes if one or multiple variables in the environment change and direct control systems take action.
Let me illustrate this using the plant monitor example. Assume that if the moisture level is below X, then your specific plant requires 100ml of water. Grafana is only good for insights visualisation, but if we’d have a platform that could not only subscribe to information from MQTT but also publish, we could automate the system to water the plant for us. When this both-ways platform receives a signal from the data collection node via MQTT that the moisture level is below X, it would send a signal also via MQTT to for example a solenoid water valve telling it to open up and release 100ml of water. This is a more useful scenario rather than just seeing that your plant is dying on Grafana dashboard when you’re holidaying in the sunny Costa del Sol. 
To scale horizontally, you could add direct sunshine sensors or image recognition devices to detect any physical changes to plants, i.e., change of colour, weed presence, change of shapes due to insects, and of course make the system to take appropriate actions to combat each of these. 

