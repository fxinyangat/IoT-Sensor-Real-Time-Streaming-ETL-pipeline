# NYC Bridge Structural Monitoring Pipeline

## Overview

This project implements a Lakeflow Declarative Pipelines (formerly Delta Live Tables) solution to monitor the structural health of five bridges in New York City using IoT sensor data. 
The pipeline ingests, processes, and aggregates real-time sensor streams, providing actionable insights via a live dashboard.
## Purpose

This solution enables proactive monitoring and rapid response to structural anomalies, improving safety and operational efficiency for NYC bridges.

## Architecture

<img width="1268" height="891" alt="image" src="https://github.com/user-attachments/assets/ea9123b8-398c-493c-8913-745f11d1a454" />

- **Data Ingestion**: IoT sensors stream temperature, vibration, and tilt data every minute. Raw data is stored in Delta tables within dedicated volumes.
- **Medallion ETL Pipeline**:
  - **Bronze Layer**: Raw sensor streams are ingested into three separate streaming tables for temperature, vibration, and tilt.
  - **Silver Layer**: Data is cleaned, joined, and enriched for further analysis. Data quality checks are enforced using Expectations, where corrupt data is either quarantined or dropped.
  - **Gold Layer**: Aggregated metrics are computed in 10-minute intervals with a 2-minute watermark to ensure timely and accurate reporting.
- **Orchestration**: The entire ETL process is orchestrated using Databricks Jobs, enabling concurrent execution and automated scheduling.
- **Dashboard**: Aggregated gold-layer data is fed into a live dashboard for real-time monitoring and visualization of bridge health metrics.

## Pipeline Components

1. **Bronze Streaming Tables**  
   - `bridge_temperature`: Raw temperature readings  
   - `bridge_vibration`: Raw vibration readings  
   - `bridge_tilt`: Raw tilt readings

2. **Silver Table(s)**  
   - Cleansed, quality checked, and joined sensor data, ready for aggregation.
   - `bridge_metadata`: Static table showing data about the five bridges e.g location, year of construction, bridge type, length, width, and total area.

3. **Gold Table**  
   - Aggregates sensor metrics per bridge in 10-minute windows, using a 2-minute watermark for late data handling.

4. **Dashboard**  
   - Visualizes live gold-layer metrics for all five bridges.
   <img width="1750" height="891" alt="image" src="https://github.com/user-attachments/assets/04ad7b32-f691-469b-92f2-e941540fceb1" />


## Monitoring & Observability

- Streaming metrics (backpressure, throughput, latency, etc.) are tracked via the Lakeflow Declarative Pipelines UI.
- Event logs and lineage are available for auditing, troubleshooting, and data quality assurance.
- Cluster utilization and cost metrics help optimize resource usage and budget.

## Orchestration

- The pipeline is scheduled and managed using Databricks Jobs.
- Each pipeline run processes new sensor data and updates all downstream tables and the dashboard.

## Getting Started

1. **Deploy IoT Sensors**: Ensure sensors are streaming data to the designated Delta volumes.
2. **Configure Pipeline**: Set up Lakeflow Declarative Pipelines with the provided Python code, following the established import and decorator conventions.
3. **Schedule Jobs**: Use Databricks Jobs to orchestrate pipeline runs.
4. **Monitor & Visualize**: Access the dashboard for real-time bridge health insights and use the Lakeflow UI for pipeline monitoring.

## Technologies Used

- Lakeflow Declarative Pipelines (DLT)
- Delta Lake
- Databricks Jobs
- Databricks Dashboard
- Spark Structured Streaming


