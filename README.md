confluent-kafka-rest-docker
============
Dockerfile for [Confluent](http://docs.confluent.io/1.0.1/index.html) configured as kafka-rest service
This configuration help to use only the kafka-rest wrapper only from Confluent.

The image is available directly from [DockerHub](https://hub.docker.com/r/dockerkafka/confluent-kafka-rest/)

You can browse the source in [GitHub](https://github.com/DockerKafka/confluent-kafka-rest-docker)

## Usage

### Pull the image.
```sh
$  docker pull dockerkafka/confluent-kafka-rest
```

### Start a server.
```sh
$  docker run -d --name kafkadocker_zookeeper_1  dockerkafka/zookeeper
$  docker run -d --name kafkadocker_kafka_1 --link kafkadocker_zookeeper_1:zookeeper dockerkafka/kafka
$  docker run -d -p 8082:8082 --name kafkarestdocker_confluent_1 dockerkafka/confluent-kafka-rest
```

### Rest examples:

#### List topics:
```sh
$  curl http://localhost:8082/topics
```

#### Send message to a topic:
```sh
$ curl -X POST -H "Content-Type: application/vnd.kafka.binary.v1+json" --data '{"records":[{"value":"S2Fma2E="}]}' http://localhost:8082/topics/test
```

#### Registrate consumer
```sh
curl -X POST -H "Content-Type: application/vnd.kafka.v1+json" --data '{"id": "test_user", "format": "binary", "auto.offset.reset": "smallest", "auto.commit.enable":"true"}' http://localhost:8082/consumers/test_user_group
```

#### Get the messages
```sh
curl -X GET -H "Accept: application/vnd.kafka.binary.v1+json" http://localhost:8082/consumers/test_user_group/instances/test_user/topics/test
```

## Customization

Check out this repository, you will found the default Kafka configuration files under image/conf.

Start the container with the following line, so now you can modify the config in your host, and then start the server.
```sh
$  docker run -it --rm --volume `pwd`/image/conf:/opt/confluent-1.0.1/etc /bin/bash
```

After you finished the customization & testing of the config changes, build your own image.
```sh
$  docker build --tag="my-image" .
```

## Useful links

[Confluent platform home](http://docs.confluent.io/1.0.1/index.html)
[Confluent rest api](http://docs.confluent.io/1.0/kafka-rest/docs/api.html)
