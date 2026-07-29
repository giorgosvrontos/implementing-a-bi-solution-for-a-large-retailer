# 🛒 Large Retailer BI Solution - Medallion Data Architecture

## 📌 Project Overview
This project implements an automated, end-to-end Business Intelligence (BI) and Data Engineering solution for a large retail company. Utilizing a **Medallion Data Architecture** (Bronze, Silver, Gold), the pipeline ingests, cleans, and transforms raw Point of Sale (POS) and Superstore batch data into a structured Star Schema. The final data is served to **Metabase** for interactive dashboarding and business analytics.

## 🏗️ Data Architecture & Pipeline
The data pipeline is orchestrated using **Databricks Workflows** and is structured into three main layers, ensuring data quality and traceability at every step:

*   **🥉 Bronze Layer (Raw Data):** 
    *   Ingests batch CSV data (`superstore.csv_batch_ingestion`).
    *   Simulates and loads streaming/incremental POS data (`pos_live_generator`, `bronze_pos_load`).
*   **🥈 Silver Layer (Cleansed & Conformed):**
    *   Applies strict Data Quality checks (`superstore.csv_data_quality_check`, `pos_data_quality_check`).
    *   Cleans, filters, and standardizes data schemas (`superstore.csv_bronze_to_silver`, `silver_pos_sales`).
*   **🥇 Gold Layer (Curated for BI):**
    *   Combines and aggregates data into a business-ready Star Schema (`silver_to_gold_layer`).
    *   Generates targeted Data Marts for downstream BI consumption (`data_marts`).

## 🛠️ Technology Stack & Infrastructure
*   **Data Processing:** Databricks, Apache Spark (PySpark)
*   **Infrastructure as Code (IaC):** Databricks Asset Bundles (DABs)
*   **CI/CD:** GitLab CI/CD (Multi-branch GitOps deployment)
*   **Cloud Infrastructure:** Hetzner Cloud Server (VPS)
*   **Containerization:** Docker & Docker Compose (Self-hosted GitLab Runner, Metabase, and PostgreSQL)

## 🚀 CI/CD & Deployment Strategy (GitOps)
We employ a robust multi-environment GitOps strategy using GitLab CI/CD, powered by a **self-hosted GitLab Runner** deployed on a **Hetzner server**:
*   **Development Environments:** Pushes to feature branches (`vrontos-develop`, `Christakidis_Develop`) automatically trigger isolated deployments to dedicated personal Databricks workspaces (`dev_giorgos`, `dev_charis`). Schedules are paused in these development environments.
*   **Production Environment:** Merging to the `main` branch triggers a strict production deployment. The pipeline is scheduled to run automatically every night at 22:00.

## 🗂️ Repository Structure
```text
├── resources/
│   └── medallion_job.yml             # Databricks Workflow DAG & tasks configuration
├── runner-metabase/
│   └── docker_compose.yml            # Docker setup for Metabase, PostgreSQL & GitLab Runner on Hetzner
├── .gitlab-ci.yml                    # CI/CD pipeline definitions
├── databricks.yml                    # Databricks Asset Bundle (DAB) targets configuration
├── *.ipynb                           # PySpark Notebooks for Bronze, Silver, Gold transformations
└── README.md
```
## ⚙️ Prerequisites & Setup
To run or contribute to this project, you will need:
1. **Databricks CLI** installed and configured.
2. Access to the GitLab repository and appropriate Databricks Service Principal tokens.

**Server Setup (Hetzner Infrastructure):**
The CI/CD runner and BI tools are hosted independently on a Hetzner Cloud Server. To initialize or update these background services via SSH:

```bash
cd runner-metabase
docker-compose up -d
```
## 👥 Contributors
* **Giorgos Vrontos** - Data Engineer 
* **Charis Christakidis** - Data Engineer