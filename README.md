# Telegraf XML input for Meteobridge weather stations

If you have a Meteobridge-powered weather software device, such as the [Ambient Weather WEATHERBRIDGE](https://ambientweather.com/amweatherbridge.html), then this Telegraf Docker plugin will allow you to connect your weather station data to an InfluxDB time-series database. By default, the Meteobridge displays raw sensor data as an XML file, and using this plugin will allow you to pull those data.

## Prerequisites

- Meteobridge-powered weather station (e.g., the [Ambient Weather WEATHERBRIDGE](https://ambientweather.com/amweatherbridge.html))
- An InfluxDB v2 instance, with a separate bucket created for your weather station data and a token set up for write access to that bucket

## Installation

In the ENVIRONMENT section of the `docker-compose.yml` file, adjust the values for your Meteobridge and InfluxDB settings accordingly. Make sure to change:

- InfluxDB database IP address, organization ID, bucket name, and token
- Meteobridge IP address
- Meteobridge username and password, if not changed from the default _meteobridge:meteobridge_ configuration

After the values have been configured to your environment, then start the environment.

## Configuration Notes

This is pre-configured to get data from the Meteobridge every 30 seconds.

## Sensor Fields

The Meteobridge returns data in standard SI units. If you need to convert them to a different unit system, you'll need to write the appropriate InfluxDB query in your database.

- Outdoor Temperature (logger/TH/@temp): ºC
- Outdoor Humidity (logger/TH/@hum): percent
- Outdoor Dewpoint (logger/TH/@dew): ºC
- Outdoor Wind Direction (logger/WIND/@dir): Degrees on a compass rose, with 0º being north
- Outdoor Wind Speed (logger/WIND/@gust): meters per second
- Outdoor Wind Speed, averaged over 2 minutes (logger/WIND/@wind): meters per second
- Outdoor Rain Rate (logger/RAIN/@rate): millimeters per hour, see note at the end
- Outdoor Rain Delta (logger/RAIN/@delta): millimeters per hour, see note at the end
- Barometer = (logger/THB/@seapress): Hectopascals at sea level
- Indoor Temperature (logger/THB/@temp): ºC
- Indoor Humidity (logger/THB/@hum): percent'

### NB on rainfall

Some weather stations will extrapolate a rainfall rate based on current preciptiation. If you are using this for weather spotting, most weather bureaus want actual rainfall, not a mathematically projected rainfall rate. Make sure you know what your weather station does with rainfall before using this for official purposes.

Additionally, because the plugin author lives in Phoenix, the rainfall function has not been tested yet to see what it does because it rarely rains here.
