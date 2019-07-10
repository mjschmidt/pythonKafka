# Pyhton Kafka Work

### Get the Kafka pod
```
#create kafka pod to work with
kubectl apply -f container_kafka.yaml
```


### Using the native tools to do kafka stuff
```
kubectl exec -it kafka-client -- /bin/bash

#create your test topic
kafka-topics --zookeeper my-confluent-oss-cp-zookeeper-headless:2181 --topic my-confluent-oss-topic --create --partitions 1 --replication-factor 1 --if-not-exists

#Put a message on the topic
MESSAGE="`date -u`"
echo "$MESSAGE" | kafka-console-producer --broker-list my-confluent-oss-cp-kafka-headless:9092 --topic my-confluent-oss-topic

#Read the topic
kafka-console-consumer --bootstrap-server my-confluent-oss-cp-kafka-headless:9092 --topic my-confluent-oss-topic --from-beginning --timeout-ms 20000000 --max-messages 10000000 | grep "$MESSAGE"

```



## With Python 

### Prep the container
```
kubectl cp producerexample.py kafka-client:/
kubectl cp readerexample.py kafka-client:/
kubectl exec -it kafka-client -- /bin/bash
pip install confluent_kafka

#create your test topic
kafka-topics --zookeeper my-confluent-oss-cp-zookeeper-headless:2181 --topic my-confluent-oss-topic --create --partitions 1 --replication-factor 1 --if-not-exists

#to place messaged on the que
python producerexample.py my-confluent-oss-cp-kafka-headless:9092 my-confluent-oss-topic
#stdin

#to read messages off the que
python readerexample.py my-confluent-oss-cp-kafka-headless:9092 G1 my-confluent-oss-topic

```

Origonal Pyhton code links
https://github.com/confluentinc/confluent-kafka-python
