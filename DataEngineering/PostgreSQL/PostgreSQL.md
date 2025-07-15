**Overview of PostgreSQL**  

**PostgreSQL** (often called **Postgres**) is a **powerful**, **open-source**, **object-relational database system** that has been in active development for over 30 years. It supports:
- SQL (Structured Query Language)
- ACID compliance
- Full Transactional Integrity
- MVCC (Multi-Version Concurrency Control)
- Extensibility with custom data types, functions, and more

---

## 1.Key Features

- Full SQL compliance (SQL:2016 standard)
- Support for complex queries and joins
- Triggers, Views, Stored Procedures
- Full-Text Search
- JSON / JSONB document storage
- Geospatial support with PostGIS
- Partitioning, indexing, and constraints
- Replication and Clustering
- Row-level Security (RLS)


## 2.Use Cases

- Enterprise-grade applications
- Data warehousing
- Analytics platforms
- Geospatial systems (GIS)
- E-commerce, fintech, IoT, and more
- Hybrid SQL-JSON applications (NoSQL-like)


## 3.Installation

### On Ubuntu
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

### On macOS (using Homebrew)

```bash
brew install postgresql
brew services start postgresql
```

## 4.Default Access

```bash
sudo -u postgres psql        # Access psql shell
\password postgres           # Set password for postgres user
```

## 5.Database Basics

**Create a Database**

```sql
CREATE DATABASE mydb;
```

**Connect to a Database**

```bash
psql -d mydb
```

**Create a User**

```sql
CREATE USER myuser WITH PASSWORD 'securepass';
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```


## 6.Data Types

- INTEGER, BIGINT, SMALLINT
- VARCHAR(n), TEXT
- DATE, TIMESTAMP, TIME
- BOOLEAN
- NUMERIC, DECIMAL, FLOAT
- JSON, JSONB
- UUID, ARRAY, ENUM

## 7.Table Operations

**Create Table**

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Alter Table**

```sql
ALTER TABLE users ADD COLUMN age INT;
```

**Drop Table**

```sql
DROP TABLE users;
```

## 8.CRUD Queries

**Insert**

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
```

**Select**

```sql
SELECT * FROM users WHERE age > 25 ORDER BY created_at DESC;
```

**Update**

```sql
UPDATE users SET name = 'Bob' WHERE id = 1;
```

**Delete**

```sql
DELETE FROM users WHERE email IS NULL;
```

## 9.Filtering & Joins

**WHERE, AND, OR**

```sql
SELECT * FROM users WHERE age > 30 AND email LIKE '%@gmail.com';
```

**JOINs**

```sql
-- Inner Join
SELECT u.name, o.total
FROM users u
JOIN orders o ON u.id = o.user_id;

-- Left Join
SELECT u.name, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

**Aggregations**

```sql
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT country, COUNT(*) FROM users GROUP BY country;
```

**Indexes**

- Create Index

```sql
CREATE INDEX idx_email ON users(email);
```

- Full Text Index

```sql
CREATE INDEX idx_fts ON articles USING GIN(to_tsvector('english', content));
```

## 10.Advanced Features

**JSONB Queries**

```sql
SELECT data->>'name' AS name FROM users WHERE data->>'role' = 'admin';
```

**Arrays**

```sql
CREATE TABLE tags (id SERIAL, name TEXT[], created TIMESTAMP);
```

**Views**

```sql
CREATE VIEW active_users AS SELECT * FROM users WHERE active = TRUE;
```

**CTE (Common Table Expressions)**

```sql
WITH recent AS (
  SELECT * FROM orders WHERE created_at > NOW() - INTERVAL '7 days'
)
SELECT * FROM recent WHERE total > 100;
```

## 11.Transactions

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

To rollback:

```sql
ROLLBACK;
```

## 12.Functions & Triggers

**User-Defined Function**

```sql
CREATE FUNCTION get_user_email(uid INT) RETURNS TEXT AS $$
  SELECT email FROM users WHERE id = uid;
$$ LANGUAGE SQL;
```

**Trigger Example**

```sql
CREATE OR REPLACE FUNCTION update_timestamp()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_update BEFORE UPDATE ON users
FOR EACH ROW EXECUTE FUNCTION update_timestamp();
```

## 13.Security

**Roles and Permissions**

```sql
CREATE ROLE readonly;
GRANT CONNECT ON DATABASE mydb TO readonly;
GRANT USAGE ON SCHEMA public TO readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
```

**Row-Level Security (RLS)**

```sql
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;
CREATE POLICY user_policy ON messages
  USING (user_id = current_setting('app.current_user')::INT);
```

## 14.Extensions

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS postgis;
```

## 15.Backup and Restore

**Backup**

```bash
pg_dump -U postgres mydb > mydb_backup.sql
```

**Restore**

```bash
psql -U postgres -d mydb < mydb_backup.sql
```

## 16.Monitoring

- **pg_stat_activity** - View current queries

```sql
SELECT * FROM pg_stat_activity;
```

- **pg_indexes, pg_tables, pg_roles** - Schema views

```sql
SELECT * FROM pg_indexes WHERE tablename = 'users';
```

## 17.Getting Help & Resources

- [PostgreSQL docs](https://www.postgresql.org/docs/)
- [PostgreSQL Wiki](https://wiki.postgresql.org/wiki/Main_Page)
- [PostgreSQL Exercises](https://pgexercises.com/)

## 18.Tips

- Always use *EXPLAIN ANALYZE* to optimize queries
- Prefer *JSONB* over *JSON* for indexing
- Use connection pooling in production (pgbouncer)
- Regularly *VACUUM ANALYZE* large tables

## 19.Example Schema for Practice

```sql
CREATE TABLE authors (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);

CREATE TABLE books (
  id SERIAL PRIMARY KEY,
  title TEXT NOT NULL,
  author_id INT REFERENCES authors(id),
  published DATE
);

INSERT INTO authors (name) VALUES ('J.R.R. Tolkien'), ('George Orwell');

INSERT INTO books (title, author_id, published) VALUES
('1984', 2, '1949-06-08'),
('The Hobbit', 1, '1937-09-21');
```

Query:

```sql
SELECT b.title, a.name
FROM books b
JOIN authors a ON b.author_id = a.id;
```

---