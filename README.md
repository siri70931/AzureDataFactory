# Azure Data Factory Project

This repository contains an **Azure Data Factory (ADF)** project designed to build scalable and dynamic data pipelines. 
The project includes pipelines, datasets, linked services, and dataflows to enable end-to-end data ingestion, transformation, and loading.



Following are the steps done in this project - 

1. Created the storage group in resource group and uploaded the files
2. Then, created the pipeline to load the data from source to destination.
3. In creation of pipeline, I used the linked services as connection to load the data. Created linked services as mentioned for CSV Files, Azure tables, github.
4. Load the data from github, and get metadata for all the files present in container and applied transformations using pipeline activites, and used CDC for incremental loading. And used the storage based triggers, to run the piepline if there is any new data activity.
6. And finally added the destination to load output and created alerts using Logic Apps to get mail if pipeline fails.
7. Firstly we load the data, and update CDC accordingly, and if there are any rows in the table, trigger will start and pipeline will run. 
---

# Project Structure

```
├── dataflow/
│   └── dataflow1.json
│
├── dataset/
│   ├── AzureTable.json
│   ├── CSV_Dynamic_DS.json
│   ├── DataFlowSink.json
│   ├── Destination_Dataset.json
│   ├── GitDataSet.json
│   ├── GitOutputData.json
│   ├── IfDataset.json
│   ├── IfSinkData.json
│   ├── MetadataDataset.json
│   ├── Metadata_ds.json
│   └── empty_json_ds.json
│
├── linkedService/
│   ├── AzureDBLinkedService.json
│   ├── GitLinkedService.json
│   ├── LinkedService1.json
│   └── RestService.json
│
├── pipeline/
│
├── factory/
│   └── SireeshaDataFactory.json
│
├── Fact_Sales_Edit.csv
└── publish_config.json
```

---
### 1.The main goal of this pipeline is to:

Ingest data from multiple sources (CSV, REST, Azure Table, etc.)
Apply transformations using Dataflows
Load processed data into destination systems
Handle dynamic and metadata-driven processing

### 2. Pipeline Trigger / Start
The pipeline can be triggered:
Manually
Using a schedule trigger
Event-based trigger (if configured)

### 3. Metadata-Driven Execution
The pipeline uses:
MetadataDataset.json
Metadata_ds.json
These datasets store:
Source file names
Paths
Load conditions

This enables:

Dynamic processing of multiple files
Reusable pipeline logic

### 4. Lookup Activity
Reads metadata from configuration files/tables
Returns list of files or parameters to process

### 5. ForEach Activity
Iterates over each item returned by Lookup
Processes multiple datasets/files dynamically

### 6. Conditional Logic (If Activity)
Uses:
IfDataset.json
IfSinkData.json
Applies conditions like:
File exists or not
Data validation checks
Routing logic

### 7. Data Ingestion (Copy Activity)
Uses datasets like:
CSV_Dynamic_DS.json
AzureTable.json
GitDataSet.json
Moves data from:
CSV files
Azure Table
REST API

### 8. Data Transformation (Dataflow)
dataflow1.json is used
Performs:
Data cleansing
Filtering
Derived columns
Aggregations

### 9. Sink / Destination Load
Uses:
Destination_Dataset.json
DataFlowSink.json
GitOutputData.json
Loads processed data into:
Azure SQL DB
Storage

### 11. Linked Services

Connections configured:
AzureDBLinkedService → Database
RestService → APIs
GitLinkedService → Version control

“First, the pipeline reads metadata to understand which files to process. 
Then, using a ForEach loop, it processes each file dynamically. It applies conditional checks, ingests data using Copy Activity, transforms it using Dataflows, and finally loads it into the destination system. 
The entire pipeline is modular, reusable, and metadata-driven.”

# Components Description

### Dataflows

  * Performs transformation logic on incoming data.
  * Used for ETL/ELT processing.

### Datasets

* Defines input/output data structures.
* Includes:

  * CSV datasets
  * Azure Table storage datasets
  * Metadata-driven datasets
  * Conditional datasets (IfDataset, IfSinkData)

### Linked Services

* Connection configurations to external systems:

  * **AzureDBLinkedService** → Azure SQL Database
  * **GitLinkedService** → Git integration
  * **RestService** → REST API connections

### Pipelines

* Orchestrates workflows using:

  * Copy activities
  * Conditional logic
  * Metadata-driven processing

## 📊 Sample Data

* **Fact_Sales.csv**
