# Flight Track - ADS-B Signal Visualization with Elastic Stack

![](https://raw.githubusercontent.com/kosho/flight-track/master/flight-track-screenshot.png)

## Prerequirements

### ADS-B Receiver and Dump1090 Software

You must have set up your own [ADS-B](https://www.faa.gov/nextgen/programs/adsb/) receiver and are receiving the signal by using [dump1090](https://github.com/antirez/dump1090). The installation instructions can be found [here](https://www.flightradar24.com/build-your-own).

### Elastic Stack

You must install Elasticsearch and Kibana version 5.0.0 or abover properly. Using [Elastic Cloud](http://cloud.elastic.co) could be a good alternative choise. Logstash 5.0.0 or higher will be used to fetch the airplace location periodically from the dump1090 and to ingest the data to Elasticsearch. 

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

1. Open Kibana and go to Management > Index Patterns. Type in `flight-track` as the index name and create the index pattern. This step does not work until the data is properly ingested into the Elasticsearch index.
2. Go to Management > Saved Objects and click on Import, and select `flight-track-kibana.json` by the file chooser.
3. Go to Dashboard and click on **Flight Track - Dashboard** from the list of the dashboards.

