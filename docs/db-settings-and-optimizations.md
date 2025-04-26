# Database Settings and Optimizations

Optimizing only SQL queries is not enough. Database-level settings, architecture, and scaling strategies are critical for achieving high performance. This guide covers best practices for DB configurations, monitoring, and scaling.

---

## 📖 Table of Contents
- [1. Slow Query Logging](#1-slow-query-logging)
- [2. Balance Normalization and Performance](#2-balance-normalization-and-performance)
- [3. Database Engines](#3-database-engines)
- [4. Read Replicas](#4-read-replicas)
- [5. Query Caching](#5-query-caching)
- [6. Partitioning Strategies](#6-partitioning-strategies)
- [7. Monitoring and Tuning DB Configs](#7-monitoring-and-tuning-db-configs)
- [8. Sharding for Large Systems](#8-sharding-for-large-systems)

---

## 1. Slow Query Logging

Enable slow query logs to capture and analyze inefficient queries automatically.This need to be enable form sql config file.

## 2. Balance Normalization and Performance
   Normalization ensures data consistency and avoids redundancy.
   Over-normalization can increase JOINs and slow reads.
    Solution: Selective Denormalization or Materialized Views for read-heavy systems.
 
  📌 Use normalized models for OLTP (transactional systems) and denormalized models for OLAP (analytical systems).

## 3. Database Engines
 Understanding storage engines impacts performance:
Engine	Features	Best Use Cases
Engine | Features | Best Use Cases
InnoDB | Transactions, row-level locking, crash recovery | General purpose
MyISAM | Faster reads, table-level locking, no transactions | Read-heavy workloads
Memory | Data stored in RAM, non-persistent | Temporary tables, caching

Check current engine:

SHOW TABLE STATUS WHERE Name = 'your_table';

## 4. Read Replicas
- Replicate the primary database asynchronously to multiple read replicas.

- Direct SELECT queries to replicas to offload read traffic.
- Useful for scaling read-heavy applications without affecting write performance.
- Note: Replication lag can occur. Plan for eventual consistency.

## 5. Query Caching
 - Caches result sets to avoid recomputing identical queries.
 - MySQL 8.0+ removed native query caching — use external caches like Redis or Memcached instead.
 - Cache expensive, frequently executed queries at the application layer.
   
## 6. Partitioning Strategies
  - Split large tables into partitions for faster query processing.
  - Partitioning methods:
  - Range Partitioning: Split by range of values (e.g., dates).
  - List Partitioning: Split by specific discrete values.
  - Hash Partitioning: Based on a hash function for even distribution.
   
```sql
CREATE TABLE orders (
  order_id BIGINT PRIMARY KEY,
  created_at DATE
)
PARTITION BY RANGE (YEAR(created_at)) (
  PARTITION p2022 VALUES LESS THAN (2023),
  PARTITION p2023 VALUES LESS THAN (2024)
);
```  

## 7. Monitoring and Tuning DB Configs
Key areas to monitor:
- Connection Pool Size: Ensure it's tuned for your concurrency needs.
- Query Execution Plans: Regularly use EXPLAIN to analyze queries.
- Buffer Pools and Cache Hit Ratios: Maximize memory utilization.
- Replication Health: Monitor lag between master and replicas.

Use tools like:
- New Relic, Datadog, Prometheus, Grafana for DB monitoring.
- Native tools like Performance Schema (MySQL), pg_stat_activity (PostgreSQL).

## 8. Sharding for Large Systems
For extreme scaling beyond vertical limits:
- Sharding splits a single database into multiple independent databases.
- Each shard handles a subset of the data (e.g., by user ID range).
- Reduces contention and allows horizontal scaling.

**Common strategies:**
- Hash-Based Sharding (e.g., MOD(user_id, number_of_shards))
- Range-Based Sharding (e.g., customers A-M in one shard, N-Z in another)

⚠️ Adds major complexity — reserved for very large systems (e.g., Instagram, Dropbox, Twitter).


