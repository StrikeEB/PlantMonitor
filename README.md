# Plant Monitor

A simple and fun project to monitor plants using [Adafruit Feather HUZZAH ESP8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/overview). The solution will provide insights at a glance into plant health using time-series data on temperature, soil humidity and moisture.


More importantly, this methodology provides a basic methodology and architecture that are flexible and could be advanced to provide further insights by adding sensors or scaled to inform about performance of multiple plants by adding devices.


## Project architecture

The project uses Feather Huzzah ESP8266 as a data collection node to which all sensors are added and data variables created using Arduino IDE. The timeseries data is collected using InfluxDB database and visualised in Grafana. 

Although not imperative for a small scale project with only a single data collection node, the architecture is set up using an MQTT protocol as a broker. This set up will be beneficial if you'd like to scale up your monitoring ecosystem. In other words, you should be able to apply the methodology proposed here to build, for example, a plant monitoring system for a commercial size vertical farm.

There are only two types of people who are good at orientation aka "not getting lost": the ones that have an innate sense of direction and the ones that can follow a map. Since I am the latter, I have sketched a high level project map for fellow map lovers:

![Alt text](https://github.com/StrikeEB/PlantMonitor/blob/main/Project%20map.jpg)

## Workstreams explained

Workstreams 1 and 2 require you to set up your hardware, connect it to a network and then deliver messages to the broker. Workstream 3 is about automating data feed from the broker to the database where it can be stored and then visualising insights.


