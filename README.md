# Kafka & Confluent Interview Prep — Clean Notes Pack

This repository-style pack splits the content into focused documents and adds **Mermaid diagrams** + **flashcards** for fast revision.

## What’s inside

- [01-fundamentals.md](01-fundamentals.md) — Kafka basics (topics, partitions, brokers, producers/consumers, groups)
- [02-kraft.md](02-kraft.md) — KRaft: Kafka without ZooKeeper (why, how, configs, multi-cluster)
- [03-core-mechanics.md](03-core-mechanics.md) — Replication/ISR, acks, ordering, offsets, rebalancing
- [04-performance.md](04-performance.md) — Producer/consumer tuning, backpressure, zero-copy, message size
- [05-streams-ksqldb.md](05-streams-ksqldb.md) — Kafka Streams vs ksqlDB (when/why), EOS notes
- [06-connect-dlq-retries.md](06-connect-dlq-retries.md) — Kafka Connect, connectors, retries/backoff/DLQ (what is and isn’t native)
- [07-schema-registry.md](07-schema-registry.md) — Schema Registry, formats, compatibility, evolution strategies
- [08-ops-monitoring-troubleshooting.md](08-ops-monitoring-troubleshooting.md) — Monitoring, alerting, consumer lag troubleshooting
- [flashcards.md](flashcards.md) — Q&A flashcards (interview quick drill)

## Diagrams
All diagrams are embedded using **Mermaid** so they render nicely on GitHub and many Markdown viewers.

> Tip: If your Markdown viewer doesn’t render Mermaid, paste the diagrams into https://mermaid.live

## Usage
- Read end-to-end in order (01 → 08), then drill with **flashcards.md**.
- Copy/paste config snippets into your internal wiki/runbooks.
