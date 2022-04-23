Phew! We are finally ready to start up our new stream processing application. In a fifth terminal, head back to your project root, the nile folder, and run this:
```shell
$ cd ~/nile
$ gradle build
$ java -jar ./build/libs/nile-0.1.0-all.jar localhost:9092 ulp-ch03-3.3   raw-events-ch03 enriched-events
```

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