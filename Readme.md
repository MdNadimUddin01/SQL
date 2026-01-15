## What is SQL ?


> SQL is the language of relational databases, or repositories of the
> information organizes into the tables.
> 
> **SQL Follows the ANSI SQL standard.**

**Advantages Of SQL**

 - DBMS independent
 - Standardized (ANSI SQL)
 - Declarative (leaves query optimization to DBMS)

**Disadvantages of SQL**

 - Complexity
 - Unpredictable performance of complexities (The **query optimizer** chooses execution plans automatically).
 - Requires procedural extensions (SQL is designed for **set-based operations**, not step-by-step logic).

> **SQL Statement :**
>  A SQL  statement is a set of instructions consisting of identifiers , parameters , variables , names, data types and SQL reserved words that compiled successfully.
> * To update, manipulate, or maintain the data in a database—or retrieve it from a database—SQL statements (or commands) are required.

**Types of SQL Command**

 1. DDL (Data Definition Language) : helps to **define the structure of the database.**
 2. DML (Data Manipulation Language) : helps to **manipulate the data.**
 3. DCL (Data Control Language) : Controls **access and permissions** to database objects.
 4. TCL (Transaction control Language) : Controls **transactions** and maintains data integrity.
 
 ```mermaid
 flowchart TB
    SQL((SQL))
    
    DML[DML: add / remove / query data<br/>SELECT<br/>INSERT<br/>UPDATE<br/>DELETE<br/>TRUNCATE]
    DDL[DDL: edit / create data schema<br/>CREATE<br/>ALTER<br/>DROP]
    DCL[DCL: Edit data access<br/>GRANT<br/>REVOKE]
    TCL[TCL: Edit transaction state<br/>COMMIT<br/>ROLLBACK<br/>SAVEPOINT]

    SQL --> DML
    SQL --> DDL
    SQL --> DCL
    SQL --> TCL
```
**Accepted Rules :**
- comments :-   [ - - ]

> SQL is a case-insensitive language

 - SQL Keywords in UPPERCASE
 - For everything else - lowercase
- string literals are quoted using single quote.
- string literals are case-sensitive.
Brief introduction to PostgreSQL

**PostgreSQL**
- Open source ORDBMS
- PostgreSQL is in top-5 of most popular systems in the world
- PostgreSQL has its own non-ANSI SQL commands
- Non-ANSI SQL command labels will be highlighted in Green in the course
- DBeaver, PgAdmin, SQLWorkbench/J can simplify working with PostgreSQL

Features of **PostgreSQL**
- ACID compliance
- Support of multiple programming languages (Python, Java, C)
- Diverse customizable indexing
- Native JSON and full-text search (FTS) support
- Lots of community-developed extensions
# PostgreSQL Datatypes

## Numeric Datatypes

| Data Type | Description | Example | Min | Max |
|----------|------------|---------|-----|-----|
| INT | Integer values | Age | -2,147,483,648 | 2,147,483,647 |
| BIGINT | Large integer values | Population | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| SERIAL | Auto-increment integer | ID (PK) | 1 | 2,147,483,647 |
| BIGSERIAL | Auto-increment big integer | ID (PK) | 1 | 9,223,372,036,854,775,807 |
| REAL / FLOAT4 | Floating-point values | Surplus | 1E-37 | 1E+37 |
| DOUBLE PRECISION / FLOAT8 | Double precision floating-point | Measurement | 1E-307 | 1E+308 |
| DECIMAL(p, s) | Exact numeric with precision and scale | 1234.5678 | Depends on p,s | Depends on p,s |

## Character Datatypes

| Data Type | Description | Example | Notes |
|----------|------------|---------|-------|
| CHAR(n) | Fixed-length, blank padded | CHAR(1) | 'Y' |
| VARCHAR(n) | Variable-length with limit | VARCHAR(3) | 'Yes' |
| TEXT | Variable-length, unlimited | TEXT | 'Yes please...' |

## Temporal Datatypes

| Data Type | Description | Format | Example |
|----------|------------|--------|---------|
| DATE | Calendar date (year, month, day) | YYYY-MM-DD | 2019-12-31 |
| TIME | Time of day (with/without time zone) | HH:MM:SS | 01:02:03 |
| TIMESTAMP | Date and time (with/without time zone) | YYYY-MM-DD HH:MM:SS | 2019-12-31 01:02:03 |

## Other Datatypes

| Data Type | Description | Example |
|----------|------------|---------|
| BOOLEAN | Logical true/false | TRUE |
| XML | Structured / semi-structured data | <tag>value</tag> |
| JSON | JSON data | {"key": "value"} |
| JSONB | Binary JSON (optimized) | {"id": 1} |
| UUID | Universally unique identifier | 550e8400-e29b-41d4-a716-446655440000 |
| INTERVAL | Time duration | 2 days |
| BIT | Fixed-length bit string | B'1010' |


> **Implicit Type conversion may produce unexpected results.**
> **It's always better to have control over datatypes in such cases**
> E.g :- 97.5 + CAST('2.5' AS DECIMAL (19,2))