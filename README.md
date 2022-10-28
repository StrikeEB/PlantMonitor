# Plant Monitor

A simple and fun project to monitor your home plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). The aim is create quick and easy way to undertsand your plant's health using time-series data on temperature, soil humidity and moisture. 


## Project architecture

The project uses Feather Huzzah ESP8266 as a data collection node where all sensors are addeded and data variables created using Arduino IDE. The timeseries data is collected using InfluxDB database and visualised in Grafana. 

Although not imperative for a small scale project with only a single data collection node, the architecture is set up using MQTT protocol as a broker. This set up will be beneficial if you'd like to scale up the monitoring ecosystem. You should be able to apply the methodology proposed here to build for example a plant monitoring syste for a commercial size vertical farm.

They say there are two types of people who are good at orientation: the ones that have a sense of direction and the ones that can follow a map. Since I am the latter, I have sketched a high level project map for you:

![Alt text](https://github.com/StrikeEB/PlantMonitor/blob/5ad135616ac6e33d87cc6a3f9e1d9f057a838443/Project%20map.jpg)

## Workstreams explained

Workstreams 1 and 2 require you to set up your hardware, connect it to a network and then deliver messages to the broker. Workstream 3 is about automating data feed from the broker to the database where it can be stored and then visualising insights.


