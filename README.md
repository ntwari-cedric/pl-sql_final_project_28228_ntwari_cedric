Road Accident Reporting & Blackspot Detection System

##  Project Overview

This is a multi-phase individual capstone project centered on Oracle database design, PL/SQL development, and Business 
Intelligence (BI) implementation. The goal is to build a production-ready database solution with analytical capabilities.

* **Course:** Database Development with PL/SQL (INSY 8311)
* **Institution:** Adventist University of Central Africa (AUCA)
* **Student:** NTWARI Cedric
* **Student ID:** 28228

---

## 1 Problem Statement (Phase I)

The system addresses the core issue of **delayed and manual identification of accident-prone areas (blackspots)**.
Conventional road accident reporting relies on manual aggregation of data, which delays critical safety interventions.

## 1.1. Solution and Innovation

The key innovation lies in the **automatic identification of blackspots** using pre-defined accident frequency and severity thresholds. 
This automation, implemented via PL/SQL triggers and stored procedures, ensures **near real-time updates and analysis**.

### Key Objectives

* Identify high-risk road segments quickly.
* Prioritize road safety improvements efficiently.
* Improve emergency response planning and resource allocation.

### Technical Approach

The system uses PL/SQL triggers and stored procedures to ensure continuous monitoring, automated blackspot updates,
and accurate analytics in near real-time.

## 1.2. Database Schema Summary

The system is built on a relational structure featuring 5 core tables:

| Table Name | Purpose | Key Attributes |
| :--- | :--- | :--- |
| **ACCIDENTS** | Detailed accident records (severity, cause, injuries, deaths). | `Accident ID` (PK), `Location ID` (FK). |
| **LOCATIONS** | Unique road segment identifiers and coordinates for mapping. | `Location ID` (PK). |
| **BLACKSPOTS** | Summary table tracking high-risk areas and risk level. | `Risk Level` ('High', 'Medium', 'Low'). |
| **USERS** | System access and user roles (Admin, Analyst, Operator). | `Role`, `Username`. |
| **ANALYTICS** | Summary data for BI reporting based on time periods. | `Time Period`, `Total Accidents`, `Fatal Accidents`. |

## 2. Phase II – Business Process Modeling (UML & BPMN)

This phase models the core business workflow of the Road Accident Reporting & Blackspot Detection System using BPMN and UML diagrams. These diagrams describe how accident information flows from field reporting to automated blackspot updates.

## 2.1 BPMN Diagram – Accident Reporting & Blackspot Detection

The BPMN diagram represents:

-Accident reporting by officers

-Data validation and storage

-Trigger-based blackspot calculation

-Risk-level notification

BPMN Diagram:
![BPMN Diagram](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/bpmn%202.png)

![BPMN Diagram](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/bpmn%201.png)

## 2.2 Explanation

The system follows a structured accident reporting workflow. Traffic officers submit accident data (severity, location, injuries, deaths) into the system. After validation, the data is saved into the ACCIDENTS table. A PL/SQL trigger evaluates whether the accident location meets the threshold to be classified as a blackspot and updates the BLACKSPOTS table automatically.

The BPMN diagram shows the business flow between users and the system, while the UML activity diagram shows the internal system operations. Together, they demonstrate how the system supports MIS objectives such as faster reporting, automated risk detection, and improved road-safety decision-making.

## 3. Phase III – Logical Database Design (ERD + Data Dictionary + 3NF)
## 3.1 ER Diagram (Logical Model)

The ERD for the Road Accident Reporting & Blackspot Detection System contains five core entities with the required relationships:

| Entity A  | Relationship | Entity B   | Cardinality                             | Notes                             |
| --------- | ------------ | ---------- | --------------------------------------- | --------------------------------- |
| LOCATIONS | 1 → Many     | ACCIDENTS  | One location can have many accidents    | `Location_ID` = FK in `ACCIDENTS` |
| LOCATIONS | 1 → 1        | BLACKSPOTS | One location has one blackspot          | `Location_ID` = UNIQUE FK         |
| LOCATIONS | 1 → Many     | ANALYTICS  | One location has many analytics entries | `Location_ID` = FK                |
| USERS     | 1 → Many     | ACCIDENTS  | One user may record many accidents      | `User_ID` optional FK             |
## ERD Structure

LOCATIONS (PK: Location_ID)
 ├── GPS_Coordinates
 ├── Road_Name
 ├── District
 ├── Sector
 └── Created_At
         │
         │ 1..∞
         ▼
ACCIDENTS (PK: Accident_ID)
 ├── Location_ID (FK)
 ├── User_ID (FK)
 ├── Severity
 ├── Cause
 ├── Injuries
 ├── Deaths
 ├── Accident_Time
 └── Created_At

LOCATIONS 1 ─── 1 BLACKSPOTS (Unique FK: Location_ID)

LOCATIONS 1 ─── ∞ ANALYTICS (FK: Location_ID)

USERS 1 ─── ∞ ACCIDENTS (FK: User_ID)


## 3.2 Data Dictionary
LOCATIONS
| Attribute       | Type          | Constraints      |
| --------------- | ------------- | ---------------- |
| Location_ID     | NUMBER(10)    | PK, NOT NULL     |
| GPS_Coordinates | VARCHAR2(50)  | NOT NULL, UNIQUE |
| Road_Name       | VARCHAR2(100) | NOT NULL         |
| District        | VARCHAR2(50)  | NOT NULL         |
| Sector          | VARCHAR2(50)  | NULL             |
| Created_At      | DATE          | DEFAULT SYSDATE  |

ACCIDENTS
| Attribute     | Type                     | Constraints                     |
| ------------- | ------------------------ | ------------------------------- |
| Accident_ID   | NUMBER(10)               | PK, NOT NULL                    |
| Location_ID   | NUMBER(10)               | FK, NOT NULL                    |
| User_ID       | NUMBER(10)               | FK, NULL                        |
| Severity      | NUMBER(1)                | CHECK(Severity BETWEEN 1 AND 5) |
| Cause         | VARCHAR2(200)            | NULL                            |
| Injuries      | NUMBER                   | DEFAULT 0                       |
| Deaths        | NUMBER                   | DEFAULT 0                       |
| Accident_Time | TIMESTAMP WITH TIME ZONE | NOT NULL                        |
| Created_At    | DATE                     | DEFAULT SYSDATE                 |

BLACKSPOTS
| Attribute       | Type         | Constraints                                  |
| --------------- | ------------ | -------------------------------------------- |
| Blackspot_ID    | NUMBER(10)   | PK, NOT NULL                                 |
| Location_ID     | NUMBER(10)   | FK, UNIQUE, NOT NULL                         |
| Risk_Level      | VARCHAR2(10) | CHECK(Risk_Level IN ('High','Medium','Low')) |
| Total_Accidents | NUMBER       | DEFAULT 0                                    |
| Updated_At      | DATE         | DEFAULT SYSDATE                              |

ANALYTICS
| Attribute       | Type       | Constraints     |
| --------------- | ---------- | --------------- |
| Analytics_ID    | NUMBER(10) | PK, NOT NULL    |
| Location_ID     | NUMBER(10) | FK, NOT NULL    |
| Time_Period     | DATE       | NOT NULL        |
| Total_Accidents | NUMBER     | NOT NULL        |
| Fatal_Accidents | NUMBER     | NOT NULL        |
| Updated_At      | DATE       | DEFAULT SYSDATE |

USERS
| Attribute     | Type          | Constraints                                  |
| ------------- | ------------- | -------------------------------------------- |
| User_ID       | NUMBER(10)    | PK, NOT NULL                                 |
| Username      | VARCHAR2(50)  | UNIQUE, NOT NULL                             |
| Role          | VARCHAR2(20)  | CHECK(Role IN ('Admin','Analyst','Officer')) |
| Password_Hash | VARCHAR2(200) | NOT NULL                                     |
| Created_At    | DATE          | DEFAULT SYSDATE                              |


## 3.3 Normalization Check (3NF)
| Table      | 3NF Check Result |
| ---------- | ---------------- |
| LOCATIONS  | Passes 3NF       |
| ACCIDENTS  | Passes 3NF       |
| BLACKSPOTS | Passes 3NF       |
| ANALYTICS  | Passes 3NF       |
| USERS      | Passes 3NF       |


📌 Conclusion: Entire database is fully normalized to Third Normal Form (3NF).

## 3.4 Assumptions

1. Time_Period in ANALYTICS represents an aggregation period (e.g., monthly).

2. Passwords stored in USERS will be encrypted/hashed.

3. A single Location_ID uniquely identifies a road segment blackspot.

4. Users can optionally be linked to accidents (for audit purposes).

5. BLACKSPOTS table is maintained automatically by PL/SQL triggers.

6. GPS coordinates are stored as unique identifiers for locations.

## 3.5 EER DIAGRAM
![EER Diagram](https://github.com/ntwari-cedric/ntwari-cedric-pl-sql_final_project_28228_ntwari_cedric/blob/main/EER%20Diagram.png?raw=true)

The EER Diagram for the Road Accident Reporting & Blackspot Detection System visually confirms the normalized data model (Phase III) required to support the project's automation goals. The model centers on LOCATIONS as the primary entity, which links to three critical tables: ACCIDENTS via a 1 to Many relationship to record transactional data; BLACKSPOTS via a 1 to 1 relationship to hold the single, current risk classification status (High, Medium, Low); and ANALYTICS via a 1 to Many relationship to store pre-aggregated key performance indicators (KPIs) over various time periods. This structure is essential because it allows the PL/SQL Compound Trigger (Phase VII) to check the history of $\text{ACCIDENTS}$ for a specific $\text{Location ID}$ and quickly update the $\text{BLACKSPOTS}$ status, providing the near real-time intelligence needed for traffic authority decision-making.
 

##  4.Database Creation(phase IV)
Objective: Create and configure Oracle pluggable database.

## 4.1 Database Creation (PDB Setup)
The environment uses a dedicated Pluggable Database named 
tue_28228_cedric_roadaccident_db.
# Command (Executed as SYSDBA):
```sql
-- Phase IV: Database Creation for PDTAS
-- Student: NTWARI CEDRIC (ID: 28228)
-- Group: Tuesday (B)
-- Date: December 2024

-- Step 1: Create Pluggable Database
CREATE PLUGGABLE DATABASE WED_28228_cedric_PDTAS_DB
  ADMIN USER cedric_admin IDENTIFIED BY ntwari
  ROLES = (DBA)
  FILE_NAME_CONVERT = (
    'C:\dbms_oracle\oradata\XE\pdbseed\',
    'C:\dbms_oracle\oradata\XE\tue_28228_cedric_PDTAS_DB\'
  );

-- Step 2: Open PDB
ALTER PLUGGABLE DATABASE tue_28228_CEDRIC_PDTAS_DB OPEN;
ALTER PLUGGABLE DATABASE TUE_28228_CEDRIC_PDTAS_DB SAVE STATE;

-- Step 3: Switch to PDB
ALTER SESSION SET CONTAINER =TUE_28228_CEDRIC__PDTAS_DB;
```
## 4.2 Tablespace Configuration and Storage
Dedicated tablespaces were created for data, indexes, and temporary files. All data files use the AUTOEXTEND ON parameter.
The main user for all object creation is road_accident_app, created with appropriate privileges (CONNECT, RESOURCE).
```sql
-- Step 4: Create Tablespaces
CREATE TABLESPACE pdta_data 
DATAFILE 'C:\dbms_oracle\oradata\XE\TUE_28228_CEDRIC_PDTAS_DB\pdta_data01.dbf'
SIZE 50M AUTOEXTEND ON NEXT 10M MAXSIZE 500M;

CREATE TABLESPACE pdta_index
DATAFILE 'C:\dbms_oracle\oradata\XE\TUE_28228_CEDRIC_PDTAS_DB\pdta_index01.dbf'
SIZE 20M AUTOEXTEND ON NEXT 5M MAXSIZE 200M;

CREATE TEMPORARY TABLESPACE pdta_temp
TEMPFILE 'C:\dbms_oracle\oradata\XE\TUE_28228_CEDRIC_PDTAS_DB\pdta_temp01.dbf'
SIZE 20M AUTOEXTEND ON NEXT 5M MAXSIZE 100M;

-- Step 5: Create Application User
CREATE USER patient_track IDENTIFIED BY NTWARI
DEFAULT TABLESPACE pdta_data
TEMPORARY TABLESPACE pdta_temp
QUOTA UNLIMITED ON pdta_data;

GRANT CONNECT, RESOURCE, DBA TO patient_track;

-- Verification Queries
SELECT name, open_mode FROM v$pdbs;
SELECT tablespace_name, status FROM dba_tablespaces;
SELECT username, account_status FROM dba_users;
```
after doing all thing ( creating and config the plugable database i create i connect it to my oracle sql developer where all the remaining phase will take place

## TABLE IMPLEMENTATION & DATA INSERTION (phase V)
## Database Schema Creation & Data Population
Successfully executed all CREATE TABLE statements with proper constraints,
primary keys, foreign keys, and business rules. The foundation of our Road
Accident Reporting System is now structurally complete and ready for data
management operations.
![table creation](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/table%20creation.png?raw=true)
## Automated Primary Key Sequences
Implemented Oracle sequences for all primary key columns to ensure unique
identifier generation. This automated system guarantees data integrity and
prevents duplicate entries while supporting scalable data growth across
all entity tables

![primary key sequences](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/create%20sequence%20id.png?raw=true)
## Geographical Road Locations Data
Populated the LOCATIONS table with 15 authentic Kigali road segments
complete with GPS coordinates. This geographical foundation enables
precise accident mapping and blackspot detection analysis throughout
the city's road network.

![table location](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/location%20table.png?raw=true)
## Comprehensive Accident Records
Loaded 10 detailed accident reports demonstrating various severity levels
(Minor, Serious, Fatal) and diverse causes. Each record includes casualty
statistics, timestamps, and location references for comprehensive traffic
safety analysis and reporting.

![table accident](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/accident%20table.png?raw=true)
## Role-Based User Management System
Configured 5 system user accounts with distinct roles (Admin, Analyst, Operator)
implementing role-based access control. This security structure ensures
appropriate data access levels while maintaining system integrity and
audit compliance.

![table user](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/user%20table.png?raw=true)

## Database Interaction & Transactions(PHASE VI)

### procedure

first i create This PL/SQL stored procedure is used to insert a new accident report into the accidents table.
It also generates a new accident ID automatically using a sequence and returns it to the caller
```sql
create or replace PROCEDURE add_accident_report (
    p_location_id IN NUMBER,
    p_date_time IN TIMESTAMP,
    p_severity IN VARCHAR2,
    p_cause IN VARCHAR2,
    p_injuries IN NUMBER,
    p_deaths IN NUMBER,
    p_accident_id OUT NUMBER
)
IS
BEGIN
    -- Generate new accident ID from sequence
    SELECT seq_accidents_id.NEXTVAL INTO p_accident_id FROM DUAL;

    -- Insert new accident record
    INSERT INTO accidents (accident_id, location_id, date_time, severity, cause, injuries, deaths)
    VALUES (p_accident_id, p_location_id, p_date_time, p_severity, p_cause, p_injuries, p_deaths);

    -- Commit the transaction
    COMMIT;

    DBMS_OUTPUT.PUT_LINE('Accident report added successfully. ID: ' || p_accident_id);

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error adding accident report: ' || SQLERRM);
        ROLLBACK;
        RAISE;
END add_accident_report;
```
The generate_blackspots procedure automatically analyzes all accident records in the system and identifies
blackspots—locations with repeated accidents. A blackspot is defined as any location that has two or more accidents.

This procedure helps the system determine danger-prone roads, calculate their risk level, and update the blackspots table accordingly.
```sql
create or replace PROCEDURE generate_blackspots
IS
    CURSOR location_cursor IS
        SELECT l.location_id, l.road_name, COUNT(a.accident_id) as accident_count
        FROM locations l
        JOIN accidents a ON l.location_id = a.location_id
        GROUP BY l.location_id, l.road_name
        HAVING COUNT(a.accident_id) >= 2;  -- Locations with 2+ accidents

    v_risk_level VARCHAR2(20);
    v_blackspot_id NUMBER;
BEGIN
    -- Clear existing blackspots
    DELETE FROM blackspots;

    FOR location_rec IN location_cursor LOOP
        -- Determine risk level based on accident count
        IF location_rec.accident_count >= 3 THEN
            v_risk_level := 'High';
        ELSIF location_rec.accident_count = 2 THEN
            v_risk_level := 'Medium';
        ELSE
            v_risk_level := 'Low';
        END IF;

        -- Generate new blackspot ID
        SELECT seq_blackspots_id.NEXTVAL INTO v_blackspot_id FROM DUAL;

        -- Insert into blackspots table
        INSERT INTO blackspots (blackspot_id, location_id, accident_count, risk_level)
        VALUES (v_blackspot_id, location_rec.location_id, location_rec.accident_count, v_risk_level);

        DBMS_OUTPUT.PUT_LINE('Blackspot identified: ' || location_rec.road_name || 
                            ' - ' || location_rec.accident_count || ' accidents - ' || v_risk_level);
    END LOOP;

    COMMIT;
    DBMS_OUTPUT.PUT_LINE('Blackspot generation completed successfully.');

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error generating blackspots: ' || SQLERRM);
        ROLLBACK;
        RAISE;
END generate_blackspots;
```

### function


The get_severity_statistics function returns a summarized breakdown of all accidents recorded in the system based on their severity level. It analyzes the accidents table and counts how many accidents are categorized as Minor, Serious, and Fatal. After calculating these totals, the function formats them into a single readable text string such as:

“Minor: X | Serious: Y | Fatal: Z”

This makes it easy to display accident severity statistics in dashboards, reports, or monitoring interfaces without writing multiple separate queries. The function also includes safe error handling to ensure that if any issue occurs during calculation, it returns a clear error message instead of interrupting the application flow.



```sql
create or replace FUNCTION get_severity_statistics 
RETURN VARCHAR2
IS
    v_minor_count    NUMBER;
    v_serious_count  NUMBER;
    v_fatal_count    NUMBER;
    v_result         VARCHAR2(200);
BEGIN
    -- Count accidents by severity
    SELECT COUNT(*) INTO v_minor_count FROM accidents WHERE severity = 'Minor';
    SELECT COUNT(*) INTO v_serious_count FROM accidents WHERE severity = 'Serious';
    SELECT COUNT(*) INTO v_fatal_count FROM accidents WHERE severity = 'Fatal';

    -- Format the result
    v_result := 'Minor: ' || v_minor_count || 
                ' | Serious: ' || v_serious_count || 
                ' | Fatal: ' || v_fatal_count;

    RETURN v_result;

EXCEPTION
    WHEN OTHERS THEN
        RETURN 'Error calculating statistics';
END get_severity_statistics;
```


### package
This system includes a fully implemented Road Accident Management Package, designed to centralize and manage all accident-related operations in one organized module. The package brings together accident reporting, blackspot analysis, and statistical monitoring, providing a clean and efficient backend for any road-safety, transport, or emergency-response application.

#### 🚦 1. Purpose of the Package

The package acts as the core engine of the Road Accident Management System.
It combines multiple database operations that work together to:

Record accident information

Analyze accident history

Detect dangerous road locations (blackspots)

Produce statistical summaries

Improve data quality and consistency

Provide a unified API for the entire system

Instead of having separate, scattered procedures and functions, the package organizes everything into one professional, maintainable structure.

#### 🧩 2. What the Package Contains

The package includes three major components, each responsible for a key part of the system:

##### A. Accident Reporting Operations

These features handle storing accident information in the database.
They:

Accept accident details from users or applications

Validate and process the input

Automatically generate unique accident IDs

Insert the data into the accidents table

Commit the transaction and confirm successful submission

This ensures every accident report is stored accurately and consistently.

##### B. Blackspot Analysis Module

This part of the package identifies high-risk road areas by analyzing accident history.
It is responsible for:

Scanning all recorded accidents

Counting accidents per location

Identifying locations with repeated accidents

Assigning a risk level (Medium or High)

Refreshing the blackspots table with new results

This helps authorities recognize dangerous areas and take preventive measures.

##### C. Statistical Summary Functions

The package includes a statistical function that summarizes accident data by severity.
It analyzes the entire accident dataset and returns a formatted report showing:

Total Minor accidents

Total Serious accidents

Total Fatal accidents

The result is returned as a clean text summary, useful for dashboards, reports, and monitoring tools.

#### 🔒 3. Why Packaging Was Necessary

Using a package instead of separate standalone procedures provides several advantages:

✔ Better Organization

All accident-related logic is located in one place, making the system clean and professional.

✔ Improved Reusability

Other systems, dashboards, or backend services can call the same packaged procedures and functions without duplication.

✔ Higher Security

Only the package specification is visible externally.
Internal logic remains protected inside the package body.

✔ Faster Performance

Oracle loads the entire package into memory once, improving execution speed for repeated calls.

✔ Easy Maintenance & Scalability

You can update logic inside the package without affecting the application using it.
This makes the system flexible and future-proof.

#### 🚀 4. Overall Benefit to the Road Accident System

The package forms the backbone of the Road Accident Management System, enabling:

Efficient accident reporting

Automated blackspot detection

Real-time severity statistics

Clean, standardized workflows

Faster development for future features

It ensures that all accident-related features operate smoothly, safely, and consistently.

 ## Advanced Programming & Auditing(PHASE VII)
