# credit-card-data-analysis

## Project Overview

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

## Tech Stack
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

## Architecture

