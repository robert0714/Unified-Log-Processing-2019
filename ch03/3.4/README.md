## 3.4.3. Testing, redux
Before we can run our app, we need to download a free copy of the MaxMind geo-IP database. You can do this like so:
```shell
$ wget \
  "https://geolite.maxmind.com/download/geoip/database/GeoLite2-City.tar.gz"
$ tar xzf GeoLite2-City_<yyyyMMdd>.tar.gz
```
To run our event processor, type in the following:
```
$ cd ~/nile
$ gradle build
$ java -jar ./build/libs/nile-0.2.0-all.jar  localhost:9092 ulp-ch03-3.4 raw-events-ch03 enriched-events bad-events ./GeoLite2-City_<yyyyMMdd>/GeoLite2-City.mmdb
```

Great—our app is now running! Note that we configured it with a different consumer group to the previous app: ulp-ch03-3.4 versus ulp-ch03-3.3. Therefore, this app will process events right back from the start of the raw-events-ch03 topic. If you’ve left everything running from section 3.3, our events should be flowing through our single-event processor now.

Check back in the fourth terminal (the console-consumer) and you should see our original three raw events appearing in the enriched-events Kafka topic, but this time with the geolocation data attached—namely, the country and city fields:


##  Docker
Apache Kafka doesn’t provide official Docker images at this time, but Confluent does. Those images are tested, supported, and used by many developers in production. In the repository of examples for this book, you’ll find a docker-compose.yaml file with preconfigured Kafka, ZooKeeper, and other components. To get all the components up and running, issue the command docker-compose up -d in the directory with the YAML file as the following listing shows.

> **⚠ NOTE:**  If you are unfamiliar with Docker or don’t have it installed, check out the official documentation at https://www.docker.com/get-started. You’ll find instructions on installation at that site as well.

Listing A.7 filename.sh for a Docker image
```shell
$ git clone \                                      ❶
  https://github.com/Kafka-In-Action-Book/Kafka-In-Action-Source-Code.git
$ cd ./Kafka-In-Action-Source-Code
$ docker-compose up -d                             ❷
 
Creating network "kafka-in-action-code_default" with the default driver
Creating Zookeeper... done                         ❸
Creating broker2   ... done
Creating broker1   ... done
Creating broker3   ... done
Creating schema-registry ... done
Creating ksqldb-server   ... done
Creating ksqldb-cli      ... done
 
$ docker ps --format "{{.Names}}: {{.State}}"      ❹
 
ksqldb-cli: running
ksqldb-server: running
schema-registry: running
broker1: running
broker2: running
broker3: running
zookeeper: running
```
❶ Clones a repository with book examples from GitHub

❷ Starts Docker Compose in the examples directory

❸ Observe the following output.

❹ Validates that all components are up and running

Runs a [Confluent Control Center](https://docs.confluent.io/platform/current/control-center/index.html) that exposes a UI at `http://localhost:9021/` .

We can follow the startup by monitoring the output :
```shell
docker-compose logs -f
```