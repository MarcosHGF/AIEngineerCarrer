**Overview of NoSQL** 

**NoSQL** stands for "**Not Only SQL**". It refers to a broad class of database systems that:
- Do **not use SQL as their primary query language** (although some support it partially),
- Do **not rely on a relational model** (no fixed schema with tables, rows, foreign keys),
- Are **optimized for large-scale, distributed, and high-throughput environments**.

> **Main Goal:** Provide flexible, scalable, and high-performance alternatives to traditional RDBMSs like MySQL, PostgreSQL, or Oracle.

---

## 1.When to Use NoSQL

| Use Case                            | NoSQL Advantage                  |
|------------------------------------|----------------------------------|
| Massive data ingestion             | High write throughput            |
| Rapid prototyping / agile schemas | Schema-less data models          |
| IoT, logs, clickstreams            | Horizontal scalability           |
| Mobile apps / social media        | Complex, nested document support |
| Real-time analytics                | Fast reads / writes              |
| Geographically distributed systems | Built-in replication/sharding    |


## 2.Four Major Types of NoSQL Databases

### Document Stores (e.g. MongoDB, Couchbase)
- Store data as **JSON / BSON / XML** documents.
- Good for hierarchical or nested data.

**Example Document:**
```json
{
  "user_id": "u123",
  "name": "Alice",
  "orders": [
    {"item": "Laptop", "price": 1200},
    {"item": "Mouse", "price": 20}
  ]
}
```

**Query Example (MongoDB):**

```json
db.users.find({ "orders.item": "Laptop" });
```

## 3.Key-Value Stores (e.g. Redis, DynamoDB, Riak)

- Every item is stored as a key and a value (binary, string, JSON, etc.)
- Blazing fast reads and writes.

**Example (Redis CLI):**

```bash
SET user:123 '{"name":"Bob","age":30}'
GET user:123
```

## 4.Column-Family Stores (e.g. Cassandra, HBase)

- Data is stored in columns instead of rows.
- Great for write-heavy, wide-table datasets (e.g. telemetry, logs).

**CQL (Cassandra Query Language):**

```sql
CREATE TABLE users (
  user_id UUID PRIMARY KEY,
  name TEXT,
  email TEXT
);

INSERT INTO users (user_id, name, email) VALUES (uuid(), 'Alice', 'alice@example.com');
SELECT * FROM users;
```

## 5.Graph Databases (e.g. Neo4j, ArangoDB, JanusGraph)

- Store entities (nodes) and relationships (edges).
- Ideal for recommendation engines, fraud detection, social networks.

**Cypher Query (Neo4j):**

```sql
MATCH (a:Person)-[:FRIENDS_WITH]->(b:Person)
WHERE a.name = 'Alice'
RETURN b.name;
```

## 6.Core Concepts in NoSQL

| Concept            | Description                                              |
| ------------------ | -------------------------------------------------------- |
| Schema-less        | No predefined structure required for data                |
| Horizontal Scaling | Add more nodes instead of stronger hardware              |
| High Availability  | Replication across data centers or clusters              |
| BASE (not ACID)    | Basic Availability, Soft-state, Eventual consistency     |
| CAP Theorem        | Choose 2: Consistency, Availability, Partition Tolerance |

## 7.NoSQL vs SQL

| Feature         | NoSQL                   | SQL                         |
| --------------- | ----------------------- | --------------------------- |
| Schema          | Flexible / dynamic      | Rigid / predefined          |
| Scaling         | Horizontal              | Vertical                    |
| ACID Compliance | Partial / eventual      | Strong                      |
| Query Language  | Proprietary / varied    | SQL                         |
| Best for        | Unstructured / big data | Structured, relational data |

## 8.Real World Use Cases

| Company  | Use Case                                 | NoSQL Type Used           |
| -------- | ---------------------------------------- | ------------------------- |
| Netflix  | Media catalog, user preferences          | Cassandra, Redis          |
| Uber     | Geolocation & ETA predictions            | MongoDB, Riak             |
| Twitter  | Tweets, timelines, social graph          | Redis, Manhattan (custom) |
| Amazon   | Shopping cart, product catalog           | DynamoDB                  |
| LinkedIn | Real-time analytics, relationships graph | Espresso, Neo4j           |

## 9.Advanced Queries and Patterns

**Document Store Queries (MongoDB)**

```json
// Insert
db.products.insertOne({ name: "TV", price: 500 });

// Find
db.products.find({ price: { $lt: 1000 } });

// Update
db.products.updateOne({ name: "TV" }, { $set: { price: 450 } });

// Aggregation
db.orders.aggregate([
  { $unwind: "$items" },
  { $group: { _id: "$user_id", total: { $sum: "$items.price" } } }
]);
```

**Key-Value Patterns (Redis)**

```json
# Simple Set/Get
SET user:1:name "Alice"
GET user:1:name

# Hashes
HSET user:1 name "Alice" age "30"
HGETALL user:1

# Pub/Sub
PUBLISH news "New blog post published!"
```

## 10.Scaling in NoSQL

- **Sharding**: Splitting data across multiple machines
- **Replication**: Copying data to multiple nodes
- **Eventual Consistency**: Updates will propagate eventually
- **Consistency Levels**: Strong, quorum, eventual (choose based on use case)

## 11.CAP Theorem Visual
> You can only pick 2 out of 3 in a distributed system:

```sql
CAP:
 ┌─────────────┐
 │ Consistency │ ← All nodes see the same data at the same time
 └────┬────────┘
      │
      ▼
┌─────────────┐
│ Availability│ ← Every request gets a (non-error) response
└────┬────────┘
     │
     ▼
┌─────────────────────┐
│ Partition Tolerance │ ← System continues even with network failures
└─────────────────────┘
```

## 12.Tools & Ecosystem

| Tool/DB   | Type          | Language Support           |
| --------- | ------------- | -------------------------- |
| MongoDB   | Document      | JS, Python, Go, Java, etc. |
| Redis     | Key-Value     | All major languages        |
| Cassandra | Columnar      | Java, Python, Go, Node     |
| Neo4j     | Graph         | Cypher query language      |
| DynamoDB  | Key-Value/Doc | AWS SDKs                   |
| Couchbase | Document      | Query via N1QL (SQL-like)  |

## 13.Trade-offs and Pitfalls

- Lack of Join support in most NoSQL DBs
- Inconsistent querying language
- Data duplication is often required
- Indexing & performance tuning vary drastically per DB
- Backup & migration are non-trivial

## 14.Getting Help & Resources

- [MongoDB University](https://learn.mongodb.com/)
- [Redis Documentation](https://redis.io/)
- [Cassandra Docs](https://cassandra.apache.org/_/index.html)
- [Neo4j GraphAcademy](https://graphacademy.neo4j.com/)
- [AWS DynamoDB Guide](https://aws.amazon.com/pt/dynamodb/)

## 15.Tip

*Choose your database based on your access patterns, not the other way around.*

NoSQL is not a replacement for SQL, it's a toolbox with multiple hammers. Use the right one depending on your data model, scalability needs, and read/write patterns.

---