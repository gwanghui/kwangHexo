---
title: kafka-cli-cheat-sheet
date: 2021-02-14 18:35:02
tags:
- kafka
---
### Daemon으로 kafka 실행
```shell
./bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
./bin/kafka-server-start.sh -daemon config/server.properties
```

### Topic 생성
```shell
./bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic notification-topic
```

### Message Publish
```shell
./bin/kafka-console-producer.sh --broker-list localhost:9092 --topic notification-topic
```
