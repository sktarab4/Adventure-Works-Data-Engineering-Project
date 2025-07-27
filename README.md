# adventure-works-azure-pipeline

This project showcases an end-to-end data pipeline built entirely on Azure. Data is ingested from external sources using Azure Data Factory, stored in Azure Data Lake Storage Gen2, and transformed using Azure Databricks. The transformed data is then queried through Azure Synapse Analytics, and the final insights are visualized in Power BI dashboards. This modular and scalable architecture follows the principles of a modern data lakehouse, enabling efficient data processing, analytics, and reporting.

### High-Level Overview

This is an end-to-end data pipeline using Microsoft Azure services with a lakehouse approach. Data from Kaggle's Adventure Works dataset is uploaded to GitHub, then ingested into Azure Data Lake Storage Gen2 using Azure Data Factory and transformed using Azure Databricks. The transformed data is then made queryable via Azure Synapse Analytics, and finally visualized using Power BI.

The architecture follows a medallion architecture with three layers:

* Bronze: Raw, ingested data
* Silver: Cleaned and transformed data
* Gold: Analytical/aggregated data for reporting

### Architecture

![](https://github.com/sktarab4/Adventure-Works-Data-Engineering-Project/blob/main/ADW%20-%20Architecture%20Diagram.png)

### Implementation Steps

### 1: Dataset Preparation

* Downloaded Adventure Works dataset from Kaggle.
* Uploaded the cleaned CSV files to a public GitHub repository to simulate a hosted source.

### 2: Data Ingestion with Azure Data Factory

* Created a pipeline in Azure Data Factory using the HTTP connector.
* Used parameterized pipelines to dynamically loop through multiple file names and URLs.
* Data was fetched from GitHub and stored in ADLS Gen2.
* Each file landed in its dedicated subfolder under the /bronze/ zone (raw zone).

### 3: Raw Storage – Bronze Layer

* Organized the ADLS Gen2 structure as:

/bronze/
    ├── customers/
    ├── products/
    └── sales/
    
* No transformations were done in this layer — purely for storing ingested raw files.



