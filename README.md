# Local Docker Kafka Cluster

Creates a [Docker](https://www.docker.com)-based [Kafka](http://kafka.apache.org/)
cluster with [docker-compose](https://docs.docker.com/compose) for local
development of Kafka-based apps.

## Prerequisites

Uses docker compose file syntax version 2.

- Docker 1.10.0 or later
- Docker Compose 1.6.0 or later
- Ruby

## Running

Build the Java and Kafka Docker images locally with

```
./build-java && ./build-kafka
```

This will create a Kafka 0.10.0.0 / Scala 2.11 image. Modify the `build-kafka`
script to produce other versions.

Bring up the cluster and specify that you want e.g. five brokers with

```
./cluster-up 5
```

This will create a `docker-compose.yml` file in the directory that will be used
by `./cluster-down` do delete the cluster.

## Connectivity

The cluster will contain one ZooKeeper node only accessible as `localhost:2181`.
The brokers will be reachable under `localhost:9092,localhost:9093,...` and
advertised their Docker container IP to Kafka clients. As these are reachable
from the Docker host and network, it is possible to connect to the cluster both
from the host machine as well as from other Docker containers.
