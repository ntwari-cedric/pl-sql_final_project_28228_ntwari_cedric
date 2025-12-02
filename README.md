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
![BPMN Diagram](https://github.com/ntwari-cedric/ntwari-cedric-pl-sql_final_project_28228_ntwari_cedric/blob/main/bpmn%20diagram.png?raw=true)

## 2.2 UML Activity Diagram – System Workflow

The UML Activity Diagram illustrates the technical workflow:

Actor interactions

Accident record processing

Blackspot evaluation logic

Analytics update

UML Activity Diagram:
![UML Diagram](https://github.com/ntwari-cedric/ntwari-cedric-pl-sql_final_project_28228_ntwari_cedric/blob/main/UML%20diagram.png?raw=true)

## 2.3 Explanation

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
![pdb creation](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/create%20pgbdata%20base.png?raw=true)
## 4.2 Tablespace Configuration and Storage
Dedicated tablespaces were created for data, indexes, and temporary files. All data files use the AUTOEXTEND ON parameter.

Commands (Executed inside the PDB):
![configaration of pdb](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/configuration.png?raw=true)

## 4.3 User Setup and Application Credentials
The main user for all object creation is road_accident_app, created with appropriate privileges (CONNECT, RESOURCE).

Commands (Executed inside the PDB):
![tablespace creation](https://github.com/ntwari-cedric/pl-sql_final_project_28228_ntwari_cedric/blob/main/tablespace%20creation.png?raw=true)

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
