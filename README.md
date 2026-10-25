# Telecom ETL Pipeline Using SSIS and SQL Server

## Project Overview

This project implements an ETL pipeline for telecom transaction data
using **SQL Server Integration Services (SSIS)** and **Microsoft SQL
Server**.

The pipeline processes flat-file data, manages batch execution, applies
data transformations, performs subscriber lookups, loads transaction
records, handles source and destination errors, and maintains audit
information.

## Technologies Used

-   SQL Server
-   SQL Server Integration Services (SSIS)
-   T-SQL
-   CSV / Flat Files
-   Git and GitHub

## Architecture

The project contains two main workflows.

### 1. File Processing and Database Loading

The Control Flow uses a `Foreach Loop Container` to process files
iteratively.

1.  **Create Batch ID** --- initializes batch tracking.
2.  **Foreach Loop Container** --- iterates through the configured input
    files.
3.  **Insert Batch ID** --- records batch information.
4.  **Load Database File** --- loads the current file into SQL Server
    through the ETL package.
5.  **Update Dim Audit** --- updates audit information after processing.
6.  **Copy File** --- copies processed files to the configured archive
    location.
7.  **Move File** --- provides a file-management step within the
    workflow.

The package uses project-level variables to support dynamic file
processing and batch tracking.

### 2. Transaction Data Flow

The Data Flow processes transaction records through several
transformation and loading steps.

-   **Flat File Source:** Reads transaction records from CSV files.
-   **Flat File Source Error Output:** Redirects source errors for
    separate handling.
-   **Create Audit ID:** Adds audit-related identification to the
    error-processing path.
-   **Row Count:** Tracks the number of records passing through selected
    pipeline stages.
-   **Derived Column:** Creates or modifies derived values used in
    subsequent processing.
-   **Lookup Subscriber ID:** Looks up subscriber information using a
    reference dataset.
-   **Create Subscriber ID:** Prepares the subscriber identifier for
    transaction loading.
-   **Create SNR and TAC:** Derives the SNR and TAC values used by the
    transaction workflow.
-   **OLE DB Destination:** Loads transaction records into SQL Server.
-   **OLE DB Destination Error Output:** Redirects destination errors
    for separate processing.

Dedicated error-output paths help separate rejected records from the
main transaction flow.

## Project Variables

The SSIS project includes the following project-level variables:

  -----------------------------------------------------------------------
  Variable                            Purpose
  ----------------------------------- -----------------------------------
  `audit_id`                          Stores the audit identifier.

  `batch_id`                          Stores the batch identifier.

  `ff_dest_archive`                   Defines the destination archive
                                      folder for processed files.

  `ff_scr_file_extension`             Stores the source file extension,
                                      such as `csv`.

  `ff_scr_file_name`                  Stores the source file name without
                                      its extension.

  `ff_scr_folder_name`                Defines the source folder
                                      containing input files.

  `ff_scr_full_path`                  Stores the full source file path,
                                      built from the file-related
                                      variables.

  `rs_dest_error`                     Stores a row-count value associated
                                      with destination errors.

  `rs_extracted_error`                Stores a row-count value associated
                                      with extracted source errors.

  `rs_extracted_process`              Stores a row-count value associated
                                      with processed or extracted
                                      records.

  `rs_pre_insert`                     Stores a row-count value associated
                                      with records before insertion.
  -----------------------------------------------------------------------

The variables support file-path configuration, batch tracking, audit
identification, and row-count monitoring.

## Parameters and Configuration

The project includes an SSIS project parameter file and
connection-manager configuration. Review the actual parameter
definitions and connection managers in Visual Studio before running the
package.

Configuration includes source file locations, file naming, archive
paths, flat-file connections, and SQL Server connections. Update local
file paths and connection settings for your environment.

## Error Handling

The pipeline includes separate error outputs for flat-file source
processing and OLE DB destination loading.

These paths support:

-   Separating invalid source records from the main data flow.
-   Capturing records that fail during destination insertion.
-   Counting records at selected pipeline stages.
-   Supporting error investigation and data-quality checks.

The repository also includes SQL scripts for source and destination
error-output tables.

## Audit Logging

Audit-related functionality is supported by:

-   `audit_id`
-   `batch_id`
-   An audit update task
-   SQL scripts for audit table creation and audit record insertion

This design helps associate processing activity with individual batches
and audit records.

## Repository Structure

``` text
ETL-Telecom-SSIS/
├── source_files/
│   ├── batch 0/
│   ├── batch 1/
│   ├── batch 2/
│   └── data clean/
├── sql_files/
│   ├── 00_create_database.sql
│   ├── 01_create_fact_transaction.sql
│   ├── 02_imsi_reference.sql
│   ├── 03_create_err_source_output.sql
│   ├── 04_create_err_destination_output.sql
│   ├── 51_create_dim_audit.sql
│   ├── 52_usp_insert_dim_audit.sql
│   └── 99_ssis_sql_tasks.sql
└── ssis/
    ├── ETL etisalat.sln
    ├── ETL etisalat.dtproj
    ├── PROJECT.dtsx
    ├── Project.params
    └── Flat File Connection Manager.conmgr
```

## How to Run

1.  Clone the repository.
2.  Open `ssis/ETL etisalat.sln` in Visual Studio with the SQL Server
    Integration Services Projects extension installed.
3.  Review the project parameters, variables, and connection managers.
4.  Update source paths, archive paths, and SQL Server connection
    settings for your environment.
5.  Review the SQL scripts and create the required database objects.
6.  Build and execute the SSIS package.
7.  Validate the loaded transactions, row counts, audit records, and
    error outputs.

## Key Skills Demonstrated

-   ETL pipeline development using SSIS
-   Dynamic file processing with Foreach Loop Container
-   Project-level variable management
-   Flat-file data ingestion
-   Data transformation using Derived Column
-   Reference-data matching using Lookup
-   SQL Server destination loading
-   Error-output handling
-   Row-count monitoring
-   Batch processing and audit logging
-   File archiving and workflow automation

## Author

**Nader Alaa Abdelwahed**

GitHub:
[naderalaaabdelwahed-lab](https://github.com/naderalaaabdelwahed-lab)

------------------------------------------------------------------------

This project demonstrates practical implementation of data integration,
transaction processing, error handling, and audit tracking using SSIS
and SQL Server.
