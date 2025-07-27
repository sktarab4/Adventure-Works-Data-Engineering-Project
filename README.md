# adventure-works-end-to-end-pipeline

## High-Level Overview

This is an end-to-end data pipeline using **Microsoft Azure** services with a lakehouse approach. Data from Kaggle's Adventure Works dataset is uploaded to **GitHub**, then ingested into **Azure Data Lake Storage Gen2** using **Azure Data Factory** and transformed using **Azure Databricks**. The transformed data is then made queryable via **Azure Synapse Analytics**, and finally visualized using **Power BI**.

The architecture follows a **medallion architecture** with three layers:

* Bronze: Raw, ingested data
* Silver: Cleaned and transformed data
* Gold: Analytical/aggregated data for reporting

## Architecture

![](https://github.com/sktarab4/adventure-works-azure-pipeline/blob/main/ADW%20-%20Architecture%20Diagram.png)

## Implementation Steps

### 1: Dataset Preparation

* Downloaded Adventure Works dataset from [Kaggle](https://www.kaggle.com/datasets/ukveteran/adventure-works).
* Uploaded the cleaned CSV files to a public [GitHub](https://github.com/sktarab4/Adventure-Works-Data-Engineering-Project/tree/main/data) repository to simulate a hosted source.

### 2: Data Ingestion with Azure Data Factory

* Created a pipeline in Azure Data Factory using the HTTP connector.
* Created a [git.json](https://github.com/sktarab4/adventure-works-azure-pipeline/blob/main/git.json) file to store paramete values for multiple file names and URLs.
* Used parameterized pipelines to dynamically loop through the [git.json](https://github.com/sktarab4/adventure-works-azure-pipeline/blob/main/git.json) file using lookup & foreach functions in ADF.
* Data was fetched using Copy activity from GitHub and stored in ADLS Gen2.
* Each file landed in its dedicated subfolder under the ```/bronze/``` layer (folder in ADLS Gen2) (raw zone).

### 3: Raw Storage – Bronze Layer

* Organized the ADLS Gen2 structure as:
```
/bronze/
    ├── customers/
    ├── products/
    └── sales/
```    
* No transformations were done in this layer, it was purely for storing ingested raw files.

### 4: Secure Access via Service Principal

* Registered an Azure Active Directory (Service Principal App) for secure access.
* Granted the Databricks workspace access to the ADLS using Service Principal.
* Used ```spark.conf.set``` to set the below in the Databricks Notebook:
```
spark.conf.set("fs.azure.account.auth.type.<storage-account>.dfs.core.windows.net", "OAuth")
spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account>.dfs.core.windows.net", "<application-id>")
spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account>.dfs.core.windows.net", service_credential)
spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account>.dfs.core.windows.net", "https://login.microsoftonline.com/<directory-id>/oauth2/token")
```

### 5: Data Transformation in Azure Databricks

* Read raw data from the ```/bronze/``` layer (folder in ADLS Gen2) using PySpark in [Databricks Notebook](https://github.com/sktarab4/adventure-works-azure-pipeline/blob/main/Silver%20Layer%20Transformation.ipynb).
* Applied:
    - Schema enforcement
    - Null checks and data type conversions
    - Join operations and basic aggregations
* Wrote the clean data to the ```/silver/``` layer (folder in ADLS Gen2) in Parquet format:
```
/silver/
    ├── customers_cleaned/
    ├── products_enriched/
    └── sales_transformed/
```

### 6: Analytics Layer with Azure Synapse SQL Serverless

* Connected Azure Synapse Serverless SQL Pool to the ```/silver/``` layer (folder in ADLS Gen2).
* Used ```OPENROWSET()``` with Parquet to query the files directly from ADLS.
* Created external tables and views for easy access for data analysts and BI tools.
* Wrote final aggregated results to ```/gold/``` layer (folder in ADLS Gen2) (ready for reporting):
```
/gold/
    ├── monthly_sales_summary/
    └── top_customers_by_region/
```
