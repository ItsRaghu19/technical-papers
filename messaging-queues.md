# Messaging Queues

## Introduction
Messaging queues are an important communication mechanism in distributed systems. They enable applications and services to exchange information without requiring direct communication.

## What are Messaging Queues?
A messaging queue is a temporary storage system where messages sent by a producer are stored until a consumer receives and processes them. A message broker manages the delivery of messages between applications.

## Why they are Used?
Messaging queues support asynchronous communication, reduce dependencies between services, improve scalability, and increase reliability through message buffering and retries.

## Popular tools:

**RabbitMQ**: It's a open-source broker(AMQP). Best for Flexible routing, task queues, general purpose messaging.

**Apache Kafka**: It's a open-source event streaming. Best for High-throughput, replayable event steams(1M+ msg/s).

**Amazon SQS**: Best for serverless / AWS-native apps, zero broker maintenance.

**NATS**: It's a lightweight pub/sub(Go). Best for low-latency, cloud-native, edge messaging

**IBM MQ**: It's a Enterprise broker. Best for Regulated industries, hybrid on-prem + cloud, legacy integrated.

## What is Enerprise Message Bus.
An **Enterprise Message Bus (EMB)** is a shared communication infrastructure that connects different enterprise applications. It enables message routing, data exchange, and integration between systems.

## Conclusion
Messaging queues help develop scalable, reliable, and loosely coupled applications, while an enterprise message bus simplifies communication across an organization's systems.
