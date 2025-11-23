Road Accident Reporting & Blackspot Detection System

## 1. Project Overview

This is a multi-phase individual capstone project centered on Oracle database design, PL/SQL development, and Business 
Intelligence (BI) implementation. The goal is to build a production-ready database solution with analytical capabilities.

* **Course:** Database Development with PL/SQL (INSY 8311)
* **Institution:** Adventist University of Central Africa (AUCA)
* **Student:** NTWARI Cedric
* **Student ID:** 28228

---

## 2. Problem Statement (Phase I)

The system addresses the core issue of **delayed and manual identification of accident-prone areas (blackspots)**.
Conventional road accident reporting relies on manual aggregation of data, which delays critical safety interventions.

## 3. Solution and Innovation

The key innovation lies in the **automatic identification of blackspots** using pre-defined accident frequency and severity thresholds. 
This automation, implemented via PL/SQL triggers and stored procedures, ensures **near real-time updates and analysis**.

### Key Objectives

* Identify high-risk road segments quickly.
* Prioritize road safety improvements efficiently.
* Improve emergency response planning and resource allocation.

### Technical Approach

The system uses PL/SQL triggers and stored procedures to ensure continuous monitoring, automated blackspot updates,
and accurate analytics in near real-time.

## 4. Database Schema Summary

The system is built on a relational structure featuring 5 core tables:

| Table Name | Purpose | Key Attributes |
| :--- | :--- | :--- |
| **ACCIDENTS** | Detailed accident records (severity, cause, injuries, deaths). | `Accident ID` (PK), `Location ID` (FK). |
| **LOCATIONS** | Unique road segment identifiers and coordinates for mapping. | `Location ID` (PK). |
| **BLACKSPOTS** | Summary table tracking high-risk areas and risk level. | `Risk Level` ('High', 'Medium', 'Low'). |
| **USERS** | System access and user roles (Admin, Analyst, Operator). | `Role`, `Username`. |
| **ANALYTICS** | Summary data for BI reporting based on time periods. | `Time Period`, `Total Accidents`, `Fatal Accidents`. |

## 5. Phase II – Business Process Modeling (UML & BPMN)

This phase models the core business workflow of the Road Accident Reporting & Blackspot Detection System using BPMN and UML diagrams. These diagrams describe how accident information flows from field reporting to automated blackspot updates.

## 5.1 BPMN Diagram – Accident Reporting & Blackspot Detection

The BPMN diagram represents:

-Accident reporting by officers

-Data validation and storage

-Trigger-based blackspot calculation

-Risk-level notification

BPMN Diagram:
![BPMN Diagram](https://github.com/ntwari-cedric/ntwari-cedric-pl-sql_final_project_28228_ntwari_cedric/blob/main/bpmn%20diagram.png?raw=true)

## 5.2 UML Activity Diagram – System Workflow

The UML Activity Diagram illustrates the technical workflow:

Actor interactions

Accident record processing

Blackspot evaluation logic

Analytics update

UML Activity Diagram:
![UML Diagram](https://github.com/ntwari-cedric/ntwari-cedric-pl-sql_final_project_28228_ntwari_cedric/blob/main/UML%20diagram.png?raw=true)

## 5.3 Phase II Explanation

The system follows a structured accident reporting workflow. Traffic officers submit accident data (severity, location, injuries, deaths) into the system. After validation, the data is saved into the ACCIDENTS table. A PL/SQL trigger evaluates whether the accident location meets the threshold to be classified as a blackspot and updates the BLACKSPOTS table automatically.

The BPMN diagram shows the business flow between users and the system, while the UML activity diagram shows the internal system operations. Together, they demonstrate how the system supports MIS objectives such as faster reporting, automated risk detection, and improved road-safety decision-making.

## 6. Phase III – Logical Database Design (ERD + Data Dictionary + 3NF)
## 6.1 ER Diagram (Logical Model)

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


## 6.2 Data Dictionary
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


## 6.3 Normalization Check (3NF)
| Table      | 3NF Check Result |
| ---------- | ---------------- |
| LOCATIONS  | Passes 3NF       |
| ACCIDENTS  | Passes 3NF       |
| BLACKSPOTS | Passes 3NF       |
| ANALYTICS  | Passes 3NF       |
| USERS      | Passes 3NF       |


📌 Conclusion: Entire database is fully normalized to Third Normal Form (3NF).

## 6.4 Assumptions

1. Time_Period in ANALYTICS represents an aggregation period (e.g., monthly).

2. Passwords stored in USERS will be encrypted/hashed.

3. A single Location_ID uniquely identifies a road segment blackspot.

4. Users can optionally be linked to accidents (for audit purposes).

5. BLACKSPOTS table is maintained automatically by PL/SQL triggers.

6. GPS coordinates are stored as unique identifiers for locations.
