# BookMyShow SQL Project
 
A SQL database project modelled on an online movie ticket booking platform, built on **Oracle Database 11g**. It covers schema design, data loading, querying, transactions, access control, and 40 scenario-based questions, all documented phase by phase.
 
## Problem statement
 
An online ticketing platform must store users, movies, theaters, show schedules, seat layouts, bookings, and payments, and answer business questions about them reliably. This project designs that database and works through it using every major SQL statement type.
 
## Objectives
 
- Design a normalized (3NF) schema with proper keys and constraints
- Practise DDL, DML, DQL, TCL, and DCL on a realistic booking flow
- Create role-based database users with controlled privileges
- Solve 40 scenario-based questions that combine subqueries, correlated subqueries, nested subqueries, normalization, single-row functions, multi-row functions, joins, and `GROUP BY`
## Tools used
 
- Database: Oracle Database 11g
- Clients: SQL*Plus (command line) and Oracle SQL Developer
- ER diagram: dbdiagram.io or draw.io
- Version control: Git and GitHub
## Database schema
 
![ER Diagram](erd/er_diagram.png)
 
The database has 8 tables. Table structures are shown below in the same format as `DESCRIBE <table>`.
 
### 1. USERS
 
| Column | Null? | Type |
|---|---|---|
| USER_ID | NOT NULL | NUMBER |
| USER_NAME | | VARCHAR2(100) |
| EMAIL | | VARCHAR2(150) |
| PHONE | | VARCHAR2(15) |
 
### 2. MOVIES
 
| Column | Null? | Type |
|---|---|---|
| MOVIE_ID | NOT NULL | NUMBER |
| MOVIE_NAME | NOT NULL | VARCHAR2(150) |
| GENRE | | VARCHAR2(50) |
| LANGUAGE | | VARCHAR2(30) |
| DURATION | | NUMBER(3) |
| RELEASE_DATE | | DATE |
 
### 3. THEATERS
 
| Column | Null? | Type |
|---|---|---|
| THEATRE_ID | NOT NULL | NUMBER |
| THEATRE_NAME | NOT NULL | VARCHAR2(100) |
| CITY | NOT NULL | VARCHAR2(50) |
 
### 4. SHOWS
 
| Column | Null? | Type |
|---|---|---|
| SHOW_ID | NOT NULL | NUMBER |
| MOVIE_ID | NOT NULL | NUMBER |
| THEATRE_ID | NOT NULL | NUMBER |
| SHOW_DATE | NOT NULL | DATE |
| SHOW_TIME | NOT NULL | VARCHAR2(20) |
| TICKET_PRICE | NOT NULL | NUMBER(8,2) |
 
### 5. SEATS
 
| Column | Null? | Type |
|---|---|---|
| SEAT_ID | NOT NULL | NUMBER |
| THEATRE_ID | NOT NULL | NUMBER |
| SEAT_NUMBER | NOT NULL | VARCHAR2(10) |
| SEAT_TYPE | | VARCHAR2(20) |
| SEAT_PRICE | NOT NULL | NUMBER(8,2) |
 
### 6. BOOKING
 
| Column | Null? | Type |
|---|---|---|
| BOOKING_ID | NOT NULL | NUMBER |
| USER_ID | NOT NULL | NUMBER |
| SHOW_ID | NOT NULL | NUMBER |
| BOOKING_DATE | NOT NULL | DATE |
| TOTAL_AMOUNT | NOT NULL | NUMBER(10,2) |
| BOOKING_STATUS | NOT NULL | VARCHAR2(20) |
 
### 7. BOOKING_SEATS
 
| Column | Null? | Type |
|---|---|---|
| BOOKING_SEAT_ID | NOT NULL | NUMBER |
| BOOKING_ID | NOT NULL | NUMBER |
| SEAT_ID | NOT NULL | NUMBER |
| SHOW_ID | NOT NULL | NUMBER |
| SEAT_PRICE | NOT NULL | NUMBER(8,2) |
 
### 8. PAYMENTS
 
| Column | Null? | Type |
|---|---|---|
| PAYMENT_ID | NOT NULL | NUMBER |
| BOOKING_ID | NOT NULL | NUMBER |
| PAYMENT_DATE | NOT NULL | DATE |
| PAYMENT_METHOD | NOT NULL | VARCHAR2(30) |
| PAYMENT_AMOUNT | NOT NULL | NUMBER(10,2) |
| PAYMENT_STATUS | NOT NULL | VARCHAR2(20) |
 
### Key relationships
 
| Parent | Child | Link column | Type |
|---|---|---|---|
| USERS | BOOKING | USER_ID | 1 : N |
| MOVIES | SHOWS | MOVIE_ID | 1 : N |
| THEATERS | SHOWS | THEATRE_ID | 1 : N |
| THEATERS | SEATS | THEATRE_ID | 1 : N |
| SHOWS | BOOKING | SHOW_ID | 1 : N |
| BOOKING | BOOKING_SEATS | BOOKING_ID | 1 : N |
| SEATS | BOOKING_SEATS | SEAT_ID | 1 : N |
| BOOKING | PAYMENTS | BOOKING_ID | 1 : N |
 
`BOOKING_SEATS` resolves the many-to-many relationship between bookings and seats.
 
## Oracle 11g notes
 
- Oracle has no `CREATE DATABASE` step for a project like this. Create a **schema (user)** named `BOOKMYSHOW` and create all tables inside it.
- Oracle 11g has no `IDENTITY` columns, so auto-increment IDs use **sequences** (and triggers where needed).
- Row limiting uses `ROWNUM` (there is no `FETCH FIRST` in 11g).
- Data types used: `NUMBER`, `VARCHAR2`, `DATE`.
- Oracle commits DDL automatically, while DML needs an explicit `COMMIT`.
## Project structure
 
```
BookMyShow/
├── README.md
├── .gitignore
├── ddl/          Schema setup, table creation, sequences, ALTER scripts
├── dml/          Sample data, UPDATE and DELETE scripts
├── dql/          SELECT examples (functions, joins, subqueries, GROUP BY)
├── tcl/          Transaction scripts (COMMIT, ROLLBACK, SAVEPOINT)
├── dcl/          User creation, GRANT and REVOKE scripts
├── scenarios/    The 40 scenario-based questions
├── erd/          ER diagram image
└── docs/         Full project documentation and screenshots
```
 
## Project phases
 
| Phase | Focus | Folder |
|---|---|---|
| 0 | Setup and documentation skeleton | `docs/` |
| 1 | DDL and normalization | `ddl/` |
| 2 | DML: loading and changing data | `dml/` |
| 3 | DQL: querying with functions, joins, subqueries | `dql/` |
| 4 | TCL: booking transactions | `tcl/` |
| 5 | DCL: users, roles, privileges | `dcl/` |
| 6 | 40 scenario-based questions | `scenarios/` |
| 7 | Final review and insights | `docs/` |
 
## How to run
 
1. Connect as a DBA user (for example `SYSTEM`) and create the project schema:
```sql
CREATE USER bookmyshow IDENTIFIED BY ChangeMe_123
  DEFAULT TABLESPACE users QUOTA UNLIMITED ON users;
GRANT CREATE SESSION, CREATE TABLE, CREATE SEQUENCE, CREATE VIEW, CREATE TRIGGER TO bookmyshow;
```
 
2. Connect as `bookmyshow`.
   - **Command line:** `sqlplus bookmyshow/<password>@<service_name>`
   - **SQL Developer:** create a new connection with the `bookmyshow` user and open each script in the SQL worksheet.
3. Run the scripts in this order:
```sql
@ddl/create_tables.sql
@dml/insert_data.sql
@dql/select_examples.sql
@tcl/transactions.sql
@dcl/user_creation.sql
@scenarios/q01_to_q40.sql
```
 
> Update the file names above if yours differ. Run `ddl` and `dml` first, since every later script depends on the tables and data. Run `dcl/user_creation.sql` as a DBA, since creating users needs DBA privileges. In SQL Developer, use **Run Script (F5)** to run a whole file.
 
## Database users and roles
 
| User | Access level |
|---|---|
| `bms_admin` | Full access |
| `bms_manager` | Manage movies, theaters, and shows |
| `bms_support` | Read bookings and payments, update booking status |
| `bms_analyst` | Read-only on all tables |
| `bms_customer` | Insert bookings, read own data |
 
Passwords in the scripts are placeholders. Change them before using this anywhere real.
 
## Scenario-based questions
 
`scenarios/` holds 40 questions in one continuous list. Each question follows the same format: Scenario, Topics used, Query, Output, Explanation. Together they cover subqueries, correlated subqueries, nested subqueries, normalization, single-row functions, multi-row functions, joins, and `GROUP BY`.
 
## Key insights
 
_Add your findings here after Phase 7, for example the top-earning movies, busiest booking days, and most-used payment methods._
 
## Documentation
 
The full write-up, with screenshots, is in [`docs/`](docs/).
 
## Author
 
**Omkar Dhembare**
[GitHub](https://github.com/omkardhembare)
