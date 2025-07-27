# adventure-works-azure-pipeline

This project showcases an end-to-end data pipeline built entirely on Azure. Data is ingested from external sources using Azure Data Factory, stored in Azure Data Lake Storage Gen2, and transformed using Azure Databricks. The transformed data is then queried through Azure Synapse Analytics, and the final insights are visualized in Power BI dashboards. This modular and scalable architecture follows the principles of a modern data lakehouse, enabling efficient data processing, analytics, and reporting.

### High-Level Overview

This project implements an end-to-end data pipeline using Microsoft Azure services with a lakehouse approach. Data from Kaggle's Adventure Works dataset is uploaded to GitHub, then ingested into Azure Data Lake Storage Gen2 using Azure Data Factory and transformed using Azure Databricks. The transformed data is then made queryable via Azure Synapse Analytics, and finally visualized using Power BI.

The architecture follows a medallion architecture with three layers:

* Bronze: Raw, ingested data
* Silver: Cleaned and transformed data
* Gold: Analytical/aggregated data for reporting



