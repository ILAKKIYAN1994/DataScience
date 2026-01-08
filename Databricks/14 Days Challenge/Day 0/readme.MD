# Databricks Day 0 – Setup & Data Loading (E-Commerce Dataset)

## 📌 Overview

This repository contains a **Databricks notebook (`.ipynb`)** that completes **Day 0 setup** for working with Databricks using a real-world **e-commerce behavior dataset**.

The notebook focuses on:

* Environment setup
* Kaggle integration
* Data ingestion using Databricks Volumes
* Schema and data validation

This acts as the **foundation before starting analytics and data engineering workflows**.

---

## 🔗 Important Links

* **Databricks Community Edition (Free Signup)**
  [https://www.databricks.com/try-databricks](https://www.databricks.com/try-databricks)

* **Dataset Overview (Kaggle)**
  [https://www.kaggle.com/code/mkechinov/ecommerce-data-overview](https://www.kaggle.com/code/mkechinov/ecommerce-data-overview)

---

## 🎯 End Goal

By the end of this notebook, you will have:

* A working Databricks Community Edition workspace
* Kaggle dataset downloaded directly inside Databricks
* October and November 2019 data loaded into Spark DataFrames
* Verified schema, row counts, and sample records

---

## 📂 Repository Contents

* `Day0_Databricks_Setup.ipynb`
  → Complete Day 0 setup notebook

---

## 🧩 Step-by-Step Execution Guide

---

### **Step 1: Create Databricks Account**

1. Go to [https://www.databricks.com/try-databricks](https://www.databricks.com/try-databricks)
2. Sign up for **Databricks Community Edition**
3. Verify your email and log in
4. Create a cluster using **default settings**

---

### **Step 2: Get Kaggle API Credentials**

1. Log in to [https://www.kaggle.com](https://www.kaggle.com)
2. Click your profile picture → **Account**
3. Scroll to **API** section
4. Click **Create New API Token**
5. Download `kaggle.json`
6. Note your:

   * `username`
   * `key`

---

### **Step 3: Import Notebook into Databricks**

1. Open your Databricks workspace
2. Navigate to **Workspace → Users → Your Name**
3. Click **Import**
4. Upload `Day0_Databricks_Setup.ipynb`
5. Attach the notebook to a running cluster

---

## 🧪 Notebook Execution (Run Cells in Order)

---

### **1️⃣ Install Dependencies**

```python
!pip install kaggle
```

---

### **2️⃣ Configure Kaggle Credentials**

```python
import os

os.environ["KAGGLE_USERNAME"] = "your_username"
os.environ["KAGGLE_KEY"] = "your_key"

print("Kaggle credentials configured!")
```

---

### **3️⃣ Create Database Schema**

```python
spark.sql("""
CREATE SCHEMA IF NOT EXISTS workspace.ecommerce
""")
```

---

### **4️⃣ Create Volume for Data Storage**

```python
spark.sql("""
CREATE VOLUME IF NOT EXISTS workspace.ecommerce.ecommerce_data
""")
```

---

### **5️⃣ Download Dataset from Kaggle**

```python
%sh
cd /Volumes/workspace/ecommerce/ecommerce_data
kaggle datasets download -d mkechinov/ecommerce-behavior-data-from-multi-category-store
```

---

### **6️⃣ Extract Downloaded Dataset**

```python
%sh
cd /Volumes/workspace/ecommerce/ecommerce_data
unzip -o ecommerce-behavior-data-from-multi-category-store.zip
ls -lh
```

---

### **7️⃣ Clean Up Zip File**

```python
%sh
cd /Volumes/workspace/ecommerce/ecommerce_data
rm -f ecommerce-behavior-data-from-multi-category-store.zip
ls -lh
```

---

### **8️⃣ Restart Python Environment**

```python
%restart_python
```

---

### **9️⃣ Load November 2019 Data**

```python
df_n = spark.read.csv(
    "/Volumes/workspace/ecommerce/ecommerce_data/2019-Nov.csv"
)
```

---

### **🔟 Load October 2019 Data**

```python
df = spark.read.csv(
    "/Volumes/workspace/ecommerce/ecommerce_data/2019-Oct.csv"
)
```

---

### **1️⃣1️⃣ Display Dataset Statistics and Schema**

```python
print(f"October 2019 - Total Events: {df.count():,}")
print("\n" + "="*60)
print("SCHEMA:")
print("="*60)
df.printSchema()
```

---

### **1️⃣2️⃣ Display Sample Data**

```python
print("\n" + "="*60)
print("SAMPLE DATA (First 5 rows):")
print("="*60)
df.show(5, truncate=False)
```

---

## 📊 Dataset Size Reference

| File         | Events | Approx. Size |
| ------------ | ------ | ------------ |
| 2019-Oct.csv | ~4.2M  | ~1.1 GB      |
| 2019-Nov.csv | ~9.3M  | ~2.2 GB      |
| **Total**    | ~13.5M | ~3.3 GB      |

---

## ✅ Final Outcome

✔ Databricks Community Edition configured
✔ Kaggle API integrated successfully
✔ Dataset downloaded and extracted inside Databricks
✔ CSV files loaded into Spark DataFrames
✔ Schema and sample data verified

This completes **Day 0 – Databricks Setup & Data Loading**.

---

## 🔮 Next Steps

* Apply explicit schema definitions
* Convert CSV files to Delta tables
* Build bronze / silver / gold layers
* Perform aggregations and transformations
* Optimize Spark performance

---

## 📌 Notes

* Resource limits apply in Databricks Community Edition
* Notebook is intended for **learning and experimentation**
* October data is recommended for faster iteration

