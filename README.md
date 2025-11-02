# Kafka Connect JDBC Distributed Reference Config

Cookbook for setting up a kafka cluster with 3 controllers and 3 brokers.

## Requirements

MariaDB latest
Kafka Binaries

## Start a MariaDB Instance 
 

```sh
docker run -p 3306:3306 --name corpo -v /home/darks/mysql-data:/var/lib/mysql:Z -e MARIADB_ROOT_PASSWORD='yourpass' -d mariadb:latest
```

## Import MariaDB Test Data (needs MySQL/MariaDB Client on host machine)

```sh
mysql -h 127.0.0.1 -u root -p < corpo.sql 
```

## Start Kafka Broker Cluster

```sh
docker compose -f docker-compose.yml up
```

## Start Kafka Standalone Broker

```sh
docker run -p 9092:9092 -d --name broker apache/kafka-native:latest
```

## Start Standalone Connector

```sh
KAFKA_FOLDER/bin/connect-distributed.sh connect-mariadb-distributed.properties corpo-sink.json corpo-emp-dep-lookup-sink.json
```


## Start Distributed Connector

```sh
KAFKA_FOLDER/bin/connect-distributed.sh connect-mariadb-distributed.properties
```

## Register Connectors

```sh
curl -X POST -H "Content-Type: application/json" --data @corpo-sink.json http://localhost:8083/connectors

curl -X POST -H "Content-Type: application/json" --data @corpo-emp-dep-lookup-sink.json http://localhost:8083/connectors
```

Reference:
- Confluent Kafka Source Connector https://docs.confluent.io/kafka-connectors/jdbc/current/source-connector/overview.html
- Docker Compose https://github.com/apache/kafka/blob/trunk/docker/examples/docker-compose-files/cluster/isolated/plaintext/docker-compose.yml
