# Databricks Day 3 – Joins, Window Functions & Advanced PySpark Concepts

## 📌 Overview

This repository contains the **Day 3 Databricks notebook**, focused on advancing PySpark skills by working with **joins, window functions, UDFs**, and understanding **when to use PySpark vs Pandas**.

Day 3 builds on:

* **Day 0** – Databricks setup & data loading
* **Day 1** – Basic DataFrame operations
* **Day 2** – Spark execution concepts & aggregations

The emphasis of Day 3 is on **real-world data engineering patterns** used in analytics pipelines.

---

## 🎯 Learning Objectives

By completing Day 3, you will be able to:

* Understand when to use **PySpark vs Pandas**
* Perform different types of joins in PySpark
* Apply **window functions** for running totals and rankings
* Use **User-Defined Functions (UDFs)** safely and correctly
* Preserve row-level detail while performing advanced analytics
* Write clearer, more readable PySpark code

---

## 📂 Repository Contents

* `Day_3_DB_Challenge.ipynb`
  → Notebook covering joins, window functions, UDFs, and intermediate PySpark concepts

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup**
* Complete **Day 1 – PySpark DataFrame Fundamentals**
* Complete **Day 2 – Spark Execution & Aggregations**
* Ensure a **running Databricks cluster**
* Attach the notebook to the active cluster

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### 1️⃣ PySpark vs Pandas – Conceptual Comparison

Introduces the difference between **local vs distributed processing**.

Key points covered:

* Pandas runs on a single machine
* PySpark runs on a distributed cluster
* PySpark supports lazy evaluation and fault tolerance
* Pandas is suitable for small datasets; PySpark is built for scale

---

### 2️⃣ Create Sample DataFrames for Practice

Creates small in-memory DataFrames to demonstrate joins and transformations.

```python
orders = spark.createDataFrame(
    [(1, "Alice"), (2, "Bob"), (3, "Charlie")],
    ["order_id", "customer"]
)

payments = spark.createDataFrame(
    [(1, 100), (2, 200)],
    ["order_id", "amount"]
)
```

---

### 3️⃣ Inner Join

Returns only records that exist in both DataFrames.

```python
orders.join(payments, "order_id", "inner").show()
```

---

### 4️⃣ Left Join

Returns all records from the left DataFrame and matching records from the right.

```python
orders.join(payments, "order_id", "left").show()
```

---

### 5️⃣ Right Join

Returns all records from the right DataFrame and matching records from the left.

```python
orders.join(payments, "order_id", "right").show()
```

---

### 6️⃣ Full Outer Join

Returns all records from both DataFrames.

```python
orders.join(payments, "order_id", "outer").show()
```

---

### 7️⃣ Introduction to Window Functions

Explains how window functions allow calculations **without collapsing rows**, unlike `groupBy`.

Key use cases:

* Running totals
* Rankings
* Moving averages

---

### 8️⃣ Running Total per User

Calculates cumulative events per user ordered by time.

```python
from pyspark.sql import Window
from pyspark.sql import functions as F

user_window = Window.partitionBy("user_id").orderBy("event_time")

events.withColumn(
    "cumulative_events",
    F.count("*").over(user_window)
).show()
```

---

### 9️⃣ Ranking Using Window Functions

Ranks records based on a numeric column.

```python
rank_window = Window.orderBy(F.desc("price"))

events.withColumn(
    "rank",
    F.rank().over(rank_window)
).show()
```

---

### 🔟 User-Defined Functions (UDFs)

Introduces UDFs for applying custom Python logic to Spark DataFrames.

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

def price_category(price):
    return "High" if price > 200 else "Low"

price_udf = udf(price_category, StringType())
```

Applying the UDF:

```python
events.withColumn(
    "price_category",
    price_udf("price")
).show()
```

---

## 🧠 Key Concepts Reinforced

* PySpark is optimized for distributed workloads
* Joins define how datasets are combined
* Window functions preserve row-level detail
* UDFs add flexibility but should be used sparingly
* Built-in Spark functions are always preferred for performance

---

## ✅ Final Outcome

✔ Clear understanding of PySpark vs Pandas
✔ All join types executed successfully
✔ Window functions applied for running totals and rankings
✔ UDFs implemented and applied correctly
✔ Ready to handle more complex analytics workflows


