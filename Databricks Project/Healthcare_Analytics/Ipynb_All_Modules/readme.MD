# 🏥 Databricks Lakehouse ML Project – End-to-End Readmission Prediction

This repository contains an **end-to-end Databricks Lakehouse project** that demonstrates how to design, build, and operationalize a **real-world analytics + machine learning solution** using modern data engineering and ML best practices.

The project is structured into **six modular notebooks**, each representing a logical layer in a production-grade data platform:

```
Configuration → Bronze → Silver → Gold → ML Model → Dashboard Queries
```

This design closely follows **industry-standard Lakehouse architecture**, ensuring scalability, governance, reproducibility, and business usability.

---

## 📂 Repository Structure

| Notebook                     | Purpose                                          |
| ---------------------------- | ------------------------------------------------ |
| `1. Configuration.ipynb`     | Environment setup and shared configurations      |
| `2. Bronze.ipynb`            | Raw data ingestion layer                         |
| `3. Silver.ipynb`            | Cleaned and validated data layer                 |
| `4. Gold.ipynb`              | Business aggregates & ML-ready datasets          |
| `5. ML Model.ipynb`          | Feature engineering, model training & evaluation |
| `6. Dashboard Queries.ipynb` | SQL analytics for dashboards & insights          |

---

## 1️⃣ Configuration Notebook

### 🎯 Objective

Establish a **single source of truth for environment configuration** and ensure consistency across all downstream notebooks.

### 🔑 Key Responsibilities

* Select catalog and schema
* Define reusable variables (paths, table names)
* Centralize project configuration
* Validate workspace context

### ✅ Outcome

A clean, reusable setup that avoids hardcoding and supports portability across environments.

---

## 2️⃣ Bronze Layer – Raw Ingestion

### 🎯 Objective

Ingest raw source data into the Lakehouse **without modifying business meaning**.

### 🔑 Key Responsibilities

* Read raw source files
* Append ingestion metadata (timestamps, source info)
* Write Delta tables in append/overwrite mode
* Enable auditability and traceability

### 🧠 Design Principle

> *Bronze data is immutable, raw, and reproducible.*

### ✅ Outcome

A reliable raw data foundation suitable for replay and debugging.

---

## 3️⃣ Silver Layer – Data Cleaning & Validation

### 🎯 Objective

Transform raw data into **analytics-ready, trustworthy datasets**.

### 🔑 Key Responsibilities

* Data type casting
* Null handling and validation
* Deduplication logic
* Business rule enforcement
* Feature preparation (derived columns)

### 🧠 Design Principle

> *Silver data represents the single version of truth for analytics.*

### ✅ Outcome

High-quality datasets suitable for BI, ML, and downstream consumption.

---

## 4️⃣ Gold Layer – Business Aggregates

### 🎯 Objective

Create **consumption-ready datasets** for analytics, dashboards, and machine learning.

### 🔑 Key Responsibilities

* Business-level aggregations
* Metric calculations (counts, rates, revenue, etc.)
* ML-friendly summary tables
* Optimized schema for fast querying

### 🧠 Design Principle

> *Gold data answers business questions directly.*

### ✅ Outcome

Curated datasets optimized for performance and usability.

---

## 5️⃣ ML Model Notebook – Predictive Analytics

### 🎯 Objective

Build and evaluate a **machine learning model for readmission prediction** using Spark ML and MLflow.

### 🔑 Key Responsibilities

* Feature engineering using Spark ML Pipelines
* Train-test split
* Model training (Random Forest, Gradient Boosted Trees)
* Evaluation using AUC, accuracy, precision, recall
* Model comparison and selection
* MLflow experiment tracking

### 🧠 Key ML Concepts Applied

* Why AUC is preferred over accuracy for imbalanced data
* Feature vectorization using `VectorAssembler`
* Handling categorical variables with `StringIndexer` & `OneHotEncoder`
* Model interpretability through metrics and plots

### ✅ Outcome

A validated, explainable ML model with experiment history stored in MLflow.

---

## 6️⃣ Dashboard Queries – SQL Analytics

### 🎯 Objective

Translate curated data into **actionable business insights**.

### 🔑 Key Responsibilities

* Write analytical SQL queries
* Compute KPIs and trends
* Support dashboard visualizations
* Enable filters and drill-downs

### 🧠 Design Principle

> *SQL is the final mile between data and decision-making.*

### ✅ Outcome

SQL-ready datasets powering dashboards and reports for stakeholders.

---

## 🧠 Architecture Summary

```
Raw Data
   ↓
Bronze (Raw Delta)
   ↓
Silver (Clean & Validated)
   ↓
Gold (Aggregated & ML-Ready)
   ↓
ML Models + Dashboards
```

This architecture ensures:

* Scalability
* Reproducibility
* Governance
* Business alignment

---

## 🚀 Key Skills Demonstrated

* Databricks Lakehouse Architecture
* Delta Lake best practices
* PySpark Data Engineering
* Spark ML Pipelines
* MLflow experiment tracking
* SQL analytics & dashboard readiness
* End-to-end ML project design

---

✨ *Built with Databricks, Spark, Delta Lake, and MLflow*

