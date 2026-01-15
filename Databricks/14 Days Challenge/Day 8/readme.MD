Databricks Day 8 – Catalogs, Schemas, Tables & Governance Concepts
📌 Overview

This repository contains the Day 8 Databricks Challenge notebook, focused on understanding data organization, governance concepts, and controlled access patterns in Databricks.

Day 8 introduces the conceptual foundation of Catalog → Schema → Table/View hierarchy, along with hands-on practice adapted for Databricks Community Edition, where Unity Catalog is not fully supported.

The goal is to understand how data governance works in enterprise Databricks environments, while practicing what is technically possible in the Community Edition.

🎯 Learning Objectives

By completing Day 8, you will be able to:

Understand the Catalog → Schema → Table/View hierarchy

Learn the purpose of catalogs and schemas for data isolation

Differentiate between managed and external Delta tables

Understand access control concepts (GRANT / REVOKE)

Create schemas and register Delta tables in Community Edition

Create views for logical, controlled data access

Understand data lineage concepts at a high level

📂 Repository Contents

Day_8_DB_Challenge.ipynb
→ Notebook covering data organization, table registration, and governance concepts

🧩 Prerequisites

Before running this notebook:

Complete Day 0 – Databricks Setup

Complete Day 1–Day 7 (Spark, Delta, Lakehouse & Workflows)

Use Databricks Community Edition

Ensure Bronze, Silver, and Gold Delta datasets already exist:

/Volumes/workspace/default/challenge/bronze_events

/Volumes/workspace/default/challenge/silver_events

/Volumes/workspace/default/challenge/gold_products

🧠 Key Concepts Covered
🔹 Catalog → Schema → Table Hierarchy (Conceptual)
Catalog
 └── Schema (Database)
      └── Table / View


Catalog: Top-level organizational boundary (project/company level)

Schema: Logical grouping (bronze, silver, gold)

Table/View: Actual datasets and abstractions

📌 Note: Catalog-level governance is conceptual only in Community Edition.

🔹 Managed vs External Tables

Managed Tables

Databricks manages data and metadata

Dropping the table deletes the data

External Tables

Databricks manages only metadata

Data remains even if the table is dropped

Commonly used in Lakehouse architectures

In this challenge, Delta tables are registered using external storage paths via AS SELECT.

🔹 Access Control (Conceptual)

Enterprise Databricks (Unity Catalog) supports:

GRANT / REVOKE

Fine-grained table, schema, and column access

Community Edition does NOT support permissions

Access control is learned conceptually and via syntax only

🔹 Data Lineage (Conceptual)

Unity Catalog automatically tracks:

Source → Transformation → Consumption

Helps with:

Impact analysis

Auditing

Safe schema changes

📌 Lineage UI is not available in Community Edition.

🛠️ Hands-on Tasks (Community Edition Compatible)
1️⃣ Create Schemas (Databases)
CREATE DATABASE IF NOT EXISTS bronze;
CREATE DATABASE IF NOT EXISTS silver;
CREATE DATABASE IF NOT EXISTS gold;


These schemas represent the Bronze, Silver, and Gold layers.

2️⃣ Register Delta Tables (Legacy Metastore)
CREATE TABLE bronze.events
USING DELTA
AS
SELECT * FROM delta.`/Volumes/workspace/default/challenge/bronze_events`;

CREATE TABLE silver.events
USING DELTA
AS
SELECT * FROM delta.`/Volumes/workspace/default/challenge/silver_events`;

CREATE TABLE gold.products
USING DELTA
AS
SELECT * FROM delta.`/Volumes/workspace/default/challenge/gold_products`;


✔ Tables registered successfully
✔ Queryable using SQL
✔ Fully supported in Community Edition

3️⃣ Create Views for Controlled Access (Logical)
CREATE OR REPLACE VIEW gold.top_products AS
SELECT
  product_id,
  revenue,
  conversion_rate
FROM gold.products
WHERE purchases > 10
ORDER BY revenue DESC
LIMIT 100;


Views provide:

Column-level abstraction

Business-friendly datasets

Logical access control (not enforced security in CE)

🧪 Verification Queries
SHOW DATABASES;
SHOW TABLES IN bronze;
SHOW TABLES IN silver;
SHOW TABLES IN gold;

SELECT * FROM gold.products LIMIT 10;
SELECT * FROM gold.top_products LIMIT 10;

⚠️ Community Edition Limitations (Important)
Feature	Community Edition
Unity Catalog	❌ Not supported
CREATE CATALOG	❌
GRANT / REVOKE	❌
Fine-grained permissions	❌
Lineage UI	❌
Delta tables	✅
Schemas (databases)	✅
Views	✅

All governance features are learned conceptually and implemented practically later in paid Databricks editions.

🧠 Key Takeaways

Catalogs provide enterprise-level governance (conceptual in CE)

Schemas organize data into Bronze, Silver, and Gold layers

Delta tables can be registered using legacy metastore

Views offer abstraction and controlled exposure

Community Edition is ideal for learning architecture and syntax

Unity Catalog is required for real governance and security

✅ Final Outcome

✔ Understood data hierarchy concepts
✔ Created Bronze, Silver, and Gold schemas
✔ Registered Delta tables successfully
✔ Built views for controlled access
✔ Learned governance patterns used in real-world Databricks platforms
