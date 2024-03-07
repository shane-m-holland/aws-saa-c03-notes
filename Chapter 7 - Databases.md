# Databases
## Relational Database Service (RDS) Overview

### Relational Databases

Relational databases have been around for many years and are the heart of many applications

- **Tables** : Data is organized into tables.  Think of a traditional spreadsheet.
- **Rows** : The data items
- **Columns** : The fields in the database.

**Relational Database Engines**

- SQL Server
- Oracle
- MySQL
- PostgreSQL
- MariaDB
- Aurora
### RDS Advantages

**Up and Running in Minutes**
Multi-AZ
Failover capability
Automated backups

A manual install in your own data center could take 8 days or longer.

**When would we use an RDS database?**

RDS is generally used for **online transaction28 processing (OLTP)**  workloads.
### OLTP and OLAP

Understand the difference between **online transaction processing (OLTP)** and **online analytical porcessing (OLAP)**.

**OLTP**

Processes data from transactions in real time (e.g. customer orders, banking transactions, payments, and booking systems).

OLTP is all about data processing and completing large numbers if small transactions in real time.

**OLAP**

Processes complex queries to analyze historical data (e.g. analyzing net profit figures from the past 3 years and sales forecasting).

OLAP is all about data analysis using large amounts of data, as well as complex queries that take a long time to complete.
### Multi-AZ

**With Multi-AZ, RDS creates an exact copy of your production database in another Availability Zone.**

AWS handles the replication for you.

When you write to your production database, this write will automatically synchronize to the standby database.

**RDS types which can be configured as Multi-AZ**

- SQL Server
- MySQL
- MariaDB
- Oracle
- PostgreSQL
### Unplanned Failures and Maintenance

RDS will **automatically fail over** to the standby during a failure so database operations can **resume quickly** without administrative intervention.

Multi-AZ is for **disaster recovery**, not for improving performance, so you **cannot** connect to the standby when the primary database is **active.**

**Update:** AWS now offers **Multi-AZ deployment clusters**, which allow **2 readable standby instances**.
### Exam Tips

- **RDS Database Types**
	  SQL Server, Oracle, MySQL, PostgreSQL, MariaDB, and Amazon Aurora
- **RDS is for OLTP Workloads**
	  Great for processing lots of small transactions, like customer orders, banking transactions, payments, and booking systems.
- **Not Suitable for OLAP Workloads**
	  Use Redshift for data warehousing and OLAP tasks, like analyzing large amounts of data, reporting, and sales forecasting

---
## Increasing Read Performance with Read Replicas

### What is a Read Replica

- A **read-replica** is a read-only copy of your primary database.
- Great for read-heavy workloads and takes the load off your primary database.
- A **read-replica** can be **cross-AZ**, or **cross-region**.
- Each read replica has its own DNS endpoint.
- Read replicas can be promoted to be their own databases.
	  This breaks the replication
### Key Facts

- **Scaling Read Performance**
	  Primarily used for scaling, **not** for disaster recovery!
- **Requires Automatic Backups**
	  Automatic backups must be enabled in order to deploy a read replica.
- **Multiple Read Replicas Are Supported**
	  MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server allow you to add up to 5 read replicas to each DB instance.

### Exam Tips

**Multi-AZ** vs **Read Replica**

**Multi-AZ**
- An exact copy of your production database in another Availability Zone.
- Used for disaster recovery.
- In the event of a failure, RDS will automatically fail over to the standby instance.

**Read Replica**
- A read-only copy of your primary database in the same AZ, cross-AZ, or cross-region.
- Used to increase or scale read performance.
- Great for read-heavy workloads and takes the load off your primary database for read-only workloads (e.g. Business Intelligence reporting jobs).

---
## Amazon Aurora

### What is Aurora

Amazon Aurora is a MySQL-and PostgreSQL-compatible relation database engine that combines the speed and availability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases.
### Aurora Performance

**5x Performance**

Amazon Aurora provides **up to 5 times better performance than MySQL** and 3 times better than PostgreSQL databases at a much lower price point, while delivering similar performance and availability.
### Basics

- Starts with 10GB.  Scales in 10-GB increments to 128 TB (storage Auto Scaling).
- Compute resources can scale up to 96 vCPUs and 768 GB of memory.
- 2 copies of your data are contained in each Availability Zone, with a minimum of 3 Availability Zones.  6 copies of your data.

### Scaling

- Aurora is designed to **transparently handle the loss of up to 2 copies of data** without affecting database write availability and up to 3 copies without affecting read availability.
- Aurora **storage is also self-healing.** Data blocks and disks are continuously scanned for errors and repaired automatically.

### Types of Aurora Replicas

Each type supports up to 15 read replicas.

- **Aurora Replicas**
- **MySQL Replicas**
- **PostgreSQL Replicas**

| Feature | Amazon Aurora Replicas | MySQL Replicas |
| --- | --- | --- |
| Number of Replicas | Up to 15 | Up to 15 |
| Replication Type | Asynchronous (milliseconds) | Asynchronous (seconds) |
| Performance impact on primary | Low | High |
| Replica location | In-region | Cross-region |
| Act as failover target | Yes (no data loss) | Yes (potentially minutes of (data loss) |
| Automated failover | Yes | No |
| Support for user-defined replication delay | No | Yes |
| Support for different data or schema vs. primary | No | Yes |

### Backups with Aurora

- Automated backups are always enabled on Amazon Aurora DB Instances.  Backups do not impact database performance.
- You can also take snapshots with Aurora.  This also does not impact performance.
- You can share Aurora snapshots with other AWS accounts.

### Amazon Aurora Serverless

An on-demand, auto-scaling configuration for the MySQL-compatible and PostgreSQL-compatible editions of Amazon Aurora.  **An Aurora Serverless DB cluster automatically starts up, shuts down, and scaled capacity up or down based on your application's needs.**

**Use Cases**

Aurora Serverless provides a relatively simple, cost-effective option for infrequent, intermittent, or unpredictable workloads.
### Exam Tips

- 2 copies of your data contained in each Availability Zone, with a minimum of 3 Availability Zones.  6 copies of your data.
- You can share Aurora snapshots with other AWS accounts.
- 3 types of replicas available: Aurora Replicas, MySQL Replicas, and PostgreSQL Replicas.  Automated failover is only available with Aurora replicas.
- Aurora has automated backups turned on by default.  You can also take snapshots with Aurora.  You can share these snapshots with other AWS accounts.
- Use Aurora Serverless if you want a simple, cost-effective option for infrequent, intermittent, or unpredictable workloads.

---
## DynamoDB

### What is DynamoDB?

Amazon DynamoDB is a **fast and flexible NoSQL database** service for all applications that need consistent, single-digit millisecond latency at any scale.

It is a fully managed database and supports both document and key-value data models.

Its flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT, and many other applications.
### High-Level DynamoDB

- Stored on SSD storage
- Spread across 3 geographically distinct data centers
- Eventually consistent reads (default)
- Configurable for strongly consistent reads 

### Read Consistency

What's the difference between **eventually consistent reads** and **strongly consistent reads?**
#### Eventually
Consistent across all **copies of data is usually reached within a second.**  Repeating a read after a short time should return the updated data.  Best read performance.
#### Strongly
A strongly consistent read **returns a result that reflects all writes** that received a successful response prior to the read.

### DynamoDB Accelerator (DAX)

- Fully managed, highly available, in-memory cache.
- 10x performance improvement
- Reduces request time from milliseconds to **microseconds** -- even under load.
- No need for developers to manage caching logic.
- Compatible with DynamoDB API calls.

### On-Demand Capacity
- **Pay-per-request** pricing
- Balance cost and performance
- No minimum capacity
- **Pay more per request** than with provisioned capacity
- Use for new product launches

### Security
- Encryption at rest using **KMS**
- Site-to-site VPN
- Direct Connect (DX)
- IAM policies and roles
- Fine-grained access
- CloudWatch and CloudTrail
- VPC endpoints

---
## When Do We Use DynamoDB Transactions

### ACID Diagram
![[./resources/img/DyanmoDB-ACID-diagram.png]]
### ACID with DynamoDB

DynamoDB transactions provide developers atomicity, consistency, isolation, and durability (ACID) across 1 or more tables within a single AWS account and region.

You can use transactions when building applications that require coordinated inserts, deletes, or updates to multiple items as part of a single logical business operation.

### All or Nothing Transactions

**ACID basically means all or nothing**

### Use Cases for DynamoDB Transactions

- Processing financial transactions
- Fulfilling and managing orders
- Building multiplayer game engines
- Coordinating actions across distributed components and services

### Transactions

- Multiple "all-or-nothing" operations
- Financial Transactions
- Fulfilling orders
- 3 options for reads: eventual consistency, strong consistency, and transactional
- 2 options for writes: standard and transactional
- Up to 25 items or 4MB of data

### Exam Tips

- If you see any scenario that mentions ACID requirements, think **DynamoDB Transactions**
- DynamoDB transactions provide developers **atomicity, consistency, isolation,** and **durability** (ACID) across 1 or more tables within a single AWS account and region.
- **All-or-nothing** transactions.

---
## DynamoDB Backups

### On-Demand Backup and Restore

- Full backups at any time
- Zero impact on table performance or availability
- Consistent within seconds and **retained until deleted**
- Operates within the same region as the source table

### Point-in-Time Recovery (PITR)

- Protects against accidental writes or deletes
- Restore to any point in the last 35 days
- Incremental backups
- Not enabled by default
- Latest restorable time is **5 minutes** in the past

---
## DynamoDB Streams and Global Tables

### Streams

- Time-ordered sequence of item-level changes in a table.
- Stored for **24 hours**
- Inserts, updates, and deletes
- Combine with Lambda functions for functionality like stored procedures
### Global Tables

**Managed Multi-Master, Multi-Region Replication**
- Great for globally distributed applications
- Based on DynamoDB streams (required)
- Multi-region redundancy for disaster recovery or high availability
- No application rewrites
- Replication latency under **1 second**

---

## Amazon DocumentDB (MongoDB)

### What is MongoDB?

MongoDB is a document database that allows for scalability and flexibility with your data as well as robust querying and indexing features.

https://www.mongodb.com/what-is-mongodb
### What is Amazon DocumentDB?

Allows you to run MongoDB on the AWS cloud.  It's a managed database service that scales with your workloads and safely and durably stores your database information.

### Why Use It?

You no longer have to worry about all the manual tasks when running MongoDB workloads, such as cluster management software, configuring backups, or monitoring production workloads.

Get rid of your operational overheads!
### Exam Tips

A typical question might be about moving your MongoDB database to AWS.

1. MongoDB On-Premises
2. AWS Database Migration Service
3. Amazon DocumentDB

For scenarios where they talk about migrating MongoDB from on-premises to AWS, **think Amazon DocumentDB**.

---

## Amazon Keyspaces (Apache Cassandra)

### What is Cassandra?

A distributed database (i.e. it runs on many machines) that uses NoSQL.  It's primarily used for big data solutions.  Enterprises, such as Netflix, use Cassandra on their backend.

https://cassandra.apache.org/_/cassandra-basics.html

### What is Keyspaces?

Amazon's Apache Cassandra database service.  It allows you to run Cassandra workloads on AWS and is a fully managed database service.

### Why Use It?

It's a fully managed database service -- you don't need to worry about managing servers, software, patching, etc.

**Keyspaces is serverless!**

You pay for the resources you use, and the service can automatically sale tables up and down in response to your applications.

### Exam Tips

**Scenario Question about Cassandra?**

If you see a scenario question about migrating a big data Cassandra cluster to AWS.. 
**Think of Amazon Keyspaces!**

---
## Amazon Neptune (Graph Database)

### What is a Graph Database?

**Data is stored just like you might sketch ideas on a whiteboard.**
A graph database stores nodes and relationships instead of tables or documents.
### Neptune

**Neptune is Amazon's graph database service.**
Neptune is a fast, reliable, fully managed graph database service that makes it easy to build and run applications.

### Use Cases

**Build Connections between Identities**
Easily build identity graphs for identity resolutions solutions such as social graphs, and accelerate updates for ad targeting, personalization, and analytics.

**Build Knowledge Graph Applications**
Add topical data to product catalogs, model generation information, and help users quickly navigate highly connected datasets.

**Detect Fraud Patterns**
Build graph queries for near real-time identity fraud pattern detection in financial and purchase transactions.

**Security Graphs to Improve IT Security**
Proactively detect and investigate IT infrastructure using a layered security approach.  Visualize all infrastructure to plan, predict, and mitigate risk.

### Exam Tips

**Neptune is often used as a distractor.**

If the scenario is **not** talking about graph databases, **do not** select Neptune as an answer.  You only need to know what Neptune does at a very high level.

---
## Amazon Quantum Ledger Database (Amazon QLDB)

### What is a Ledger Database?

It's a NOSQL database that is immutable, transparent, and has a cryptographically verifiable transaction log that is owned by one authority

**You cannot update a record (i.e. replace old content) in a ledger database.  Instead, an update adds a new record to the database.**

- It's used for cryptocurrencies, such as Bitcoin, Ethereum, etc.
- Shipping companies use it to track items, boxes, shipping containers, deliveries, etc.
- Pharmaceutical companies use it to track creation and distribution of drugs and ensure no counterfeits are produced.
### Amazon Quantum Ledger Database (QLDB)

A fully managed ledger database that provides a transparent, immutable, and cryptographically verifiable transaction log.

### QLDB Use Cases

**Store Financial Transactions**
Create a complete and accurate record of all financial transactions, such as credit and debit transactions.

**Reconcile Supply Chain Systems**
Record the history of each transaction, and provide details of every batch manufactured, shipped, stored, and sold from facility to store.

**Maintain Claims History**
Track a claim over its lifetime, and cryptographically verify data integrity to make the application resilient against data entry errors and manipulation.

**Centralize Digital Records**
Implement a system-of-record application to create a complete, centralized record of employee details, such as payroll, bonus, and benefits.

### Exam Tips

**QLDB is often used as a distractor.**

If the scenario is not talking about immutable databases, do not select Amazon QLDB as an answer.  You need to know what QLDB does only at a very high level.

---
## Amazon Timestream (Time-Series Database)

### Time-Series Data

Data points that are logged over a series of time, allowing you to track your data.  Examples could be temperature readings from weather stations around the world, on the hour, every hour for years.

### Time-Series Data Examples

- **IoT**
	  IoT sensors relay thousands, millions, and billions of points of information depending on the setup.  One use case is for agriculture.
- **Analytics**
	  Large websites such as Netflix serve millions of users per second.  Need to analyze incoming and outgoing web traffic.
- **DevOps Applications**
	   Applications that change in response to users needs may need to be monitored continuously so they can scale correctly.
### Amazon Timestream

A serverless, fully managed database service for time-series data.  You can analyze **trillions** of events per day up to **1,000 times faster** and at as little as **1/10th the cost** of traditional relations databases.
### Exam Tips

**Scenario Question about Time-Series Data?**

If you see a scenario question about where to store a large amount of time-series data for analysis...
**Think of Timestream.**