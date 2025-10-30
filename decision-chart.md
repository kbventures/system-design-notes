# System Design Decision Charts

## Table of Contents

- [Uptime Requirements](#uptime-requirements)
- [Database Choice](#database-choice)
- [Caching Strategy](#caching-strategy)
- [Consistency Models](#consistency-models)
- [Read vs Write Patterns](#read-vs-write-patterns)
- [Storage Estimates](#storage-estimates)
- [Latency Numbers](#latency-numbers)
- [Traffic Scaling](#traffic-scaling)
- [CAP Theorem Priorities](#cap-theorem-priorities)
- [Data Retention](#data-retention)
- [Load Balancing](#load-balancing)
- [API Rate Limiting](#api-rate-limiting)
- [Partitioning Strategy](#partitioning-strategy)

---

## Uptime Requirements

| Service Type         | Uptime  | Downtime/Year | Examples                            |
| -------------------- | ------- | ------------- | ----------------------------------- |
| **Mission Critical** | 99.999% | 5 min         | Banks, payments, 911 systems        |
| **High Priority**    | 99.99%  | 52 min        | Social media, URL shorteners, email |
| **Standard**         | 99.9%   | 8.7 hours     | Blogs, internal tools, analytics    |
| **Low Priority**     | 99%     | 3.65 days     | Dev environments, batch jobs        |

## Database Choice

| Need                              | Use                         | Why                              |
| --------------------------------- | --------------------------- | -------------------------------- |
| **Complex queries, transactions** | SQL (PostgreSQL, MySQL)     | ACID, joins, relations           |
| **High write throughput**         | NoSQL (Cassandra, DynamoDB) | Horizontally scalable            |
| **Key-value lookups**             | Redis, Memcached            | Fastest reads, caching           |
| **Full-text search**              | Elasticsearch               | Search optimization              |
| **Time-series data**              | InfluxDB, TimescaleDB       | Analytics, metrics               |
| **Graphs/relationships**          | Neo4j                       | Social networks, recommendations |

## Caching Strategy

| Cache Hit Rate | Action  | Impact                 |
| -------------- | ------- | ---------------------- |
| **>80%**       | Ideal   | Minimal DB load        |
| **50-80%**     | Good    | Acceptable performance |
| **<50%**       | Problem | Cache not helping much |

## Consistency Models

| Model                    | Use When                   | Examples                     |
| ------------------------ | -------------------------- | ---------------------------- |
| **Strong consistency**   | Money, inventory, seats    | Banking, e-commerce checkout |
| **Eventual consistency** | Social feeds, likes, views | Twitter feed, YouTube views  |
| **Session consistency**  | User's own data            | Shopping cart, user profile  |

## Read vs Write Patterns

| Pattern         | Ratio | Optimization                   | Examples                    |
| --------------- | ----- | ------------------------------ | --------------------------- |
| **Read-heavy**  | 100:1 | Cache, read replicas, CDN      | News, blogs, URL shorteners |
| **Write-heavy** | 1:10  | Write buffers, async, sharding | Logging, analytics, IoT     |
| **Balanced**    | 1:1   | General optimization           | Social media posts          |

## Storage Estimates

| Data Type              | Size      | Notes                        |
| ---------------------- | --------- | ---------------------------- |
| **User profile**       | 1 KB      | Name, email, metadata        |
| **Tweet/post**         | 200 bytes | Short text                   |
| **Image (compressed)** | 200 KB    | JPEG, PNG                    |
| **Video (1 min)**      | 10 MB     | Compressed streaming quality |
| **Video (1 min HD)**   | 50 MB     | High quality                 |

## Latency Numbers

| Operation                       | Latency | Notes             |
| ------------------------------- | ------- | ----------------- |
| **L1 cache**                    | 1 ns    | Fastest           |
| **L2 cache**                    | 4 ns    |                   |
| **RAM**                         | 100 ns  |                   |
| **SSD read**                    | 100 μs  | 100,000 ns        |
| **HDD seek**                    | 10 ms   | 10,000,000 ns     |
| **Network (same datacenter)**   | 500 μs  | 0.5 ms            |
| **Network (cross-country)**     | 150 ms  | US coast to coast |
| **Network (intercontinental)**  | 300 ms  | US to Europe/Asia |
| **Packet roundtrip (internet)** | 150 ms  | Average           |

### Key Takeaways

- **Memory is ~100x faster than SSD**
- **SSD is ~100x faster than HDD**
- **Network within datacenter is fast (~0.5ms)**
- **Cross-country adds significant latency (150ms)**

## Traffic Scaling

| Users (DAU)  | Avg QPS  | Peak QPS | DB Strategy         |
| ------------ | -------- | -------- | ------------------- |
| **1K-10K**   | 1-10     | 5-50     | Single server       |
| **10K-100K** | 10-100   | 50-500   | Read replicas       |
| **100K-1M**  | 100-1K   | 500-5K   | Sharding starts     |
| **1M-10M**   | 1K-10K   | 5K-50K   | Multi-region        |
| **10M-100M** | 10K-100K | 50K-500K | Distributed systems |
| **100M+**    | 100K+    | 500K+    | Full distribution   |

## CAP Theorem Priorities

| System Type            | Choose | Why                                    |
| ---------------------- | ------ | -------------------------------------- |
| **Banking, inventory** | CP     | Consistency critical                   |
| **Social media, DNS**  | AP     | Availability critical                  |
| **Analytics, logs**    | AP     | Availability + eventual consistency OK |

## Data Retention

| Data Type     | Retention     | Storage              |
| ------------- | ------------- | -------------------- |
| **Hot data**  | Days to weeks | SSD, RAM cache       |
| **Warm data** | Months        | SSD, cheaper tier    |
| **Cold data** | Years         | S3, Glacier          |
| **Archive**   | Forever       | Glacier Deep Archive |

## Load Balancing

| Algorithm             | Use When                   | Trade-off             |
| --------------------- | -------------------------- | --------------------- |
| **Round Robin**       | Equal capacity servers     | Simple, fair          |
| **Least Connections** | Variable request times     | More intelligent      |
| **Weighted**          | Different server specs     | Resource optimization |
| **IP Hash**           | Session persistence needed | Sticky sessions       |

## API Rate Limiting

| User Tier      | Requests/Hour | Requests/Second |
| -------------- | ------------- | --------------- |
| **Free**       | 1,000         | ~0.3            |
| **Basic**      | 10,000        | ~3              |
| **Pro**        | 100,000       | ~30             |
| **Enterprise** | Unlimited     | Custom          |

## Partitioning Strategy

| Method         | Use When           | Example           |
| -------------- | ------------------ | ----------------- |
| **Range**      | Sequential data    | Timestamps, IDs   |
| **Hash**       | Even distribution  | User IDs          |
| **Geographic** | Location-based     | Multi-region apps |
| **Functional** | Different services | Microservices     |
