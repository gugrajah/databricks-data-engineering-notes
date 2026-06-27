# Databricks Data Engineering Notes

Welcome to this repository for learning Databricks Data Engineering! This repository contains structured study notes, concepts, and code snippets focusing on modern data engineering workflows in Databricks.

---

## 📚 Table of Contents

### 📂 [Module 1: Data Ingestion with Lakeflow Connect]
Core methods for ingesting, structure management, and loading data into the Lakehouse.
*   📄 [01 Lakeflow Connect.md]
    *   *Topics*: Lakeflow Connect components, Connectors overview, Batch vs. Incremental Batch vs. Streaming ingestion methods.
*   📄 [02 Delta Lake review.md]
    *   *Topics*: Delta Lake core concepts, ACID transactions, time travel, schema enforcement, Bronze/Silver/Gold layers, Delta logs, and Parquet storage.
*   📄 [03 Cloud Storage Ingestion.md]
    *   *Topics*: Ingestion using standard connectors via `CREATE TABLE AS SELECT (CTAS)`, `COPY INTO`, and `Auto Loader`. Includes metadata column injection, rescued data column management, and ingesting semi-structured JSON data (using `STRING`, `STRUCT` with `schema_of_json` & `from_json`, and `VARIANT` types).
*   📄 [04 Ingesting Enterprise Data with Lakeflow Connect.md]
    *   *Topics*: Overview of managed connectors and Partner Connect for streamlined enterprise ingestion.

---

### 📂 [Module 2: Deploy Workloads with Lakeflow Jobs]
*Work in progress. Focuses on orchestrating and scheduling batch and streaming jobs in Databricks.*

---

### 📂 [Module 3: Build Data Pipelines]
*Work in progress. Explores Declarative Pipelines, streaming tables, and materialized views.*

---

### 📂 [Module 4: DevOps Essential for Data Engineering]
*Work in progress. Details CI/CD, Git integration, and deployment practices on Databricks.*
