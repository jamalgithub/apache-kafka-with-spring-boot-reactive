# Spring Boot Kafka Reactive Project

This is a sample Spring Boot project that demonstrates the integration of Kafka with a Spring Reactive application. The project showcases key concepts related to Kafka, including topics, partitions, serialization, consumer groups, and offset management.

## Table of Contents

- [Requirements](#requirements)
- [Getting Started](#getting-started)
    - [Running Kafka](#running-kafka)
    - [Project Setup](#project-setup)
- [Key Concepts](#key-concepts)
    - [Kafka Cluster](#kafka-cluster)
    - [Kafka Broker](#kafka-broker)
    - [Kafka Consumer](#kafka-consumer)
    - [Kafka Producer](#kafka-producer)
    - [Kafka Topics](#kafka-topics)
    - [Partitions](#partitions)
    - [Offsets](#offsets)
    - [Consumer Groups](#consumer-groups)
    - [Serialization](#serialization)
- [Demo Application](#demo-application)


## Requirements

- Java 17 or later
- Apache Kafka (for running locally)
- Maven (or use the included `./mvnw` script)

## Getting Started

### Running Kafka

You can run Kafka in two ways:

#### Option 1: Using Docker Compose (Recommended)

This project includes a `docker-compose.yml` file that sets up a complete Kafka environment with:
- 1 Zookeeper instance
- 3 Kafka brokers (forming a cluster)
- Kafka UI for monitoring and management

To start the Kafka environment:

```bash
# Start all services
docker-compose up -d

# To check the status of the services
docker-compose ps

# To view logs
docker-compose logs -f
```

The Kafka UI will be available at http://localhost:8080, where you can:
- Monitor topics, partitions, and messages
- Create new topics
- View consumer groups and their offsets

Kafka brokers will be accessible at:
- Broker 1: localhost:9193
- Broker 2: localhost:9293
- Broker 3: localhost:9393

#### Option 2: Manual Installation

Alternatively, you can install Kafka manually. Download Kafka from the [official Apache Kafka website](https://kafka.apache.org/).

Start a ZooKeeper server (required by Kafka):

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

Start a Kafka broker:
```bash
bin/kafka-server-start.sh config/server.properties
```


### Project Setup

Clone this repository and navigate to the project folder:

```bash
git clone https://github.com/ali-bouali/apache-kafka-with-spring-boot-reactive
cd apache-kafka-with-spring-boot-reactive
```

## Key Concepts

Apache Kafka is a distributed, fault-tolerant, and highly scalable message broker and stream processing platform. It was originally developed by LinkedIn and later open-sourced as an Apache Software Foundation project. Kafka is designed to handle large volumes of data streams in a real-time and fault-tolerant manner.

Key Components of Kafka:
- **Producer**: An application that publishes messages to Kafka topics.
- **Consumer**: An application that subscribes to topics and processes the published messages.
- **Broker**: Kafka brokers form the core of the Kafka cluster, storing and managing the streams of records.
- **Topic**: A category or feed name to which records are published. Topics in Kafka are used to categorize messages.
- **Partition**: Topics are divided into partitions, allowing Kafka to parallelize processing and scale horizontally.
- **Zookeeper**: Kafka relies on Apache ZooKeeper for distributed coordination and management of the Kafka cluster.
  ![Kafka topics](img/kafka_overview.png)

### Kafka CLuster
A Kafka cluster is a group of one or more Kafka brokers (servers) working together to manage the storage and exchange of data in the form of messages. Kafka is designed to operate as a distributed system, and a cluster allows it to scale horizontally, provide fault tolerance, and handle large volumes of data across multiple nodes.
![Kafka topics](img/kafka_cluster.png)

### Kafka Broker
Kafka brokers form the core of the Kafka cluster, storing and managing the streams of records.
![Kafka topics](img/kafka_broker.png)

### Kafka Producer
Producers are applications or systems that publish messages to Kafka topics. Producers determine which partition within a topic a message should be sent to.
![Kafka topics](img/kafka_producer.png)

### Kafka Consumer
Consumers are applications or systems that subscribe to Kafka topics and process the messages. Consumers can be part of a consumer group, allowing them to parallelize the processing of messages.
![Kafka topics](img/kafka_consumer.png)

### Kafka Topics

A Kafka topic is a category or feed name to which records (messages) are published. Topics help organize and categorize messages.
![Kafka topics](img/kafka_topic.png)
---

### Partitions

Topics are divided into partitions, allowing for parallel processing and scalability. Each partition is assigned to a specific broker.
![Kafka topics](img/kafka_partitions.png)
---

### Offsets

Offsets represent the position of a message within a partition. Consumers use offsets to track their progress and resume processing from the last committed offset.
![Kafka topics](img/kafka_offset.png)
---

### Consumer Groups

Consumer groups allow for parallel and scalable message processing. Consumers within a group coordinate to consume messages from one or more partitions.
![Kafka topics](img/kafka_consumer_groups.png)
---

### Serialization

Serialization is the process of converting data to a format suitable for transmission. Kafka allows custom serializers for key and value objects.


## Demo Application

![Kafka topics](img/kafka_demo_application.png)

To showcase the functionality of the application, I have developed two distinct projects:

1. **Producer Project:**
   The producer project leverages the **Spring Boot Reactive framework** to extract a data stream from [Wikimedia Recent Changes](https://stream.wikimedia.org/v2/stream/recentchange). This project is designed to efficiently read the streaming data and subsequently transmit the messages to a Kafka broker.

2. **Consumer Project:**
   The consumer project complements the aforementioned producer by retrieving messages from the Kafka topic. The retrieved messages are then persisted into a DynamoDB database. This dual-project configuration not only demonstrates the integration of Spring Boot Reactive with Kafka for real-time data processing but also showcases the seamless storage of this data in a DynamoDB database, emphasizing end-to-end functionality within the application.

## Running the Application

### Prerequisites

1. Start the Kafka environment using Docker Compose as described in the [Running Kafka](#running-kafka) section.
2. Make sure the Kafka UI is accessible at http://localhost:8080 to verify that the Kafka cluster is running properly.

### Configuration

Both the producer and consumer applications are configured to connect to the Kafka brokers. The configuration is in the respective `application.yml` files:

#### Producer Configuration

The producer is configured to connect to the first Kafka broker (localhost:9193) and publish messages to the "wikimedia-recentchange" topic.

#### Consumer Configuration

The consumer is configured to connect to the same Kafka broker and consume messages from the "wikimedia-recentchange" topic.

### Running the Producer

1. Navigate to the producer directory:
   ```bash
   cd producer
   ```

2. Start the producer application:
   ```bash
   ./mvnw spring-boot:run
   ```

3. The producer will start and expose an API endpoint at http://localhost:8081/api/v1/wikimedia. When you access this endpoint, it will start consuming the Wikimedia stream and publishing messages to Kafka.

### Running the Consumer

1. Navigate to the consumer directory:
   ```bash
   cd consumer
   ```

2. Start the consumer application:
   ```bash
   ./mvnw spring-boot:run
   ```

3. The consumer will automatically start consuming messages from the Kafka topic and store them in DynamoDB.

### Monitoring

You can monitor the Kafka topics, messages, and consumer groups using the Kafka UI at http://localhost:8080. This allows you to:

- See the number of messages in each topic
- View the message content
- Monitor consumer group offsets
- Check for any lag in message processing
