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

Enable slow query logs to capture and analyze inefficient queries automatically.

### MySQL Example
```sql
SET GLOBAL slow_query_log = 1;
SET GLOBAL long_query_time = 2; -- Log queries taking longer than 2 seconds
SHOW VARIABLES LIKE 'slow_query_log%';
