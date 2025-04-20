---
title: "Apache Kafka: From Zero to First Topic"
datePublished: Sun Apr 20 2025 17:16:54 GMT+0000 (Coordinated Universal Time)
cuid: cm9pwt6fs001709jr8gyc5vty
slug: apache-kafka-from-zero-to-first-topic
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745169390520/483e7317-68a2-44b6-b18f-0d0807f357ba.png

---

# Introduction

Imagine you’re building a next‑generation streaming platform. Every time a user uploads a video, you need to:

* Generate thumbnails in multiple resolutions
    
* Scan for inappropriate content
    
* Update personalized recommendation feeds
    
* Log upload events for real‑time analytics
    

If you tried to perform all these steps **synchronously**—making the user’s browser wait until each task completes—you’d quickly run into timeouts, blocked resources, and frustrated users staring at loading spinners. Instead, you want to say:

> “Thanks! Your video is on its way—check back in a moment.”

Behind the scenes, you enqueue each job so that specialized workers can pick them up and process independently. This decoupling keeps your APIs snappy and your backend resilient.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165220273/51bef8da-d379-404c-bd34-6588a5435c5f.png align="center")

**Why Kafka?**

* **Massive throughput**: absorb millions of events per second without breaking a sweat.
    
* **Durable, replayable log**: retain your data for hours, days, or weeks, and replay history if you ever need to debug or rebuild a downstream service.
    
* **Multi‑consumer fan‑out**: let transcoding, captioning, analytics, and notification services all consume the same stream independently.
    

In this blog, you’ll learn how to:

1. **Install and start Kafka** on your laptop in under five minutes
    
2. **Create topics**, publish JSON messages, and consume them via the CLI
    
3. **Integrate Kafka** into a simple Node.js app using KafkaJS
    
4. **Understand Kafka’s architecture**—topics, partitions, brokers, and consumer groups
    
5. **Plan for scale** with partitions, replication, and multi‑cluster setups
    

By the end, you’ll have a working Kafka setup and the key concepts to architect real‑world, event‑driven systems. Ready? Let’s dive in!

# Synchronous vs. Asynchronous Processing

Before we dive into Kafka, it’s crucial to understand **why** we need asynchronous workflows—and how they differ from the traditional synchronous model.

### Quick Recap of Synchronous Calls

In a **synchronous** interaction, the client sends a request and **waits** for the server to complete processing before it can continue.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745160004775/7be45801-410a-478f-87c0-b93978b454d8.png align="center")

**Characteristics:**

* **Blocking:** The client (or HTTP connection) remains open until the server responds.
    
* **Resource‑intensive:** Server threads or containers stay occupied for the entire duration.
    
* **Timeout risk:** If processing takes too long (e.g., &gt;30 s), HTTP timeouts may abort the request.
    

---

### Why Long‑Running Tasks Demand Async

Consider a task that takes **10 minutes** to complete—such as video transcoding or generating a complex report. In a synchronous model:

* **User frustration:** They stare at a spinner or progress bar for minutes.
    
* **HTTP limits:** Most load balancers and browsers will time out long‑running requests.
    
* **Scalability issues:** Server resources (CPU, threads) stay tied up, reducing overall throughput.
    

To avoid these pitfalls, we shift to **asynchronous** processing.

---

### Asynchronous Programming

With **asynchronous** design, the flow becomes:

1. **Client** sends a request.
    
2. **Server** immediately responds: “Your job is queued (ID 123).”
    
3. **Server** enqueues a task message in a broker.
    
4. **Client** is free to continue or poll status later.
    
5. **Worker** pulls the message, processes it, then sends a completion notification.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165648499/b9f7794e-cc9b-45a2-a4a6-d8e040d9b560.png align="center")

But we don’t send the request to worker directly, instead we use a message broker in between:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165776415/266bf123-4883-4147-b27b-cc5486209c72.png align="center")

### Why Introduce a Message Broker?

You might ask: *Why not call the worker directly?* A message broker in between gives you:

1. **Reliability**
    
    * If the producer (API) crashes after enqueueing, the job isn’t lost—workers still see it.
        
2. **Retry & Dead‑Letter**
    
    * Failed tasks remain in the broker and can be retried or moved to a dead‑letter queue for inspection.
        
3. **Decoupling**
    
    * Producers and consumers can scale, deploy, and evolve independently without tight coupling.
        
4. **Load Buffering**
    
    * Spikes in incoming jobs are smoothed out—workers process at their own pace.
        
5. **Visibility & Monitoring**
    
    * You get a central queue of all pending and processed jobs for auditing and metrics.
        

# What Is a Message Broker?

A **message broker** is middleware that sits between the parts of your system that **produce** work and the parts that **consume** it. Instead of coupling your API directly to your workers, you hand off tasks as messages to the broker. Workers pull messages when they’re ready, process them, and then acknowledge completion.

---

### Role of the Message Broker

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745165966135/f6521e93-1d6c-46b5-a983-07095d61a915.png align="center")

* **Intermediary**  
    Acts as a buffer and router for messages, decoupling producers and consumers.
    
* **Durable Store**  
    Persists messages until they’re successfully processed.
    
* **Traffic Smoother**  
    Absorbs spikes by queueing messages for later processing.
    

---

### Key Benefits

1. **Reliability**
    
    * Messages aren’t lost if producers or consumers crash—they remain in the broker until acknowledged.
        
2. **Retry Capability**
    
    * Failed processing attempts leave messages in place (or move them to a dead‑letter queue) for automatic retries.
        
3. **Decoupling**
    
    * Producers and consumers evolve, scale, and deploy independently—no direct dependencies.
        
4. **Load Buffering**
    
    * Surges in incoming work are absorbed, preventing downstream overload.
        
5. **Observability**
    
    * Centralized visibility into pending, in‑flight, and failed messages for monitoring and metrics.
        

---

### Producer vs. Consumer Terminology

| Role | Definition |
| --- | --- |
| **Producer** | The component (API, service, microservice) that **publishes** messages into the broker. |
| **Consumer** | The worker or service that **subscribes** to, **pulls**, and **processes** messages from the broker. |

* **Producer Responsibilities**
    
    * Create and publish well‑formed messages (e.g., JSON payloads).
        
    * Handle broker connection errors and retries on publish.
        
* **Consumer Responsibilities**
    
    * Subscribe to one or more queues/topics.
        
    * Process messages exactly once (or at‑least once) and acknowledge upon success.
        
    * Handle poison messages and route to dead‑letter queues if necessary.
        

# Message Queues vs. Message Streams

Not all message brokers behave the same. They fall into two broad patterns: **message queues** for one‑to‑one delivery, and **message streams** for one‑to‑many delivery. Choose the right pattern based on your use case.

---

### One‑to‑One Delivery: Message Queues

A **message queue** ensures each message is delivered to exactly one consumer. Once that consumer processes the message, it’s removed from the queue.

#### Characteristics

* **Exclusive consumption**: Each message is consumed (and deleted) by a single worker.
    
* **Linear scaling**: Add more consumers to increase processing throughput; each picks a unique message.
    
* **Simple retry semantics**: Failed messages can be re‑queued or sent to a dead‑letter queue.
    

#### Typical Use Cases

* **Task offloading**: Video transcoding, image resizing, email sending.
    
* **Work queues**: Distribute independent jobs among a pool of workers.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745166186615/63f20b2c-9a26-4f57-8a7c-996a871b0505.png align="center")

---

### One‑to‑Many Delivery: Message Streams

A **message stream** (or log) allows multiple, independent consumer groups to each read the full sequence of messages—without removing them.

#### Characteristics

* **Broadcast semantics**: Every consumer group sees every message.
    
* **Configurable retention**: Messages stay available for a defined window (e.g., 7 days).
    
* **Replayability**: Consumers can rewind their offset to reprocess history.
    

#### Typical Use Cases

* **Event‑driven architectures**: Multiple services (analytics, auditing, notifications) consume the same events.
    
* **Real‑time analytics**: Dashboards, stream processing engines, machine‑learning pipelines.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745166360169/cf9af67d-9ece-4dbf-9821-8a03d4ec1a12.png align="center")

---

### 4.3 When to Choose Which

| Criteria | Message Queue | Message Stream |
| --- | --- | --- |
| **Delivery model** | One‑to‑one | One‑to‑many |
| **Retention** | Deleted on successful consume | Retained for a time or size window |
| **Reprocessing** | Harder (message gone) | Easy (rewind offset) |
| **Scaling** | Add more consumers | Add consumer groups & partitions |
| **Use cases** | Job queues, background tasks | Event sourcing, analytics, pub/sub |

* **Choose a message queue** when you have discrete jobs that each need processing by exactly one worker—e.g., transcoding video files or sending transactional emails.
    
* **Choose a message stream** when the same data must feed multiple independent consumers, and you want the ability to replay history—e.g., clickstream analytics, audit logs, or multi‑service event dissemination.
    

# Introduction to Apache Kafka

Apache Kafka is a **distributed streaming platform** built to handle real‑time data feeds with high throughput, low latency, and strong durability guarantees. It combines the best of message queues (decoupling, reliability) and publish‑subscribe systems (fan‑out, replay) into a single, scalable log‑based architecture.

---

### Kafka as a Distributed Stream Platform

* **Append‑Only Log**  
    Kafka stores each topic as an ordered, immutable sequence of records. Producers *append* to the log; consumers *read* at their own pace.
    
* **Partitioning for Scale**  
    Topics are sharded into **partitions**, each hosted on one of many brokers. More partitions = more parallelism.
    
* **Replication for Durability**  
    Each partition is replicated across multiple brokers. If one broker fails, another replica seamlessly takes over.
    
* **Consumer Offset Tracking**  
    Consumers maintain their own **offset** within each partition. They decide when to commit, rewind, or skip messages.
    
* **Retention Policies**  
    Messages are retained for a configurable window (time‑based or size‑based), enabling replay and auditability.
    

### Real‑World Use‑Cases

1. **GPS Telemetry (e.g., Ride‑hail Apps)**
    
    * **Challenge:** Thousands of drivers send location pings every few seconds.
        
    * **Solution:** Write all pings to Kafka instantly; batch‑persist to database every few minutes to avoid overwhelming your datastore.
        
2. **Video Upload Pipelines**
    
    * **Challenge:** An upload triggers transcoding, thumbnail generation, content scanning, and analytics logging.
        
    * **Solution:** Upload service publishes a single “videoUploaded” event; multiple downstream services consume and process concurrently.
        
3. **Website Clickstreams**
    
    * **Challenge:** Track user clicks and page views in real time for personalization and A/B testing.
        
    * **Solution:** Front‑end pushes events to Kafka; real‑time dashboards and recommendation engines consume the same stream.
        
4. **IoT Sensor Networks**
    
    * **Challenge:** Millions of sensors emit temperature/humidity data at high frequency.
        
    * **Solution:** Sensors publish readings to Kafka; stream processors aggregate, detect anomalies, and drive alerting systems.
        

# Kafka Core Concepts & Internals

To master Kafka, you need to understand its foundational building blocks: **topics**, **partitions**, **offsets**, and how **brokers**, **leaders/followers**, and **consumer groups** work together to deliver a scalable, fault‑tolerant stream platform.

### Topics, Partitions & Offsets

#### Topics

A **topic** is a named feed to which records are appended. Think of it as a log file for a particular event type (e.g., `user-signups`, `order-events`).

#### Partitions

Each topic is split into one or more **partitions**, which are independent, ordered logs. Partitions allow you to:

* **Parallelize** consumption: multiple consumers can read different partitions concurrently.
    
* **Scale** producers: you can spread writes across brokers.
    

#### Offsets

An **offset** is a sequential ID assigned to each message within its partition. Consumers use offsets to track their position in the log:

* **Commit offset** when a message is processed.
    
* **Rewind** by resetting to an earlier offset for replay or recovery.
    

### Brokers, Leaders/Followers & Consumer Groups

#### Brokers

A **broker** is a Kafka server instance that hosts one or more partitions. A Kafka **cluster** is a set of brokers working together.

* More brokers → more capacity and fault tolerance.
    

#### Leaders & Followers

Within each partition:

* One broker acts as the **leader** (handles all reads/writes).
    
* Other brokers host **follower** replicas, staying in sync with the leader.
    

If the leader fails, a follower is automatically promoted—ensuring continuity.

#### Consumer Groups

A **consumer group** is a set of consumers identified by a **group ID**. Kafka guarantees:

* **Each partition** is consumed by **exactly one** consumer in the group.
    
* **Rebalance** on group membership changes (consumers join/leave).
    

This provides horizontal scaling and fault tolerance for message processing.

With these core concepts—topics as logs, partitions for parallelism, offsets for position tracking, brokers for storage, leader/follower replication for durability, and consumer groups for scale—you’re now equipped to design and run Kafka clusters that power real‑time, event‑driven applications.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745166934155/31e52ee3-6b3d-489e-a360-f70197d17b7e.png align="center")

# Kafka Cluster Architecture

A Kafka **cluster** is a coordinated group of server processes (brokers) managed by a controller layer (ZooKeeper or KRaft), designed for high availability and fault tolerance. Below is an overview of its key components and behaviours.

### Controllers: ZooKeeper vs. KRaft

* **ZooKeeper (pre‑3.3)**
    
    * An external service that stores Kafka’s metadata (topics, partitions, ACLs).
        
    * Manages leader elections: when a broker fails, ZooKeeper selects new partition leaders.
        
    * Requires a separate ensemble of ZooKeeper nodes (odd number, e.g., 3 or 5).
        
* **KRaft (Kafka Raft, 3.3+)**
    
    * Built‑in consensus layer replacing ZooKeeper.
        
    * Brokers themselves form a quorum, storing metadata in an internal “controller” role.
        
    * Simplifies deployment: no external ZooKeeper cluster needed.
        

### Brokers & Partition Placement

Each **broker** in the cluster hosts zero or more **partitions** for each topic. Kafka distributes partitions across brokers to balance load.

### Replication for Durability

Kafka replicates each partition across multiple brokers:

* **Replication Factor**
    
    * Configurable per topic (e.g., `replication-factor=3`).
        
    * Ensures up to two broker failures without data loss.
        
* **Leader & Followers**
    
    * **Leader**: Handles all reads and writes for that partition.
        
    * **Followers**: Continuously fetch and replicate the leader’s log.
        

### Failover & High Availability

When a broker hosting a partition leader goes down:

1. **Controller** detects the broker failure via ZooKeeper/KRaft heartbeat.
    
2. **Controller** elects a new leader from the in‑sync replicas (ISRs).
    
3. **Clients** automatically redirect reads and writes to the new leader.
    

With controllers orchestrating the cluster, brokers storing and serving partitions, replication safeguarding data, and automatic failover maintaining availability, Kafka’s architecture delivers a robust foundation for real‑time, mission‑critical streaming applications.

![Kafka's architecture (illustrated with 3 partitions, 3 replicas and 5... |  Download Scientific Diagram](https://www.researchgate.net/publication/326564203/figure/fig2/AS:849339324837888@1579509683392/Kafkas-architecture-illustrated-with-3-partitions-3-replicas-and-5-brokers.ppm align="left")

# Installing & Getting Started

Before we can publish or consume any messages, we need a running Kafka cluster on our machine. Follow these steps to install Java, download Kafka, and launch either the classic ZooKeeper‑backed broker or the newer KRaft mode.

#### Java Requirement

Kafka runs on the JVM, so you’ll need **Java 11 or newer** installed:

```bash
# Verify your Java version
java -version
# Example output: openjdk version "11.0.16" 2022‑07‑19 LTS
```

#### Download & Unpack Kafka

1. **Download the latest Kafka release** (e.g., 3.4.0):
    
    ```bash
    curl -O https://downloads.apache.org/kafka/3.4.0/kafka_2.13-3.4.0.tgz
    ```
    
2. **Extract the archive**:
    
    ```bash
    tar -xzf kafka_2.13-3.4.0.tgz
    cd kafka_2.13-3.4.0
    ```
    
3. **Directory layout** (abbreviated):
    
    ```bash
    kafka_2.13-3.4.0/
    ├── bin/            # CLI scripts
    ├── config/         # Default configs (broker, ZooKeeper)
    ├── libs/           # Kafka and dependencies
    └── logs/           # (created at runtime)
    ```
    

#### Starting Kafka in ZooKeeper Mode

> **Note:** This is the “classic” mode available in all Kafka versions prior to 3.3 (or when you choose not to enable KRaft).

1. **Launch ZooKeeper** (broker metadata store):
    
    ```bash
    bin/zookeeper-server-start.sh config/zookeeper.properties &
    ```
    
2. **Start the Kafka broker**:
    
    ```bash
    bin/kafka-server-start.sh config/server.properties &
    ```
    
3. **Verify both processes** are running (on macOS/Linux):
    
    ```bash
    ps aux | grep -E 'zookeeper|kafka-server'
    ```
    
4. **Check broker logs** (`logs/server.log`) for a “started” message.
    

# Command‑Line Quickstart Demo

Let’s verify your Kafka installation end‑to‑end using the built‑in CLI tools. We’ll:

1. **Create** a topic named `test-topic` with 3 partitions.
    
2. **Produce** a couple of JSON messages into it.
    
3. **Consume** all messages from the beginning.
    

```bash

# 1. Create “test-topic” with 3 partitions and replication factor 1
bin/kafka-topics.sh --create \
  --topic test-topic \
  --bootstrap-server localhost:9092 \
  --partitions 3 \
  --replication-factor 1

# 2. Produce two JSON messages
echo '{"user":"alice","action":"login"}' | \
  bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092

echo '{"user":"bob","action":"purchase","item":"book"}' | \
  bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092

# 3. Consume all messages from the beginning
bin/kafka-console-consumer.sh \
  --topic test-topic \
  --bootstrap-server localhost:9092 \
  --from-beginning
```

## Node.js App Integration Demo

In this section, we’ll build a minimal Node.js app that:

1. **Produces** JSON messages into `test-topic`
    
2. **Consumes** messages from `test-topic` in real time
    

We’ll use the **kafkajs** library for simplicity and clarity.

### Project Setup

1. **Create a new directory** and initialize npm:
    
    ```bash
    mkdir hello-kafka
    cd hello-kafka
    npm init -y
    ```
    
2. **Install kafkajs**:
    
    ```bash
    npm install kafkajs
    ```
    

### Producer (`producer.js`)

Create a file named `producer.js` with the following content:

```javascript
const { Kafka } = require('kafkajs');

// 1. Configure the client to connect to our local broker
const kafka = new Kafka({
  clientId: 'my-producer',
  brokers: ['localhost:9092']
});

// 2. Create a producer instance
const producer = kafka.producer();

async function runProducer() {
  // 3. Establish connection
  await producer.connect();

  // 4. Send messages to 'test-topic'
  await producer.send({
    topic: 'test-topic',
    messages: [
      { key: 'alice', value: JSON.stringify({ action: 'login', timestamp: Date.now() }) },
      { key: 'bob',   value: JSON.stringify({ action: 'purchase', item: 'book', timestamp: Date.now() }) }
    ]
  });

  console.log('Messages sent successfully');

  // 5. Disconnect when done
  await producer.disconnect();
}

runProducer().catch(error => {
  console.error('Error in producer:', error);
  process.exit(1);
});
```

### Consumer (`consumer.js`)

Create a file named `consumer.js` with the following content:

```javascript
const { Kafka } = require('kafkajs');

// 1. Configure the client to connect to our local broker
const kafka = new Kafka({
  clientId: 'my-consumer',
  brokers: ['localhost:9092']
});

// 2. Create a consumer instance in the group 'demo-group'
const consumer = kafka.consumer({ groupId: 'demo-group' });

async function runConsumer() {
  // 3. Establish connection
  await consumer.connect();

  // 4. Subscribe to our topic from the very beginning
  await consumer.subscribe({ topic: 'test-topic', fromBeginning: true });

  console.log('🔔 Consumer is running and listening for messages...');

  // 5. Process each incoming message
  await consumer.run({
    eachMessage: async ({ partition, message }) => {
      const prefix = `Partition ${partition} | Offset ${message.offset}`;
      console.log(`${prefix} | Key: ${message.key.toString()} | Value: ${message.value.toString()}`);
    }
  });
}

runConsumer().catch(error => {
  console.error('Error in consumer:', error);
  process.exit(1);
});
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745169035325/62d7b2e0-5ee4-4f91-b217-5b7f5ba589bd.png align="center")

**Next steps to deepen your Kafka expertise:**

* **Schema Management:** Explore Confluent Schema Registry with Avro/Protobuf.
    
* **Stream Processing:** Build real‑time transforms using Kafka Streams or ksqlDB.
    
* **Exactly‑Once Semantics:** Understand idempotent producers and transactional messaging.
    
* **Operational Best Practices:** Monitor with Prometheus/Grafana, configure security (TLS/SASL), and perform rolling upgrades.