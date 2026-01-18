# Databricks Day 12 – MLflow Basics & Experiment Tracking

## 📌 Overview

This repository contains the **Day 12 Databricks Challenge notebook**, focused on **introducing MLflow and the fundamentals of machine learning experiment tracking**.

Day 12 marks the transition from **data engineering and analytics** into **machine learning and MLOps fundamentals**.
The emphasis is on building an **ML-ready Gold table**, training a simple regression model, and tracking experiments using **MLflow**.

---

## 🎯 Learning Objectives

By completing Day 12, you will be able to:

* Understand MLflow core components
* Prepare ML-ready datasets from Gold tables
* Train a simple regression model
* Log parameters, metrics, and models using MLflow
* View and compare experiment runs using the MLflow UI
* Debug common ML and MLflow issues

---

## 📂 Repository Contents

* `Day_12_DB_Challenge.ipynb`
  → Notebook covering Gold table correction, ML model training, and MLflow experiment tracking

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Day 11 Databricks Challenges**
* Use **Databricks Community Edition**
* Ensure the following table exists:

  * `workspace.default.silver_events_part`
* MLflow must be enabled (available by default in Databricks)

---

## 🧠 Key Concepts Covered

---

### 🔹 MLflow Components

MLflow consists of four key components:

* **Tracking** – Logs parameters, metrics, and artifacts for each run
* **Models** – Stores trained models in a standardized format
* **Experiments** – Groups related runs together
* **MLflow UI** – Visual interface to explore and compare runs

---

### 🔹 Importance of ML-Ready Gold Tables

Machine learning pipelines depend heavily on **data quality**.
Incorrect aggregations or NULL-heavy metrics can silently break model training.

Day 12 reinforces the principle:

> **Data quality matters more than algorithms**

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Build a Correct Gold Table (ML-Ready)

Gold aggregation rewritten to ensure:

* No NULL metrics
* Numeric features suitable for ML

```python
from pyspark.sql import functions as F

silver = spark.table("workspace.default.silver_events_part")

gold_products = silver.groupBy("product_id").agg(
    F.sum(F.when(F.col("event_type") == "view", 1).otherwise(0)).alias("views"),
    F.sum(F.when(F.col("event_type") == "purchase", 1).otherwise(0)).alias("purchases"),
    F.sum(
        F.when(F.col("event_type") == "purchase", F.col("price"))
         .otherwise(0)
    ).alias("revenue"),
    F.count("*").alias("total_events")
)

gold_products = gold_products.withColumn(
    "conversion_rate",
    F.when(F.col("views") > 0,
           F.col("purchases") / F.col("views"))
     .otherwise(0)
)
```

Table written with schema overwrite to handle evolution safely.

---

### 2️⃣ Prepare Data for Machine Learning

```python
df = spark.table("gold.products").toPandas()

df_ml = df[["views", "purchases"]]
df_ml = df_ml.dropna()
```

Clean data is then split into train and test sets.

---

### 3️⃣ Train a Simple Regression Model

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

X = df_ml[["views"]]
y = df_ml["purchases"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)
```

---

### 4️⃣ Track Experiments with MLflow

```python
import mlflow
import mlflow.sklearn

with mlflow.start_run(run_name="linear_regression_day12"):
    mlflow.log_param("model_type", "LinearRegression")
    mlflow.log_param("feature_used", "views")
    mlflow.log_param("test_size", 0.2)

    r2_score = model.score(X_test, y_test)
    mlflow.log_metric("r2_score", r2_score)

    mlflow.sklearn.log_model(model, "model")
```

---

### 5️⃣ View & Compare Runs in MLflow UI

Steps:

1. Open **Experiments** from Databricks sidebar
2. Select **Default Experiment**
3. Open individual runs to inspect:

   * Parameters
   * Metrics
   * Model artifacts
4. Select multiple runs and use **Compare** to evaluate performance differences

---

## 🧠 Key Learnings from Day 12

* MLflow provides reproducibility for ML experiments
* Parameters describe experiment setup
* Metrics describe model performance
* Models are versioned artifacts
* Data preparation errors are the most common cause of ML failures
* Schema enforcement and evolution are critical in Delta Lake

---

## 🧪 Common Issues Encountered & Resolved

* Schema mismatch while overwriting Delta tables
* NULL metrics in Gold tables
* Empty training datasets after cleaning
* Feature column mismatches
* Python indentation and syntax errors in MLflow runs

Each issue reinforced real-world ML debugging practices.

---

## ✅ Final Outcome

✔ ML-ready Gold table created
✔ Regression model trained successfully
✔ Parameters, metrics, and model logged in MLflow
✔ MLflow UI used to inspect and compare runs
✔ End-to-end ML experiment lifecycle completed
