# Celebal_Week6_Assignment
This Azure Data Factory solution implements retry and wait logic to handle transient data retrieval failures (e.g., “try again” errors). It improves pipeline resilience using built-in retry policies or custom loops with wait activities, ensuring stable and reliable data extraction.

--Configure Self-hosted Integration Runtime to Extract Data from Local Server and Load into Azure SQL Database:--
# ADF SHIR: Local SQL Server to Azure SQL Database Integration

This project sets up a Self-hosted Integration Runtime (SHIR) in Azure Data Factory to extract data from an **on-premises SQL Server** and load it into **Azure SQL Database**.

---

## 🚀 Use Case

Enable secure and automated cloud ingestion from on-premise environments using Azure Data Factory.

---

## 🛠️ Components

- **Self-hosted Integration Runtime (SHIR)**
- **Azure SQL Database**
- **Azure Data Factory Linked Services, Datasets, Pipelines**
- **Sample Table + SQL script**

---

## 📦 Folder Guide

- `datafactory/`: JSON files to import into ADF via ARM template or UI
- `shir-installation/`: Steps to set up SHIR
- `scripts/`: Sample SQL script to create the source table

---

## 🔧 Prerequisites

- Azure Subscription
- Local SQL Server (with firewall port 1433 open)
- Azure SQL Database setup
- Azure Data Factory instance created

---

## 📝 Steps

1. **Install SHIR on Local Server**
   - Follow instructions in `shir-installation/SHIR_Setup_Guide.md`

2. **Create Linked Services**
   - Use the JSON in `datafactory/linkedservices/`:
     - `LocalSQLServer.json`
     - `AzureSQLDatabase.json`

3. **Create Datasets**
   - Use:
     - `SourceDataset.json` (from Local SQL Server)
     - `SinkDataset.json` (to Azure SQL Database)

4. **Create Pipeline**
   - Use `CopyPipeline.json` to copy data

5. **Deploy via Azure Portal or ARM Templates**

6. **Test the Pipeline**

---

## 📜 Sample Table Script

Check `scripts/SampleTableScript.sql` to create a sample source table in your local SQL Server.

---

## 🤝 Contribution

PRs welcome! File an issue for suggestions.

---

## 📄 License

MIT License

--Configure FTP/SFTP Server and Create an ADF Pipeline for Data Extraction:--
# ADF FTP/SFTP to Blob Pipeline

This project sets up an Azure Data Factory pipeline to extract data files (e.g., CSV, JSON) from **FTP/SFTP servers** and load them into **Azure Blob Storage**.

---

## 🔧 Use Case

- Fetch files from partner systems or external servers (FTP/SFTP)
- Store in cloud for analytics and integration
- Scheduled, secure, and automated file transfers

---

## 💡 Highlights

- Supports SFTP authentication with key or password
- Azure Blob Sink for centralized cloud storage
- Easily extendable to Data Lake, SQL DB, etc.

---

## 📦 Project Structure

- `datafactory/`: JSON configurations for Linked Services, Datasets, Pipelines
- `scripts/`: Sample CSV file used as test data
- `docs/`: Step-by-step guide to configure SFTP and ADF

---

## 🛠️ Prerequisites

- Azure Subscription
- SFTP server access (can use a public test server or private)
- Azure Blob Storage container created
- Azure Data Factory created

---

## 📝 Steps

1. **Create Linked Services**
   - `SFTPLinkedService.json` — points to the FTP/SFTP server
   - `AzureBlobStorage.json` — points to your Blob container

2. **Create Datasets**
   - `SourceSFTPFile.json` — source file config
   - `SinkBlobFile.json` — target file in blob

3. **Create Pipeline**
   - `SFTPtoBlobPipeline.json` — copy pipeline for SFTP to Blob transfer

4. **Deploy and Run**
   - Import JSONs via ADF Author UI or ARM template

---

## 🧪 Sample File

Check `scripts/SampleCSVFile.csv` to upload to your SFTP server for testing.

---

## 🤝 Contributions

Pull requests are welcome.

---

## 📄 License

MIT License

--Create Incremental Load Pipeline and Automate This on a Daily Basis:--
# ADF Incremental Load Pipeline with Daily Automation

This repository demonstrates how to implement an **incremental data load pipeline** using **Watermarking** in Azure Data Factory and schedule it to run **daily**.

---

## 🔍 Features

- Only loads **new or changed records** since the last run
- Uses **Watermark (LastModifiedDate)** for tracking changes
- Automatically **updates watermark** after each run
- Scheduled to run **daily** using ADF trigger

---

## 📦 Project Structure

- `datafactory/`: JSON files for ADF pipeline, datasets, linked services
- `trigger/`: Daily trigger config
- `scripts/`: SQL setup for source/sink tables and watermark table
- `docs/`: Detailed guide

---

## 🛠️ Requirements

- Azure Subscription
- On-prem or Azure-hosted SQL Server (source)
- Azure SQL Database (sink)
- Azure Data Factory setup

---

## 📝 Setup Steps

1. **Create SQL Tables**
   - Run `scripts/SetupTables.sql` on source and sink DBs

2. **Import ADF Components**
   - Use JSON files to create Linked Services, Datasets, Pipeline

3. **Set Initial Watermark**
   - Manually insert a small datetime in the `WatermarkTable`

4. **Deploy Trigger**
   - Import `DailyTrigger.json` to ADF to automate daily execution

---

## 🤝 Contributions

PRs welcome! File an issue for bugs or features.

---

## 📄 License

MIT License

--Automate a Pipeline to Trigger Every Last Saturday of the Month:--
# ADF: Pipeline Trigger on Last Saturday of the Month

This project demonstrates how to automate an Azure Data Factory pipeline to run on the **last Saturday of each month** using a workaround approach with an Azure Function + HTTP trigger or tumbling window logic.

---

## 🔄 Problem

Azure Data Factory **does not natively support complex date-based triggers** like "last Saturday of the month."

---

## 💡 Solution

Use a custom setup:
1. Schedule a pipeline to run **weekly on Saturdays**
2. Inside pipeline, use **Azure Function (or logic app)** to:
   - Check if today is the **last Saturday of the month**
   - If yes → proceed with the main pipeline
   - If no → do nothing (or exit gracefully)

---

## 🧰 Tools Used

- Azure Data Factory
- Azure Function (Timer + HTTP)
- Conditional activity in ADF
- Optional: Azure Logic Apps or Tumbling Window

---

## 🛠️ Setup Steps

1. Deploy the `MonthlyReportingPipeline.json` to ADF
2. Create Azure Function using `CalculateLastSaturdayFunction.json`
3. Add the function URL in your ADF pipeline Web activity
4. Schedule ADF pipeline to run **every Saturday**
5. The function will return `true` only if it’s the last Saturday

---

## 📄 License

MIT License

--Retrieving data. Wait a few seconds and try to cut or copy again: --
# ADF Resilient Pipeline with Retry and Wait Logic

This project sets up an Azure Data Factory pipeline that **automatically retries data extraction** if a transient failure occurs. Ideal for unstable sources like FTP, SFTP, REST, or intermittent SQL.

---

## 💡 Features

- Graceful handling of transient errors
- Retry mechanism with delay between attempts
- Configurable retry count and interval
- Improves pipeline reliability and availability

---

## 🧰 Use Cases

- “Wait a few seconds and try again” scenarios
- Failing API calls or FTP reads
- Reducing failed pipeline runs due to momentary issues

---

## 📦 Structure

- `pipelines/ResilientCopyPipeline.json` - Implements retry/wait logic
- `linkedservices/` - Placeholder for source/sink
- `datasets/` - Source & sink config

---

## 📄 License

MIT License
