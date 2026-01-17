# Databricks Day 10 – Performance Optimization

## 📌 Overview

This repository contains the **Day 10 Databricks Challenge notebook**, focused on **performance optimization techniques** in Apache Spark and Delta Lake.

Day 10 moves beyond correctness and functionality into **efficiency, scalability, and production-readiness**, helping understand how Spark executes queries and how to optimize them for faster analytics workloads.

The learning combines **theory, hands-on experimentation, and benchmarking** using Databricks Community Edition.

---

## 🎯 Learning Objectives

By completing Day 10, you will be able to:

* Understand **Spark query execution plans**
* Analyze how Spark scans, filters, and shuffles data
* Apply **partitioning strategies** for large tables
* Use **OPTIMIZE and ZORDER** for Delta Lake performance
* Benchmark query performance before and after optimization
* Understand **caching concepts** and serverless limitations

---

## 📂 Repository Contents

* `Day_10_DB_Challenge.ipynb`
  → Notebook covering Spark execution plans, partitioning, optimization, and benchmarking

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Day 9 Databricks Challenges**
* Use **Databricks Community Edition**
* Ensure the following tables exist:

  * `silver.events`
  * `silver.events_part` (created during this challenge)
* Data should be clean and analytics-ready

---

## 🧠 Key Concepts Covered

### 🔹 Query Execution Plans

* Spark converts SQL queries into logical and physical execution plans
* Execution plans show:

  * File scans
  * Filters and predicate pushdown
  * Shuffles and exchanges
* Understanding execution plans helps identify performance bottlenecks

---

### 🔹 Partitioning Strategies

* Partitioning physically organizes data into directories
* Helps Spark **prune partitions** and reduce data scans
* Best practices:

  * Use low-to-medium cardinality columns
  * Avoid over-partitioning

---

### 🔹 OPTIMIZE & ZORDER

* **OPTIMIZE**

  * Compacts small files into larger ones
  * Reduces file open overhead
* **ZORDER**

  * Reorders data within files
  * Improves data skipping for selective queries
* Partitioning and ZORDER complement each other

---

### 🔹 Caching Techniques

* Caching stores frequently used datasets in memory
* Improves performance for repeated queries
* In **Databricks Serverless SQL**:

  * Table-level caching is **not supported**
  * Query result caching is handled automatically
* Explicit caching requires cluster-backed Spark compute

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Analyze Query Execution Plan

```python
spark.sql(
    "SELECT * FROM silver.events WHERE event_type='purchase'"
).explain(True)
```

This reveals:

* Logical plan
* Optimized plan
* Physical execution plan

---

### 2️⃣ Create a Partitioned Table

```sql
CREATE TABLE silver.events_part
USING DELTA
PARTITIONED BY (event_date, event_type)
AS
SELECT * FROM silver.events;
```

Partitions improve performance for date- and event-based filters.

---

### 3️⃣ Apply OPTIMIZE & ZORDER

```sql
OPTIMIZE silver.events_part
ZORDER BY (user_id, product_id);
```

This improves query efficiency for selective filters.

---

### 4️⃣ Benchmark Query Performance

```python
import time

start = time.time()
spark.sql(
    "SELECT * FROM silver.events WHERE user_id=12345"
).count()

print(f"Time: {time.time() - start:.2f}s")
```

Benchmarks are compared:

* Before partitioning
* After partitioning
* After OPTIMIZE and ZORDER

---

### 5️⃣ Caching (Conceptual in Serverless)

```python
# Conceptual example (works only on cluster-backed compute)
df = spark.table("silver.events")
df.cache()
df.count()
```

In Community Edition serverless SQL:

* Query result caching is automatic
* Table-level caching is not supported

---

## 🧪 Verification & Observations

* Execution plans show reduced scans after partitioning
* OPTIMIZE reduces small-file overhead
* ZORDER improves selective query speed
* Benchmark timings confirm performance improvements

---

## ⚠️ Community Edition Notes

| Feature             | Community Edition |
| ------------------- | ----------------- |
| Execution plans     | ✅                 |
| Partitioning        | ✅                 |
| OPTIMIZE & ZORDER   | ✅                 |
| Query result cache  | ✅                 |
| Table-level caching | ❌                 |
| Dedicated clusters  | ⚠️ Limited        |

---

## 🧠 Key Takeaways

* Performance optimization is critical for production workloads
* Execution plans reveal Spark’s internal behavior
* Partitioning reduces data scanned
* OPTIMIZE and ZORDER improve query efficiency
* Benchmarking validates optimization impact
* Serverless SQL relies on result caching rather than in-memory caching

---

## ✅ Final Outcome

✔ Learned how Spark executes queries
✔ Applied partitioning strategies
✔ Optimized Delta tables using OPTIMIZE & ZORDER
✔ Benchmarked query performance improvements
✔ Understood caching limitations in serverless compute
