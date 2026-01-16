# Databricks Day 9 – SQL Analytics & Dashboards

## 📌 Overview

This repository contains the **Day 9 Databricks Challenge notebook**, focused on **SQL-based analytics and dashboard creation** using Databricks SQL.

Day 9 shifts the focus from data engineering pipelines to **data consumption and business analytics**, demonstrating how cleaned and curated datasets can be transformed into **insights, visualizations, and dashboards**.

The notebook and SQL workspace activities cover analytical queries, visualizations, filters, and dashboard design.

---

## 🎯 Learning Objectives

By completing Day 9, you will be able to:

* Understand the role of **SQL Warehouses** in Databricks
* Write **complex analytical SQL queries**
* Perform **time-series, aggregation, and funnel analysis**
* Build interactive **dashboards**
* Add **filters and parameters** to SQL queries
* Understand dashboard refresh and scheduling concepts

---

## 📂 Repository Contents

* `Day_9_DB_Challenge.ipynb`
  → Notebook documenting SQL analytics logic and learning notes

---

## 🧩 Prerequisites

Before running this notebook and building dashboards:

* Complete **Day 0 – Day 8 Databricks Challenges**
* Use **Databricks Community Edition**
* Ensure Silver and Gold Delta tables exist:

  * `silver.events`
  * `gold.products`
* Data should be clean, validated, and analytics-ready

---

## 🧠 Key Concepts Covered

### 🔹 SQL Warehouses

* SQL Warehouses are optimized compute resources for analytics and BI workloads
* In Community Edition, SQL queries run on default SQL compute
* In enterprise editions, warehouses can be sized and scheduled independently

---

### 🔹 Analytical SQL Queries

Day 9 focuses on business-driven analytics:

* Revenue trends over time
* Funnel analysis (view → purchase)
* Top-performing products
* Aggregations and grouping
* Time-based analysis

---

### 🔹 Dashboards

* Dashboards are collections of saved SQL visualizations
* Each visualization is backed by a SQL query
* Dashboards enable sharing insights with stakeholders

---

### 🔹 Filters & Parameters

* Filters make dashboards interactive
* Parameters allow dynamic query execution
* In Databricks SQL, parameters are defined in the UI and referenced using `{{parameter_name}}`

---

## 🛠️ Hands-on Tasks

---

### 1️⃣ Revenue Trend Analysis

```sql
SELECT
  date(event_time) AS event_date,
  SUM(price) AS daily_revenue
FROM silver.events
WHERE event_type = 'purchase'
GROUP BY event_date
ORDER BY event_date;
```

Used for:

* Line charts
* Revenue trend dashboards

---

### 2️⃣ Funnel Analysis (View → Purchase)

```sql
SELECT
  COUNT(DISTINCT CASE WHEN event_type = 'view' THEN user_id END) AS views,
  COUNT(DISTINCT CASE WHEN event_type = 'purchase' THEN user_id END) AS purchases,
  ROUND(
    COUNT(DISTINCT CASE WHEN event_type = 'purchase' THEN user_id END) * 100.0 /
    COUNT(DISTINCT CASE WHEN event_type = 'view' THEN user_id END),
    2
  ) AS conversion_rate
FROM silver.events;
```

Used for:

* Funnel metrics
* Conversion analysis

---

### 3️⃣ Top Products by Revenue

```sql
SELECT
  product_id,
  SUM(price) AS revenue
FROM silver.events
WHERE event_type = 'purchase'
GROUP BY product_id
ORDER BY revenue DESC
LIMIT 10;
```

Used for:

* Leaderboards
* Product performance analysis

---

### 4️⃣ Parameterized Revenue Query (Dashboard Use)

```sql
SELECT
  date(event_time) AS event_date,
  SUM(price) AS revenue
FROM silver.events
WHERE event_type = 'purchase'
AND event_time BETWEEN {{start_date}} AND {{end_date}}
GROUP BY event_date
ORDER BY event_date;
```

Parameters (`start_date`, `end_date`) are defined in the **SQL UI** before execution.

---

## 📊 Dashboard Creation Steps

1. Open **SQL Editor**
2. Run analytical queries
3. Click **Add visualization**
4. Choose chart types:

   * Line → Revenue trend
   * Bar → Top products
   * Table / KPI → Funnel metrics
5. Save visualizations
6. Create a dashboard and add visuals
7. Add filters (date range, product)

---

## 🧪 Verification & Validation

```sql
SELECT COUNT(*) FROM silver.events;
SELECT COUNT(*) FROM gold.products;
```

Confirm that:

* Queries return expected results
* Visualizations update correctly
* Filters dynamically change outputs

---

## ⚠️ Community Edition Notes

| Feature                  | Community Edition |
| ------------------------ | ----------------- |
| SQL Editor               | ✅                 |
| Dashboards               | ✅                 |
| Parameters & filters     | ✅                 |
| Scheduled refresh        | ⚠️ Limited        |
| Dedicated SQL Warehouses | ❌                 |

Dashboards work fully for learning, but enterprise features are limited.

---

## 🧠 Key Takeaways

* SQL Warehouses power analytics workloads
* Gold and Silver tables drive business insights
* Dashboards turn queries into decisions
* Filters and parameters enable dynamic analysis
* SQL is central to analytics in the Lakehouse

---

## ✅ Final Outcome

✔ Analytical SQL queries written
✔ Revenue, funnel, and product insights created
✔ Dashboards built with multiple visualizations
✔ Filters and parameters implemented
✔ Strong foundation in SQL analytics established
