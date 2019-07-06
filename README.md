

```
kubectl exec -it kafka-client -- /bin/bash

kafka-topics --zookeeper my-confluent-oss-cp-zookeeper-headless:2181 --topic my-confluent-oss-topic --create --partitions 1 --replication-factor 1 --if-not-exists  
MESSAGE="`date -u`"
echo "$MESSAGE" | kafka-console-producer --broker-list my-confluent-oss-cp-kafka-headless:9092 --topic my-confluent-oss-topic
kafka-console-consumer --bootstrap-server my-confluent-oss-cp-kafka-headless:9092 --topic my-confluent-oss-topic --from-beginning --timeout-ms 20000000 --max-messages 10000000 | grep "$MESSAGE"

```



## With Python

```
#get your confluent kafka deps
pip install confluent_kafka


kubectl cp producerexample.py kafka-client:/
kubectl cp readerexample.py kafka-client:/
kubectl exec -it kafka-client -- /bin/bash
#to place messaged on the que
python producerexample.py my-confluent-oss-cp-kafka-headless:9092 my-confluent-oss-topic
#stdin

#to read messages off the que
python readerexample.py my-confluent-oss-cp-kafka-headless:9092 G1 my-confluent-oss-topic

