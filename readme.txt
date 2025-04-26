Step 1: run docker-compose
--------------------------
docker compose -f docker-compose.yml up -d

Step 2: open a command terminal on the Kafka container
------------------------------------------------------
docker exec -ti -w /opt/bitnami/kafka/bin kafka-broker-1 sh

Step 3: Create a topic to store your events
-------------------------------------------
kafka-topics.sh --create --topic topic-1 --partitions 3 --bootstrap-server localhost:9192

Take note of the --bootstrap-server flag. 
Because we're connecting to Kafka inside the container, we use broker:9192 for the host:port. 
If we were to use a client outside the container to connect to Kafka, a producer application running on our laptop for example, we'd use localhost:9193 instead.

kafka-topics.sh --describe --topic topic-1 --bootstrap-server localhost:9192

Step 4: Write some events into the topic
----------------------------------------
kafka-console-producer.sh --topic topic-1 --bootstrap-server localhost:9192
>This is my first event
>This is my second event

kafka-console-producer.sh --topic topic-1 --bootstrap-server localhost:9192 --property "parse.key=true" --property "key.separator=:"
>key1:This is my first event
>key2:This is my second event

You can stop the producer client with Ctrl-C at any time.

Step 5: Read the events
-----------------------
kafka-console-consumer.sh --topic topic-1 --group group-1 --from-beginning --bootstrap-server localhost:9192

You can stop the consumer client with Ctrl-C at any time.

