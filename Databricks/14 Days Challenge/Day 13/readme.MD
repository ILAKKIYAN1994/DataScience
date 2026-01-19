# Databricks Day 13 – Model Comparison & Spark ML Pipelines

## 📌 Overview

This repository contains the **Day 13 Databricks Challenge notebook**, focused on **training multiple machine learning models, comparing their performance, and building scalable Spark ML pipelines**.

Day 13 represents a key milestone where **feature engineering, MLflow experiment tracking, and distributed machine learning** come together to support **model selection and production-ready ML workflows**.

---

## 🎯 Learning Objectives

By completing Day 13, you will be able to:

* Train and evaluate multiple ML models
* Compare model performance using MLflow
* Understand model complexity vs performance trade-offs
* Build Spark ML Pipelines using `VectorAssembler`
* Evaluate Spark ML models using regression metrics
* Select the most appropriate model based on business and technical criteria

---

## 📂 Repository Contents

* `Day_13_DB_Challenge.ipynb`
  → Notebook covering multi-model training, MLflow comparison, and Spark ML pipeline construction

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Day 12 Databricks Challenges**
* Use **Databricks Community Edition**
* Ensure the following table exists and is ML-ready:

  * `gold.products`
* Required columns in `gold.products`:

  * `views`, `revenue`, `total_events`, `purchases`

---

## 🧠 Key Concepts Covered

---

### 🔹 Training Multiple Models

Training multiple models provides perspective on:

* Model complexity
* Bias–variance trade-off
* Interpretability vs performance

Models trained include:

* Linear Regression
* Decision Tree Regressor
* Random Forest Regressor

---

### 🔹 Hyperparameter Awareness

Although full grid search is not implemented, model configuration choices such as:

* Tree depth
* Number of estimators

demonstrate how hyperparameters influence model behavior and overfitting.

---

### 🔹 Feature Importance

Different models interpret features differently:

* Linear models use coefficients
* Tree-based models use feature importance
* Spark ML models handle features through vectors

Understanding feature influence is critical for explainability.

---

### 🔹 Spark ML Pipelines

Spark ML Pipelines provide:

* Consistent feature assembly
* Scalable model training
* Production-ready ML workflows

Pipelines encapsulate transformations and models into a single reusable unit.

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Train Multiple Models (sklearn)

```python
models = {
    "linear": LinearRegression(),
    "decision_tree": DecisionTreeRegressor(max_depth=5),
    "random_forest": RandomForestRegressor(n_estimators=50)
}
```

Each model is trained and evaluated using the same train-test split to ensure fair comparison.

---

### 2️⃣ Compare Models Using MLflow

For each model:

* Parameters are logged
* R² score is logged as a metric
* The trained model is logged as an artifact

MLflow UI is used to:

* Compare runs
* Analyze performance differences
* Select the best-performing model

---

### 3️⃣ Build a Spark ML Pipeline

```python
assembler = VectorAssembler(
    inputCols=["views", "revenue", "total_events"],
    outputCol="features"
)

lr = SparkLR(
    featuresCol="features",
    labelCol="purchases"
)

pipeline = Pipeline(stages=[assembler, lr])
```

This pipeline standardizes feature engineering and model training.

---

### 4️⃣ Evaluate Spark ML Model

```python
predictions = spark_model.transform(test)
predictions.select("purchases", "prediction").show(5)
```

This step validates:

* End-to-end pipeline execution
* Reasonable prediction behavior
* Feature effectiveness

Optional metric evaluation using `RegressionEvaluator` is also supported.

---

## 🧠 Model Selection Criteria

The best model is selected based on:

| Criterion        | Consideration           |
| ---------------- | ----------------------- |
| R² Score         | Predictive strength     |
| Overfitting Risk | Stability across data   |
| Interpretability | Business explainability |
| Scalability      | Big data readiness      |

Model selection is not based solely on the highest metric.

---

## 🧠 Key Takeaways from Day 13

* Multiple models provide better insight than a single approach
* MLflow enables systematic and reproducible comparison
* Feature engineering directly impacts model quality
* Spark ML Pipelines are essential for scalable ML
* The best model balances performance, simplicity, and scalability

---

## ✅ Final Outcome

✔ Three ML models trained and compared
✔ Experiments tracked and analyzed using MLflow
✔ Spark ML pipeline built and evaluated
✔ Model selection principles understood
✔ End-to-end ML workflow completed
