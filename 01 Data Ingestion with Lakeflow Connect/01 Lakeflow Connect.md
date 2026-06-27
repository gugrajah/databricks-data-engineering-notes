
### Databricks Lakeflow

Lakeflow provides a unified platform for data ingestion, transformation,a nd orchestration, and has the following components:
* Lakeflow Connect: A set of efficient ingestion connctors
* Lakelflow Declarative Pipelines: framework for building batch and streaming data pipelines
* Lakelflow Jobs: workflow automation tool


Built on top of unified governance with Unity Catalog. UC is a centralized data catalog that provides access control, auditing, data lineage, quality monitoring, and data discovery across Databricks workspaces.

Key benefits of using Lakeflow Connect (LfC):
* managed and efficient solution 
* self-service interface
* unified observability and governance 

#### LFC - Connectors Overview

* Upload files
* Standard connectors:
    - Supported sources: Kafka, Cloud object storage, various other sources
* Ingestion Methods
    - Batch
    - incremental batch
    - streaming
* Managed connectors: Leverages efficient **incremental reads** and **writes**. Purpose-built for ingesting data from enterprise applications
    - SaaS applications
    - databases

#### Ingestion Methods

* Batch ingestion loads data as batches of rows into Databricks, often based on a schedule
    - _CREATE TABLE AS SELECT (CTAS)_
    - _spark.read.load()_
* Incremental batch
    - only new data is ingested
    - provides faster & more **resource efficient** ingestion by processing less data
    - _COPY INTO_,_spark.read.Stream_ (Auto Loader with timed trigger)
    - Declarative pipelines (_CREATE OR REFRESH STREAMING TABLE_)
* Streaming
    - _continuously load data_ rows or batches of data as it is generated so it can be queriesd as it **arrives in near real-time**
    - _spark.readStream_ (Autoloader with continuous trigger)
    - Declarative pipelines (trigger mode continuous)