# Databricks Day 11 – Statistics, Hypothesis Testing & Feature Engineering

## 📌 Overview

This repository contains the **Day 11 Databricks Challenge notebook**, focused on **exploratory data analysis, statistical reasoning, and feature engineering** using PySpark.

Day 11 bridges the gap between **data engineering and machine learning**, helping understand how raw event data can be analyzed statistically and transformed into **model-ready features**.

The emphasis is on **understanding user behavior**, validating assumptions, and preparing high-quality features for downstream ML workflows.

---

## 🎯 Learning Objectives

By completing Day 11, you will be able to:

* Compute **descriptive statistics** to understand data distributions
* Perform **hypothesis-driven analysis** (weekday vs weekend behavior)
* Identify **relationships and correlations** between variables
* Engineer **time-based, behavioral, and numerical features**
* Understand how features translate into **machine learning signals**

---

## 📂 Repository Contents

* `Day_11_DB_Challenge.ipynb`
  → Notebook covering descriptive statistics, hypothesis testing, and feature engineering using PySpark

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Day 10 Databricks Challenges**
* Use **Databricks Community Edition**
* Ensure the following table exists:

  * `workspace.default.silver_events_part`
* Dataset should include:

  * `event_time`, `event_type`, `price`, `user_id`, `product_id`

---

## 🧠 Key Concepts Covered

---

### 🔹 Descriptive Statistics

Descriptive statistics help summarize data using metrics such as:

* Count
* Mean
* Standard deviation
* Minimum and maximum values

These metrics provide a first-level understanding of data quality, spread, and outliers.

---

### 🔹 Hypothesis Testing (Exploratory)

A hypothesis-driven approach is used to analyze:

* User behavior on **weekdays vs weekends**
* Differences in event types (views, purchases, carts)

While this is not formal statistical testing (t-test / chi-square), it provides **directional insights** useful for product and marketing decisions.

---

### 🔹 Correlation Analysis

Correlation measures the **strength and direction of linear relationships** between numeric variables.

Important considerations:

* Correlation applies only to **row-level numeric columns**
* Correlation does not imply causation
* Aggregate metrics should not be used directly in correlation analysis

---

### 🔹 Feature Engineering

Feature engineering transforms raw data into **signals usable by ML models**.

Key feature categories:

* Time-based features
* Behavioral features
* Numerical transformations

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Descriptive Statistics

```python
events.describe(["price"]).show()
```

This computes count, mean, standard deviation, min, and max for price values.

---

### 2️⃣ Hypothesis: Weekday vs Weekend Behavior

Weekend flag derived directly from timestamps:

```python
from pyspark.sql import functions as F

events_flagged = events.withColumn(
    "is_weekend",
    F.dayofweek("event_time").isin([1, 7])  # Sunday=1, Saturday=7
)
```

Behavior comparison:

```python
events_flagged.groupBy("is_weekend", "event_type") \
    .count() \
    .orderBy("is_weekend", "event_type") \
    .show()
```

---

### 3️⃣ Correlation Analysis

Example correlation between numeric features:

```python
events.stat.corr("price", "hour")
```

Used to identify linear relationships between variables.

---

### 4️⃣ Feature Engineering (ML-Ready)

#### Time-based Features

```python
features = events \
    .withColumn("hour", F.hour("event_time")) \
    .withColumn("day_of_week", F.dayofweek("event_time"))
```

Captures time-of-day and weekly behavior patterns.

---

#### Log Transformation

```python
features = features.withColumn(
    "price_log",
    F.log(F.col("price") + 1)
)
```

Normalizes skewed price distributions.

---

#### Behavioral Feature: Time Since First Event

```python
from pyspark.sql import Window

window = Window.partitionBy("user_id") \
               .orderBy("event_time") \
               .rowsBetween(Window.unboundedPreceding, Window.currentRow)

features = features.withColumn(
    "first_event_time",
    F.first("event_time").over(window)
).withColumn(
    "time_since_first_view",
    F.unix_timestamp("event_time") -
    F.unix_timestamp("first_event_time")
)
```

Measures user engagement intensity over time.

---

## 🧠 Feature-to-ML Mapping

| Feature               | Purpose                     |
| --------------------- | --------------------------- |
| hour                  | Time-of-day buying behavior |
| day_of_week           | Weekly usage patterns       |
| is_weekend            | Behavioral segmentation     |
| price_log             | Normalized numeric input    |
| time_since_first_view | Engagement intensity        |

---

## 🧪 Verification

```python
features.select(
    "event_time",
    "hour",
    "day_of_week",
    "is_weekend",
    "price",
    "price_log",
    "time_since_first_view"
).show(10, truncate=False)
```

Verify:

* No unexpected null values
* Correct timestamp-based calculations
* Logical feature values

---

## 🧠 Key Takeaways

* Descriptive statistics provide foundational understanding
* Hypothesis-driven analysis guides exploration
* Correlation highlights relationships, not causality
* Feature engineering is critical for ML success
* Well-designed features matter more than algorithms

---

## ✅ Final Outcome

✔ Data distributions analyzed
✔ Behavioral hypotheses explored
✔ Correlations evaluated
✔ Time-based and behavioral features engineered
✔ Dataset prepared for machine learning
