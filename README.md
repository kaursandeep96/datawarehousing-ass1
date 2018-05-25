# datawarehousing-ass1


# ELASTIC SEARCH
## INSTALLATION STEPS:

Elastic Search requires java installation first, for adding oracle java ppa in the remote server, following commands were used:
```sh
sudo add-apt-repository -y ppa:webupd8team/java
sudo apt-get update
```
For installing oracle java8 installer:
```sh
sudo apt-get -y install oracle-java8-installer
```
For importing Elastic Search public GPG key, command used was:
```sh
wget -qO -https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - 
```

For enabling Http transport:
```sh
sudo apt-get install apt-transport-https
```
Elastic Search source list was created using:
```sh
echo “deb https://artifacts.elastic.co/packages/6.x/apt stable main” | sudo tee – a/etc/apt/sources.list.d/elastic-6.x.list
```
For updating all the apt packages following command is used:
```sh
sudo apt-get update
```
Finally Elastic Search is installed using the following command:
```sh
sudo apt-get -y install elatsicsearch
```
Following configuration file is searched for, new cluster and node name are given and are uncommented:
```sh
sudo nano/etc/elasticsearch/elasticsearch.yml
```
Also, network host is configured to 0.0.0.0 and saved in the same file as above.
Then, Elastic Search service is restarted using:
```sh
sudo service elasticsearch restart
```
To start elastic search on boot up:
```sh
sudo update-rc.d elasticsearch defaults 95 10
```
To test if it is running, following command is used:
```sh
sudo service elasticsearch status
```
To test if everything is configured well, following request is sent in browser, getting 200 response will ensure everything is good:
```
http://<the_ec2_ip>:9200/
```

## QUERIES:
Then, Insomnia REST client was used to run elastic search queries. Before importing data, index was created. Index was created using request: POST http:// 52.60.209.172:9200/index6. After that data was inserted in bulk in three different files. The Request used for inserting data was: PUT http:// 52.60.209.172:9200/index6/stops/_bulk , in the same data was inserted from the other two JSON files stopTimes and trips too. Then, following queries were used to search and to check response time of different queries:

### Query 1:
INPUT: bus stop name
OUTPUT: list of all buses
It was run in parts of 3:
```
1. GET http:// 52.60.209.172:9200/index6/stops/_search?q=name_stop:”Ferry Stop – Halifax”
2. GET http:// 52.60.209.172:9200/index6/stopTimes/_search?q=stop_id:1073
3. GET http:// 52.60.209.172:9200/index6/stops/_search?q=trip_id:”5807962-2012_05M-12MferWD-Weekday-00”
```

### Query 2:
INPUT: Time Range
OUTPUT: list of all buses
It was run in parts of 2:
```
1. GET http:// 52.60.209.172:9200/index6/stopTimes/_search?q=arrival_time:”13:00:00”
2. GET http:// 52.60.209.172:9200/index6/trips/_search?q=trip_id:”6519079-2012_05M-1205BRsu-Sunday-02”
```
### Query 3:
INPUT: Bus Name, Route Name
OUTPUT: list of all routes
It was run as:
```
1. GET http:// 52.60.209.172:9200/index6/trips/_search
```
### Query 4:
INPUT: Top 3 busiest Bus Stops
OUTPUT: list of all buses
It was run in parts of 2:
```
1. {“size”: 0,“aggregations”: {“aggregation”: {“terms”: {“field” : “stop_id” }}}}
2. GET http:// 52.60.209.172:9200/index6/stops/_search?q=stop_id:8643
```
