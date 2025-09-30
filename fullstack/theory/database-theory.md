# Database Theory: Data Management and Storage

## What is a Database? 💾

A database is an organized collection of data that is stored and accessed electronically. Think of it as a super-organized digital filing cabinet where information is stored in a structured way that makes it easy to retrieve, update, and analyze.

## Types of Databases

### Relational Databases (SQL)

**Structure:** Data organized in tables with rows and columns, connected through relationships.

```
┌─────────────────┐    ┌─────────────────┐
│     Users       │    │    Orders       │
├─────────────────┤    ├─────────────────┤
│ id │ name │ email │  │ id │ user_id │ total │
├────┼──────┼───────┤  ├────┼─────────┼───────┤
│ 1  │ John │ j@e.c │  │ 1  │    1    │ 99.99 │
│ 2  │ Jane │ j@e.c │  │ 2  │    1    │ 49.99 │
└────┴──────┴───────┘  └────┴─────────┴───────┘
             │                       │
             └───────────────────────┘
                   Relationship
```

**Examples:** MySQL, PostgreSQL, SQLite, SQL Server

### NoSQL Databases

**Document Databases:** Store data as JSON-like documents
**Key-Value Stores:** Simple key-value pairs
**Column-Family Stores:** Data stored in columns rather than rows
**Graph Databases:** Data stored as nodes and relationships

**Examples:** MongoDB (Document), Redis (Key-Value), Cassandra (Column-Family), Neo4j (Graph)

## Database Design Principles

### Entity-Relationship Modeling

**Entities:** Objects or concepts (Users, Products, Orders)
**Attributes:** Properties of entities (name, email, price)
**Relationships:** Connections between entities (User has Orders, Product belongs to Category)

### Normalization

**First Normal Form (1NF):** Eliminate repeating groups
**Second Normal Form (2NF):** Remove partial dependencies
**Third Normal Form (3NF):** Remove transitive dependencies

**Benefits:**
- Reduces data redundancy
- Improves data integrity
- Simplifies maintenance

### Denormalization

**When to Denormalize:**
- Performance is critical
- Read operations far outnumber writes
- Complex joins are slowing queries

**Trade-offs:**
- Faster reads
- Slower writes
- Data consistency challenges

## SQL Fundamentals

### Data Definition Language (DDL)

**Creating Tables:**
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Modifying Structure:**
```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users DROP COLUMN phone;
```

### Data Manipulation Language (DML)

**Inserting Data:**
```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
INSERT INTO users (name, email) VALUES
    ('Jane Smith', 'jane@example.com'),
    ('Bob Wilson', 'bob@example.com');
```

**Querying Data:**
```sql
SELECT * FROM users;
SELECT name, email FROM users WHERE id = 1;
SELECT COUNT(*) FROM users WHERE created_at > '2024-01-01';
```

**Updating Data:**
```sql
UPDATE users SET name = 'John Smith' WHERE id = 1;
UPDATE users SET last_login = NOW() WHERE email = 'john@example.com';
```

**Deleting Data:**
```sql
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE created_at < '2023-01-01';
```

### Data Control Language (DCL)

**Permissions:**
```sql
GRANT SELECT, INSERT ON users TO 'app_user'@'localhost';
REVOKE DELETE ON users FROM 'app_user'@'localhost';
```

## Indexing and Performance

### Why Indexes Matter

**Without Index:**
```
Table Scan: O(n) - checks every row
```

**With Index:**
```
Index Lookup: O(log n) - binary search on sorted index
```

### Index Types

**B-Tree Index (Default):**
- Good for equality and range queries
- Balanced tree structure
- Used in most relational databases

**Hash Index:**
- Excellent for equality lookups
- Poor for range queries
- Used in key-value stores

**Full-Text Index:**
- For text search within documents
- Supports fuzzy matching
- Used in search engines

### Index Best Practices

**When to Create Indexes:**
- Foreign key columns
- Columns frequently used in WHERE clauses
- Columns used in JOIN conditions
- Columns used for sorting (ORDER BY)

**Index Maintenance:**
- Rebuild indexes periodically
- Monitor index usage
- Remove unused indexes
- Consider composite indexes

## Transactions and ACID Properties

### ACID Principles

**Atomicity:** All operations succeed or all fail
**Consistency:** Database remains in valid state
**Isolation:** Transactions don't interfere with each other
**Durability:** Committed changes survive failures

### Transaction Example

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- If either fails, rollback both
IF @@ERROR <> 0
    ROLLBACK;
ELSE
    COMMIT;
```

## Database Relationships

### One-to-One Relationship

```
┌─────────────┐    ┌─────────────────┐
│   Users     │    │  UserProfiles   │
├─────────────┤    ├─────────────────┤
│ id (PK)     │    │ id (PK, FK)     │
│ name        │    │ bio             │
│ email       │    │ avatar_url      │
└─────────────┘    └─────────────────┘
```

**Use Case:** User profile information

### One-to-Many Relationship

```
┌─────────────┐    ┌─────────────┐
│   Users     │    │   Posts     │
├─────────────┤    ├─────────────┤
│ id (PK)     │    │ id (PK)     │
│ name        │    │ user_id (FK)│
│ email       │    │ title       │
└─────────────┘    │ content     │
                   └─────────────┘
```

**Use Case:** User posts, order items

### Many-to-Many Relationship

```
┌─────────────┐    ┌─────────────────┐    ┌─────────────┐
│   Users     │    │ UserRoles       │    │   Roles     │
├─────────────┤    ├─────────────────┤    ├─────────────┤
│ id (PK)     │    │ user_id (FK)    │    │ id (PK)     │
│ name        │    │ role_id (FK)    │    │ name        │
│ email       │    └─────────────────┘    │ permissions │
└─────────────┘                           └─────────────┘
```

**Use Case:** Users with multiple roles, products in multiple categories

## NoSQL Data Modeling

### Document Databases (MongoDB)

**Document Structure:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "John Doe",
  "email": "john@example.com",
  "posts": [
    {
      "title": "My First Post",
      "content": "Hello world!",
      "tags": ["introduction", "hello"]
    }
  ],
  "createdAt": ISODate("2024-01-15T00:00:00Z")
}
```

**Embedding vs Referencing:**
- **Embedding:** Store related data in same document
- **Referencing:** Store IDs to link documents

### Key-Value Stores (Redis)

**Simple Structure:**
```
user:123 → {"name": "John", "email": "john@example.com"}
cart:456 → ["item1", "item2", "item3"]
session:789 → "active"
```

**Use Cases:**
- Caching
- Session storage
- Real-time data
- Counters and queues

## Database Scaling

### Vertical Scaling

**Scale Up:**
- More powerful hardware
- Increased CPU, memory, storage
- Simpler implementation
- Hardware limitations

### Horizontal Scaling

**Scale Out:**
- Multiple database servers
- Load balancing
- Data partitioning (sharding)
- Replication for redundancy

### Read Replicas

```
┌─────────────────┐
│   Primary DB    │ ← Writes
│   (Master)      │
└─────────────────┘
    │        │
    ▼        ▼
┌──────┐  ┌──────┐
│Replica│  │Replica│ ← Reads
│  1   │  │  2   │
└──────┘  └──────┘
```

## Backup and Recovery

### Backup Types

**Full Backup:**
- Complete database copy
- Time-consuming
- Easy restoration

**Incremental Backup:**
- Only changes since last backup
- Faster backup process
- Complex restoration

**Differential Backup:**
- Changes since last full backup
- Balanced approach

### Recovery Strategies

**Point-in-Time Recovery:**
- Restore to specific timestamp
- Requires transaction logs

**Disaster Recovery:**
- Off-site backups
- Failover systems
- Geographic redundancy

## Database Security

### Access Control

**Authentication:**
- User credentials
- Certificate-based auth
- Multi-factor authentication

**Authorization:**
- Role-based access control (RBAC)
- Attribute-based access control (ABAC)
- Least privilege principle

### Data Protection

**Encryption:**
- Data at rest (storage encryption)
- Data in transit (TLS/SSL)
- Application-level encryption

**Auditing:**
- Query logging
- Access monitoring
- Change tracking

## Performance Monitoring

### Key Metrics

**Query Performance:**
- Slow query logs
- Query execution plans
- Index usage statistics

**System Resources:**
- CPU utilization
- Memory usage
- Disk I/O
- Network traffic

**Database Health:**
- Connection pool status
- Lock waits
- Deadlock detection
- Replication lag

### Monitoring Tools

**Database-Specific:**
- MySQL: Performance Schema, Slow Query Log
- PostgreSQL: pg_stat_statements, pgBadger
- MongoDB: Database Profiler, explain()

**General Tools:**
- Prometheus, Grafana
- DataDog, New Relic
- Custom monitoring scripts

## Database Migration

### Schema Evolution

**Migration Scripts:**
```sql
-- Version 1: Initial schema
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(100));

-- Version 2: Add email column
ALTER TABLE users ADD COLUMN email VARCHAR(255) UNIQUE;

-- Version 3: Add index for performance
CREATE INDEX idx_users_email ON users(email);
```

### Migration Tools

**Version Control for Schema:**
- Flyway (Java-based)
- Liquibase (XML/YAML/SQL)
- Alembic (Python)
- Prisma Migrate (Node.js)

## Choosing the Right Database

### Decision Factors

**Data Structure:**
- Structured → Relational (SQL)
- Semi-structured → Document (MongoDB)
- Key-Value → Redis, DynamoDB
- Relationships → Graph (Neo4j)

**Workload Patterns:**
- Read-heavy → Consider caching, read replicas
- Write-heavy → Optimize for writes, consider sharding
- Analytics → Columnar databases (ClickHouse, Snowflake)

**Scalability Needs:**
- Small application → SQLite, single PostgreSQL
- Growing app → Managed databases (RDS, Cloud SQL)
- High scale → Distributed systems (CockroachDB, YugabyteDB)

### Migration Strategies

**SQL to NoSQL:**
- Start with hybrid approach
- Migrate one feature at a time
- Maintain data consistency
- Plan rollback strategy

**Database Replacement:**
- Assess current pain points
- Prototype with new database
- Data migration planning
- Gradual rollout with feature flags

## Future of Databases

### NewSQL Databases

**Combine SQL and NoSQL:**
- ACID transactions
- Horizontal scaling
- SQL interface
- Examples: CockroachDB, TiDB, YugabyteDB

### Cloud-Native Databases

**Serverless Databases:**
- Auto-scaling
- Pay-per-use
- Managed backups
- Built-in monitoring

**Multi-Model Databases:**
- Support multiple data models
- Flexible schema
- Single database for multiple use cases

### AI-Enhanced Databases

**Intelligent Features:**
- Automatic query optimization
- Predictive scaling
- Anomaly detection
- Natural language queries

Remember: Databases are the foundation of your application. Choose wisely based on your current needs while planning for future growth. Focus on data integrity, performance, and maintainability!
