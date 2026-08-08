# Banking_Analytics_DE_Project

# 🏦 Banking Analytics Data Engineering Project

An end-to-end **Banking Analytics Data Engineering project** built using **Microsoft Azure, Azure Data Lake Storage Gen2, Azure Databricks, Delta Lake, Unity Catalog, and Azure Synapse Analytics**.

The project demonstrates how raw banking data can be ingested, processed, transformed, governed, and converted into business-ready analytical datasets using a **Medallion Architecture (Bronze → Silver → Gold)**.

---

## 📌 Project Overview

The objective of this project is to build a scalable banking analytics platform that processes multiple banking datasets and produces curated analytical datasets for business intelligence and reporting.

The pipeline processes **10 banking datasets** covering customers, accounts, transactions, loans, credit cards, branches, employees, fraud, insurance, and customer support.

### Key Objectives

* Store raw banking data in Azure Data Lake Storage Gen2
* Process data using Azure Databricks and PySpark
* Implement Medallion Architecture
* Store processed data in Delta Lake
* Apply data cleansing and standardization
* Create business-level aggregations
* Implement Unity Catalog for data governance
* Expose Gold-layer data through Azure Synapse Serverless SQL
* Create reusable analytical datasets for reporting and BI

---

# 🏗️ Architecture

```text
                         ┌──────────────────────┐
                         │   Banking CSV Files  │
                         │      10 Datasets     │
                         └──────────┬───────────┘
                                    │
                                    ▼
                    ┌──────────────────────────────┐
                    │     Azure Data Lake Gen2     │
                    │          RAW Layer           │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │      Azure Databricks         │
                    │       PySpark Processing      │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │       BRONZE Layer            │
                    │      Raw Delta Tables         │
                    │   + Ingestion Metadata        │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │        SILVER Layer           │
                    │ Cleaning & Standardization    │
                    │ Deduplication & Enrichment    │
                    │ Business Rules & Validation   │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │         GOLD Layer            │
                    │ Business Aggregations         │
                    │ Analytical Data Products      │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │ Azure Synapse Serverless SQL  │
                    │       Analytical Views        │
                    └──────────────┬───────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │       BI / Reporting          │
                    └──────────────────────────────┘
```

---

# 🛠️ Technology Stack

| Technology                       | Purpose                              |
| -------------------------------- | ------------------------------------ |
| **Azure Data Lake Storage Gen2** | Cloud data lake                      |
| **Azure Databricks**             | Distributed data processing          |
| **Apache Spark / PySpark**       | Data transformation                  |
| **Delta Lake**                   | ACID-compliant storage               |
| **Unity Catalog**                | Data governance and cataloging       |
| **Azure Synapse Analytics**      | Serverless SQL analytics             |
| **Python**                       | Data engineering and transformations |
| **SQL**                          | Analytical queries and views         |
| **GitHub**                       | Source-code management               |

---

# 📂 Source Datasets

The project uses 10 CSV datasets:

| Dataset                  | Records |
| ------------------------ | ------: |
| Customers                |   1,000 |
| Accounts                 |   1,400 |
| Transactions             |  10,000 |
| Loans                    |     800 |
| Credit Cards             |     900 |
| Branches                 |      50 |
| Employees                |     500 |
| Fraud Transactions       |     600 |
| Insurance Products       |     700 |
| Customer Support Tickets |   1,200 |

The datasets represent different operational areas of a banking organization.

---

# ☁️ Azure Infrastructure

The project uses the following Azure resources:

```text
Resource Group
└── rg-banking-analytics-prod
    │
    ├── Azure Data Lake Storage Gen2
    │   └── bankingdatalake789
    │
    ├── Azure Databricks
    │   └── db-banking-analytics-prod
    │
    ├── Access Connector for Databricks
    │   └── adls-access-connector-banking
    │
    └── Azure Synapse Analytics
        └── synapse-banking-prod
```

The ADLS Gen2 storage account contains the following containers:

```text
raw/
bronze/
silver/
gold/
archive/
```

These containers form the foundation of the Medallion Architecture.

---

# 🔐 Unity Catalog

Unity Catalog is used to organize and govern the Delta tables.

### Catalog

```text
banking_catalog
```

### Schemas

```text
banking_catalog
│
├── bronze
├── silver
└── gold
```

The project creates separate schemas for raw/bronze data, cleaned silver data, and aggregated gold analytics.

---

# 🥉 Bronze Layer

The Bronze layer stores the banking datasets in **Delta format**.

### Processing

```text
RAW CSV
   ↓
Explicit Schema
   ↓
PySpark DataFrame
   ↓
Add Metadata
   ↓
Delta Lake
```

Additional ingestion metadata includes:

* `ingestion_timestamp`
* `source_file`
* `ingestion_batch_id`

This provides basic data lineage and ingestion tracking.

### Bronze Tables

```text
bronze/
├── customers/
├── accounts/
├── transactions/
├── loans/
├── credit_cards/
├── branches/
├── employees/
├── fraud_transactions/
├── insurance_products/
└── customer_support_tickets/
```

---

# 🥈 Silver Layer

The Silver layer contains **cleaned, standardized, deduplicated, and enriched data**.

Each Bronze dataset is transformed independently using PySpark.

### Common Transformations

* Duplicate removal
* Null filtering
* Data standardization
* Derived columns
* Categorization
* Date transformations
* Business rules
* Boolean indicators
* Data enrichment
* Processing metadata

---

## 👤 Customer Transformation

The customer dataset is enhanced with:

* Age
* Age group
* Full name
* Standardized gender
* Income bracket
* Email domain
* Customer tenure
* KYC verification flag

For example:

```text
Age
   ↓
Age Group
   ├── Young (<25)
   ├── Adult (25-39)
   ├── Middle Age (40-59)
   └── Senior (60+)
```

The project also standardizes gender and creates customer tenure and KYC indicators.

---

## 💳 Account Transformation

Account data includes derived attributes such as:

* Standardized account status
* Active account indicator
* Balance category
* Nominee indicator
* Account age
* Account age in years

---

## 💰 Transaction Transformation

Transaction data is enriched with:

* Transaction year
* Transaction month
* Transaction day
* Quarter
* Week
* Day of week
* Amount category
* Debit/Credit indicators
* Success/Failed/Reversed indicators
* Standardized payment mode

Payment modes are standardized into categories such as:

```text
UPI
Net Banking
RTGS
IMPS
NEFT
ATM
Cheque
Other
```

---

## 🏦 Loan Transformation

Loan data is enhanced with:

* Standardized loan status
* Active loan indicator
* Default indicator
* Total interest
* Total payable amount
* Interest rate category
* Loan amount category
* Collateral indicator
* Loan age

---

## 💳 Credit Card Transformation

Credit-card processing includes:

* Standardized card status
* Active card indicator
* Credit utilization percentage
* Utilization category
* Standardized card type
* Credit limit category
* Available limit percentage
* Card age
* Days to expiry
* Near-expiry indicator

Credit utilization is categorized into Low, Medium, High, and Critical levels.

---

## 🏢 Branch & Employee Transformation

Branch data is cleaned using:

* Trim
* Case standardization
* Clean city/state names
* Manager name standardization
* Branch age

Employee data is enriched with:

* Clean employee name
* First/last name
* Standardized designation
* Salary bracket
* Active employee indicator
* Employee tenure

---

## 🚨 Fraud Transformation

Fraud data is enhanced with:

* Risk level
* Confirmed fraud indicator
* False-positive indicator
* Open investigation indicator
* Loss category
* Detection delay

Risk scores are categorized as:

```text
90+     → Critical
70-89   → High
50-69   → Medium
<50     → Low
```

---

## 🛡️ Insurance Transformation

Insurance data includes:

* Standardized policy type
* Active policy indicator
* Expired policy indicator
* Premium category
* Coverage ratio
* Remaining policy days
* Near-expiry indicator

---

## 🎧 Customer Support Transformation

Support tickets are transformed with:

* Priority level
* Resolved/open indicators
* Resolution time
* Resolution-time category
* Standardized channel
* Standardized issue type
* Critical/high-priority indicators

---

# 🥇 Gold Layer

The Gold layer contains business-ready analytical datasets created from the Silver layer.

The project creates multiple analytical data products.

---

## 1. 👤 Customer 360

The Customer 360 dataset combines:

* Customer information
* Account metrics
* Loan metrics
* Credit-card metrics
* Insurance metrics
* Support-ticket metrics

Key metrics include:

```text
Total Accounts
Active Accounts
Total Balance
Average Balance
Total Loans
Active Loans
Total Loan Amount
Defaulted Loans
Total Cards
Active Cards
Total Credit Limit
Card Outstanding
Credit Utilization
Total Policies
Active Policies
Total Premium
Total Tickets
Resolved Tickets
Average Resolution Days
Total Assets
Risk Score
```

This provides a consolidated view of each customer.

---

# 2. 🏢 Branch Performance

Branch-level analytics include:

* Total accounts
* Total deposits
* Average deposit
* Total employees
* Average salary
* Employee efficiency
* Total loans
* Loan disbursement
* Average interest rate
* Total transactions
* Debit volume
* Credit volume
* Net flow

---

# 3. 💰 Loan Portfolio Analysis

The Loan Portfolio dataset provides analysis by:

* Loan type
* Loan status
* Age group
* Income bracket

Metrics include:

* Loan count
* Total loan amount
* Average loan amount
* Average interest rate
* Total interest income
* Total receivable

---

# 4. 🚨 Fraud Analysis

Fraud analytics provide:

* Fraud count
* Total loss
* Average loss
* Confirmed frauds
* False positives
* Confirmation rate

The data is aggregated by fraud year, month, fraud type, and risk level.

---

# 5. 📈 Daily Transaction Trends

Daily transaction analytics include:

* Transaction count
* Total amount
* Average transaction amount
* Successful transactions
* Failed transactions
* Success rate

The analysis is grouped by transaction date, transaction type, and payment mode.

---

# 6. 🎧 Customer Support Metrics

Support analytics provide:

* Total tickets
* Resolved tickets
* Open tickets
* Average resolution time
* Minimum resolution time
* Maximum resolution time
* Resolution rate

---

# 7. 💵 Monthly Financial Summary

Monthly financial analytics include:

* Total transactions
* Total credits
* Total debits
* UPI volume
* Net Banking volume
* RTGS volume
* IMPS volume
* Net financial flow

---

# 8. 👨‍💼 Employee Performance

Employee analytics are generated based on:

* Branch
* Designation
* Experience level

Metrics include:

* Employee count
* Average salary
* Active employee count

---

# 9. 🔄 Product Cross-Sell Insights

Customer product relationships are analyzed across:

```text
Accounts
Loans
Credit Cards
Insurance
```

This enables analysis of product adoption and cross-sell opportunities across customer segments.

---

# 🔄 End-to-End Data Flow

```text
Banking CSV Files
       │
       ▼
Azure Data Lake Gen2
       │
       │ RAW
       ▼
+-------------------+
|      BRONZE       |
|   Delta Tables    |
|  + Metadata       |
+---------┬---------+
          │
          ▼
+-------------------+
|      SILVER       |
| Cleaning          |
| Standardization   |
| Deduplication     |
| Enrichment        |
+---------┬---------+
          │
          ▼
+-------------------+
|       GOLD        |
| Business Metrics  |
| Aggregations      |
| Analytics         |
+---------┬---------+
          │
          ▼
+-------------------+
| Synapse Serverless|
|      SQL          |
+---------┬---------+
          │
          ▼
    BI / Analytics
```

---

# 📓 Databricks Notebooks

The project is organized into processing notebooks:

```text
notebooks/
│
├── 00_read_all_datasets
│
├── 01_read_with_schemas
│
├── 02_create_bronze_delta_tables
│
├── 03_silver_transformations
│
└── 04_gold_transformations
```

### Notebook Responsibilities

| Notebook                        | Responsibility                               |
| ------------------------------- | -------------------------------------------- |
| `00_read_all_datasets`          | Initial dataset ingestion and validation     |
| `01_read_with_schemas`          | Read datasets using explicit PySpark schemas |
| `02_create_bronze_delta_tables` | Create Bronze Delta tables                   |
| `03_silver_transformations`     | Clean and enrich Bronze data                 |
| `04_gold_transformations`       | Generate business-ready Gold datasets        |

The project explicitly defines PySpark schemas rather than relying only on schema inference, improving control over data types.

---

# 📁 Suggested GitHub Repository Structure

```text
Banking-Analytics-Data-Engineering/
│
├── README.md
│
├── notebooks/
│   ├── 00_read_all_datasets.py
│   ├── 01_read_with_schemas.py
│   ├── 02_create_bronze_delta_tables.py
│   ├── 03_silver_transformations.py
│   └── 04_gold_transformations.py
│
├── sql/
│   ├── create_catalog.sql
│   ├── create_schemas.sql
│   ├── create_external_locations.sql
│   └── synapse_views.sql
│
├── data/
│   └── sample/
│
├── architecture/
│   └── banking_architecture.png
│
├── docs/
│   └── project_documentation.pdf
│
└── .gitignore
```

> **Note:** Do not upload Azure credentials, subscription IDs, access keys, connection strings, or other secrets to GitHub.

---

# 🚀 How to Run the Project

## Step 1 — Create Azure Resources

Create:

* Resource Group
* ADLS Gen2
* Azure Databricks
* Databricks Access Connector
* Azure Synapse Analytics

The project documentation defines the Azure infrastructure and storage containers in Phase 1.

---

## Step 2 — Upload Banking Data

Upload the 10 CSV datasets into:

```text
raw/banking/
```

Example:

```text
raw/banking/
├── customers.csv
├── accounts.csv
├── transactions.csv
├── loans.csv
├── credit_cards.csv
├── branches.csv
├── employees.csv
├── fraud_transactions.csv
├── insurance_products.csv
└── customer_support_tickets.csv
```

---

## Step 3 — Configure Unity Catalog

Create:

```text
banking_catalog
```

with:

```text
bronze
silver
gold
```

schemas.

Configure the ADLS storage credential and external locations for the data lake containers.

---

## Step 4 — Run Bronze Pipeline

Run:

```text
00_read_all_datasets
        ↓
01_read_with_schemas
        ↓
02_create_bronze_delta_tables
```

The raw CSV files are converted into Delta tables with ingestion metadata.

---

## Step 5 — Run Silver Transformation

Run:

```text
03_silver_transformations
```

This performs:

```text
Deduplication
     ↓
Data Cleaning
     ↓
Standardization
     ↓
Derived Columns
     ↓
Business Rules
     ↓
Silver Delta Tables
```

---

## Step 6 — Run Gold Transformation

Run:

```text
04_gold_transformations
```

This creates business-level analytical datasets such as:

```text
customer_360
branch_performance
loan_portfolio
fraud_analysis
daily_transaction_trends
customer_support_metrics
monthly_financial_summary
employee_performance
product_cross_sell
```

---

# 🔎 Synapse Analytics

Azure Synapse Analytics is used as the SQL analytics layer on top of the curated Gold data.

The Gold layer can be exposed through **Serverless SQL views**, allowing analysts to query curated banking datasets using SQL without managing a traditional dedicated data warehouse.

Example analytical view:

```sql
CREATE VIEW vw_customer_360
AS
SELECT *
FROM OPENROWSET(
    BULK 'https://<storage-account>.dfs.core.windows.net/gold/customer_360/',
    FORMAT = 'DELTA'
) AS customer_360;
```

Replace the storage account and path according to your Azure environment.

---

# 📊 Business Use Cases

This platform can support banking business teams with:

### Customer Analytics

* Customer 360
* Customer segmentation
* Product ownership analysis
* Customer risk analysis

### Financial Analytics

* Transaction trends
* Credit/debit analysis
* Monthly financial summaries
* Branch-level financial performance

### Loan Analytics

* Loan portfolio analysis
* Default analysis
* Interest income analysis
* Loan segmentation

### Fraud Analytics

* Fraud detection analysis
* Risk-level analysis
* Fraud loss analysis
* Confirmation-rate monitoring

### Customer Service

* Support ticket analysis
* Resolution-time monitoring
* Priority analysis
* Channel performance

### Product Analytics

* Product adoption
* Cross-selling opportunities
* Customer product combinations

---

# 🔐 Data Governance & Security

The project uses:

* Azure RBAC
* Managed Identity
* Databricks Access Connector
* Unity Catalog
* External Locations
* Storage Credentials
* Private ADLS containers

The Databricks Access Connector is granted **Storage Blob Data Contributor** access to the ADLS storage account in the documented setup.

---

# ⚡ Key Data Engineering Concepts Demonstrated

This project demonstrates practical experience with:

* Azure Data Lake Storage Gen2
* Azure Databricks
* Apache Spark
* PySpark
* Delta Lake
* Medallion Architecture
* Unity Catalog
* External Locations
* Managed Identity
* RBAC
* Explicit Spark Schemas
* Data Cleansing
* Data Standardization
* Deduplication
* Data Enrichment
* Business Transformations
* Aggregations
* Customer 360
* Data Governance
* Serverless SQL
* Analytical Data Modeling

---

# 🎯 Project Outcome

The final solution transforms raw banking CSV files into governed, analytical datasets through a structured cloud data platform:

```text
Raw Banking Data
       ↓
     ADLS
       ↓
    Bronze
       ↓
    Silver
       ↓
     Gold
       ↓
Synapse Serverless SQL
       ↓
Business Analytics
```

The result is a scalable **Azure-based Banking Analytics Data Platform** that separates raw ingestion, data transformation, and business analytics while using Delta Lake and Unity Catalog for reliable and governed data processing.

---

# 👨‍💻 Author

**Data Engineering Project**

Built using:

```text
Azure
Azure Data Lake Gen2
Azure Databricks
PySpark
Delta Lake
Unity Catalog
Azure Synapse Analytics
SQL
Python
```

---

# ⭐ If You Find This Project Useful

If this project helps you understand Azure Data Engineering and Medallion Architecture, consider giving the repository a ⭐.

```
Azure Data Engineering
        +
Medallion Architecture
        +
Databricks
        +
Delta Lake
        +
Synapse Analytics
        =
Banking Analytics Platform
```

