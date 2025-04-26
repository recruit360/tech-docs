# SQL Performance Tuning: Best Practices for Faster Queries

Over time, many teams face challenges when scaling SQL databases. Interestingly, the root cause isn't usually the size of the data, but rather **how the database is used** — through poorly written queries, missing indexes, or inefficient designs.

In this post, we'll walk through **practical tips** that every developer should understand and apply to write faster SQL queries.

---

## 1. Code Section

### 📌 Use DB Connection Pool
A connection pool reuses database connections, improving performance and reducing overhead.

**Benefits:**
- **Reduces Connection Overhead:** Reuses authenticated, established connections.
- **Improves Response Time:** Faster query execution under load.
- **Limits DB Load:** Controls max concurrent connections.
- **Supports Concurrency:** Threads reuse connections efficiently.
- **Better Resource Management:** Handles stale/idle connections.

---

### 📌 Use Prepared Statements
- Prepared queries are precompiled and reused with different parameters.
- Reduces parsing and compiling time.
- Speeds up repeated query executions.

---

### 📌 Avoid `SELECT *` (Wildcard)
- Retrieve only necessary columns.
- Saves bandwidth and memory.

---

### 📌 Avoid Certain SQL Functions in Critical Queries
Functions in WHERE, JOIN, or GROUP BY clauses can harm performance:

| ❌ Avoid | ✅ Prefer |
| :-- | :-- |
| `WHERE YEAR(order_date) = 2024` | `WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31'` |
| `WHERE LOWER(username) = 'john'` | Store username lowercase during INSERT/UPDATE |
| `WHERE email LIKE '%@gmail.com'` | Store domain separately if possible |
| `TRIM(customer_id) = customers.id` | Clean data during insert/update |

> Functions like `CAST`, `TO_CHAR`, `ABS`, `MOD`, and `DISTINCT` should also be used carefully.

---

### 📌 Use Transactions Carefully
- Keep transactions **small and efficient**.
- Always **commit** or **rollback** promptly.
- Avoid unnecessary logic inside transactions.

---

### 📌 Indexing in SQL
Indexes use a **B+ Tree** for fast lookups. Improves reads but can slow writes.

**Checklist:**
- Index columns used in WHERE, JOIN, ORDER BY.
- Avoid indexing low-selectivity columns (e.g., gender).
- Minimize redundant indexes.
- Prefer composite indexes if queries always use the same columns together.
- Avoid indexing huge text/blob columns unless needed.

> 📜 Check existing indexes:
> ```
> SHOW INDEX FROM table_name;
> ```

> 🛠️ Create an index:
> ```
> CREATE INDEX idx_name ON table_name (column1, column2);
> ```

---

### 📌 Use `EXPLAIN` Query
Understand how the database executes your queries:

```sql
EXPLAIN SELECT * FROM users WHERE email = 'someone@example.com';
