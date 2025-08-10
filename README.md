# GCP Financial Data Pipeline

> **End-to-end financial data processing pipeline using Google Cloud Platform services**

##  Architecture Overview

```
Alpha Vantage API → Apache NiFi → GCS → Cloud Function → Dataproc/PySpark → BigQuery + GCS Backup
```

**Data Flow:**
1. **Extract**: Apache NiFi pulls financial data from Alpha Vantage API
2. **Store**: Raw data lands in Google Cloud Storage buckets
3. **Trigger**: Cloud Function monitors bucket changes and triggers processing
4. **Transform**: PySpark jobs on Dataproc clean and transform data
5. **Load**: Processed data flows into BigQuery for analytics + backup to GCS

##  Technologies Used

- **Infrastructure**: GCP Deployment Manager (IaC)
- **Data Extraction**: Apache NiFi on Google Compute Engine
- **Event Processing**: Google Cloud Functions
- **Data Processing**: Apache Spark on Google Dataproc
- **Analytics Storage**: Google BigQuery
- **Object Storage**: Google Cloud Storage
- **Orchestration**: Dataproc Workflow Templates

## 📁 Repository Structure

```
gcp-financial-pipeline/
├── README.md
├── .gitignore
├── config/
│   ├── environment-variables.template
│   └── gcp-config.yaml
├── deployment/
│   ├── deployment-manager/
│   │   ├── main.yaml
│   │   ├── resources/
│   │   │   ├── gcs-buckets.yaml
│   │   │   ├── bigquery-dataset.yaml
│   │   │   ├── compute-instance.yaml
│   │   │   ├── iam-roles.yaml
│   │   │   └── cloud-functions.yaml
│   │   └── properties/
│   │       ├── dev.yaml
│   │       └── prod.yaml
│   └── scripts/
│       ├── deploy.sh
│       ├── cleanup.sh
│       └── setup-project.sh
├── nifi/
│   ├── templates/
│   │   ├── alpha-vantage-to-gcs.xml
│   │   └── market-data-flow.xml
│   ├── processors/
│   │   └── custom-processors/
│   ├── setup/
│   │   ├── install-nifi.sh
│   │   ├── configure-nifi.sh
│   │   └── nifi.properties.template
│   └── flows/
│       └── financial-data-ingestion.json
├── cloud-functions/
│   ├── gcs-trigger/
│   │   ├── main.py
│   │   ├── requirements.txt
│   │   ├── deploy.sh
│   │   └── function-config.yaml
│   └── dataproc-launcher/
│       ├── main.py
│       ├── requirements.txt
│       └── deploy.sh
├── dataproc/
│   ├── pyspark-jobs/
│   │   ├── financial-data-transformer.py
│   │   ├── technical-indicators.py
│   │   ├── data-quality-checks.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       ├── data_utils.py
│   │       └── gcs_utils.py
│   ├── workflow-templates/
│   │   ├── financial-processing-workflow.yaml
│   │   └── daily-batch-processing.yaml
│   ├── cluster-configs/
│   │   ├── standard-cluster.yaml
│   │   └── preemptible-cluster.yaml
│   └── scripts/
│       ├── submit-job.sh
│       └── create-cluster.sh
├── bigquery/
│   ├── schemas/
│   │   ├── raw_stock_data.json
│   │   ├── processed_stock_data.json
│   │   └── technical_indicators.json
│   ├── sql/
│   │   ├── create-tables.sql
│   │   ├── data-transformations.sql
│   │   └── analytics-queries.sql
│   └── views/
│       ├── daily-market-summary.sql
│       └── portfolio-analytics.sql
├── monitoring/
│   ├── logging/
│   │   └── log-config.yaml
│   ├── alerting/
│   │   └── alert-policies.yaml
│   └── dashboards/
│       └── pipeline-monitoring-dashboard.json
├── tests/
│   ├── unit/
│   │   ├── test_data_transformation.py
│   │   └── test_gcs_utils.py
│   ├── integration/
│   │   └── test_pipeline_e2e.py
│   └── data/
│       └── sample-market-data.json
├── docs/
│   ├── setup-guide.md
│   ├── architecture-diagram.png
│   ├── troubleshooting.md
│   ├── api-documentation.md
│   └── cost-optimization.md
└── scripts/
    ├── data-validation.py
    ├── pipeline-status.sh
    └── emergency-shutdown.sh
```

##  Quick Start

### Prerequisites
- Google Cloud Platform account with billing enabled
- gcloud CLI installed and configured
- Python 3.8+ installed
- Alpha Vantage API key (store in environment variables)


##  Data Sources

- **Primary**: Alpha Vantage API (stocks, forex, crypto, technical indicators)
- **Frequency**: Daily batch processing with real-time capabilities
- **Volume**: 500+ stocks, 20+ years historical data (~2.5M+ records)
- **Format**: JSON/CSV ingestion, Parquet storage, BigQuery analytics

##  Pipeline Components

### 1. Data Ingestion (Apache NiFi)
- Scheduled pulls from Alpha Vantage API
- Data validation and error handling
- Automatic retry mechanisms
- Rate limiting compliance

### 2. Data Processing (PySpark on Dataproc)
- Data cleaning and normalization
- Technical indicator calculations
- Data quality checks
- Partitioning and optimization

### 3. Analytics Layer (BigQuery)
- Time series analysis ready
- Pre-aggregated views for performance
- Real-time query capabilities
- Cost-optimized table structures

##  Security & Best Practices

- All secrets managed via GCP Secret Manager
- IAM roles follow principle of least privilege
- Data encrypted in transit and at rest
- Audit logging enabled for all components
- Network security via VPC and firewall rules

##  Cost Optimization

- Preemptible instances for Dataproc clusters
- Lifecycle policies for GCS buckets
- BigQuery slot reservations for predictable workloads
- Automatic scaling and shutdown policies

##  Monitoring & Alerting

- Comprehensive logging via Cloud Logging
- Custom metrics and dashboards
- Automated alerting for pipeline failures
- Performance monitoring and optimization


## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.


