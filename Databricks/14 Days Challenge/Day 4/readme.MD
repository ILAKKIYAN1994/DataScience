# Databricks Day 4 – Delta Lake Fundamentals & Data Reliability Challenge

## 📌 Overview

This repository contains the **Day 4 Databricks Challenge notebook**, focused on working with **Delta Lake** to build **reliable, production-ready data pipelines**.

Day 4 shifts the learning from pure Spark transformations to **data reliability, correctness, and governance**, which are critical in real-world data engineering projects.

The notebook covers:

* Converting raw data to Delta format
* Creating Delta tables using PySpark and SQL
* Understanding and testing schema enforcement
* Handling duplicate inserts safely

---

## 🎯 Learning Objectives

By completing this challenge, you will be able to:

* Convert existing data into **Delta Lake format**
* Create **managed Delta tables** using PySpark
* Create Delta tables using **SQL**
* Understand and test **Delta schema enforcement**
* Identify and prevent **duplicate data inserts**
* Apply best practices for **reliable data ingestion**

---

## 📂 Repository Contents

* `Day_4_DB_Challenge.ipynb`
  → Notebook covering Delta Lake fundamentals and reliability challenges

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup & Data Loading**
* Complete **Day 1 – PySpark DataFrame Fundamentals**
* Complete **Day 2 – Spark Execution & Aggregations**
* Complete **Day 3 – Joins, Window Functions & UDFs**
* Ensure a **running Databricks cluster**
* Ensure required **Volumes are created** in the workspace

---

## 🧪 Challenge Walkthrough (Run Cells in Order)

---

### 1️⃣ Convert Raw Data to Delta Format

The first task converts existing data into **Delta format**, enabling ACID transactions and reliability.

```python
events.write \
    .format("delta") \
    .mode("overwrite") \
    .save("/Volumes/workspace/default/delta/events")
```

**Outcome:**

* Data stored as Parquet
* `_delta_log` created automatically
* Delta transaction tracking enabled

---

### 2️⃣ Create Delta Tables Using PySpark

Creates a **managed Delta table** registered in the metastore.

```python
events.write \
    .format("delta") \
    .saveAsTable("events_table")
```

**Key Point:**

* Managed tables are controlled by Databricks
* Storage location is handled automatically

---

### 3️⃣ Create Delta Tables Using SQL

Creates a Delta table using SQL from an existing source.

```sql
CREATE TABLE events_delta
USING DELTA
AS
SELECT * FROM events_table;
```

This demonstrates how Delta tables can be created and managed using SQL workflows.

---

### 4️⃣ Test Delta Schema Enforcement

Validates Delta Lake’s ability to **prevent invalid writes**.

```python
try:
    wrong_schema = spark.createDataFrame(
        [("a", "b", "c")],
        ["x", "y", "z"]
    )

    wrong_schema.write \
        .format("delta") \
        .mode("append") \
        .save("/Volumes/workspace/default/delta/events")

except Exception as e:
    print(f"Schema enforcement triggered: {e}")
```

**Expected Result:**

* Write operation fails
* Error confirms schema enforcement is working

---

### 5️⃣ Understand Schema Enforcement

Delta Lake enforces:

* Column names
* Data types
* Schema structure

This prevents:

* Silent data corruption
* Accidental schema drift
* Invalid pipeline writes

---

### 6️⃣ Handle Duplicate Inserts (Conceptual)

The challenge highlights why **blind appends** can cause duplicate data when jobs rerun.

```python
events.write \
    .format("delta") \
    .mode("append") \
    .saveAsTable("events_table")
```

This approach is **not safe** in production.

---

### 7️⃣ Safe Deduplication Strategies

Two approaches discussed:

**Option 1: Drop duplicates before write**

* Simple but limited

**Option 2: MERGE (Upsert)**

* Production-grade solution
* Prevents duplicates
* Supports updates and inserts atomically

---

## 🧠 Key Concepts Reinforced

* Delta Lake provides **ACID guarantees** on data lakes
* Schema enforcement protects data quality
* Volumes must exist before writing data
* Managed Delta tables simplify governance
* MERGE enables safe, idempotent pipelines

---

## ✅ Final Outcome

✔ Raw data converted to Delta format
✔ Delta tables created using PySpark and SQL
✔ Schema enforcement tested and validated
✔ Duplicate insert risks identified
✔ Best practices for reliable ingestion understood


