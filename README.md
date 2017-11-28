# Flight Track - ADS-B Signal Visualization with Elastic Stack

![](https://raw.githubusercontent.com/kosho/flight-track/master/flight-track-screenshot.png)

## Prerequirements

### ADS-B Receiver and Dump1090 Software

You must have set up your own [ADS-B](https://www.faa.gov/nextgen/programs/adsb/) receiver and are receiving the signal by using [dump1090](https://github.com/antirez/dump1090). The installation instructions can be found [here](https://www.flightradar24.com/build-your-own).

### Elastic Stack

You must install Elasticsearch and Kibana version 6.0.0 or abover properly. Using [Elastic Cloud](http://cloud.elastic.co) could be a good alternative choise. Logstash 6.0.0 or higher will be used to fetch the airplace location periodically from the dump1090 and to ingest the data to Elasticsearch. 

## Setup and Run

### Adjust Logstash Configuration File

1. Open `flight-track-logstash.conf` by a text editor and set the URL of the dump1090 web service under the `http_poller` of the input plugin configuration.
2. Go to the output plugin configuration and make sure the hosts setting of elasticsearch output is properly set.

### Run

Type in the below command to run dump1090 and logstash.

```
$ dump1090 --net --aggressive --quiet &
$ logstash -f flight-track-logstash.conf
```

### Import Kibana Visuals and Dashboard

Type in the following command to load the dashboard into Kibana. It will create the index pattern, visualizations and the dashboard.

```
$ curl -XPOST -d @flight-track-kibana.json \
 "your_kibana_host:5601/api/kibana/dashboards/import" \
  -H 'kbn-xsrf: true' \
  -H 'Content-type: application/json'
```
