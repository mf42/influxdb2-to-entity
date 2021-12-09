
# influxdb2-to-entity

AppDaemon app to query an InfluxDB 2 database daily and create/refill a Home Assistant entity with the results in the entity's attributes as key: value pairs.

Useful if you want to generate reports on historical influx data (I know, that's not the focus of HomeAssistant).

Inspired by https://github.com/markcocker/influxdb-query-to-entity.git and https://github.com/ReneTode/create-entities-app.git. Thanks for that nice projects.

## Installation

AppDaemon required. 

install influxdb-client ('pip install influxdb-client').

Copy influxdb2_to_entity to your appdaemon app dir. Edit apps.yaml with all the required setting and see what happens. Configuration please see below.

See apps.yaml.example for an example configuration.

appDaemon-Log is useful for checking the constructed query.  

## Syntax for service

| Parameter | Type | Required? | Default | Description |
| --- | --- | --- | --- | --- |
| <a name="bucket">`bucket`</a> | string | ✅ | | Influx bucket|
| <a name="org">`org`</a> | string | ✅ | | Influx org|
| <a name="token">`token`</a> | string | ✅ | | Influx auth token|
| <a name="host">`host`</a> | string | ✅ | | Influx host|
| <a name="port">`port`</a> | string | | | Influx port (required if not in included host)|
| <a name="entity_settings">`entity_settings`</a> | list | | | ...followed by a list of entities which queries use the same InfluxDB-Connection|
| <a name="entities">`entities`</a> | list | | | each entity correspond to a Sensor Entity|
| <a name="range start">`range_start`</a> | string | | -15m | range start of time series. Don't forget to import required libs (see below)|
| <a name="range stop">`range_stop`</a> | string | | now() | range stop of time series|
| <a name="options">`options`</a> | list | | | useful for timezone handling|
| <a name="imports">`imports`</a> | list | | | list of required libs to execute query|
| <a name="query">`query`</a> | string | ✅ |  | influxdb query|
| <a name="group_function">`group_function`</a> | string | |  | group function at the end. Groups by "_value" |
| <a name="daily_fetch_time">`daily_fetch_time`</a> | string | ✅ | | format "HH:MM:SS" or everything useable in  parse_time (so sunrise + .. etc works as well)|
| <a name="attributes">`attributes`</a> | string | | | additional attributes for the entity|
| <a name="key_field_name">`key_field_name`</a> | string | | _time | Key Field name. Take care that this field is in your query result set.|
| <a name="value_field_name">`value_field_name`</a> | string | | _value | Value field name. Take care that this field is in your query result set |


## Results

The service will:

* connect to InfluxDB and send the query every day at daily_fetch_time for each entity
* create and update the entities and remove all previous attributes
* optionally set entity attributes like unit_of_measurement, friendly_name, icon 
* for each point returned in the query, extract the fields specified by key_field_name and value_field_name and use them to add as a entity attribute

## Example

For use cases see https://github.com/markcocker/influxdb-query-to-entity.git. The combination with [apexcharts-card](https://github.com/RomRider/apexcharts-card) is really lovely.

## Feedback

Please use GitHub issues to raise questions, and suggestions.