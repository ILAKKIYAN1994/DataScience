# Databricks Day 2 – Spark Execution Concepts & Advanced Transformations

## 📌 Overview

This repository contains the **Day 2 Databricks notebook** focused on understanding **how Spark works internally** while applying those concepts through hands-on PySpark operations.

Day 2 builds on **Day 0 (Setup)** and **Day 1 (DataFrame fundamentals)** and introduces execution-level concepts such as:

* Spark architecture (Driver, Executors, DAG)
* Lazy evaluation
* Pivoting and aggregations
* Window functions
* Working with real e-commerce–style event data
* Exporting and validating results using Databricks Volumes

The goal of Day 2 is to move beyond *what* Spark does and start understanding *how* and *why* it works.

---

## 🎯 Learning Objectives

By completing Day 2, you will be able to:

* Understand Spark’s execution architecture
* Differentiate between DataFrames and RDDs
* Explain lazy evaluation and actions vs transformations
* Perform advanced aggregations using `groupBy` and `pivot`
* Calculate business metrics such as conversion rates
* Use window functions for running totals
* Export and verify Spark outputs correctly
* Work confidently with Databricks Volumes

---

## 📂 Repository Contents

* `Day_2_DB_Challenge.ipynb`
  → Notebook covering Spark internals, aggregations, pivoting, and window functions

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup**
* Complete **Day 1 – Basic PySpark DataFrame Operations**
* Ensure a **running Databricks cluster**
* Attach the notebook to the active cluster

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### 1️⃣ Verify Spark Session

Confirms that Spark is available and the cluster is attached.

```python
spark
```

**Expected:**

* SparkSession object displayed
* Confirms cluster readiness

---

### 2️⃣ Load E-Commerce Events Data

Reads event data stored in a Databricks Volume.

```python
events = spark.read.parquet(
    "/Volumes/workspace/default/challenge/sample_commerce_events"
)

display(events)
```

---

### 3️⃣ Explore Event Distribution

Counts events by category and event type.

```python
events.groupBy("category_code", "event_type").count().show()
```

---

### 4️⃣ Pivot Event Types into Columns

Converts event types (`view`, `cart`, `purchase`) into columns.

```python
events.groupBy("category_code") \
      .pivot("event_type") \
      .count() \
      .show()
```

This reshapes the data from long format to wide format.

---

### 5️⃣ Calculate Conversion Rate

Calculates purchase-to-view conversion rate per category.

```python
from pyspark.sql import functions as F

events.groupBy("category_code") \
      .pivot("event_type") \
      .count() \
      .withColumn(
          "conversion_rate",
          F.when(F.col("view") > 0,
                 F.col("purchase") / F.col("view") * 100)
           .otherwise(0)
      ) \
      .show()
```

---

### 6️⃣ Understand Spark Architecture (Theory)

Key concepts covered in the notebook:

* **Driver** manages SparkSession and builds the DAG
* **Executors** run tasks in parallel and store data in memory
* **DAG (Directed Acyclic Graph)** defines execution order of transformations

---

### 7️⃣ DataFrames vs RDDs (Conceptual Comparison)

* RDDs: Low-level, flexible, manual optimization
* DataFrames: High-level, schema-aware, optimized by Catalyst
* Preferred approach for analytics and data engineering

---

### 8️⃣ Lazy Evaluation Demonstration

Spark transformations are not executed immediately.

```python
filtered_df = events.filter(events.event_type == "purchase")
```

Execution happens only when an action is triggered:

```python
filtered_df.count()
```

---

### 9️⃣ Window Function – Running Total per User

Calculates cumulative event count per user ordered by time.

```python
from pyspark.sql import Window
from pyspark.sql import functions as F

user_window = Window.partitionBy("user_id").orderBy("event_time")

events.withColumn(
    "cumulative_events",
    F.count("*").over(user_window)
).show()
```

Window functions preserve row-level detail while performing aggregations.

---

### 🔟 Export Aggregated Results to CSV

Writes results safely using Databricks Volumes.

```python
events.groupBy("category_code") \
      .pivot("event_type") \
      .count() \
      .write \
      .mode("overwrite") \
      .csv("/Volumes/workspace/default/challenge/output_samplecomm")
```

---

### 1️⃣1️⃣ Verify Exported Output

```python
%fs ls /Volumes/workspace/default/challenge/output_samplecomm
```

Expected files:

* `part-*.csv`
* `_SUCCESS`
* Spark metadata files

---

## 🧠 Key Concepts Reinforced

* Spark always writes **directories**, not single files
* `pivot()` must be applied immediately after `groupBy`
* Only grouped or aggregated columns appear after aggregation
* Window functions ≠ groupBy (rows are preserved)
* Volumes are the recommended storage approach in Databricks

---

## ✅ Final Outcome

✔ Spark execution concepts understood
✔ Event-level analytics performed
✔ Pivoting and conversion metrics calculated
✔ Window functions applied successfully
✔ Data exported and verified correctly
✔ Ready for intermediate Spark workflows




