# SQL Procedural Loops & Optimization

## Project Overview
This project demonstrates advanced SQL procedural programming by refactoring a hardcoded sequence of consecutive stored procedure calls (`CALL procedure(...)`) with varying timestamps into an optimized, dynamic, and transaction-safe loop. Solutions are provided for both **PostgreSQL** and **Snowflake**.

## Files in this Project
* `postgres_solution.sql` - PL/pgSQL implementation using `RECORD` types.
* `snowflake_solution.sql` - Snowflake Scripting implementation featuring both `CURSOR` and `RESULTSET` approaches.

## Key Engineering Concepts Demonstrated

### 1. Separation of Data and Logic
Removed hardcoded values and `UNION ALL` statements from the procedure body. The dataset of varying intervals and timestamps is now stored in a dedicated configuration table (`test_2_params`). The procedure logic is entirely decoupled from the data, making it highly scalable — new dates can be processed simply by inserting rows into the table without altering the SQL code.

### 2. Transactional Safety (Rollback Protection)
Addressed the critical risk of logging execution states directly to a physical database table (`INSERT INTO execution_log`). If a row fails during the loop, a database `ROLLBACK` would erase all previous log entries within that transaction. 
**Solution:** The logs are captured using the `INTO` keyword and appended to an in-memory `ARRAY`. This ensures logging integrity and guarantees the log history is preserved regardless of the database's transactional state.

### 3. Snowflake Advanced Memory Management
Implemented two distinct versions in Snowflake to showcase different memory utilization strategies:
* **Cursor Version:** Uses traditional, row-by-row pointer iteration. This approach is highly memory-efficient and safe for processing massive datasets where loading everything into RAM is not feasible.
* **ResultSet Version:** Materializes the entire query result in-memory before iteration. This approach avoids keeping a database cursor open and executes significantly faster for smaller configuration tables.

### 4. PostgreSQL (PL/pgSQL) Dynamic Execution
Utilized `RECORD` types and a `FOR ... IN SELECT` loop to dynamically iterate through the parameter table, seamlessly passing row attributes as arguments to the base procedure.