# Kafka & Confluent - Flashcards (Q&A)

Use this file for quick drilling. Read the question, answer aloud, then check.

---

## Fundamentals

**Q:** What is Kafka?

**A:** A distributed streaming platform for real-time pipelines and streaming apps; durable log storage + pub/sub at scale.

---

**Q:** What are Kafka’s core components?

**A:** Producers, consumers, topics, partitions, brokers, (ZooKeeper legacy / KRaft modern), consumer groups.

---

**Q:** What does a partition provide?

**A:** Ordering within the partition and parallelism across partitions.

---

**Q:** Is ordering guaranteed across partitions?

**A:** No—ordering is guaranteed only within a partition.

---

**Q:** What is a consumer group rule?

**A:** Within a group, each partition is assigned to exactly one consumer.

---

## KRaft

**Q:** What is KRaft?

**A:** Kafka’s Raft-based metadata mode that removes ZooKeeper and uses a controller quorum.

---

**Q:** Name key KRaft configs.

**A:** `process.roles`, `node.id`, `controller.quorum.voters`, listeners + controller listener names.

---

**Q:** Can you merge two DCs into one Kafka cluster?

**A:** Not in typical production; run independent clusters and replicate using MirrorMaker2 or Cluster Linking.

---

## Replication & Reliability

**Q:** What is ISR?

**A:** In-sync replicas; replicas caught up enough with the leader to be eligible for leadership.

---

**Q:** What does `acks=all` mean?

**A:** Producer waits for acknowledgments from all ISR replicas.

---

**Q:** What is a safe production baseline?

**A:** RF=3, `acks=all`, `min.insync.replicas=2`.

---

## Offsets & Rebalancing

**Q:** Where are offsets stored?

**A:** In Kafka internal topic `_consumer_offsets` (Kafka >= 0.9).

---

**Q:** What causes consumer duplicates?

**A:** Processing succeeds, but crash occurs before offset commit → message is re-read.

---

**Q:** What causes message loss with consumers?

**A:** Committing offsets before processing completes.

---

**Q:** What is a rebalance?

**A:** Partition reassignment among consumers in the group when membership/partitions change.

---

## Performance

**Q:** Name top producer throughput knobs.

**A:** `linger.ms`, `batch.size`, `buffer.memory`, `compression.type`, async sends, partitioning, idempotence.

---

**Q:** Name top consumer throughput knobs.

**A:** partitions, `max.poll.records`, fetch sizes, batching, parallelism, rebalance avoidance, manual commits.

---

**Q:** What is backpressure?

**A:** Consumers (or downstream) can’t keep up; lag grows.

---

**Q:** How do you mitigate backpressure?

**A:** Scale consumers, increase partitions, batch downstream writes, rate-limit producers, pause/resume, DLQ.

---

## Semantics

**Q:** What is Kafka’s default delivery guarantee pattern?

**A:** At-least-once (duplicates possible) when committing after processing.

---

**Q:** How do you achieve exactly-once?

**A:** Idempotent producer + transactions + read_committed consumers; easiest with Kafka Streams (`exactly_once_v2`).

---

## Connect / Schema

**Q:** What is Kafka Connect?

**A:** Separate integration runtime to move data in/out of Kafka via connectors.

---

**Q:** Does Kafka broker provide DLQ/backoff retries?

**A:** No; implemented in apps or via Kafka Connect (connector-level) or Streams patterns.

---

**Q:** Why Schema Registry?

**A:** Compatibility enforcement, safe evolution, smaller payloads, type safety across many services.

---

## Ops

**Q:** Top broker health alerts?

**A:** Offline partitions > 0, under-replicated partitions > 0, active controller != 1.

---

**Q:** First step in high lag troubleshooting?

**A:** Measure lag per partition/group, then identify whether it’s processing speed, partition skew, downstream bottleneck, rebalances, or network.
