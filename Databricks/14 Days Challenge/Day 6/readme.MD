# Databricks Day 6 – Lakehouse Architecture (Bronze → Silver → Gold)

## 📌 Overview

This repository contains the **Day 6 Databricks Challenge notebook**, focused on designing and implementing the **Bronze–Silver–Gold Lakehouse architecture** using Delta Lake.

Day 6 represents a major milestone in the learning journey, where individual Spark and Delta concepts come together to form a **real-world, production-style data pipeline**.

The notebook demonstrates:

* Layered data architecture design
* Raw data ingestion into Bronze
* Data cleaning and validation in Silver
* Business-ready aggregations in Gold
* Best practices for incremental processing

---

## 🎯 Learning Objectives

By completing Day 6, you will be able to:

* Understand the **Bronze → Silver → Gold** Lakehouse pattern
* Design responsibilities for each data layer
* Ingest raw data reliably into the Bronze layer
* Clean, deduplicate, and validate data in the Silver layer
* Build business-level aggregates in the Gold layer
* Apply best practices for scalable and maintainable pipelines

---

## 📂 Repository Contents

* `Day_6_DB_Challenge.ipynb`
  → Notebook implementing a complete Bronze–Silver–Gold pipeline using Delta Lake

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup & Data Loading**
* Complete **Day 1 – PySpark DataFrame Fundamentals**
* Complete **Day 2 – Spark Execution & Aggregations**
* Complete **Day 3 – Joins, Window Functions & UDFs**
* Complete **Day 4 – Delta Lake Fundamentals**
* Complete **Day 5 – Advanced Delta Lake Operations**
* Ensure a **running Databricks cluster**
* Ensure required **Volumes exist**:

  * `workspace.default.raw_data`
  * `workspace.default.challenge`
  * `workspace.default.delta`

---

## 🏗️ Lakehouse Architecture Overview

| Layer  | Purpose       | Description                                                 |
| ------ | ------------- | ----------------------------------------------------------- |
| Bronze | Raw ingestion | Stores data exactly as received with minimal transformation |
| Silver | Cleaned data  | Applies validation, deduplication, and enrichment           |
| Gold   | Business data | Aggregated, analytics-ready datasets                        |

Each layer serves a **single responsibility**, improving reliability and maintainability.

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### 1️⃣ Bronze Layer – Raw Ingestion

Reads raw CSV data from a Volume and writes it to the Bronze Delta layer with ingestion metadata.

```python
raw = spark.read.csv(
    "/Volumes/workspace/default/raw_data/events.csv",
    header=True,
    inferSchema=True
)
```

```python
bronze = raw.withColumn("ingestion_ts", F.current_timestamp())

bronze.write \
    .format("delta") \
    .mode("append") \
    .save("/Volumes/workspace/default/challenge/bronze_events")
```

**Key Characteristics:**

* Append-only
* No business logic
* Preserves raw history

---

### 2️⃣ Silver Layer – Cleaning & Validation

Reads Bronze data and applies data quality rules and transformations.

```python
silver = (
    bronze
    .filter(F.col("price") > 0)
    .filter(F.col("price") < 10000)
    .dropDuplicates(["user_session", "event_time"])
    .withColumn("event_date", F.to_date("event_time"))
    .withColumn(
        "price_tier",
        F.when(F.col("price") < 10, "budget")
         .when(F.col("price") < 50, "mid")
         .otherwise("premium")
    )
)
```

```python
silver.write \
    .format("delta") \
    .mode("overwrite") \
    .save("/Volumes/workspace/default/challenge/silver_events")
```

**Key Characteristics:**

* Data quality enforcement
* Deduplication
* Enriched, trusted dataset

---

### 3️⃣ Gold Layer – Business Aggregates

Builds analytics-ready datasets for reporting and insights.

```python
product_perf = (
    silver
    .groupBy("product_id")
    .agg(
        F.countDistinct(
            F.when(F.col("event_type") == "view", F.col("user_id"))
        ).alias("views"),
        F.countDistinct(
            F.when(F.col("event_type") == "purchase", F.col("user_id"))
        ).alias("purchases"),
        F.sum(
            F.when(F.col("event_type") == "purchase", F.col("price"))
        ).alias("revenue")
    )
    .withColumn(
        "conversion_rate",
        F.when(F.col("views") > 0,
               F.col("purchases") / F.col("views") * 100)
         .otherwise(0)
    )
)
```

```python
product_perf.write \
    .format("delta") \
    .mode("overwrite") \
    .save("/Volumes/workspace/default/challenge/gold_products")
```

**Key Characteristics:**

* Aggregated metrics
* Business-ready
* Optimized for analytics and BI

---

## 🔄 Incremental Processing Pattern

* **Bronze** → Append new raw data
* **Silver** → Clean and deduplicate (can use MERGE for incremental loads)
* **Gold** → Recompute or incrementally update aggregates

This pattern supports **rerunnable, fault-tolerant pipelines**.

---

## 🧠 Key Concepts Reinforced

* Separation of concerns across layers
* Delta Lake as the foundation for all layers
* Append-only ingestion for traceability
* Cleaned and trusted Silver datasets
* Business-focused Gold tables
* Scalable Lakehouse design principles

---

## ✅ Final Outcome

✔ End-to-end Bronze–Silver–Gold pipeline implemented
✔ Raw data ingested reliably
✔ Cleaned and validated Silver layer created
✔ Business aggregates produced in Gold
✔ Strong foundation for real-world data engineering projects
