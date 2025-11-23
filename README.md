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

