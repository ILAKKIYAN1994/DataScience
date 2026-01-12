# Databricks Day 5 – Advanced Delta Lake Operations & Performance Management

## 📌 Overview

This repository contains the **Day 5 Databricks Challenge notebook**, focused on **advanced Delta Lake capabilities** that are essential for building **scalable, reliable, and production-grade data pipelines**.

Day 5 builds on:

* **Day 4 – Delta Lake Fundamentals & Reliability**
* Earlier Spark and PySpark concepts (DataFrames, joins, window functions)

The emphasis of Day 5 is on **incremental data processing, historical data access, performance optimization, and storage cleanup** using Delta Lake features.

---

## 🎯 Learning Objectives

By completing Day 5, you will be able to:

* Perform **incremental upserts** using Delta `MERGE`
* Query **historical versions** of Delta tables using time travel
* Understand and apply **OPTIMIZE** and **ZORDER** for performance
* Use **VACUUM** safely to clean up obsolete files
* Understand Delta retention policies and time travel limitations
* Design rerun-safe, idempotent data pipelines

---

## 📂 Repository Contents

* `Day_5_DB_Challenge.ipynb`
  → Notebook covering advanced Delta Lake operations and performance management

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup & Data Loading**
* Complete **Day 1 – PySpark DataFrame Fundamentals**
* Complete **Day 2 – Spark Execution & Aggregations**
* Complete **Day 3 – Joins, Window Functions & UDFs**
* Complete **Day 4 – Delta Lake Fundamentals**
* Ensure a **running Databricks cluster**
* Ensure the Delta table and required **Volumes** already exist

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### 1️⃣ Incremental MERGE (Upserts)

Demonstrates how to safely insert new records and update existing records using Delta `MERGE`.

```python
from delta.tables import DeltaTable

deltaTable = DeltaTable.forPath(
    spark,
    "/Volumes/workspace/default/delta/events"
)
```

```python
deltaTable.alias("t").merge(
    updates.alias("s"),
    """
    t.user_session = s.user_session
    AND t.event_time = s.event_time
    """
).whenMatchedUpdateAll() \
 .whenNotMatchedInsertAll() \
 .execute()
```

**Key Outcome:**

* Prevents duplicate records
* Supports idempotent pipeline execution

---

### 2️⃣ Time Travel (Version History)

Explores Delta Lake’s ability to query historical data.

```sql
DESCRIBE HISTORY events_table;
```

Query a specific version:

```python
spark.read.format("delta") \
    .option("versionAsOf", 0) \
    .load("/Volumes/workspace/default/delta/events")
```

Query by timestamp:

```python
spark.read.format("delta") \
    .option("timestampAsOf", "2024-01-01") \
    .load("/Volumes/workspace/default/delta/events")
```

---

### 3️⃣ OPTIMIZE for File Compaction

Improves query performance by compacting small files.

```sql
OPTIMIZE events_table;
```

---

### 4️⃣ ZORDER for Data Skipping

Optimizes query performance for frequently filtered columns.

```sql
OPTIMIZE events_table
ZORDER BY (event_type, user_id);
```

**Use case:**

* Faster filtering and selective queries

---

### 5️⃣ VACUUM for Cleanup

Removes obsolete files that are no longer needed.

```sql
VACUUM events_table RETAIN 168 HOURS;
```

**Important Notes:**

* Default retention is **7 days**
* Preserves time travel within retention window
* Aggressive retention can permanently delete history

---

## 🧠 Key Concepts Reinforced

* `MERGE` enables safe, incremental data ingestion
* Time travel supports debugging, auditing, and recovery
* `OPTIMIZE` improves read performance
* `ZORDER` enhances data skipping
* `VACUUM` manages storage while respecting retention policies
* Retention settings determine how long historical data remains accessible

---

## ✅ Final Outcome

✔ Incremental data loads handled safely
✔ Historical versions queried successfully
✔ Delta tables optimized for performance
✔ Storage cleanup performed correctly
✔ Clear understanding of Delta retention behavior

