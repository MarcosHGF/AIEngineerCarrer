**Overview of Apache Kafka**  

Apache Kafka is a **distributed event streaming platform** used to build real-time data pipelines and streaming applications. It was originally developed by LinkedIn and later open-sourced as part of the Apache Software Foundation.

Kafka is designed for **high-throughput, low-latency, fault-tolerant** handling of real-time data feeds. It decouples **producers** (event senders) from **consumers** (event receivers), enabling scalable and robust data architectures.

---

## 1.Core Concepts

### 1. **Topics**

- Logical channels to which records are sent.
- Data is written to a topic by a producer and read by consumers.
- Topics are split into **partitions** for parallelism and scalability.

### 2. **Partitions**

- Sub-units of a topic.
- Each partition is an ordered, immutable sequence of records.
- Each record in a partition has a unique offset.

### 3. **Producers**

- Applications or systems that publish (write) data to Kafka topics.
- Can use key-based partitioning to control message routing.

### 4. **Consumers**

- Applications or services that subscribe to topics and process records.
- Consumers are part of a **consumer group**, which allows parallel processing and load balancing.

### 5. **Brokers**

- Kafka servers that store and serve data.
- Each broker handles some set of partitions.

### 6. **ZooKeeper (Legacy)**

- Used for managing cluster state. Kafka is moving towards removing this dependency with **KRaft (Kafka Raft)** mode.


## 2.Key Features

- High Throughput and Scalability
- Durability and Reliability (via replication)
- Real-time Stream Processing
- Horizontal Scaling with Partitioning
- Message Retention Policies


## 3.Use Cases

- Log Aggregation
- Event Sourcing
- Real-time Analytics
- Stream Processing
- Messaging System
- Data Integration between microservices or systems


## 4.Architecture Overview

```
[Producer] --> [Kafka Broker (Topic:Partition)] --> [Consumer Group]
```

- **Cluster**: A set of Kafka brokers
- **Replication**: Each partition is replicated for fault tolerance
- **Controller**: Broker that manages leadership for partitions


## 5.Kafka CLI Commands

### Create a Topic

```bash
kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:9092 --partitions 3 --replication-factor 2
```

### List Topics

```bash
kafka-topics.sh --list --bootstrap-server localhost:9092
```

### Describe a Topic

```bash
kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:9092
```

### Produce Messages

```bash
kafka-console-producer.sh --topic my-topic --bootstrap-server localhost:9092
```

### Consume Messages

```bash
kafka-console-consumer.sh --topic my-topic --from-beginning --bootstrap-server localhost:9092
```

## 6.Kafka APIs

### 1. **Producer API**

Used to publish records to Kafka topics.

### 2. **Consumer API**

Used to subscribe to topics and process streams of records.

### 3. **Streams API**

Used to build applications that process streams of data using transformations, aggregations, and joins.

### 4. **Connect API**

Used to integrate Kafka with external systems (databases, cloud services, etc.) using **connectors**.


## 7.Kafka Streams Example

```java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> stream = builder.stream("input-topic");

KStream<String, String> uppercased = stream.mapValues(value -> value.toUpperCase());
uppercased.to("output-topic");

KafkaStreams streams = new KafkaStreams(builder.build(), properties);
streams.start();
```

## 8.Advanced Topics

### Consumer Offsets

- Kafka tracks the last read offset per consumer group.
- Allows fault-tolerant, scalable consumption.

### Log Compaction

- Keeps only the latest value per key in a topic.
- Useful for change data capture (CDC).

### Kafka Security

- Authentication (SASL)
- Authorization (ACLs)
- Encryption (SSL/TLS)

### Kafka Schema Registry

- Enforces and manages Avro, JSON, or Protobuf schemas.
- Ensures data compatibility and evolution.


## 9.Real-Time Use with Kafka

- Pair with **Apache Flink**, **Apache Spark Streaming**, **ksqlDB**, or **Kafka Streams** for stream processing.
- Use Kafka Connect to sync with:
  - PostgreSQL, MySQL
  - Elasticsearch
  - MongoDB
  - Cloud storage (S3, GCS)


## 10.Monitoring and Metrics

- Kafka exposes metrics via JMX.
- Use Prometheus + Grafana for dashboards.

Key Metrics:

- Under-replicated partitions
- Consumer lag
- Request latency
- Throughput per broker


## 11.Tips for Production

- Set correct **retention policies** per topic.
- Monitor disk usage and consumer lag.
- Tune producer acks and retries.
- Use **idempotent producers** to avoid duplicates.
- Leverage **Kafka MirrorMaker** for replication between clusters.


## 12.Summary Table

| Component     | Role                          |
| ------------- | ----------------------------- |
| Broker        | Kafka server                  |
| Topic         | Stream channel                |
| Partition     | Topic subdivision             |
| Producer      | Sends data                    |
| Consumer      | Reads data                    |
| Zookeeper     | Metadata manager (deprecated) |
| Kafka Streams | In-app stream processing      |
| Kafka Connect | External data integration     |


## 13.Getting Help & Resources

- Official Docs: [https://kafka.apache.org](https://kafka.apache.org)
- Book: *Kafka: The Definitive Guide* by Neha Narkhede et al.
- GitHub: [https://github.com/apache/kafka](https://github.com/apache/kafka)
- Tutorials: [https://developer.confluent.io](https://developer.confluent.io)

---