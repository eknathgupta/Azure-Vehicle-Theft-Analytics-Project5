Project Name- Vehicle Theft Analytics Platform (Azure End-to-End Project)

- Project Overview

This project is a complete end-to-end Azure Data Engineering pipeline that ingests, cleans, transforms,
 and visualizes vehicle theft data using ADF, ADLS Gen2, Databricks, Delta Lake, and Power BI.

It follows the Raw → Bronze → Silver → Gold Lakehouse architecture and demonstrates real-world enterprise data engineering practices.


 Azure Resources Created

Resource Group                  - vehicle-manager
Storage Account (ADLS Gen2)     - stvehicle
Azure Data Factory              - vehicleadfstudio
Self Hosted Integration Runtime  vehicleintegrationruntime
Azure Databricks Workspace      - DBvehicle
Azure Key Vault                 - vehicle-key
--
 Data Architecture
raw     -> Source datasets from Blob
bronze  -> Ingested raw structured data
silver  -> Cleaned and standardized data
gold    -> Analytics ready data for Power BI
1- Data Pipeline Flow

 Step 1 – Raw Ingestion (ADF)

Pipeline Name: `vehvile_theft_pipeline`

   Ingests multiple vehicle theft datasets in parallel
 Loads files from Azure Blob Storage into Raw Layer
 Uses AutoResolveIntegrationRuntime
 Pipeline executed successfully

* Raw Data Ingestion Pipeline (ADF)
This pipeline forms the base layer for downstream transformations.

---

 Step 2 – Databricks Processing

 Container Mounting

```python
bronze_path = "abfss://bronze@stvehicle.dfs.core.windows.net/"
dbutils.fs.ls(bronze_path)
``

 Step 3 – Bronze Layer

  CSV files loaded into Spark DataFrames
   Column names standardized
  Data types corrected (string → int) after schema validation

 Step 4 – Silver Layer

 `location_df` cleaned and stored in Silver container
 Null analysis performed:

```python
null_count_make_details = make_details_df.select(
    [sum(col(column).isNull().cast('int')).alias(column)
     for column in make_details_df.columns]
)

 Step 5– Gold Layer

Null replacement and data quality fixes applied:

```python
stolen_vehicles_df = stolen_vehicles_df.fillna({
    "vehicle_type": "No data",
    "make_id": 0,
    "Model_year": 0,
    "vehicle_desc": "No data",
    "color": "No data"
})


Final gold output written:

```python
location_df.write.option("header","true") \
.csv("abfss://gold@stvehicle.dfs.core.windows.net/location_df.csv")

Temporary views created for SQL analytics.

Imp-Power BIvisualization

 Connected Power BI to Azure Data Lake (Gold Layer)
 Loaded transformed data
 Built Bar Chart to visualize theft trends

Connection:

https://stvehicle.dfs.core.windows.net/gold

Used- Technologies Used

 Azure Data Factory
 Azure Data Lake Gen2
 Azure Databricks (PySpark)
 Delta Lake
 Azure Key Vault
 Power BI

********* Business Outcome

  Automated vehicle theft analytics platform
  Clean, governed, analytics-ready datasets
  Real-time BI dashboards for decision making

Author-

Eknath Gupta
Azure Data analyst


