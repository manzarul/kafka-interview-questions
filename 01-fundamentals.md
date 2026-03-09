# 01 — Fundamentals

## Q1: What is Apache Kafka and what problems does it solve?

**Apache Kafka** is a **distributed streaming platform** for building **real-time data pipelines** and **stream processing applications**.

### Problems it solves
- **Decoupling**: producers and consumers evolve independently.
- **Scalability**: scale horizontally by adding brokers and partitions.
- **Fault tolerance**: replication reduces data loss risk.
- **High throughput**: designed for very large message rates.
- **Durability**: messages are persisted to disk.
- **Low latency**: supports near real-time processing patterns.

### Common use cases
- Event sourcing
- Log aggregation
- Metrics collection
- Stream processing
- Microservices communication via events
- CDC (Change Data Capture)

---

## Q2: Explain Kafka’s core components

### Producer
- Publishes messages to topics.
- Selects partition (round-robin, key-based, or custom partitioner).
- Supports batching and compression.

### Consumer
- Subscribes to topics and reads messages.
- Participates in a **consumer group**.
- Tracks progress using **offsets**.

### Broker
- Kafka server node.
- Stores partitions and serves read/write requests.

### Topic
- Logical feed/category.
- Split into partitions.

### Partition
- Ordered, immutable log.
- Primary unit of parallelism.
- Ordering is guaranteed **within** a partition.

### ZooKeeper (legacy) / KRaft (modern)
- Controller/metadata system.
- KRaft replaces ZooKeeper (details in `02-kraft.md`).

### Consumer Group
- A set of consumers sharing work.
- Each partition is assigned to **exactly one** consumer in the group.

---

## Diagram: High-level Kafka components

```mermaid
flowchart LR
  P[Producer(s)] -->|produce| T[(Topic)]
  T -->|partitioned| B1[Broker 1]
  T -->|partitioned| B2[Broker 2]
  T -->|partitioned| B3[Broker 3]

  C1[Consumer Group A] -->|poll| B1
  C1 -->|poll| B2
  C2[Consumer Group B] -->|poll| B2
  C2 -->|poll| B3

  K[Controller (KRaft)] -. metadata .-> B1
  K -. metadata .-> B2
  K -. metadata .-> B3
```

---

## Q3: What is a partition and why is it important?

A **partition** is an ordered, immutable sequence of records within a topic.

### Why it matters
- **Parallelism**: more partitions → more consumers can process in parallel.
- **Ordering**: strict ordering **within** a partition.
- **Scalability**: partitions spread across brokers.
- **Fault tolerance**: partitions are replicated across brokers.

### Key point
Ordering is **not guaranteed across partitions**.

---

## Q4: What is a consumer group and how does it work?

A **consumer group** is a set of consumers that share the work of consuming a topic.

### How it works
- Each partition is assigned to **one** consumer in the group.
- If consumers > partitions → some consumers idle.
- If a consumer fails → partitions reassign (rebalance).

### Benefits
- Horizontal scaling
- Fault tolerance
- Load balancing

---

## Diagram: Partition assignment in a consumer group

```mermaid
flowchart TB
  subgraph Topic[Topic: orders (6 partitions)]
    P0[P0]:::p
    P1[P1]:::p
    P2[P2]:::p
    P3[P3]:::p
    P4[P4]:::p
    P5[P5]:::p
  end

  subgraph Group[Consumer Group: order-processors]
    C1[Consumer 1]
    C2[Consumer 2]
    C3[Consumer 3]
  end

  P0 --> C1
  P1 --> C1
  P2 --> C2
  P3 --> C2
  P4 --> C3
  P5 --> C3

  classDef p fill:#eef,stroke:#88a,stroke-width:1px;
```

---

## Q5: What is the difference between a queue and Kafka?

### Traditional queue (e.g., RabbitMQ/SQS)
- Message removed after consumption.
- Usually one consumer processes each message.
- Limited replay.
- Often push-based.

### Kafka
- Messages retained by time/size retention.
- Multiple consumer groups can read the same message.
- Replay by resetting offsets.
- Pull-based.

### Rule of thumb
- **Queue**: task distribution / one-time processing.
- **Kafka**: event streaming / fan-out / replay / analytics.
