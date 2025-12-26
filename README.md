#Kafka Setup (Apache Kafka – KRaft Mode)

This module provides a local Apache Kafka setup using Docker Compose, configured in KRaft mode (Kafka without ZooKeeper).

It is used by:
1. Job Scheduler → publishes job events
2. Executor services → consume job events

This setup is intended for local development and mirrors modern Kafka production architecture.


#Prerequisites

1. Docker
2. Docker Compose
3. Ports available: 9092 (Kafka – host access)

#Docker Compose Configuration

Kafka is configured with two listeners:
1. One for host machine access
2. One for container-to-container access

This avoids common networking issues with Kafka in Docker.


#Starting Kafka
`docker compose up -d`

#Verify Kafka is running:
`docker logs kafka`

#Accessing the Kafka Container
`docker exec -it kafka bash
cd /opt/kafka/bin`

#Kafka CLI tools are located in:
`/opt/kafka/bin`


#Creating Topics
Create job-events topic

`./kafka-topics.sh \
--bootstrap-server kafka:29092 \
--create \
--topic job-events \
--partitions 3 \
--replication-factor 1`


**List topics**
`./kafka-topics.sh \
--bootstrap-server kafka:29092 \
--list`

**Describe topic**
./kafka-topics.sh \
--bootstrap-server kafka:29092 \
--describe \
--topic job-events



**Connecting Applications to Kafka**

1. From host machine (Spring Boot services)
`spring.kafka.bootstrap-servers=localhost:9092`
2. From other Docker containers
`   bootstrap.servers=kafka:29092`

