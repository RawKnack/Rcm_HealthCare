# 🏥 RCM_HealthCare – Scalable Data Engineering Pipeline

A robust, metadata-driven Data Engineering pipeline tailored for Revenue Cycle Management (RCM) in the healthcare domain. Built on Azure Databricks using Delta Lake and Medallion Architecture for scalable, incremental, and modular data processing.

---

## 🚀 Overview

This project streamlines the processing of healthcare revenue cycle data through a multi-layered, metadata-controlled pipeline. The architecture is modular and designed to support:

- **Landing ➝ Bronze ➝ Silver ➝ Gold** transformation layers (Medallion Architecture)  
- **Metadata-driven execution** using configuration flags (active/inactive)  
- **Incremental data ingestion and transformation**  
- **Audit & logging support** for observability and troubleshooting  

---

## 🏗️ Architecture

The pipeline follows the **Medallion Architecture** pattern for progressive data refinement:

     ┌───────────┐
     │  Landing  │ ◄── Raw files (CSV, JSON, Parquet, etc.)
     └────┬──────┘
          ▼
    ┌────────────┐
    │   Bronze   │ ◄── Basic validation, schema enforcement
    └────┬───────┘
          ▼
    ┌────────────┐
    │   Silver   │ ◄── Enrichment, joins, business logic
    └────┬───────┘
          ▼
    ┌────────────┐
    │   Gold     │ ◄── Aggregated, analytics-ready data
    └────────────┘

Each layer increases data reliability and usability while keeping the pipeline modular and auditable.

---

## 🔧 Tech Stack

- **Azure Databricks**  
- **Delta Lake**  
- **PySpark**  
- **Azure Data Lake Storage Gen2**  
- **Azure Key Vault** (for secret management)  
- **Unity Catalog** (for secure governance)

---

## 📁 Project Structure

## ▶️ Notebook Execution Flow

Most transformation notebooks are invoked through Azure Data Factory (ADF). As a user, you only need to run the setup and API extract notebooks manually. The rest of the pipeline is triggered and orchestrated automatically via ADF.

### 1. 🧱 Manual Setup (Pre-requisite)

| Notebook                        | Description                                        |
|----------------------------------|----------------------------------------------------|
| `2. adls_mount`                | Mounts the ADLS Gen2 containers to `/mnt/*`        |
| `ICD Code API extract`         | Pulls ICD reference data from public API           |
| `NPI API extract`              | Pulls NPI registry data from public API            |

### 2. ⚙️ ADF Orchestrated Pipeline

The following notebooks are triggered automatically via **Azure Data Factory pipelines**:

- **Pipeline: `source_to_bronze`**  
   ⮡ Ingests source data into the Landing/Bronze layer

- **Pipeline: `Silver_to_gold`**  
   ⮡ Executes Silver transformation notebooks  
   ⮡ Then runs corresponding Gold layer aggregation notebooks

- **Pipeline: `archive_emr_configs`**  
   ⮡ Uses metadata to loop through configs and archive processed files

- **Pipeline: `Gold Layer Notebooks` (Detailed)**  
   Each Gold notebook is preceded by its Silver input:


## 📈 Key Highlights

- ✅ Scalable and modular ETL framework  
- 🔁 Supports full & incremental loads  
- 📜 Extensible metadata control for flexible scheduling  
- 🛡️ Built-in data governance and access control with Unity Catalog

---

4. ## ▶️ Run Notebooks (In Order)

Only the initial setup notebooks require manual execution. All other transformations are triggered automatically via **Azure Data Factory** pipelines.

### 🔹 Step 1: Manual Execution (One-time Setup)

1. **Mount Storage**
   - `2. adls_mount`  
     ⮡ Mounts your ADLS Gen2 containers to Databricks at `/mnt/`

2. **Extract Reference Data via APIs**
   - `ICD Code API extract`  
   - `NPI API extract`  
     ⮡ Pulls metadata from public healthcare APIs and stores in Landing layer

---

### 🔹 Step 2: Automated via Azure Data Factory

All remaining notebooks, including transformation and aggregation logic, are executed through **ADF pipelines**:

- `source_to_bronze` → Loads raw files from source into Bronze
- `Silver_to_gold`   → Executes Silver transformations → Gold layer aggregation
- `archive_emr_configs` → Archives processed configs based on metadata

No manual intervention is required beyond Step 1. ADF handles the entire medallion flow from raw ingestion to analytics-ready datasets.

---

---

## 📬 Contact

For any questions, feel free to reach out via [LinkedIn](https://www.linkedin.com/in/raunak-suman-262379180/).

---

