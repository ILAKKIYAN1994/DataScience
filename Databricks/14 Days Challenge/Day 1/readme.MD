# Databricks Day 1 – Basic PySpark DataFrame Operations

## 📌 Overview

This repository contains the **Day 1 Databricks notebook** focused on learning and practicing **basic PySpark DataFrame operations**.

Day 1 builds on the **Day 0 setup** and introduces hands-on Spark fundamentals such as:

* Creating DataFrames
* Inspecting schemas
* Selecting, filtering, and sorting data
* Adding and renaming columns
* Aggregations and null checks

The goal of Day 1 is to become comfortable working with **Spark DataFrames** and core transformations.

---

## 🎯 Learning Objectives

By completing Day 1, you will be able to:

* Verify Spark session availability
* Create DataFrames manually
* Understand DataFrame schema
* Apply common DataFrame transformations
* Perform filtering, sorting, and aggregations
* Inspect data quality (null checks)
* Display controlled outputs

---

## 📂 Repository Contents

* `Day_1_DB_Challenge.ipynb`
  → Notebook covering basic PySpark DataFrame commands

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup**
* Ensure a **running Databricks cluster**
* Attach the notebook to the active cluster

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### **1️⃣ Verify Spark Is Running**

This cell confirms that the Spark session is active.

```python
spark
```

Expected:

* SparkSession object details displayed
* Confirms cluster is attached correctly

---

### **2️⃣ Create a Simple DataFrame**

A small dataset is created manually to practice Spark operations.

```python
data = [
    ("iPhone", "Electronics", 999),
    ("MacBook", "Electronics", 1999),
    ("Shoes", "Fashion", 120),
    ("Watch", "Accessories", 250),
    ("TV", "Electronics", 1500)
]

columns = ["product", "category", "price"]

df = spark.createDataFrame(data, columns)
df.show()
```

---

### **3️⃣ View DataFrame Schema**

Displays column names, data types, and nullability.

```python
df.printSchema()
```

---

### **4️⃣ Select Specific Columns**

Selects only required columns from the DataFrame.

```python
df.select("product").show()
```

---

### **5️⃣ Filter Data Based on Price**

Filters products with price greater than 1000.

```python
df.filter(df.price > 1000).show()
```

---

### **6️⃣ Add a New Column**

Creates a new column using an expression.

```python
from pyspark.sql.functions import col

# WithColumn creates a new column with the name and expression
df.withColumn("discounted_price", col("price") * 0.9).show()
```

---

### **7️⃣ Rename a Column**

Renames an existing column for better clarity.

```python
df.withColumnRenamed("price", "price_INR").show()
```

---

### **8️⃣ Sort Data by Column**

Sorts products by price in descending order.

```python
# OrderBy sorts the dataframe by the column name
df.orderBy("price", ascending=False).show()
```

---

### **9️⃣ Count Distinct Values**

Counts unique categories present in the dataset.

```python
df.select("category").distinct().show()
```

---

### **🔟 Group and Aggregate Data**

Performs aggregations such as average and maximum price per category.

```python
from pyspark.sql.functions import avg, max

df.groupBy("category").agg(
    avg("price").alias("avg_price"),
    max("price").alias("max_price")
).show()
```

---

### **1️⃣1️⃣ Check for Null Values**

Filters rows where the `product` column contains null values.

```python
df.filter(df.product.isNull()).show()
```

---

### **1️⃣2️⃣ Display Limited Rows**

Displays a controlled number of rows without truncation.

```python
df.show(5, truncate=False)
```

---

## ✅ Final Outcome

✔ Spark session verified
✔ DataFrame created successfully
✔ Schema inspected and understood
✔ Core DataFrame transformations applied
✔ Filtering, sorting, and aggregation completed
✔ Data quality checks performed

---


