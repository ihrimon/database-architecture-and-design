# Database Architecture & Design Guide

> In modern software engineering, a well-designed database forms the **backbone** of every scalable and high-performance application. This repository serves as a structured knowledge base covering core **database fundamentals**, essential **system design principles**, practical **query optimization** techniques, and real-world implementation examples.

## 📑 PostgreSQL

| Topics                                                                                     | Overview                                         |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| [02. SQL Query Foundations](#02-sql-query-foundations)                                     | CRUD operations, filtering, sorting, constraints |
| [03. Advanced Query Techniques](#03-advanced-query-techniques)                             | Joins, subqueries, aggregation, grouping         |
| [04. PostgreSQL-Specific Features](#04-postgresql-specific-features)                       | Data types, JSON, UUID, arrays                   |
| [05. Indexing & Performance Optimization](#05-indexing--performance-optimization)          | Index types, query planning, EXPLAIN             |
| [06. Transactions & Concurrency](#06-transactions--concurrency)                            | ACID, isolation levels, locking                  |
| [07. Schema Design & Normalization](#07-schema-design--normalization)                      | Database design principles                       |
| [08. Security & Roles](#08-security--roles)                                                | Users, roles, permissions                        |
| [09. Backup, Migration & Production Practices](#09-backup-migration--production-practices) | pg_dump, restore, migrations                     |
| [10. PostgreSQL DBA & Scaling Concepts](#10-postgresql-dba--scaling-concepts)              | Replication, partitioning, scaling               |

### 02. SQL Query Foundations

- What is SQL?

# What is SQL?

**SQL** (Structured Query Language) is the standard language used to communicate with relational databases. You write declarative statements that describe _what_ you want, and the database engine figures out _how_ to retrieve it.

---

## SQL vs the Database Engine

SQL is the **language**; an RDBMS (Relational Database Management System) like PostgreSQL is the **engine** that stores data and interprets your SQL. Multiple engines speak SQL, but each has its own dialect with small differences.

```
Your Application
      │
      │  SQL query  (e.g. SELECT * FROM users WHERE age > 18)
      ▼
  RDBMS Engine  (PostgreSQL, MySQL, SQLite…)
      │
      │  parsed + executed
      ▼
 Tables on disk  (rows, columns, indexes)
```

---

## Key Ideas

| Concept                        | Explanation                                  |
| ------------------------------ | -------------------------------------------- |
| **Declarative**                | You say _what_ you want, not _how_ to get it |
| **Relational model**           | Data lives in tables linked by keys          |
| **Multiple sublanguages**      | DDL · DML · DCL · TCL                        |
| **ISO standard with dialects** | PostgreSQL, MySQL, T-SQL vary slightly       |
| **Set-based results**          | Returns zero or more rows at once            |

---

## SQL Sublanguages

SQL is divided into four sublanguages:

| Sublanguage | Full Name                    | Key Commands                           | Purpose              |
| ----------- | ---------------------------- | -------------------------------------- | -------------------- |
| **DDL**     | Data Definition Language     | `CREATE`, `ALTER`, `DROP`              | Define structure     |
| **DML**     | Data Manipulation Language   | `SELECT`, `INSERT`, `UPDATE`, `DELETE` | Work with data       |
| **DCL**     | Data Control Language        | `GRANT`, `REVOKE`                      | Manage permissions   |
| **TCL**     | Transaction Control Language | `COMMIT`, `ROLLBACK`                   | Control transactions |

---

## A Simple Example

```sql
-- Create a table (DDL)
CREATE TABLE users (
    id    SERIAL PRIMARY KEY,
    name  VARCHAR(100) NOT NULL,
    age   INT
);

-- Insert a row (DML)
INSERT INTO users (name, age) VALUES ('Alice', 30);

-- Query data (DML)
SELECT name FROM users WHERE age > 18;

-- Delete a row (DML)
DELETE FROM users WHERE name = 'Alice';
```

---

## Key Takeaways

- SQL is **declarative** — describe the result, not the steps to get there.
- SQL is **not** a database itself; it is the language you use to talk to one.
- PostgreSQL, MySQL, and SQLite all speak SQL but each adds its own extensions and quirks.
- Everything in the PostgreSQL study plan — queries, joins, indexes, transactions — builds on these fundamentals.

- Data Types (INT, VARCHAR, TEXT, DATE, BOOLEAN)
# Data Types (INT, VARCHAR, TEXT, DATE, BOOLEAN)

Every column in a SQL table has a **data type** that defines what kind of value it can store. Choosing the right type enforces data integrity, saves storage space, and enables correct comparisons and sorting.

---

## The Five Core Types

### 1. INT — Whole Numbers

Stores integers with no decimal part. Use for counts, IDs, ages, quantities.

```sql
CREATE TABLE products (
    id       INT,
    quantity INT
);
```

**Variants by storage size:**

| Type | Storage | Range |
|---|---|---|
| `SMALLINT` | 2 bytes | −32,768 to 32,767 |
| `INT` / `INTEGER` | 4 bytes | −2,147,483,648 to 2,147,483,647 |
| `BIGINT` | 8 bytes | −9.2 × 10¹⁸ to 9.2 × 10¹⁸ |

> Use `BIGINT` for user IDs or anything that might grow large. Use `SMALLINT` to save space on small lookup tables.

---

### 2. VARCHAR(n) — Variable-Length Text with a Limit

Stores text up to `n` characters. Only uses as much space as the actual value.

```sql
CREATE TABLE users (
    username  VARCHAR(50),
    email     VARCHAR(255)
);
```

- The `(n)` limit is a **constraint**, not a fixed allocation.
- Values exceeding `n` raise an error.
- Good for: names, emails, codes, short descriptions where a max length makes sense.

---

### 3. TEXT — Unlimited-Length Text

Stores text of any length — no limit needed.

```sql
CREATE TABLE posts (
    body TEXT
);
```

- No `(n)` required or allowed.
- Internally, PostgreSQL stores `VARCHAR` and `TEXT` almost identically — performance is the same.
- Good for: article bodies, comments, logs, JSON strings, anything open-ended.

**VARCHAR vs TEXT — when to use which:**

| Use `VARCHAR(n)` | Use `TEXT` |
|---|---|
| You want the DB to enforce a max length | No meaningful upper bound exists |
| Short, bounded values (username, country code) | Long or unpredictable content (body, notes) |

---

### 4. DATE — Calendar Dates

Stores a year, month, and day. No time component.

```sql
CREATE TABLE employees (
    hire_date   DATE,
    birth_date  DATE
);

INSERT INTO employees VALUES ('2024-03-15', '1990-07-01');
```

- Format: `YYYY-MM-DD`
- Supports arithmetic: `hire_date + INTERVAL '30 days'`
- Use `TIMESTAMP` when you also need a time of day.

**Date-related types at a glance:**

| Type | Stores | Example |
|---|---|---|
| `DATE` | Date only | `2024-03-15` |
| `TIME` | Time only | `14:30:00` |
| `TIMESTAMP` | Date + time | `2024-03-15 14:30:00` |
| `TIMESTAMPTZ` | Date + time + timezone | `2024-03-15 14:30:00+06` |

> Prefer `TIMESTAMPTZ` in production — it stores UTC and converts on retrieval, avoiding timezone bugs.

---

### 5. BOOLEAN — True / False

Stores a logical value: `TRUE` or `FALSE`.

```sql
CREATE TABLE accounts (
    is_active    BOOLEAN,
    is_verified  BOOLEAN DEFAULT FALSE
);

INSERT INTO accounts VALUES (TRUE, FALSE);
```

- PostgreSQL also accepts: `'t'`, `'f'`, `'yes'`, `'no'`, `'1'`, `'0'`
- Can be `NULL` if the column allows it — meaning *unknown*, not false.
- Common in `WHERE` clauses: `WHERE is_active = TRUE` or simply `WHERE is_active`

---

## Full Comparison Table

| Type | Storage | Null-able | Use Case |
|---|---|---|---|
| `SMALLINT` | 2 bytes | Yes | Small counters, status codes |
| `INT` | 4 bytes | Yes | IDs, quantities, ages |
| `BIGINT` | 8 bytes | Yes | Large IDs, financial amounts |
| `VARCHAR(n)` | Variable (≤ n chars) | Yes | Names, emails, codes |
| `TEXT` | Variable (unlimited) | Yes | Bodies, notes, logs |
| `DATE` | 4 bytes | Yes | Birthdays, deadlines |
| `TIMESTAMP` | 8 bytes | Yes | Event times (no timezone) |
| `TIMESTAMPTZ` | 8 bytes | Yes | Event times (with timezone) |
| `BOOLEAN` | 1 byte | Yes | Flags, on/off states |

---

## Practical Example — Putting It Together

```sql
CREATE TABLE users (
    id           INT,
    username     VARCHAR(50),
    bio          TEXT,
    birth_date   DATE,
    is_active    BOOLEAN DEFAULT TRUE
);

INSERT INTO users (id, username, bio, birth_date, is_active)
VALUES (
    1,
    'alice',
    'Software engineer and open source contributor.',
    '1992-05-20',
    TRUE
);
```

---

## Key Takeaways

- Use `INT` for whole numbers; scale up to `BIGINT` for large IDs.
- Use `VARCHAR(n)` when a max length is meaningful; use `TEXT` when it is not.
- Use `DATE` for calendar dates; use `TIMESTAMPTZ` when time and timezone matter.
- `BOOLEAN` can be `NULL` — that means *unknown*, not `FALSE`.
- Choosing the right type up front prevents bad data and avoids costly migrations later.
- Primary Key vs Unique Key
- Foreign Key & Relationships
- Constraints (NOT NULL, CHECK, DEFAULT)
- NULL Handling (IS NULL vs = NULL)
- CRUD Operations (CREATE, READ, UPDATE, DELETE)
- TRUNCATE vs DELETE
- WHERE Clause & Filtering
- ORDER BY & Sorting
- LIMIT & OFFSET
- DISTINCT
- Aliases (AS)
- Aggregate Functions (COUNT, SUM, AVG, MAX, MIN)
- GROUP BY
- HAVING vs WHERE
- Basic Subqueries
- EXISTS vs IN

### 03. Advanced Query Techniques

- JOIN Fundamentals
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- SELF JOIN
- CROSS JOIN
- Multiple Table Joins
- Join Order & Performance Impact
- Correlated vs Non-Correlated Subqueries
- Common Table Expressions (CTE – WITH)
- Recursive CTE
- Window Functions (ROW_NUMBER, RANK, DENSE_RANK)
- PARTITION BY
- Window vs GROUP BY
- CASE Statements
- Conditional Aggregation
- UNION vs UNION ALL
- INTERSECT
- EXCEPT

### 04. PostgreSQL-Specific Features

- PostgreSQL Architecture Overview
- MVCC (Multi-Version Concurrency Control)
- PostgreSQL Data Types
- SERIAL
- UUID
- JSON vs JSONB
- ARRAY
- ENUM
- TIMESTAMP WITH TIME ZONE
- Generated Columns
- Extensions (uuid-ossp, pg_trgm, PostGIS)
- Full Text Search
- GIN & GiST Indexing
- Materialized Views
- Stored Procedures vs Functions
- Triggers
- Sequences
- Upsert (ON CONFLICT)
- Returning Clause

### 05. Indexing & Performance Optimization

- What is an Index?
- Why Index is Needed?
- B-Tree Index (Default)
- Hash Index
- GIN Index
- GiST Index
- BRIN Index
- Composite Index
- Partial Index
- Expression Index
- When NOT to Use Index
- EXPLAIN
- EXPLAIN ANALYZE
- Query Execution Plan
- Sequential Scan vs Index Scan
- Vacuum & Autovacuum
- Write-Ahead Logging (WAL)
- Query Optimization Techniques
- Connection Pooling (PgBouncer)

### 06. Transactions & Concurrency Control

- ACID Properties
- Transaction Lifecycle
- BEGIN / COMMIT / ROLLBACK
- Savepoints
- Isolation Levels
- Read Uncommitted
- Read Committed
- Repeatable Read
- Serializable
- Dirty Reads
- Phantom Reads
- Non-Repeatable Reads
- Locks (Row-level, Table-level)
- Deadlocks
- Lock Monitoring
- Concurrency Handling in PostgreSQL

### 07. Schema Design & Normalization

- Database Design Principles
- One-to-One Relationship
- One-to-Many Relationship
- Many-to-Many Relationship
- Junction Tables
- Normalization
- 1NF
- 2NF
- 3NF
- BCNF
- Denormalization
- Index Strategy Design
- Naming Conventions
- Data Integrity Strategies
- Soft Delete vs Hard Delete
- Audit Columns (created_at, updated_at)

### 08. Security & Roles

- Roles & Users
- CREATE ROLE
- GRANT
- REVOKE
- Superuser vs Normal User
- Row-Level Security (RLS)
- Schema-level Permissions
- Database-level Permissions
- Authentication Methods
- Password Encryption

### 09. Backup, Migration & Production Practices

- pg_dump
- pg_restore
- Logical Backup vs Physical Backup
- Streaming Replication
- Logical Replication
- Failover Strategy
- High Availability Setup
- Partitioning
- Range Partitioning
- List Partitioning
- Hash Partitioning
- Vertical Scaling
- Horizontal Scaling
- Read Replicas
- Database Monitoring
- Production Deployment Best Practices

### 10. PostgreSQL DBA & Scaling Concepts

- PostgreSQL Internal Architecture
- Background Processes
- WAL Internals
- Checkpoints
- Autovacuum Tuning
- Query Planner & Optimizer
- Memory Configuration (shared_buffers, work_mem)
- Connection Management
- Sharding Strategies
- Load Balancing
- Disaster Recovery Planning
- Performance Benchmarking

## 🍃 MongoDB

> MongoDB is a modern NoSQL document-oriented database designed for high scalability, flexibility, and rapid development. This guide covers core MongoDB fundamentals, data modeling strategies, query techniques, performance optimization, and production-ready architecture design.

| Topics                                                                            | Overview                             |
| --------------------------------------------------------------------------------- | ------------------------------------ |
| [01. MongoDB Fundamentals](#01-mongodb-fundamentals)                              | Core NoSQL concepts & document model |
| [02. CRUD & Query Foundations](#02-crud--query-foundations)                       | Insert, find, update, delete         |
| [03. Advanced Query & Aggregation](#03-advanced-query--aggregation)               | Aggregation pipeline & operators     |
| [04. Data Modeling & Schema Design](#04-data-modeling--schema-design)             | Embedding vs referencing             |
| [05. Indexing & Performance Optimization](#05-indexing--performance-optimization) | Index strategies & explain           |
| [06. Transactions & Concurrency](#06-transactions--concurrency)                   | ACID, sessions, write concerns       |
| [07. Replication & High Availability](#07-replication--high-availability)         | Replica sets                         |
| [08. Sharding & Horizontal Scaling](#08-sharding--horizontal-scaling)             | Distributed scaling                  |
| [09. Security & Access Control](#09-security--access-control)                     | Users, roles, authentication         |
| [10. Production, Backup & DBA Concepts](#10-production-backup--dba-concepts)      | Monitoring & tuning                  |

### 01. MongoDB Fundamentals

- What is NoSQL?
- Document-Oriented Database
- BSON Format
- Documents vs Rows
- Dynamic Schema
- MongoDB Architecture Overview
- MongoDB Server (mongod)
- Mongo Shell (mongosh)
- MongoDB Atlas
- Database & Collection Concept
- \_id Field & ObjectId
- CAP Theorem
- BASE vs ACID
- Introduction to MongoDB

### 02. CRUD & Query Foundations

- Insert One (insertOne)
- Insert Many (insertMany)
- Find Documents (find), Find One (findOne)
- Projection
- Update One (updateOne), Update Many (updateMany)
- Replace One
- Delete One (deleteOne), Delete Many (deleteMany)
- Upsert
- Query Operators
- Comparison Operators ($eq, $gt, $lt, $in)
- Logical Operators ($and, $or, $not)
- Element Operators ($exists, $type)
- Sorting (sort)
- Limit & Skip
- Count Documents
- Distinct
- Bulk Write Operations

### 03. Advanced Query & Aggregation

- Aggregation Framework Overview
- Aggregation Pipeline
- $match
- $group
- $project
- $sort
- $limit
- $skip
- $lookup (Join equivalent)
- $unwind
- $addFields
- $set
- $facet
- $bucket
- $count
- $merge
- $out
- Pipeline Optimization
- MapReduce (Legacy concept)
- Text Search
- Geospatial Queries
- Array Query Operators
- Conditional Operators

### 04. Data Modeling & Schema Design

- Embedding vs Referencing
- One-to-One Relationship
- One-to-Many Relationship
- Many-to-Many Relationship
- Data Duplication Strategy
- Denormalization in MongoDB
- Schema Validation
- JSON Schema Validation
- Document Size Limit (16MB)
- Bucket Pattern
- Attribute Pattern
- Polymorphic Pattern
- Event Sourcing Pattern
- Time-Series Collections
- Capped Collections
- Naming Conventions
- Soft Delete Strategy
- Audit Fields (createdAt, updatedAt)

### 05. Indexing & Performance Optimization

- What is an Index?
- Default \_id Index
- Single Field Index
- Compound Index
- Multikey Index
- Text Index
- Geospatial Index (2dsphere)
- Hashed Index
- TTL Index
- Partial Index
- Sparse Index
- Unique Index
- Index Intersection
- Covered Queries
- Explain Plan
- Query Execution Stats
- Read vs Write Trade-offs
- Performance Monitoring
- Connection Pooling
- Slow Query Analysis

### 06. Transactions & Concurrency

- ACID Transactions (MongoDB 4.0+)
- Single Document Atomicity
- Multi-Document Transactions
- Sessions
- Write Concern
- Read Concern
- Read Preference
- Optimistic Concurrency
- Locks in MongoDB
- Snapshot Isolation
- Retryable Writes
- Change Streams

### 07. Replication & High Availability

- Replica Set Architecture
- Primary Node
- Secondary Node
- Arbiter
- Election Process
- Oplog
- Automatic Failover
- Read Scaling
- Replication Lag
- Backup from Secondary
- Monitoring Replica Set

### 08. Sharding & Horizontal Scaling

- What is Sharding?
- Shard Key Concept
- Choosing a Good Shard Key
- Shard Cluster Architecture
- Config Server
- Query Router (mongos)
- Shards
- Range-Based Sharding
- Hashed Sharding
- Zone Sharding
- Balancer Process
- Chunk Migration
- Resharding
- Cross-Shard Transactions
- Scaling Strategy

### 09. Security & Access Control

- SCRAM
- X.509
- LDAP
- Role-Based Access Control (RBAC)
- Built-in Roles
- Custom Roles
- Create User
- Grant Roles
- Field-Level Encryption
- TLS/SSL Encryption
- IP Whitelisting
- Auditing
- Network Security Best Practices

### 10. Production, Backup & DBA Concepts

- mongodump
- mongorestore
- Logical Backup
- Cloud Backup (Atlas)
- Point-in-Time Recovery
- Monitoring Tools
- MongoDB Compass
- Atlas Monitoring
- Prometheus + Grafana
- Performance Metrics
- CPU & Memory Monitoring
- Disk I/O Optimization
- WiredTiger Storage Engine
- Journaling
- Compression
- Production Deployment Best Practices
- Disaster Recovery Planning
- Load Testing
- Capacity Planning
