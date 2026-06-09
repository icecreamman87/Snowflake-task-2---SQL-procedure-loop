# SQL Procedure Loop: PostgreSQL vs Snowflake

## 📌 Project Overview
This repository contains a solution for iterative data processing across two different database management systems. 
**The Goal:** Write a script that iterates through a predefined set of dates (a static dataset) and sequentially calls a stored procedure for each row, passing the required parameters.

## 🛠 Tech Stack
* **PostgreSQL** (PL/pgSQL)
* **Snowflake** (Snowflake Scripting)

## 📁 Repository Structure
* `postgres_solution.sql` — The classic implementation of a loop using `FOR ... LOOP` within an anonymous `DO $$` block. It includes real-time log output using `RAISE NOTICE`.
* `snowflake_solution.sql` — The cloud-native implementation using an `EXECUTE IMMEDIATE` block and an explicit `CURSOR`.

## 💡 Key Engineering Patterns Applied
1. **Database Dialect Adaptation:** The solution addresses and solves the architectural differences between traditional relational databases (Postgres) and cloud data warehouses (Snowflake).
2. **Advanced Cursor Handling:** In Snowflake, an explicit cursor combined with variable unpacking (`:=`) was implemented to bypass the limitation of passing multi-column record fields directly into procedures.
3. **Enterprise DWH Logging:** The Snowflake script automatically creates an audit table (`execution_log`) and records every loop step using `INSERT` statements, ensuring full process traceability.