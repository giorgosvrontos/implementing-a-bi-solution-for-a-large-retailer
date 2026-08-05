# 🛒 Large Retailer BI Solution - Medallion Data Architecture

## 📌 Project Overview
This project implements an automated, end-to-end Business Intelligence (BI) and Data Engineering solution for a large retail company. Utilizing a **Medallion Data Architecture** (Bronze, Silver, Gold), the pipeline ingests, cleans, and transforms raw Point of Sale (POS) and Superstore batch data into a structured Star Schema. The final data is served to **Metabase** for interactive dashboarding and business analytics.

## 🏗️ Data Architecture & Pipeline
The data pipeline is orchestrated using **Databricks Workflows** and is structured into three main layers, ensuring data quality and traceability at every step:

*   **🥉 Bronze Layer (Raw Data):** 
    *   Ingests batch CSV data (`superstore.csv_batch_ingestion.ipynb`).
    *   Simulates and loads streaming/incremental POS data (`pos_live_generator.ipynb`, `bronze_pos_load.ipynb`).
*   **🥈 Silver Layer (Cleansed & Conformed):**
    *   Applies strict Data Quality checks (`superstore.csv_data_quality_check.ipynb`, `pos_data_quality_check.ipynb`).
    *   Cleans, filters, and standardizes data schemas (`superstore.csv_bronze_to_silver.ipynb`, `silver_pos_sales.ipynb`).
*   **🥇 Gold Layer (Curated for BI):**
    *   Combines and aggregates data into a business-ready Star Schema (`silver_to_gold_layer.ipynb`).
    *   Generates targeted Data Marts for downstream BI consumption (`data_marts.ipynb`).

## 📊 BI & Analytics (Metabase)
The gold layer data is seamlessly connected to a self-hosted Metabase instance via Databricks Unity Catalog. 

<img width="1342" height="938" alt="Screenshot_650" src="https://github.com/user-attachments/assets/8e16d2cc-14fd-425d-a5bb-006baf56956f" />

## 🛠️ Technology Stack & Infrastructure
*   **Data Processing:** Databricks, Apache Spark (PySpark)
*   **Infrastructure as Code (IaC):** Databricks Asset Bundles (DABs)
*   **CI/CD:** GitHub Actions (Multi-branch GitOps deployment)
*   **Containerization:** Docker & Docker Compose (Metabase, and PostgreSQL)

## 🚀 CI/CD & Deployment Strategy (GitOps)
We employ a robust multi-environment GitOps strategy using **GitHub Actions**:
*   **Development Environments:** Pushes to feature branches (`vrontos-develop`, `Christakidis_Develop`) automatically trigger isolated deployments to dedicated personal Databricks workspaces (`dev_giorgos`, `dev_charis`). Schedules are paused in these development environments.
*   **Production Environment:** Merging to the `main` branch triggers a strict production deployment. The pipeline is scheduled to run automatically every night at 22:00 via the `medallion_job.yml` definition.

## 🗂️ Repository Structure
```text
├── .github/workflows/
│   └── databricks-ci-cd.yml          # GitHub Actions CI/CD pipeline definitions
├── metabase/
│   ├── .env                          # Environment variables & secrets (Database credentials)
│   └── docker-compose.yml            # Docker setup for Metabase & PostgreSQL locally
├── resources/
│   └── medallion_job.yml             # Databricks Workflow DAG & tasks configuration
├── .gitignore                        # Git ignore rules
├── databricks.yml                    # Databricks Asset Bundle (DAB) targets configuration
├── *.ipynb                           # PySpark Notebooks for Bronze, Silver, Gold transformations
└── README.md                         # Project documentation
```
## ⚙️ Prerequisites & Setup
To run or contribute to this project, you will need:
1. **Databricks CLI** installed and configured locally.
2. Docker Desktop installed on your machine.
3. Access to the GitHub repository and appropriate Databricks Service Principal tokens.

**Local BI Setup**:
The BI tool (Metabase) and its metadata database (PostgreSQL) are hosted locally using Docker volumes for data persistence. To start the services on your local machine, open your terminal and run:

```bash
cd metabase
docker-compose up -d
```
## 👥 Contributors
* **Giorgos Vrontos** - Data Engineer 
* **Charis Christakidis** - Data Engineer
