# GCP_Healthcare_project

# 🏥 GCP Healthcare Revenue Cycle Management (RCM) – Data Engineering Project

## Project Overview

This project implements an end-to-end data lake and analytics pipeline on Google Cloud Platform (GCP) for the Healthcare Revenue Cycle Management (RCM) domain.

The objective is to ingest, standardize, and transform healthcare data from multiple source systems—Electronic Medical Records (EMR), insurance claims, and reference datasets—and make it analytics-ready using a Bronze → Silver → Gold (Medallion) architecture.

The project is designed to reflect real-world enterprise healthcare data engineering systems, including:
- Multi-hospital source isolation
- Metadata-driven ingestion
- Audit and observability
- CI/CD for orchestration
- Spark + BigQuery based processing

---

## What is Revenue Cycle Management (RCM)?

Revenue Cycle Management (RCM) tracks the financial lifecycle of a patient visit:
1. Patient registration and insurance verification  
2. Medical services (encounters, procedures)  
3. Billing generation  
4. Insurance claim submission and review  
5. Payments, denials, and follow-ups  
6. Revenue tracking and optimization  

This project focuses on building the data pipelines that enable reporting and analytics on these financial workflows.

---

## Architecture Overview

Cloud SQL (EMR) + Flat Files (Claims) + Reference Data  
→ Dataproc (PySpark Ingestion)  
→ GCS Landing Zone  
→ BigQuery Bronze  
→ BigQuery Silver (CDM + SCD Type 2)  
→ BigQuery Gold (KPIs & Reporting)

Orchestration: Cloud Composer (Apache Airflow)  
CI/CD: GitHub + Cloud Build  

---

## Data Sources

### EMR (Electronic Medical Records)

Two independent hospital systems are modeled:
- Hospital A → hospital_a_db
- Hospital B → hospital_b_db

Each hospital contains:
- patients
- providers
- departments
- encounters
- transactions

Sample EMR CSV files and DDL scripts are stored in the repository under `data/EMR/` for development and source initialization.

---

### Claims Data

Insurance claims are delivered as monthly flat files, one per hospital.
Claims include billing and payment outcomes such as:
- claim status
- billed vs paid amount
- denial reasons
- payer information

Sample files are stored under `data/claims/`.

---

### Reference Data

Reference datasets used for standardization:
- CPT Codes
- ICD Codes
- NPI Codes

These are modeled as shared master data and stored under `data/cptcodes/`.

---

## Medallion Architecture

Bronze Layer:
- Raw ingestion
- Minimal transformation
- Source-aligned schemas

Silver Layer:
- Common Data Model (CDM)
- Data cleansing and deduplication
- SCD Type 2 for historical tracking

Gold Layer:
- Aggregated, analytics-ready tables
- Revenue and claims KPIs
- Reporting and dashboards

---

## Metadata-Driven Ingestion

Ingestion behavior is controlled using configuration files:
- `load_config.csv` defines source systems, tables, load type (full/incremental), keys, and targets.
- This eliminates hardcoded logic and improves scalability.

---

## Audit & Monitoring

A BigQuery audit table tracks:
- pipeline execution status
- row counts
- timestamps
- error messages

This provides observability and troubleshooting capabilities.

---

## Orchestration (Cloud Composer)

- parent_dag.py → Controls overall workflow
- pyspark_dag.py → Submits Dataproc Spark ingestion jobs
- bq_dag.py → Executes BigQuery SQL (Bronze → Silver → Gold)

Business logic is kept outside DAGs for clean orchestration.

---

## CI/CD for DAG Deployment

- Code is pushed to GitHub
- Cloud Build trigger runs
- Utility script syncs DAGs into Cloud Composer
- No manual DAG uploads required

---

## Repository Structure

data/
- EMR/            → Sample EMR data + DDL
- ingestion/      → PySpark ingestion jobs
- claims/         → Insurance claim flat files
- cptcodes/       → CPT reference data
- configs/        → Metadata & audit configs
- BQ/             → BigQuery SQL (Bronze/Silver/Gold)

workflows/        → Airflow DAGs  
utils/            → Deployment utilities  
infra/            → Cloud Build configuration  

All data included is sample/demo data only.

---

## Key Skills Demonstrated

- GCP Data Engineering (GCS, BigQuery, Dataproc, Composer)
- Spark-based ingestion pipelines
- Healthcare RCM domain modeling
- Medallion architecture
- SCD Type 2 implementation
- Metadata-driven design
- CI/CD for data pipelines
- Audit and observability

---

## Future Enhancements

- Real-time ingestion using Pub/Sub
- Automated data quality checks
- Partitioning and clustering optimizations
- BI dashboards (Looker / Tableau)
