# Plant Monitor

A simple and fun project to monitor plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). The solution will provide _insights at a glance_ into plant health using time-series data on temperature, soil humidity and moisture.

This project focuses on sensing and insights visualisation, which could potentially call for an action. Control systems to water plants when soil humidity drops below a certain level could be the next potential iteration of the project. 

The project is based around a single plant, but the methodology and solution architecture introduced are easily scalable. You can use this project as a basis to then add sensors for further insights, or scale by increasing the number of data collection nodes to inform about performance of multiple plants – even a whole farm!


## Project architecture

The project uses Feather Huzzah ESP8266 as a data collection node to which all sensors are added and data variables created using Arduino IDE. The timeseries data is collected using InfluxDB database and visualised in Grafana. 

Although not imperative for a small scale project with only a single data collection node, the architecture is set up using an MQTT protocol as a broker. This set up will be beneficial if you'd like to scale up your monitoring ecosystem. In other words, you should be able to apply the methodology proposed here to build, for example, a plant monitoring system for a commercial size vertical farm.

There are only two types of people who are good at orientation aka "not getting lost": the ones that have an innate sense of direction and the ones that can follow a map. Since I am the latter, I have sketched a high level project map for fellow map lovers:

![Alt text](https://github.com/StrikeEB/PlantMonitor/blob/24e39f4671f419e6fca6abf293e37a72f6e9d009/Project%20map.jpg)

## Workstreams explained

Workstreams 1 and 2 require you to set up your hardware, connect it to a network and enable . Workstream 3 is about automating data feed from the broker to the database where it can be stored and then visualising insights.


