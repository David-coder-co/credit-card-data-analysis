# credit-card-data-analysis

## 1. Project Overview

The goal of this project is to analyze credit card transactions in near real-time to detect potential fraud risks. Fraudulent transactions can cause significant financial losses, so the project focuses on creating a robust ETL (Extract, Transform, Load) pipeline that processes data from multiple sources, validates it, performs transformations to calculate fraud risk, and stores the processed results for analysis.

This project is implemented on Google Cloud Platform (GCP) using services such as Cloud Storage, BigQuery, Dataproc Serverless, and Cloud Composer (Airflow). Python and PySpark are used to write the data processing scripts, while GitHub and GitHub Actions provide version control and CI/CD functionality.

The main features of the project include:
- Loading static cardholder data into BigQuery.
- Processing daily transaction files stored in Google Cloud Storage.
- Performing data validation and transformation using PySpark on Dataproc Serverless.
- Calculating fraud risk scores for transactions.
- Automating the entire workflow using Cloud Composer (Airflow).
- Archiving processed files for future reference.
- Deploying scripts and DAGs automatically using GitHub Actions with unit testing.

## 2. Tech Stack
| Component                      | Purpose                                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------------------- |
| **Python**                     | Primary programming language for ETL scripts.                                                |
| **PySpark**                    | Handles large-scale data processing and transformation for fraud risk calculation.           |
| **Google Cloud Storage (GCS)** | Stores raw input data, application scripts, and archived files.                              |
| **BigQuery**                   | Cloud-based analytical database to store structured data for analysis.                       |
| **Dataproc Serverless**        | Runs PySpark jobs without needing to manage clusters, making it scalable and cost-efficient. |
| **Cloud Composer (Airflow)**   | Orchestrates the ETL pipeline, triggering jobs when new data arrives.                        |
| **GitHub**                     | Stores the source code and Airflow DAGs.                                                     |
| **GitHub Actions**             | Provides CI/CD automation to deploy DAGs and PySpark scripts and run unit tests.             |
| **PyTest**                     | Performs unit testing for data processing scripts to ensure correctness.                     |

## 3. Architecture

![Architecture](https://github.com/David-coder-co/credit-card-data-analysis/blob/c61fe8266dfaab6a32ee765c520ab5cf6a735dbb/GCP%20Project.drawio.png) 

### 3.1 Google Cloud Storage (GCS)
Cloud Storage serves as the central repository for all raw and processed data. The bucket is organized into three folders:
- customers/: Contains the static cardholder.csv file, which holds details about all credit card holders (e.g., card ID, customer name, contact info, card type).
- transactions/: Receives daily JSON transaction files. These files contain transaction details such as transaction ID, timestamp, amount, merchant, location, and card ID. Each day, new transaction files are uploaded to this folder.
- archive/: After transactions are processed and loaded into BigQuery, the files are moved here to keep a record of processed data and prevent re-processing.
The storage bucket acts as the primary input source for the pipeline and ensures that raw data is always retained.

### 3.2 BigQuery

BigQuery is a serverless, fully-managed data warehouse used to store structured data for analysis. Two main tables are used:
- Card Holders Table: Created from cardholder.csv in customers/ folder. This table contains information about each credit card holder, which is used to enrich transaction data during processing.
- Transactions Table: Stores transformed transaction data with calculated fraud risk scores. Data from daily transaction files is processed using PySpark and then loaded into this table.
BigQuery allows fast querying and analysis of large datasets and is essential for running reports or feeding dashboards for fraud detection.

### 3.3 Cloud Composer (Airflow)

Cloud Composer is used to automate and orchestrate the ETL pipeline. Its role includes:
- Monitoring incoming transaction files in transactions/ folder of Cloud Storage.
- Triggering PySpark jobs on Dataproc Serverless whenever a new file is detected.
- Managing workflow dependencies to ensure that data processing occurs in the correct order.
- Archiving processed files into the archive/ folder after successful transformation.
Airflow DAGs (Directed Acyclic Graphs) define how the pipeline executes tasks in sequence, including monitoring, processing, validation, and archiving.

### 3.4 Dataproc Serverless (PySpark)

Dataproc Serverless provides a scalable environment for running PySpark ETL jobs without managing clusters.
The PySpark job performs the following functions:

Reading Data:
- Static cardholder data from BigQuery.
- Daily transaction files from Cloud Storage.

Data Validation:
- Checks for missing or malformed fields.
- Ensures transaction amounts are numeric and timestamps are valid.

Data Transformation:
- Joins transaction data with cardholder info.
- Computes fraud risk scores based on defined business logic.
- Formats data to match the schema of the BigQuery Transactions Table.

Loading Data:
- Writes processed transactions to BigQuery.

Post-Processing:
- Once data is successfully loaded, the original transaction file is moved to archive/

### 3.5 CI/CD (GitHub & GitHub Actions)

The project uses GitHub Actions to automate deployment and testing:
- Deploy DAGs and PySpark Scripts: When code is pushed to the main branch, Actions automatically deploy DAGs and PySpark scripts to the GCS bucket.
- Run Unit Tests with PyTest: Ensures that transformation logic in PySpark scripts works correctly before deployment.
- Version Control: GitHub ensures that all changes are tracked and rollback is possible if a deployment fails.

## 4. Data Flow in Detail

Here’s the step-by-step data flow:
1. Static Cardholder Data Load
   - cardholder.csv is uploaded to customers/ folder.
   - Manual load process creates a BigQuery table called CardHolders.

2. Daily Transaction Processing
- New JSON files arrive in transactions/.
- Cloud Composer DAG detects the new file and triggers Dataproc Serverless.
- PySpark reads:
  - Transaction JSON file.
  - Card Holders data from BigQuery.
- PySpark performs:
  - Data validation (checks for missing fields, correct data types).
  - Transformation and enrichment (joining with cardholder info).
  - Fraud risk calculation using business rules or models.
- Processed data is written to BigQuery Transactions Table.

3. Archiving
- After successful processing, the transaction file is moved to archive/ folder in Cloud Storage.

4. Continuous Integration & Deployment
- Any update to PySpark scripts or Airflow DAGs is pushed to GitHub.
- GitHub Actions runs tests and deploys the scripts to GCS automatically.

## 5. Testing & Validation

Unit tests are written using PyTest to ensure that the PySpark scripts behave as expected.
- Tests include:
  - Ensuring that missing or invalid fields are detected.
  - Validating that fraud risk calculations are correct.
  - Ensuring that transformed data matches the schema of BigQuery Transactions Table.
Unit testing ensures reliability and prevents errors during automated execution.

## 6. Key Outcomes & Learnings

- Implemented a fully automated ETL pipeline using GCP services.
- Gained experience with Cloud Storage, BigQuery, Dataproc, and Cloud Composer.
- Learned orchestration of workflows with Airflow DAGs.
- Applied data validation, transformation, and fraud risk calculation using PySpark.
- Implemented CI/CD practices with GitHub Actions for automated deployment and testing.
- Designed a pipeline suitable for near real-time fraud detection, which can be extended to larger datasets.
