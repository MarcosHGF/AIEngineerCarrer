**Overview of Data Warehousing**  

A Data Warehouse (DW) is a centralized repository designed to store, manage, and query large volumes of structured and semi-structured data for analytical and reporting purposes.

*It integrates data from multiple heterogeneous sources and transforms it into a single source of truth optimized for querying and analytics.*

## Why Use a Data Warehouse?

| Benefit                    | Description                                            |
| -------------------------- | ------------------------------------------------------ |
| Historical analysis      | Stores years of business data.                         |
| Data consistency         | Centralizes inconsistent source data.                  |
| High-performance queries | Optimized for OLAP workloads.                          |
| Business Intelligence    | Enables dashboards and visualizations.                 |
| Scalability              | Handles petabytes of data (e.g., BigQuery, Snowflake). |

## Core Components

| Component               | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **Staging Area**        | Raw data landing zone before transformation.                 |
| **ETL/ELT Tools**       | Extract, Transform, and Load (or just Load, then Transform). |
| **Data Warehouse DB**   | Core analytical storage (e.g., Snowflake, Redshift).         |
| **Metadata Repository** | Stores data definitions, lineage, and governance info.       |
| **BI Tools**            | Dashboards and reports (e.g., Tableau, Power BI).            |

## Data Warehouse Architecture

1. Single-Tier (Not used)
    - Rare. Combines storage and analytics, often leads to performance bottlenecks.  

2. Two-Tier
    - Client & Server
    - Not scalable for modern use  

3. Three-Tier (Modern Standard)

```python
+-----------------+        +---------------------+        +------------------+
|   Data Sources  |  -->   |   ETL/ELT Process   |  -->   |  Data Warehouse  |
+-----------------+        +---------------------+        +--------+---------+
                                                                    |
                                                                    v
                                                          +-------------------+
                                                          |     BI Tools      |
                                                          +-------------------+
```

## ETL vs ELT

| Feature  | ETL (Extract-Transform-Load)  | ELT (Extract-Load-Transform)    |
| -------- | ----------------------------- | ------------------------------- |
| When     | Legacy systems, on-prem       | Cloud-native, modern DWs        |
| Where    | Transform in middleware layer | Transform inside DW (e.g., dbt) |
| Examples | Talend, Informatica           | dbt, BigQuery SQL, Spark SQL    |

## Data Modeling Techniques

1. Star Schema
    - Central fact table connected to dimension tables.
    - Simplified querying.

```python
        +-------------+
        |  Customers  |
        +-------------+
              |
              v
  +-----------+------------+
  |    Sales Fact Table    |
  +-----------+------------+
              ^
              |
        +-------------+
        |  Products   |
        +-------------+
```

2. Snowflake Schema
    -  Normalized dimension tables.
    - Reduces data redundancy.  

3. Data Vault
    - Scalable, auditable model using:
        - Hubs (business keys)
        - Links (relationships)
        - Satellites (context info)

## Query Examples

Assume a retail DW with tables: sales_fact, products, customers, time_dim.

1. Total Revenue by Product

```sql
SELECT p.product_name, SUM(s.total_amount) AS revenue
FROM sales_fact s
JOIN products p ON s.product_id = p.product_id
GROUP BY p.product_name
ORDER BY revenue DESC;
```

2. Monthly Sales Trend

```sql
SELECT t.month, SUM(s.total_amount) AS monthly_sales
FROM sales_fact s
JOIN time_dim t ON s.time_id = t.time_id
GROUP BY t.month
ORDER BY t.month;
```

3. Top 5 Customers by Spend

```sql
SELECT c.customer_name, SUM(s.total_amount) AS spend
FROM sales_fact s
JOIN customers c ON s.customer_id = c.customer_id
GROUP BY c.customer_name
ORDER BY spend DESC
LIMIT 5;
```

## Popular Data Warehouse Technologies

| Platform               | Type          | Notes                                        |
| ---------------------- | ------------- | -------------------------------------------- |
| **Amazon Redshift**    | Cloud DW      | High-performance, integrates with AWS        |
| **Google BigQuery**    | Serverless DW | Fast, scalable, uses SQL                     |
| **Snowflake**          | SaaS DW       | Multi-cloud, separation of compute & storage |
| **Azure Synapse**      | Cloud DW      | Tight integration with Microsoft tools       |
| **Teradata**           | On-prem/Cloud | Enterprise-grade legacy DW                   |
| **PostgreSQL + Citus** | Hybrid        | Distributed Postgres                         |

## Use Cases

| Domain     | Use Case                            |
| ---------- | ----------------------------------- |
| Retail     | Sales analytics, Inventory trends   |
| Healthcare | Patient outcome analysis            |
| Finance    | Fraud detection, Risk analysis      |
| Marketing  | Campaign performance, Attribution   |
| Logistics  | Supply chain and route optimization |
| SaaS       | User behavior and feature usage     |

## Best Practices

- Data Quality First – Validate and cleanse at the staging layer.
- Use Slowly Changing Dimensions (SCD) – Track historical changes.
- Optimize Partitions & Clustering – For query performance.
- Govern Metadata & Lineage – Track origin, changes, usage.
- Secure Your DW – Row-level security, encryption, access policies.
- Monitor Usage – Query cost, performance, user patterns.
- Adopt CI/CD for DW – Automate SQL models via dbt or Airflow.

## Data Lake vs Data Warehouse

| Feature        | Data Lake                          | Data Warehouse                |
| -------------- | ---------------------------------- | ----------------------------- |
| Data Types     | Raw, unstructured, semi-structured | Structured, transformed       |
| Storage Format | Parquet, Avro, JSON, CSV           | Tables/Columns in SQL         |
| Cost           | Cheap storage, high compute cost   | Optimized for querying        |
| Usage          | ML, Big Data, Exploration          | BI, Dashboards, Reporting     |
| Tools          | Hadoop, S3, Delta Lake, Databricks | Snowflake, BigQuery, Redshift |

*Modern Architecture = Data Lakehouse (e.g., Databricks, Snowflake with Unistore)*

## Getting Help & Resources

- The Data Warehouse Toolkit – Ralph Kimball
- Fundamentals of Data Engineering – Joe Reis & Matt Housley
- [dbt Learn – Interactive SQL Data Modeling Platform](https://learn.getdbt.com/catalog)