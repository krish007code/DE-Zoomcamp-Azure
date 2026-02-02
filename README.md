# NYC Taxi Data Pipeline: Kestra to Azure Synapse

This repository contains an end-to-end ETL pipeline developed and the project orchestrates the ingestion, storage, and warehousing of NYC Taxi trip data.

## 🛠 Tech Stack
* **Orchestration:** [Kestra](https://kestra.io/) (utilizing KV stores for secure variable management).
* **Cloud Storage:** Azure Blob Storage (secured with Shared Access Signatures).
* **Data Warehouse:** Azure Synapse Analytics (Serverless SQL Pool).
* **Data Access:** PolyBase / HDFS Bridge for external table management.
* **Environment:** Docker & Docker Compose on Fedora Linux.

## Pipeline Architecture
1. **Extract:** Fetches `.csv.gz` files from the NYC TLC GitHub repository, decompresses them, and prepares them for transit.
2. **Load:** Uploads the cleaned `.csv` to an Azure Blob Storage container using a SAS token.
3. **Transform/Warehouse:** * Creates a Database Scoped Credential for secure storage access.
    * Establishes an External Data Source pointing to the Blob container.
    * Defines an External Table to allow SQL queries directly on the CSV data.



##  Security & DevSecOps Practices
* **Secret Masking:** No Azure keys or tokens are hardcoded. All credentials are managed through Kestra's internal Key-Value (KV) store or environment variables.
* **Network Security:** Storage access is restricted to "Trusted Microsoft Services," ensuring Synapse can access data without exposing the storage account to the public internet.
* **Atomic DDL:** SQL commands are broken into atomic tasks to ensure idempotency and satisfy Synapse Serverless transaction constraints.

##  Repository Structure
* `ETL_Azure_pipeline.yaml`: The main Kestra flow definition.
* `docker-compose.yml`: Local environment setup for Kestra.
* `.gitignore`: Configured to prevent accidental leaks of `.env` files or raw taxi data.

##  Future Improvements
* Implement a `ForEach` orchestrator for automated 2021 backfills.
* Add data validation checks using Kestra's validation plugins.
* Integrate DevSecOps scanning for YAML configurations.
