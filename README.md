## End-to-End Data Engineering Pipeline (AWS + Snowflake + Snowpark)
### Project Overview
This project demonstrates a complete end-to-end data engineering pipeline that ingests multi-format data from GitHub, processes it using AWS services, and loads it into Snowflake for transformation and analytics.
The pipeline handles CSV, JSON, and Parquet datasets and processes them through multiple layers (Staging → Raw → Transformed → Curated) using Snowpark for scalable data transformation.
The goal of the project is to simulate a modern cloud data warehouse workflow used in real data engineering environments.
![Capture](https://github.com/user-attachments/assets/508bbaaa-6c03-40a6-a218-e4f1ee86900f)
## Architecture Workflow
GitHub (CSV / JSON / Parquet)
        │
        ▼
AWS Glue (Data Processing)
        │
        ▼
Amazon S3 (Data Lake Storage)
        │
        ▼
Snowflake Storage Integration
        │
        ▼
Snowflake Stage
        │
        ▼
STAGING TABLES
        │
        ▼
RAW TABLES
        │
        ▼
TRANSFORMED TABLES
        │
        ▼
CURATED ANALYTICS TABLES
## Data Warehouse Layer Design

STAGING
   │
   ▼
  RAW
   │
   ▼
TRANSFORMED
   │
   ▼
CURATED

## Data Sources
The project uses datasets stored in GitHub in three different formats:
CSV file (India Sales Orders)
Parquet file (USA Sales Orders)
JSON file (France Sales Orders)

Each dataset contains mobile sales order information such as:
Order ID
Customer Name
Mobile Model
Quantity
Price
Order Amount
Payment Information
Shipping Status

## Pipeline Implementation
### 1. Data Ingestion
Data files stored in GitHub are processed using AWS Glue.
Glue reads the source datasets and moves them into Amazon S3 where they are stored as part of the data lake.
### 2. Data Storage in S3
The processed files are stored in an S3 bucket that acts as the central data storage layer.
This bucket is later connected to Snowflake using Snowflake Storage Integration.
### 3. Snowflake Integration
A Storage Integration is created to securely connect Snowflake with the S3 bucket.
This allows Snowflake to read files directly from S3 without exposing AWS credentials.
The data is accessed through an External Stage.
### 4. Staging Layer
A Snowflake Stage is created to access files stored in S3.
From this stage, data is loaded into copy tables using the COPY INTO command.
### Example workflow:
S3 Files
   ↓
Snowflake Stage
   ↓
Copy Tables (STAGING Schema)
### 5. RAW Layer (Snowpark Processing)
Using Snowpark, the staged data is loaded into RAW tables.
#### Operations performed:
Parsing CSV, JSON, and Parquet formats
Column mapping
Adding insert timestamp
Structuring datasets into relational tables
#### RAW tables created:
RAW.INDIA_SALES_ORDER
RAW.USA_SALES_ORDER
RAW.FRANCE_SALES_ORDER
### 6. TRANSFORMED Layer
The transformed layer standardizes the data structure across all countries.
Transformations include:
Renaming columns
Adding a Country column
Merging datasets using Union
Filling missing values
Splitting mobile model into attributes:
 Mobile Brand
 Version
 Color
 RAM
 Memory
### Final transformed table:
TRANSFORMED.GLOBAL_SALES_ORDER
![WhatsApp Image 2026-03-12 at 4 55 09 AM](https://github.com/user-attachments/assets/663b60cb-01f9-4a12-8eef-b8d3cfdb3d78)

### 7. CURATED Layer
The curated layer contains analytics-ready datasets for reporting and business analysis.
Three curated tables are generated.
#### 1. Delivered Orders Table
##### CURATED.GLOBAL_SALES_ORDER_DELIVERED
 Contains only orders with Delivered shipping status.
#### 2. Sales by Mobile Brand
##### CURATED.GLOBAL_SALES_ORDER_BRAND
Aggregates total sales by:
 Mobile Brand
 Mobile Model
#### 3. Sales by Country and Quarter
##### CURATED.GLOBAL_SALES_ORDER_COUNTRY
#### Aggregates sales metrics based on:
 Country
 Year
 Quarter
#### Metrics generated:
 Total Sales Volume
 Total Sales Amount
Final result would appear like it in curated layer. 
<img width="499" height="216" alt="image" src="https://github.com/user-attachments/assets/61c51969-06a9-4996-b73c-ced1d55e1c9c" />
 
