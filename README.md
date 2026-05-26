# Smart Manufacturing Analytics Platform

## Project Overview

The Smart Manufacturing Analytics Platform is an end-to-end data engineering and analytics solution built using Snowflake for managing, processing, transforming, and analyzing manufacturing data in real time.

The project focuses on:

* Real-time manufacturing data ingestion
* Automated ETL pipelines
* Stream and task-based processing
* Dimensional data modeling
* Dynamic analytics generation
* Governance and access control
* Production and defect analytics dashboards

This project simulates a modern Industry 4.0 manufacturing analytics environment where factory operations, machine performance, production batches, maintenance logs, and quality alerts are continuously monitored.

---

# Technology Stack

* Snowflake
* Snowpipe
* Streams
* Tasks
* Dynamic Tables
* Materialized Views
* AWS S3
* SQL
* Power BI

---

# Project Architecture

The architecture follows a layered data engineering approach:

1. RAW Layer
2. CORE Layer
3. MART Layer
4. Analytics & Reporting Layer

---

# Workflow of the Project

```text
                  +----------------+
                  |   AWS S3 Data  |
                  | CSV / JSON     |
                  +--------+-------+
                           |
                           v
                 +---------+---------+
                 |      STAGES       |
                 | CSV_STAGE         |
                 | JSON_STAGE        |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |     SNOWPIPE      |
                 | Auto Data Ingest  |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |     RAW LAYER     |
                 | Raw Tables        |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |      STREAMS      |
                 | Change Tracking   |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |       TASKS       |
                 | Data Processing   |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |     CORE LAYER    |
                 | Cleaned Data      |
                 +---------+---------+
                           |
                           v
                 +---------+---------+
                 |     FACT & DIM    |
                 | Star Schema       |
                 +---------+---------+
                           |
                           v
          +----------------+----------------+
          |                                 |
          v                                 v
+---------+---------+            +----------+----------+
| Dynamic Tables    |            | Materialized Views |
| Real-time Metrics |            | Optimized Queries  |
+---------+---------+            +----------+----------+
          |                                 |
          +----------------+----------------+
                           |
                           v
                 +---------+---------+
                 | Power BI Dashboard|
                 | Manufacturing KPI |
                 +-------------------+
```

---

# Project Modules

## 1. Environment Setup

File:

* `01_SET_UP.sql`

This script creates:

* Snowflake Warehouse
* Database
* Schemas

### Created Components

### Warehouse

* FACTORY_ANALYTICS_WH

### Database

* FACTORY_ANALYTICS_DB

### Schemas

* RAW
* CORE
* MART

---

# 2. RAW Layer

File:

* `RAW.sql`

The RAW layer stores incoming unprocessed data directly from source systems.

## RAW Tables

### RAW_FACTORY

Stores factory information.

### RAW_MACHINE

Stores machine details.

### RAW_PRODUCTION_BATCH

Stores production transaction data.

### RAW_MAINTENANCE_LOG

Stores maintenance records.

### RAW_QUALITY_ALERT

Stores JSON-based quality alerts.

---

# 3. File Formats and Stages

The project supports:

* CSV ingestion
* JSON ingestion

## File Formats

### CSV_FILE_FORMAT

Used for CSV manufacturing datasets.

### JSON_FILE_FORMAT

Used for quality alert JSON files.

## Stages

### CSV_STAGE

Loads CSV data from AWS S3.

### JSON_STAGE

Loads JSON files from AWS S3.

---

# 4. Snowpipe Automation

File:

* `SNOWPIPES.sql`

Snowpipe is used for automated continuous ingestion.

## Created Pipes

### PIPE_FACTORY_NEW

Loads factory data.

### PIPE_MACHINE_NEW

Loads machine data.

### PIPE_PRODUCTION_BATCH_NEW

Loads production batch data.

### PIPE_MAINTENANCE_LOG_NEW

Loads maintenance logs.

### PIPE_QUALITY_ALERT_NEW

Loads quality alerts.

## Features

* AUTO_INGEST enabled
* Real-time ingestion
* Pattern-based file loading
* Supports CSV and JSON

---

# 5. Streams

File:

* `STREAMS.sql`

Streams capture CDC (Change Data Capture) changes from RAW tables.

## Streams Created

* STREAM_FACTORY
* STREAM_MACHINE
* STREAM_PRODUCTION_BATCH
* STREAM_MAINTENANCE_LOG
* STREAM_QUALITY_ALERT

## Benefits

* Incremental processing
* Real-time tracking
* Efficient pipeline execution

---

# 6. Task Automation

File:

* `TASK.sql`

Tasks automate transformation logic from RAW to CORE.

## Tasks Created

### TASK_FACTORY

Processes factory records.

### TASK_MACHINE

Processes machine records.

### TASK_PRODUCTION_BATCH

Calculates:

* Good Units
* Defect Percentage
* Efficiency Percentage
* Batch Status

### TASK_MAINTENANCE_LOG

Processes maintenance analytics.

### TASK_QUALITY_ALERT

Processes quality alert information.

## Automation Features

* Runs every 1 minute
* Scheduled processing
* Fully automated ETL
* Incremental loading

---

# 7. CORE Layer

File:

* `CORE.sql`

The CORE layer stores cleaned and transformed business-ready data.

## CORE Tables

### CORE_FACTORY

Master factory data.

### CORE_MACHINE

Machine master information.

### CORE_PRODUCTION_BATCH

Production KPIs and transformed metrics.

### CORE_MAINTENANCE_LOG

Machine maintenance analytics.

### CORE_QUALITY_ALERT

Quality monitoring data.

## Derived Metrics

The project calculates:

### Good Units

```sql
QUANTITY_PRODUCED - DEFECT_COUNT
```

### Defect Percentage

```sql
(DEFECT_COUNT / QUANTITY_PRODUCED) * 100
```

### Efficiency Percentage

```sql
(GOOD_UNITS / QUANTITY_PRODUCED) * 100
```

---

# 8. MART Layer

Files:

* `MART.sql`
* `FACT.sql`

The MART layer implements dimensional modeling for analytics.

## Dimension Tables

### DIM_FACTORY

Factory dimension.

### DIM_MACHINE

Machine dimension.

### DIM_DATE

Date dimension.

## Fact Table

### FACT_PRODUCTION

Stores measurable manufacturing KPIs.

## Star Schema Design

The architecture follows a star schema:

```text
           DIM_FACTORY
                 |
                 |
DIM_MACHINE --- FACT_PRODUCTION --- DIM_DATE
```

---

# 9. Views

File:

* `VIEWS.sql`

Views simplify analytical querying.

## Analytical Views

### VW_MAINTENANCE_ANALYTICS

Maintenance insights.

### VW_QUALITY_ALERT_ANALYTICS

Quality monitoring.

### VW_PRODUCTION_EFFICIENCY

Production efficiency analytics.

### VW_DEFECT_ANALYSIS

Defect trend analysis.

### VW_FACTORY_PERFORMANCE

Factory-level performance reporting.

---

# 10. Materialized Views

File:

* `MATERIALIZED_VIEWS.sql`

Materialized views improve query performance.

## Materialized Views

### MV_PRODUCTION_SUMMARY

Aggregated production metrics.

### MV_MACHINE_PERFORMANCE

Machine-level KPI summary.

## Advantages

* Faster reporting
* Reduced query cost
* Precomputed aggregations
* Optimized dashboard performance

---

# 11. Dynamic Tables

File:

* `DYNAMIC TABLES.sql`

Dynamic tables provide continuously updated analytics.

## Dynamic Tables

### DT_FACTORY_ANALYTICS

Factory KPI tracking.

### DT_MACHINE_PERFORMANCE

Machine efficiency tracking.

### DT_DEFECT_ANALYTICS

Defect monitoring analytics.

## Features

* Auto-refresh
* Near real-time analytics
* Low maintenance
* Continuous updates

---

# 12. Governance and Security

File:

* `GOVERNANCE.sql`

Role-based access control is implemented.

## Roles Created

### ADMIN_ROLE

Full access.

### ANALYST_ROLE

Read-only analytical access.

### QUALITY_MANAGER_ROLE

Quality monitoring access.

### FACTORY_MANAGER_ROLE

Factory operations access.

## Security Features

* Warehouse-level access
* Database-level access
* Schema-level access
* Role-based permissions

---

# Data Flow Summary

## Step 1

Manufacturing data files are uploaded to AWS S3.

## Step 2

Snowflake stages access the S3 bucket.

## Step 3

Snowpipe automatically ingests files.

## Step 4

Data lands in RAW tables.

## Step 5

Streams capture incremental changes.

## Step 6

Tasks process and transform data.

## Step 7

CORE tables store cleaned business data.

## Step 8

FACT and DIM tables support analytics.

## Step 9

Dynamic tables and materialized views optimize reporting.

## Step 10

Power BI dashboards visualize KPIs.

---

# KPIs Generated

The platform generates several manufacturing KPIs:

* Total Production
* Total Defects
* Good Units
* Efficiency Percentage
* Defect Percentage
* Machine Performance
* Factory Performance
* Maintenance Status
* Quality Alert Tracking
* Downtime Analysis

---

# Power BI Dashboard Suggestions

## Dashboard 1: Factory Overview

Visuals:

* KPI Cards
* Production Trends
* Factory Performance
* Defect Analysis
* Map Visualization

## Dashboard 2: Machine Analytics

Visuals:

* Machine Efficiency
* Downtime Tracking
* Maintenance Analysis
* Key Influencer Visual

## Dashboard 3: Quality Monitoring

Visuals:

* Quality Score Analysis
* Alert Priority Distribution
* Defect Trends
* Batch Status Analytics

---

# Key Features of the Project

* Real-time ingestion
* Automated ETL pipelines
* Incremental processing
* Dynamic analytics
* Star schema design
* Real-time KPI tracking
* Snowflake governance
* Power BI integration
* Manufacturing intelligence

---

# Business Benefits

## Operational Benefits

* Faster decision making
* Automated monitoring
* Improved manufacturing efficiency
* Reduced downtime
* Better quality control

## Technical Benefits

* Scalable architecture
* Real-time data processing
* Optimized query performance
* Secure access management
* Cloud-native analytics

---

# Future Enhancements

Potential future improvements:

* Predictive maintenance using ML
* IoT sensor integration
* Real-time anomaly detection
* AI-based defect prediction
* Streaming dashboards
* Alert notification systems
* Advanced forecasting models

---

# Execution Order

Run the SQL files in the following order:

1. `01_SET_UP.sql`
2. `RAW.sql`
3. `SNOWPIPES.sql`
4. `STREAMS.sql`
5. `CORE.sql`
6. `TASK.sql`
7. `MART.sql`
8. `FACT.sql`
9. `VIEWS.sql`
10. `MATERIALIZED_VIEWS.sql`
11. `DYNAMIC TABLES.sql`
12. `GOVERNANCE.sql`

---

# Conclusion

This project demonstrates a complete enterprise-grade Smart Manufacturing Analytics architecture using Snowflake. The solution supports automated ingestion, real-time processing, dimensional modeling, analytics optimization, governance, and dashboard reporting.

The architecture is scalable, modular, and suitable for modern manufacturing analytics use cases in Industry 4.0 environments.
