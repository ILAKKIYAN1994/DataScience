# Databricks Day 7 – Workflows & Job Orchestration

## 📌 Overview

This repository contains the **Day 7 Databricks Challenge notebook**, focused on **orchestrating data pipelines** using **Databricks Jobs and Workflows**.

Day 7 marks the transition from *manual notebook execution* to **automated, scheduled, production-grade pipelines**.
The focus is on parameterization, task dependencies, scheduling, and error handling—key skills for real-world data engineering.

---

## 🎯 Learning Objectives

By completing Day 7, you will be able to:

* Understand the difference between **Databricks Jobs and Notebooks**
* Parameterize notebooks using **widgets**
* Build **multi-task workflows** (Bronze → Silver → Gold)
* Configure **task dependencies**
* Schedule jobs for automatic execution
* Handle failures safely using workflow controls

---

## 📂 Repository Contents

* `Day_7_DB_Challenge.ipynb`
  → Notebook demonstrating parameterized execution and workflow-ready logic

---

## 🧩 Prerequisites

Before running this notebook:

* Complete **Day 0 – Databricks Setup**
* Complete **Day 1–Day 6 (Spark, Delta & Lakehouse Architecture)**
* Ensure a **running Databricks cluster**
* Ensure required **Volumes exist**:

  * `workspace.default.raw_data`
  * `workspace.default.challenge`

---

## 🧠 Key Concepts Covered

### 🔹 Databricks Jobs vs Notebooks

* **Notebooks** are interactive and used for development
* **Jobs** automate notebook execution with scheduling, retries, and monitoring

### 🔹 Multi-Task Workflows

* Break pipelines into independent tasks
* Define execution order using dependencies
* Stop downstream tasks automatically on failure

### 🔹 Parameters & Scheduling

* Widgets enable dynamic, reusable notebooks
* Job parameters override widget defaults
* Schedules automate execution (e.g., daily at 2 AM)

### 🔹 Error Handling

* Failures propagate to dependent tasks
* Logs and run history help with debugging
* Validation checks prevent bad downstream runs

---

## 🧪 Notebook Walkthrough (Run Cells in Order)

---

### 1️⃣ Add Parameter Widgets

Widgets allow passing runtime parameters to the notebook.

```python
dbutils.widgets.text(
    "source_path",
    "/Volumes/workspace/default/raw_data/2019-Oct.csv"
)

dbutils.widgets.dropdown(
    "layer",
    "bronze",
    ["bronze", "silver", "gold"]
)
```

---

### 2️⃣ Read Widget Values

Widget values must be explicitly retrieved.

```python
source_path = dbutils.widgets.get("source_path")
layer = dbutils.widgets.get("layer")

print("Source Path:", source_path)
print("Layer:", layer)
```

---

### 3️⃣ Layer-Based Execution Logic

A single notebook executes different logic based on the `layer` parameter.

```python
def run_layer(layer_name):
    if layer_name == "bronze":
        # Bronze ingestion logic
        pass
    elif layer_name == "silver":
        # Silver cleaning logic
        pass
    elif layer_name == "gold":
        # Gold aggregation logic
        pass
    else:
        raise ValueError("Invalid layer")
```

Execution:

```python
run_layer(layer)
```

---

### 4️⃣ Manual Layer Execution (Learning Mode)

* Set widget `layer = bronze` → run Bronze
* Change widget to `silver` → run Silver
* Change widget to `gold` → run Gold

This helps understand each layer independently.

---

### 5️⃣ Multi-Task Job Design (Workflow)

In Databricks **Workflows UI**:

| Task   | Notebook       | Parameters     | Dependency |
| ------ | -------------- | -------------- | ---------- |
| Bronze | Day 7 notebook | `layer=bronze` | —          |
| Silver | Day 7 notebook | `layer=silver` | Bronze     |
| Gold   | Day 7 notebook | `layer=gold`   | Silver     |

---

### 6️⃣ Scheduling

Jobs are scheduled via the **Workflows UI**:

* Frequency: **Daily**
* Time: **02:00 AM**
* Timezone: Local timezone

Scheduling is handled at the **job level**, not in notebook code.

---

## 🔄 Execution Flow

```text
Bronze Task
   ↓
Silver Task
   ↓
Gold Task
```

* Downstream tasks run **only if upstream tasks succeed**
* Failures automatically stop the pipeline

---

## 🧠 Best Practices Reinforced

* Keep notebooks **stateless and parameterized**
* Use Jobs for orchestration, not notebooks
* Never hardcode environment-specific values
* Validate upstream data before running downstream layers
* Let workflows manage dependencies and retries

---

## ✅ Final Outcome

✔ Parameterized notebooks using widgets
✔ Layer-based execution logic implemented
✔ Multi-task workflow designed
✔ Task dependencies enforced
✔ Job scheduling configured
✔ Foundation for production automation established

