# Local Docker Kafka Cluster

[![docker pulls](https://img.shields.io/docker/pulls/andschroeder/local-kafka-cluster.svg)](https://hub.docker.com/r/andschroeder/local-kafka-cluster/)
[![docker stars](https://img.shields.io/docker/stars/andschroeder/local-kafka-cluster.svg)](https://hub.docker.com/r/wurstmeister/kafka/)

Creates a [Docker](https://www.docker.com)-based [Kafka](http://kafka.apache.org/)
cluster with [docker-compose](https://docs.docker.com/compose) for local
development of Kafka-based apps.

## Prerequisites

Works for Linux and Mac (with the latest Docker for Mac), and uses docker
compose file syntax version 2.

- [Docker](https://www.docker.com) 1.10.0 or later
- [Docker Compose](https://docs.docker.com/compose) 1.6.0 or later
- [Ruby](https://www.ruby-lang.org)

Also, the ports `2181`, `9092`, `9093`, and `9094` (for a default three broker
setup) should be available.

## Running

Bring up the cluster and specify that you want e.g. five brokers (the default
cluster size is three brokers) with

```
./cluster-up 5
```

This will create a `docker-compose.yml` file in the directory that will be used
by `./cluster-down` do delete the cluster (i.e. this stops and removes all data).
If you do not want to delete data, use the `docker-compose` commands directly.

## Connectivity

The cluster will contain one ZooKeeper node only accessible as `localhost:2181`.
The brokers will be reachable under `localhost:9092,localhost:9093,...` and
advertise Docker host IP to Kafka clients. As this is reachable from the host
and from the Docker network, it is possible to connect to the cluster both
from the host machine as well as from other Docker containers.

## The Thing With the Docker Network and Kafka

Kafka Brokers advertise themselves to each other and clients via the Kafka
protocol, and announce hostname/IP and port. It is therefore necessary to have
fixed (distinct) ports, and have a hostname or IP that is resolvable or routable
from both the docker network (for broker-to-broker communication) and from the
docker host (for local development).

To achieve this for both Linux and Mac - in the face of
[some verified existing issues](https://github.com/docker/docker/issues/22753)
with Mac - the best way to go is to use the [docker host IP](./cluster-up#L9-L14).
Without that issue, it would be more sensible to use the docker container IPs.

Using the docker host IP as advertised broker address makes broker-to-broker
traffic pass through the docker host, but this overhead is considered an
acceptable price for a solution working both on Linux and Mac.
