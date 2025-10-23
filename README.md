# Customer-Data-Integration-Pipeline-Using-Informatica-PowerCenter
This project demonstrates a simple ETL pipeline built using Informatica PowerCenter to extract, join, and load customer data from relational sources into a target table.

## 🔧 Technologies Used
- Informatica PowerCenter (Mapping Designer)
- SQL Server (source)
- ODBC & Relational Connections
- Target: SQL Server

## 📌 Pipeline Overview
Source Qualifier (Customers_info) → Target (Customers_info_tgt)

## 🗂️ Components
- **Source**: Customer data with fields `Name`, `Age`, `Country`
- **Joiner**: Combines multiple source streams based on `CustomerID` or `Country`
- **Target**: Unified customer table for reporting

## 🔐 Connection Setup
ODBC and Relational connections configured via Repository Manager and Workflow Manager.
To enable seamless data integration between Informatica PowerCenter and external databases, this project uses both ODBC and Relational Connections:
- ODBC Connection:
Configured via Informatica Repository Manager to establish connectivity with SQL Server. This allows metadata import and runtime access to source tables. The ODBC driver ensures compatibility and secure communication between Informatica and the database.
- Relational Connection:
Defined in Workflow Manager to specify runtime database credentials, connection strings, and environment-specific parameters. This connection is used during session execution to fetch source data and load it into the target.
## 🔄 Mapping Flow
Source Qualifier (Customers_info)  → Target (Customers_info_tgt)
- Source Qualifier (Customers_info):
Extracts customer data from a relational source using a defined SQL query.
- Target (Customers_info_tgt):
Stores the final, joined dataset in a structured format for reporting or downstream processing.
## 🚀 Workflow Execution & Monitoring
Once mapping is complete in Informatica PowerCenter Designer—where Customers_info acts as the source qualifier and Customers_info_tgt is the target—then move to Workflow Manager to execute and monitor the data flow. The first step is to create a session task that links to the mapping. This session acts as the runtime container that controls how data is extracted, transformed, and loaded.
In Workflow Manager’s Task Developer,  begin by creating a new session, named s_Customers_info. During session creation, it’ll prompt to associate it with the mapping that is built earlier. Once linked, need to configure the session properties to define how Informatica connects to the source and target systems.For the source, assign ODBC connection that points to the database containing the Customers_info table. For the target, assign a connection that allows Informatica to write to the destination table Customers_info_tgt.
Next, switch to Workflow Designer to create a workflow that orchestrates the session. Create a new workflow, named wf_Customers_info, and drag the session task into the workflow canvas. Then connect the Start task to the session to define the execution flow.
Once the workflow is built,  start it by right-clicking and selecting “Start Workflow.” This triggers the session execution. To monitor progress,  open Workflow Monitor, to view the session status in real time. Check whether the session succeeded, failed, or completed with warnings. If any errors occur, the Log Viewer provides detailed diagnostics to troubleshoot.
When everything is configured correctly, the data present in Customers_info will be extracted by the Source Qualifier, and loaded into Customers_info_tgt. The Workflow Monitor confirms the success of this operation.

## SQL Query used
create database InformaticaRepo
create schema etl authorization dbo;
go
USE InformaticaRepo;
go
CREATE TABLE etl.Customers_info (
    CustomerID INT PRIMARY KEY,
    CustomerName NVARCHAR(100),
    Country NVARCHAR(50)
);
BULK INSERT etl.Customers_info
FROM 'C:\Users\geeti\Downloads\customers_info.csv'
WITH (
  
    FIRSTROW = 1
);
go
select * from etl.Customers_info;
go

CREATE SCHEMA etl_tgt AUTHORIZATION dbo;
GO

CREATE TABLE etl_tgt.Customers_info_tgt (
    CustomerID INT PRIMARY KEY,
    CustomerName NVARCHAR(100),
    Country NVARCHAR(50)
);
GO

SELECT count(*) FROM etl_tgt.Customers_info_tgt ;
go

## 📈 Business Impact
- Unified customer data from multiple systems
- Improved data quality and consistency
- Reusable mapping template for future datasets



## 🧪 Screenshots

![SourceQualifier:Customers_info] (SQ_customers_info.png)
