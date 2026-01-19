# Databricks Day 14 – AI-Powered Analytics (Genie & Mosaic AI)

## 📌 Overview

This repository contains the **Day 14 Databricks Challenge notebook**, focused on **AI-powered analytics using Databricks Genie and Mosaic AI concepts**, along with hands-on practice using **open-source generative AI models**.

Day 14 introduces how **Generative AI integrates with the Lakehouse** to enable:

* Natural language analytics
* AI-assisted insights
* Experiment tracking for AI workflows

While some enterprise features are not available in Databricks Community Edition, this day emphasizes **conceptual understanding + practical GenAI workflows**.

---

## 🎯 Learning Objectives

By completing Day 14, you will be able to:

* Understand Databricks Genie and its role in natural language analytics
* Explain Mosaic AI capabilities and enterprise GenAI workflows
* Translate business questions into SQL queries
* Perform basic NLP tasks using pretrained models
* Track AI experiments using MLflow
* Generate AI-powered insights from unstructured text

---

## 📂 Repository Contents

* `Day_14_DB_Challenge.ipynb`
  → Notebook covering Genie concepts, Mosaic AI overview, NLP practice, and MLflow logging

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Day 13 Databricks Challenges**
* Use **Databricks Community Edition**
* Internet access for Python package installation
* MLflow available (enabled by default in Databricks)

---

## ⚠️ Community Edition Scope

| Feature             | Availability            |
| ------------------- | ----------------------- |
| Databricks Genie    | ❌ Not available         |
| Mosaic AI Studio    | ❌ Not available         |
| MLflow tracking     | ✅ Available             |
| Hugging Face models | ✅ Available (CPU-based) |

This day focuses on **learning enterprise concepts and practicing open-source alternatives**.

---

## 🧠 Key Concepts Covered

---

### 🔹 Databricks Genie (Conceptual)

Databricks Genie enables:

* Natural language → SQL translation
* Query execution on Databricks SQL Warehouses
* Democratized analytics for non-technical users

Example business questions:

* *Show total revenue by category*
* *Find customers who viewed but never purchased*

Genie automatically generates optimized SQL behind the scenes.

---

### 🔹 Mosaic AI (Conceptual)

Mosaic AI is Databricks’ enterprise generative AI platform that supports:

* Foundation model training and fine-tuning
* Secure AI deployment
* Retrieval-Augmented Generation (RAG)
* AI governance and monitoring

It integrates deeply with Lakehouse data for enterprise-grade AI.

---

### 🔹 Generative AI Integration

Generative AI allows:

* Natural language understanding
* Text classification
* Sentiment analysis
* AI-assisted insights on business data

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Natural Language Analytics (Conceptual Practice)

Mapped business questions to SQL queries, simulating Genie behavior:

* Revenue by category
* Top converting products
* Daily purchase trends
* Users who viewed but never purchased

This builds intuition for AI-driven analytics.

---

### 2️⃣ NLP Task – Sentiment Analysis (Hands-on)

Installed required libraries:

```python
%pip install transformers torch
```

Performed sentiment analysis using a pretrained model:

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")

reviews = [
    "This product is amazing!",
    "Terrible quality, waste of money",
    "Good value for the price",
    "Not worth buying"
]

results = classifier(reviews)
```

---

### 3️⃣ AI-Powered Insights

Converted raw predictions into interpretable insights:

```python
for review, result in zip(reviews, results):
    print(f"Review: {review}")
    print(f"Sentiment: {result['label']} (score={result['score']:.2f})")
```

This simulates:

* Customer feedback analysis
* Product sentiment monitoring
* AI-driven decision support

---

### 4️⃣ Track AI Experiments with MLflow

```python
import mlflow

with mlflow.start_run(run_name="sentiment_analysis_day14"):
    mlflow.log_param("model", "distilbert-sentiment")
    mlflow.log_param("task", "sentiment-analysis")
    mlflow.log_metric("sample_accuracy", 0.95)
```

This demonstrates:

* AI experiment tracking
* Model metadata logging
* Metric comparison

---

## 🧠 Key Takeaways from Day 14

* Natural language analytics simplifies data exploration
* Generative AI enhances insight discovery
* AI models integrate naturally with Lakehouse data
* MLflow is central for AI experiment management
* Open-source GenAI mirrors enterprise AI workflows

---

## ✅ Final Outcome

✔ Understood Databricks Genie & Mosaic AI concepts
✔ Practiced AI-assisted analytics workflows
✔ Built a simple NLP application
✔ Logged AI experiments using MLflow
✔ Connected GenAI concepts to real analytics use cases
