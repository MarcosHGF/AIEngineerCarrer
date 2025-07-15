**Overview of SQL** 
Is the standard language for **relational database management systems (RDBMS)**. It is used to **create, read, update, and delete (CRUD)** data and manage database structures.

It is supported by most RDBMS such as:
- PostgreSQL
- MySQL / MariaDB
- SQLite
- Microsoft SQL Server
- Oracle DB

## 🔧 SQL Overview

| Category              | Examples                                |
|-----------------------|------------------------------------------|
| **Data Querying**     | `SELECT`, `WHERE`, `JOIN`, `GROUP BY`   |
| **Data Manipulation** | `INSERT`, `UPDATE`, `DELETE`            |
| **Data Definition**   | `CREATE TABLE`, `ALTER`, `DROP`         |
| **Transaction Control** | `BEGIN`, `COMMIT`, `ROLLBACK`          |
| **Access Control**    | `GRANT`, `REVOKE`, `CREATE USER`        |

---

## 1.Basic Syntax

Explains how to write simple queries to retrieve data, filter rows, sort results, and limit output.

```sql
-- Select all columns
SELECT * FROM table_name;

-- Select specific columns
SELECT name, age FROM users;

-- Where clause
SELECT * FROM products WHERE price > 100;

-- Order results
SELECT * FROM users ORDER BY age DESC;

-- Limit results
SELECT * FROM products LIMIT 10;
```


## 2.Filtering & Logic

Shows how to filter data using conditions (WHERE) and logical operators (AND, OR, NOT), and match patterns using LIKE.

```sql
-- Comparisons
=, !=, <, >, <=, >=

-- Logic
AND, OR, NOT

-- Pattern Matching
LIKE 'A%'    -- Starts with A
LIKE '%A'    -- Ends with A
LIKE '%A%'   -- Contains A
ILIKE        -- Case-insensitive (PostgreSQL)
```

## 3.Aggregations

Teaches how to use functions like SUM, AVG, COUNT, and group data using GROUP BY with HAVING filters.

```sql
-- Aggregate functions
SELECT COUNT(*), AVG(price), MAX(age), MIN(salary), SUM(revenue)
FROM employees;

-- Grouping
SELECT department, COUNT(*) FROM employees
GROUP BY department;

-- Group with filter
HAVING COUNT(*) > 5;
```

## 4.JOINs

Explains how to combine data from multiple tables.

```sql
-- Inner Join (common rows)
SELECT * FROM orders
JOIN customers ON orders.customer_id = customers.id;

-- Left Join (all from left + matched from right)
SELECT * FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id;

-- Right Join (all from right + matched from left)
SELECT * FROM orders
RIGHT JOIN customers ON customers.id = orders.customer_id;

-- Full Outer Join
SELECT * FROM table1
FULL OUTER JOIN table2 ON table1.id = table2.id;

-- Cross Join (cartesian product)
SELECT * FROM table1
CROSS JOIN table2;
```

## 5.INSERT, UPDATE, DELETE

How to add new data, change existing records, and remove data from a table.

```sql
-- Insert single row
INSERT INTO users (name, age) VALUES ('Alice', 30);

-- Insert multiple rows
INSERT INTO products (name, price) VALUES 
('Pen', 1.50),
('Notebook', 5.00);

-- Update values
UPDATE users SET age = 31 WHERE name = 'Alice';

-- Delete rows
DELETE FROM users WHERE age < 18;
```

## 6.DDL: Data Definition Language

How to define the structure of your database: create, alter, and drop tables with types, constraints, and defaults.

```sql
-- Create Table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  email TEXT UNIQUE
);

-- Alter Table
ALTER TABLE users ADD COLUMN is_active BOOLEAN DEFAULT TRUE;

-- Drop Table
DROP TABLE users;
```

## 7.Constraints

Rules applied to table columns.

```sql
-- Primary Key
id INT PRIMARY KEY

-- Unique
email TEXT UNIQUE

-- Not Null
name TEXT NOT NULL

-- Default Value
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

-- Foreign Key
user_id INT REFERENCES users(id)
```

## 8.Subqueries

Queries inside other queries, used for complex filters, computed values, or nested data retrieval.

```sql
-- Subquery in WHERE
SELECT name FROM users WHERE id IN (
  SELECT user_id FROM orders WHERE total > 500
);

-- Subquery in SELECT
SELECT name,
  (SELECT COUNT(*) FROM orders WHERE orders.user_id = users.id) AS order_count
FROM users;
```

## 9.Views

Virtual tables created from queries that behave like real tables but don’t store data themselves.

```sql
-- Create a view
CREATE VIEW active_users AS
SELECT * FROM users WHERE is_active = TRUE;

-- Query a view
SELECT * FROM active_users;

-- Drop a view
DROP VIEW active_users;
```

## 10.Transactions

Group multiple queries into a unit, they either all succeed (COMMIT) or fail (ROLLBACK) to keep data consistent.

```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT; -- or ROLLBACK;
```

## 11.Indexes

Performance boosters that speed up searches and lookups on large datasets.

```sql
-- Create index
CREATE INDEX idx_users_name ON users(name);

-- Drop index
DROP INDEX idx_users_name;
```

## 12.Users & Permissions (PostgreSQL example)

Creating database users and controlling who can read, write, or manage data and structure.

```sql
-- Create user
CREATE USER dev_user WITH PASSWORD 'password123';

-- Grant access
GRANT SELECT, INSERT ON TABLE users TO dev_user;

-- Revoke access
REVOKE INSERT ON users FROM dev_user;
```

## 13.Advanced Queries

Covers CASE, window functions (RANK, OVER), and CTEs (WITH), used for analytics and cleaner queries.

```sql
-- CASE WHEN
SELECT name,
  CASE
    WHEN age < 18 THEN 'Minor'
    WHEN age BETWEEN 18 AND 65 THEN 'Adult'
    ELSE 'Senior'
  END AS category
FROM users;

-- WINDOW FUNCTIONS
SELECT name, salary,
  RANK() OVER (ORDER BY salary DESC) AS rank
FROM employees;

-- CTE (Common Table Expressions)
WITH recent_orders AS (
  SELECT * FROM orders WHERE created_at > NOW() - INTERVAL '30 days'
)
SELECT * FROM recent_orders WHERE total > 500;
```

## 14. 🌐 Ecosystem & Extensibility

SQL is at the heart of the modern data ecosystem. It integrates with virtually every data technology and scales from embedded databases to petabyte-scale data warehouses.

### 🔌 SQL Ecosystem Overview

| Technology Type           | Examples                                                   |
|---------------------------|-------------------------------------------------------------|
| **Databases**             | PostgreSQL, MySQL, SQLite, Oracle, MS SQL Server            |
| **Data Warehouses**       | BigQuery, Snowflake, Redshift, Azure Synapse                |
| **ORMs (Object Mapping)** | SQLAlchemy (Python), Prisma (JavaScript), Hibernate (Java)  |
| **ETL Tools**             | Apache Airflow, dbt, Talend, Fivetran                       |
| **BI Tools**              | Power BI, Looker, Tableau, Metabase                         |
| **Data Science**          | Pandas, Jupyter (via `%sql`), Apache Superset               |
| **Languages with SQL Libs** | Python, R, Java, C#, Node.js, Rust, Go                    |

### ⚙️ Extensibility Features

- **User-Defined Functions (UDFs)**: Custom logic written in SQL or other languages.
- **Stored Procedures**: Encapsulate logic inside the database.
- **Extensions**: PostgreSQL allows plugins like `PostGIS`, `pg_trgm`, `hstore`.
- **Foreign Data Wrappers**: Access external data sources like CSV, APIs, or NoSQL.


## 15. 📈 Common Use Cases

SQL is a foundational technology in nearly every data-driven workflow:

### 🧮 Business Intelligence (BI)
- Dashboards
- KPI monitoring
- Ad-hoc reporting

### 🛠️ Application Development
- User authentication and profile storage
- Transactional apps (e.g., e-commerce orders)
- CRUD operations through APIs

### 📊 Data Analytics
- Cohort analysis
- Funnel analysis
- Churn prediction

### 🧪 Data Science
- Data exploration and cleaning
- Feature extraction
- Connecting to ML pipelines

### 🔄 ETL/ELT Pipelines
- Extracting from operational DBs
- Transforming via SQL logic
- Loading into warehouses (e.g., dbt + BigQuery)

### 🔐 Auditing & Logging
- Querying logs
- Creating access control reports
- Building audit trails


## 16. 🆘 Getting Help & Resources

### 📚 Learning Platforms
- [SQLBolt](https://sqlbolt.com)
- [Mode SQL Tutorial](https://mode.com/sql-tutorial)
- [W3Schools SQL](https://www.w3schools.com/sql/)
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/)
- [Kaggle SQL Courses](https://www.kaggle.com/learn/advanced-sql)

### 📖 Documentation
- [PostgreSQL Docs](https://www.postgresql.org/docs/)
- [MySQL Docs](https://dev.mysql.com/doc/)
- [SQLite Docs](https://www.sqlite.org/docs.html)
- [SQL Server Docs](https://learn.microsoft.com/en-us/sql/)

### 💬 Communities
- [Stack Overflow — SQL](https://stackoverflow.com/questions/tagged/sql)
- [Reddit /r/SQL](https://reddit.com/r/sql)
- [DBA StackExchange](https://dba.stackexchange.com/)
- [SQLServerCentral](https://www.sqlservercentral.com/)

### 👩‍💻 Tools
- **DBeaver** — Universal GUI client  
- **TablePlus**, **DataGrip**, **pgAdmin**  
- **JupyterLab SQL Magic** (`%sql` in notebooks)  

---