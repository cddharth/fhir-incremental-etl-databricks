# FHIR Incremental ETL Pipeline

An incremental healthcare data engineering pipeline built using **Azure Databricks, PySpark, Delta Lake, and GitHub**.

## Architecture

FHIR source data is processed through a Medallion Architecture:

```text
FHIR Data
   │
   ▼
Ingestion
   │
   ▼
Bronze
   │
   ▼
Silver
   │
   ▼
Gold
```

Pipeline Flow

The Databricks Job contains four dependent tasks:

Ingestion – Ingests raw FHIR data.
Bronze – Stores the ingested data in Delta format.
Silver – Cleans and transforms the data and applies incremental processing logic.
Gold – Produces curated datasets for analytics and reporting.

The tasks are orchestrated using Databricks Workflows with the following dependency chain:

```Ingestion → Bronze → Silver → Gold```

Each downstream task runs only after its upstream task succeeds.


Technologies
Azure Databricks
PySpark
Python
SQL
Delta Lake
Databricks Workflows
GitHub
Databricks Declarative Automation Bundles


Project Structure

```
fhir-incremental-etl-databricks/
│
├── Notebooks/
│   ├── 01_Ingestion_raw
│   ├── 02_Bronze
│   ├── 03_Silver
│   └── 04_Gold
│
├── resources/
│   └── fhir_incremental_etl_job.yml
│
├── databricks.yml
├── .gitignore
└── README.md
```

Medallion Architecture
1. Ingestion

The ingestion layer loads raw FHIR source data into the data platform and prepares it for downstream processing.

2. Bronze

The Bronze layer stores the ingested source data in Delta format while preserving the raw structure for traceability and further processing.

3. Silver

The Silver layer performs data cleaning, transformation and incremental processing to create reliable datasets for downstream consumption.

4. Gold

The Gold layer contains curated datasets optimized for analytics, reporting and business consumption.

Incremental ETL

The pipeline is designed for incremental data processing rather than repeatedly processing the complete source dataset.

This approach helps reduce unnecessary processing and supports scalable data engineering workflows.

Databricks Workflow

The pipeline is orchestrated as a Databricks Job:

```
┌─────────────┐
│  Ingestion  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Bronze    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Silver    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    Gold     │
└─────────────┘
```

The workflow uses task dependencies so that each stage executes only after the previous stage completes successfully.

GitHub Integration

The project is maintained in GitHub with the Databricks notebooks and job configuration stored as source-controlled artifacts.

The Databricks Job uses the GitHub repository and main branch as its Git source.

This provides version control for:

ETL notebooks
Job configuration
Pipeline dependencies
Databricks deployment configuration
Databricks Declarative Automation Bundle

The project includes a databricks.yml configuration file:

```
bundle:
  name: fhir-incremental-etl-databricks

include:
  - resources/*.yml

targets:
  dev:
    mode: development
```


The Databricks Job configuration is maintained under:

```resources/fhir_incremental_etl_job.yml```

This allows the Databricks workflow configuration to be managed as code alongside the ETL notebooks.

Job Configuration

The Databricks Job contains the following tasks:

```
| Task      | Notebook                     | Dependency |
| --------- | ---------------------------- | ---------- |
| Ingestion | `Notebooks/01_Ingestion_raw` | None       |
| Bronze    | `Notebooks/02_Bronze`        | Ingestion  |
| Silver    | `Notebooks/03_Silver`        | Bronze     |
| Gold      | `Notebooks/04_Gold`          | Silver     |

```


Key Features
Incremental ETL processing
Medallion architecture
Delta Lake-based data processing
FHIR healthcare data pipeline
PySpark transformations
Databricks Workflows
Task dependency orchestration
GitHub source control
Infrastructure/configuration as code
Databricks Declarative Automation Bundle structure
Serverless Databricks compute



Repository Contents
The repository intentionally contains the source code and deployment configuration required to reproduce the pipeline.
Runtime-generated data, catalog tables and temporary Databricks artifacts are not stored in Git.


Future Improvements
Potential future enhancements include:
Automated CI/CD deployment through GitHub Actions
Data quality checks
Automated testing
Pipeline monitoring and alerting
Additional FHIR resource types
Power BI integration
Production and staging deployment targets
Unity Catalog governance and access controls


Author
Siddhartha Sarkar
Data Engineer | Azure Databricks | PySpark | SQL | Delta Lake

