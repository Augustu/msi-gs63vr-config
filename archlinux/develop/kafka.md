### Kafka

```bash

# kafka-topics.sh
bin/kafka-topics.sh --zookeeper 127.0.0.1:2181 --describe

bin/kafka-topics.sh --zookeeper 127.0.0.1:2181 --describe --topic test-3p

bin/kafka-topics.sh --zookeeper 127.0.0.1:2181 --create --topic test-3p --partitions 3 --replication-factor 1

# kafka-console-producer.sh
bin/kafka-console-producer.sh --bootstrap-server 127.0.0.1:9092 --topic test-3p

# kafka-console-consumer.sh
bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic test-3p

bin/kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic test-3p --from-beginning

# kafka-consumer-groups.sh
bin/kafka-consumer-groups.sh --bootstrap-server 127.0.0.1:9092 --describe --all-groups

bin/kafka-consumer-groups.sh --bootstrap-server 127.0.0.1:9092 --list



```











