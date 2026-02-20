# Database Architecture & Design Guide

> In modern software engineering, a well-designed database forms the **backbone** of every scalable and high-performance application. This repository serves as a structured knowledge base covering core **database fundamentals**, essential **system design principles**, practical **query optimization** techniques, and real-world implementation examples.

## 📑 PostgreSQL

| Topics                                                                                     | Overview                                         |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| [01. Database Fundamentals](#01-database-fundamentals)                                     | Core database concepts, RDBMS, SQL basics        |
| [02. SQL Query Foundations](#02-sql-query-foundations)                                     | CRUD operations, filtering, sorting, constraints |
| [03. Advanced Query Techniques](#03-advanced-query-techniques)                             | Joins, subqueries, aggregation, grouping         |
| [04. PostgreSQL-Specific Features](#04-postgresql-specific-features)                       | Data types, JSON, UUID, arrays                   |
| [05. Indexing & Performance Optimization](#05-indexing--performance-optimization)          | Index types, query planning, EXPLAIN             |
| [06. Transactions & Concurrency](#06-transactions--concurrency)                            | ACID, isolation levels, locking                  |
| [07. Schema Design & Normalization](#07-schema-design--normalization)                      | Database design principles                       |
| [08. Security & Roles](#08-security--roles)                                                | Users, roles, permissions                        |
| [09. Backup, Migration & Production Practices](#09-backup-migration--production-practices) | pg_dump, restore, migrations                     |
| [10. PostgreSQL DBA & Scaling Concepts](#10-postgresql-dba--scaling-concepts)              | Replication, partitioning, scaling               |

### 01. Database Fundamentals

- What is a Database?
- What is RDBMS?
- Relational vs Non-Relational Databases
- What is SQL?
- SQL vs RDBMS
- ACID Properties Overview
- Client–Server Architecture
- Tables, Rows, Columns
- Primary Key Concept
- Foreign Key Concept
- Basic ER Modeling
- Introduction to PostgreSQL

### 02. SQL Query Foundations

- What is SQL?
- SQL vs RDBMS
- Data Types (INT, VARCHAR, TEXT, DATE, BOOLEAN)
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

| Topics                                                                | Overview                             |
| --------------------------------------------------------------------- | ------------------------------------ |
| [01. MongoDB Fundamentals](#01-mongodb-fundamentals)                  | Core NoSQL concepts & document model |
| [02. CRUD & Query Foundations](#02-crud--query-foundations)           | Insert, find, update, delete         |
| [03. Advanced Query & Aggregation](#03-advanced-query--aggregation)   | Aggregation pipeline & operators     |
| [04. Data Modeling & Schema Design](#04-data-modeling--schema-design) | Embedding vs referencing             |
| [05. Indexing & Performance Optimization](#05-indexing--performance-optimization) | Index strategies & explain |
| [06. Transactions & Concurrency](#06-transactions--concurrency) | ACID, sessions, write concerns |
| [07. Replication & High Availability](#07-replication--high-availability) | Replica sets |
| [08. Sharding & Horizontal Scaling](#08-sharding--horizontal-scaling) | Distributed scaling |
| [09. Security & Access Control](#09-security--access-control) | Users, roles, authentication |
| [10. Production, Backup & DBA Concepts](#10-production-backup--dba-concepts) | Monitoring & tuning |

### 01. MongoDB Fundamentals

- What is NoSQL?
- Document-Oriented Database
- BSON Format
- Collections vs Tables
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
- Find Documents (find)
- Find One (findOne)
- Projection
- Update One (updateOne)
- Update Many (updateMany)
- Replace One
- Delete One (deleteOne)
- Delete Many (deleteMany)
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

- Authentication Methods
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
