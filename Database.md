# Complete AWS Database Services Knowledge for SAP-C02 Exam

## 1. Amazon RDS (Relational Database Service)

### Core Concepts

**What is RDS:**
- Managed relational database service
- Automates provisioning, patching, backup, recovery, scaling
- Multi-AZ for high availability
- Read replicas for scaling reads
- Supports 6 database engines

**Supported Database Engines:**
1. **MySQL** (5.7, 8.0)
2. **PostgreSQL** (10-16)
3. **MariaDB** (10.2-10.11)
4. **Oracle** (12c, 19c, 21c)
5. **Microsoft SQL Server** (2016, 2017, 2019, 2022)
6. **Amazon Aurora** (MySQL and PostgreSQL compatible - covered separately)

### Storage

**Storage Types:**

**General Purpose SSD (gp2):**
- 3 IOPS/GB, burst up to 3,000 IOPS
- Size: 20 GB - 64 TB (SQL Server: 20 GB - 16 TB)
- Good for: Most workloads

**General Purpose SSD (gp3):**
- Baseline: 3,000 IOPS, 125 MB/s
- Can provision up to 64,000 IOPS, 4,000 MB/s independently
- More cost-effective than gp2
- Size: 20 GB - 64 TB

**Provisioned IOPS SSD (io1/io2):**
- Consistent, high IOPS performance
- io1: Up to 64,000 IOPS
- io2: Up to 256,000 IOPS (for specific engines)
- Size: 100 GB - 64 TB
- Use for: I/O intensive databases, production workloads

**Magnetic (deprecated):**
- Legacy storage type
- Not recommended for new databases

**Storage Auto Scaling:**
- Automatically scales storage when running out of space
- Set maximum storage threshold
- Prevents downtime due to storage exhaustion
- Triggers: Free storage < 10%, low storage lasts 5 min, 6 hours since last modification

### High Availability - Multi-AZ

**Multi-AZ Deployment:**
- Synchronous replication to standby in different AZ
- Automatic failover (typically 60-120 seconds)
- Same endpoint (DNS name doesn't change)
- Standby cannot serve read traffic
- No additional read capacity

**Automatic Failover Triggers:**
- Primary instance failure
- AZ failure
- Instance type change
- OS patching
- Manual failover (reboot with failover)

**Multi-AZ DB Cluster (newer option for MySQL/PostgreSQL):**
- 1 writer + 2 readers in different AZs
- Readers can serve read traffic
- Faster failover (~35 seconds or less)
- Uses cluster endpoint for writes, reader endpoint for reads

### Read Replicas

**Features:**
- Asynchronous replication from primary
- Can be in same AZ, different AZ, or different region
- Can be promoted to standalone database
- Each replica has own DNS endpoint
- Up to 15 read replicas (5 for SQL Server)
- Can create read replica of read replica (replication lag increases)

**Use Cases:**
- Scale read-heavy workloads
- Run analytics/reporting queries without impacting primary
- Disaster recovery (cross-region replicas)
- Data locality (replicas near users)

**Replication:**
- MySQL/MariaDB/PostgreSQL: Asynchronous replication
- Oracle: Active Data Guard (requires license)
- SQL Server: Availability Groups

**Read Replica Requirements:**
- Automatic backups must be enabled
- Replication lag (monitored via CloudWatch)

### Backup and Recovery

**Automated Backups:**
- Daily full backup during maintenance window
- Transaction logs backed up every 5 minutes
- Retention: 1-35 days (default 7 days, 0 to disable)
- Point-in-time recovery (PITR) to any second within retention period
- Stored in S3 (AWS managed)
- No performance impact on Multi-AZ (backup from standby)

**Manual Snapshots:**
- User-initiated backups
- Retained until explicitly deleted
- Can copy snapshots to other regions
- Can share snapshots with other AWS accounts

**Restore:**
- Creates new RDS instance with new DNS endpoint
- Cannot restore to existing instance
- Can restore to different engine version (within same family)
- Can restore from S3 (for MySQL, MariaDB, PostgreSQL)

### Security

**Encryption:**

**At Rest:**
- AWS KMS encryption
- Must enable at creation time
- Includes: DB instance storage, automated backups, snapshots, read replicas, logs
- Cannot enable on existing unencrypted DB (workaround: snapshot → copy with encryption → restore)

**In Transit:**
- SSL/TLS connections
- Force SSL via parameter group (PostgreSQL: rds.force_ssl=1)
- Download SSL certificates from AWS

**Network Security:**
- Deploy in VPC
- Security groups control network access
- No public IP by default (can enable)
- Can use VPC peering, Direct Connect, VPN

**IAM Authentication:**
- Available for MySQL, PostgreSQL, MariaDB
- Use IAM users/roles instead of database passwords
- Authentication token valid for 15 minutes
- SSL connection required

**Access Control:**
- Database users and permissions (native RBAC)
- IAM policies for RDS API actions
- Resource-based policies not supported

**Secrets Manager Integration:**
- Store and rotate database credentials
- Automatic password rotation

### Performance Optimization

**Parameter Groups:**
- Database engine configuration
- Default or custom parameter groups
- Dynamic parameters: Apply immediately
- Static parameters: Require reboot

**Option Groups:**
- Additional features (Oracle: Transparent Data Encryption, SQL Server: Native Backup/Restore)
- Engine-specific

**Performance Insights:**
- Database performance monitoring and analysis
- Visualize database load
- Identify bottlenecks
- Free tier: 7 days of history
- Paid: Up to 2 years of history

**Enhanced Monitoring:**
- Real-time OS metrics
- Granularity: 1-60 seconds
- Stored in CloudWatch Logs
- More detailed than CloudWatch metrics

**CloudWatch Metrics:**
- Standard metrics: CPU, Memory, Disk I/O, Network, Database connections
- Free (1-minute granularity)

### Maintenance and Upgrades

**Maintenance Window:**
- Weekly window for system maintenance
- OS patching, database engine upgrades
- Can specify or use default
- Can defer maintenance (limited time)

**Engine Version Upgrades:**
- Minor version upgrades: Automatic or manual
- Major version upgrades: Manual only
- Can test upgrades on read replica or snapshot

**Blue/Green Deployments:**
- Create separate environment (green)
- Test changes, promote when ready
- Minimal downtime switchover
- Available for MySQL, MariaDB, PostgreSQL

### Scaling

**Vertical Scaling (Compute):**
- Change instance class
- Requires downtime (Multi-AZ minimizes)
- Can schedule during maintenance window

**Horizontal Scaling (Storage):**
- Storage auto-scaling
- Cannot decrease storage size
- Some downtime for certain engines

**Read Scaling:**
- Add read replicas
- Application must route read traffic to replicas

### RDS Proxy

**Features:**
- Fully managed database proxy
- Connection pooling and sharing
- Reduces database connections and memory usage
- Improved application availability (automatic failover handling)
- Enforces IAM authentication
- Stores credentials in Secrets Manager

**Use Cases:**
- Lambda functions with many connections
- Applications with unpredictable workloads
- Improve failover time (up to 66% faster)
- IAM-based authentication

**Supported Engines:**
- MySQL, PostgreSQL, MariaDB, SQL Server
- Aurora MySQL and Aurora PostgreSQL

### Exam-Focused Scenarios

**When to use RDS:**
- Need managed relational database
- Standard SQL databases (not extreme scale)
- Reduce operational overhead (patching, backups, HA)
- Existing applications using MySQL, PostgreSQL, Oracle, SQL Server
- Not suitable for: Massive scale (use Aurora or DynamoDB), data warehousing (use Redshift)

**Multi-AZ vs Read Replicas:**
- **Multi-AZ:** High availability, synchronous, automatic failover, same region, standby not readable
- **Read Replicas:** Scalability, asynchronous, manual promotion, can be cross-region, readable

**Cost Optimization:**
- Right-size instance types
- Use gp3 instead of gp2
- Reserved Instances (1 or 3 years)
- Stop non-production databases when not in use (up to 7 days)
- Delete automated backups for terminated instances
- Use Aurora for high-scale MySQL/PostgreSQL

**High Availability:**
- Multi-AZ deployment
- Automated backups with appropriate retention
- Cross-region read replicas for DR
- Monitor with CloudWatch alarms

**Security Best Practices:**
- Encryption at rest (KMS)
- Encryption in transit (SSL/TLS)
- IAM authentication where possible
- Deploy in private subnets
- Security groups (least privilege)
- Secrets Manager for credential management
- Enable deletion protection for production

## 2. Amazon Aurora

### Core Concepts

**What is Aurora:**
- AWS proprietary database engine
- MySQL and PostgreSQL compatible
- 5x performance of MySQL, 3x performance of PostgreSQL on RDS
- Cloud-native design
- Automated storage scaling
- Up to 15 read replicas
- Continuous backup to S3
- Replication across 3 AZs

**Architecture:**
- Storage layer separated from compute
- Storage auto-scales from 10 GB to 128 TB
- 6 copies of data across 3 AZs (2 copies per AZ)
- 4/6 copies needed for writes (quorum)
- 3/6 copies needed for reads
- Self-healing (peer-to-peer replication)
- Storage striped across 100s of volumes

### High Availability

**Aurora Cluster:**
- 1 primary instance (writes)
- Up to 15 Aurora replicas (reads)
- Automatic failover (typically < 30 seconds)
- Shared storage volume (all replicas access same data)

**Endpoints:**
- **Cluster Endpoint (Writer):** Points to primary instance, used for writes
- **Reader Endpoint:** Load balances across all read replicas
- **Custom Endpoint:** User-defined subset of instances
- **Instance Endpoint:** Direct connection to specific instance

**Failover Priority:**
- Tier 0 (highest) to Tier 15 (lowest)
- Within same tier: Largest instance promoted
- If no replicas: Aurora attempts to create new primary (could take longer)

**Aurora Replicas vs MySQL Read Replicas:**
- **Aurora Replicas:** Automated failover, same storage, < 10ms replication lag, up to 15
- **MySQL Read Replicas:** Manual promotion, separate storage, seconds replication lag, up to 5

### Global Database

**Features:**
- 1 primary region (read/write)
- Up to 5 secondary regions (read-only)
- Up to 16 read replicas per secondary region
- Replication lag: Typically < 1 second
- RTO < 1 minute for region failure

**Use Cases:**
- Disaster recovery with low RTO/RPO
- Global read scaling (low latency worldwide)
- Database migration across regions

**Failover:**
- Promote secondary region to primary
- Manual or scripted process
- Typical RTO: < 1 minute

### Aurora Serverless

**Aurora Serverless v2:**
- Auto-scales capacity based on demand
- Scales in fine-grained increments (0.5 ACU)
- Min/max capacity specified in ACUs (Aurora Capacity Units)
- Supports all Aurora features (replicas, Global Database, Multi-AZ)
- Instant scaling (fraction of a second)
- Can mix serverless and provisioned instances

**Aurora Serverless v1 (older):**
- Auto-scales between min/max ACU
- Scales to zero (pauses after inactivity)
- Pay per second when active
- Good for: Infrequent, intermittent, unpredictable workloads
- Limitations: Single AZ, no read replicas, no Global Database, slower scaling

**Use Cases:**
- Variable workloads
- Dev/test environments
- Infrequent applications
- Multi-tenant applications (scale per tenant)

### Aurora Multi-Master

**Features:**
- Multiple read/write instances
- All instances can accept writes
- Immediate failover (no promotion needed)
- Application handles conflict resolution

**Use Cases:**
- Continuous write availability
- Applications requiring multiple write endpoints
- Sharded applications

**Limitations:**
- Aurora MySQL only (not PostgreSQL)
- Complex application design

### Performance Features

**Aurora Storage:**
- Auto-scaling (10 GB to 128 TB)
- Automatic striping and replication
- High IOPS (100,000+ IOPS)
- Low-latency reads (10-20 ms)

**Parallel Query:**
- Pushes query processing to storage layer
- Parallelizes queries across 1000s of nodes
- Up to 2x faster for analytical queries
- Aurora MySQL only
- Good for: OLAP-style queries on OLTP database

**Aurora Machine Learning:**
- Integrate with SageMaker, Comprehend
- SQL queries can call ML models
- Use for: Fraud detection, recommendations, sentiment analysis

**Fast Clones:**
- Copy-on-write cloning
- Create clone in minutes
- Minimal storage cost initially
- Use for: Test/dev, analysis without affecting production

**Backtrack:**
- "Rewind" database to specific point in time
- No need to restore from backup
- In-place time travel (seconds to minutes)
- Aurora MySQL only
- Enable at creation, specify backtrack window (up to 72 hours)

### Backup and Recovery

**Automated Backups:**
- Continuous backup to S3
- No performance impact
- Retention: 1-35 days
- Point-in-time recovery

**Manual Snapshots:**
- User-initiated
- Retained until deleted
- Can share and copy across regions

**Restore:**
- Creates new cluster
- Can restore to point in time
- Can restore from S3 (MySQL/PostgreSQL dumps)

**Database Cloning:**
- Fast clone for testing
- Copy-on-write protocol

### Security

**Encryption:**
- At rest: KMS (enable at creation)
- In transit: SSL/TLS
- Snapshots, backups, replicas encrypted if source encrypted

**Network:**
- VPC isolation
- Security groups
- Private subnets recommended

**Authentication:**
- IAM database authentication (MySQL/PostgreSQL)
- Kerberos authentication (PostgreSQL)
- Native database users

**Secrets Manager:**
- Store and rotate credentials automatically

### Migration to Aurora

**Migration Methods:**

**Snapshot Restore:**
- Take RDS MySQL/PostgreSQL snapshot
- Restore to Aurora
- Downtime during migration

**Percona XtraBackup (MySQL):**
- Backup from on-premises MySQL
- Restore to Aurora
- Can use S3

**Database Migration Service (DMS):**
- Minimal downtime migration
- Continuous replication during migration
- From: RDS, on-premises, other clouds

**Aurora MySQL Cloning:**
- Clone from RDS MySQL
- Faster than snapshot

### Aurora Monitoring

**Performance Insights:**
- Advanced performance monitoring
- Visualize database load
- Identify bottlenecks

**Enhanced Monitoring:**
- OS-level metrics
- Real-time monitoring

**CloudWatch Metrics:**
- Standard database metrics
- Custom metrics available

**Database Activity Streams:**
- Near real-time stream of database activity
- Audit and compliance
- Sends to Kinesis Data Streams

### Exam-Focused Scenarios

**When to use Aurora:**
- Need high performance MySQL/PostgreSQL
- High availability and durability critical
- Auto-scaling storage
- Global database with low-latency reads
- Superior to RDS for: Large scale, high availability, performance
- Cost: More expensive than RDS but better performance/features

**Aurora vs RDS:**
- **Aurora:** 5x MySQL / 3x PostgreSQL performance, storage auto-scales to 128 TB, 15 replicas, faster failover, higher cost
- **RDS:** Standard performance, manual storage scaling, fewer replicas, lower cost, more engine options

**Serverless vs Provisioned:**
- **Serverless v2:** Variable workloads, instant scaling, pay for actual usage, all Aurora features
- **Serverless v1:** Infrequent use, can scale to zero, dev/test, limitations
- **Provisioned:** Predictable workloads, consistent performance, full control

**Global Database:**
- Multi-region disaster recovery (RTO < 1 min)
- Global read scaling
- Low-latency reads worldwide
- Database migration across regions

**Cost Optimization:**
- Aurora Serverless for variable workloads
- Reserved Instances for predictable workloads
- Delete unused snapshots
- Use appropriate instance sizes
- Consider RDS if Aurora features not needed

## 3. Amazon DynamoDB

### Core Concepts

**What is DynamoDB:**
- Fully managed NoSQL database
- Key-value and document database
- Serverless (no servers to provision)
- Scales to any size (single-digit millisecond latency)
- Multi-AZ replication (3 copies)
- Built-in security, backup, restore
- In-memory caching with DAX

**Data Model:**
- **Tables:** Collection of items (like rows)
- **Items:** Collection of attributes (like columns)
- **Attributes:** Key-value pairs (name and value)
- Schema-less (except primary key)
- Max item size: 400 KB

### Primary Keys

**Partition Key (Hash Key):**
- Simple primary key
- Must be unique
- DynamoDB uses hash function to determine partition
- Even distribution is important

**Composite Primary Key (Partition + Sort Key):**
- Partition key + sort key
- Partition key can be same across items
- Combination must be unique
- Items with same partition key stored together (sorted by sort key)
- Enables rich query patterns

**Choosing Keys:**
- High cardinality for partition key (many unique values)
- Avoid "hot" partitions (uneven access patterns)
- Sort key enables range queries, sorting

### Capacity Modes

**Provisioned Mode:**

*Read Capacity Units (RCU):*
- 1 RCU = 1 strongly consistent read/sec for item up to 4 KB
- 1 RCU = 2 eventually consistent reads/sec for item up to 4 KB
- Transactional reads: 2 RCUs

*Write Capacity Units (WCU):*
- 1 WCU = 1 write/sec for item up to 1 KB
- Transactional writes: 2 WCUs

*Features:*
- Specify RCU/WCU per table
- Auto-scaling available
- Burst capacity (300 seconds of unused capacity)
- Use for: Predictable workloads

**On-Demand Mode:**
- Pay per request
- No capacity planning
- Scales automatically
- More expensive per request than provisioned
- Use for: Unpredictable workloads, new applications, spiky traffic

### Indexes

**Local Secondary Index (LSI):**
- Alternate sort key, same partition key
- Must be created at table creation
- Up to 5 LSIs per table
- Shares RCU/WCU with base table
- Enables different sort order on same partition

**Global Secondary Index (GSI):**
- Alternate partition and/or sort key
- Can be created anytime
- Up to 20 GSIs per table
- Separate RCU/WCU from base table
- Eventually consistent reads only
- Acts like a separate table
- Can be added/deleted after table creation

**Index Projections:**
- KEYS_ONLY: Only key attributes
- INCLUDE: Specific non-key attributes
- ALL: All attributes (largest, most expensive)

### Querying and Scanning

**Query:**
- Find items based on primary key
- Optional filter on sort key (range operations)
- Returns sorted by sort key
- Eventually or strongly consistent
- Efficient (uses index)

**Scan:**
- Read entire table
- Apply optional filter (after reading)
- Inefficient (reads all data, consumes capacity)
- Parallel scan available (faster but expensive)
- Use sparingly

**Best Practices:**
- Use Query over Scan
- Create GSI for different access patterns
- Use filter expressions carefully (applied after read)
- Parallel scans for large tables (sparingly)

### Consistency Models

**Eventually Consistent Reads (default):**
- May not reflect recently completed write
- Uses half the RCUs
- Usually consistent within 1 second

**Strongly Consistent Reads:**
- Returns most up-to-date data
- Uses double the RCUs
- Not supported on GSIs
- Available on table and LSI

### DynamoDB Streams

**Features:**
- Ordered stream of item-level changes
- Retention: 24 hours
- Stream records: Insert, Update, Delete
- Process with Lambda, Kinesis Data Streams, KCL

**View Types:**
- KEYS_ONLY: Only key attributes
- NEW_IMAGE: Entire item after change
- OLD_IMAGE: Entire item before change
- NEW_AND_OLD_IMAGES: Both before and after

**Use Cases:**
- Real-time analytics
- Cross-region replication
- Trigger Lambda on data changes
- Materialized views
- Messaging and notifications

### DynamoDB Accelerator (DAX)

**Features:**
- Fully managed in-memory cache
- Microsecond latency (vs milliseconds)
- Compatible with DynamoDB API (minimal code changes)
- Cluster of cache nodes (Multi-AZ)
- 10x performance improvement

**Cache Types:**
- Item cache: GetItem, BatchGetItem
- Query cache: Query, Scan results
- TTL: 5 minutes default (configurable)

**Use Cases:**
- Read-heavy workloads
- Burst read capacity
- Repeated reads of same data
- Microsecond response time required

**DAX vs ElastiCache:**
- **DAX:** DynamoDB-specific, API-compatible, simpler
- **ElastiCache:** General purpose, more features, application manages caching logic

### Global Tables

**Features:**
- Multi-region, multi-active replication
- Fully replicated across regions
- Sub-second replication lag
- Active-active (read/write in any region)
- Disaster recovery and low latency globally

**Requirements:**
- DynamoDB Streams must be enabled
- Must use on-demand or auto-scaling provisioned capacity

**Conflict Resolution:**
- Last Writer Wins (based on timestamp)

### Backup and Recovery

**On-Demand Backups:**
- Full backups anytime
- No performance impact
- Retained until explicitly deleted
- Restore creates new table
- Can restore to same or different region

**Point-in-Time Recovery (PITR):**
- Continuous backups (last 35 days)
- Restore to any second in the window
- Enable per table
- No performance impact

**AWS Backup Integration:**
- Centralized backup management
- Cross-account, cross-region backup
- Backup policies and retention

### Time To Live (TTL)

**Features:**
- Automatically delete expired items
- No additional cost
- Specify TTL attribute (epoch timestamp)
- Items deleted within 48 hours of expiration
- Deleted items appear in DynamoDB Streams

**Use Cases:**
- Session data
- Temporary data
- Regulatory compliance (data retention)
- Reduce storage costs

### Transactions

**Features:**
- ACID transactions across multiple items/tables
- TransactWriteItems: Up to 100 items or 4 MB
- TransactGetItems: Up to 100 items or 4 MB
- All-or-nothing operations
- Uses 2x WCUs/RCUs

**Use Cases:**
- Financial transactions
- Order processing
- Multiplayer game state
- Managing relationships between items

### Security

**Encryption:**
- At rest: KMS encryption (default)
- In transit: TLS
- Client-side encryption available

**Access Control:**
- IAM policies for table/item-level permissions
- Fine-grained access control (conditions on attributes)
- VPC endpoints for private access
- Resource-based policies not supported

**Monitoring:**
- CloudWatch metrics
- CloudWatch Logs (API requests via CloudTrail)
- DynamoDB Streams for audit

### Advanced Features

**PartiQL:**
- SQL-compatible query language
- SELECT, INSERT, UPDATE, DELETE
- Simplifies queries for SQL users

**DynamoDB Export to S3:**
- Export table data to S3
- Point-in-time snapshot
- JSON or DynamoDB JSON format
- No impact on performance or capacity

**DynamoDB Import from S3:**
- Import data from S3 to new table
- CSV, DynamoDB JSON, ION formats
- Creates new table (not into existing)

**Change Data Capture:**
- Kinesis Data Streams for DynamoDB
- Alternative to DynamoDB Streams
- Longer retention (up to 1 year)
- More consumers (Kinesis Analytics, Firehose)

### Best Practices

**Data Modeling:**
- Denormalize data (NoSQL paradigm)
- Design for access patterns
- Use composite keys for relationships
- Avoid hot partitions
- One table design for complex applications

**Performance:**
- Use DAX for read-heavy workloads
- Batch operations (BatchGetItem, BatchWriteItem)
- Use GSI for different query patterns
- Avoid scans
- Monitor partition metrics

**Cost Optimization:**
- On-demand for unpredictable workloads
- Provisioned with auto-scaling for predictable
- TTL to delete old data
- Delete unused GSIs
- Use projections wisely (KEYS_ONLY vs ALL)

### Exam-Focused Scenarios

**When to use DynamoDB:**
- Serverless applications
- Massive scale (millions of requests/sec)
- Key-value access patterns
- Unpredictable workloads (on-demand)
- Mobile/web/gaming backends
- IoT data
- Session storage

**DynamoDB vs RDS/Aurora:**
- **DynamoDB:** NoSQL, horizontal scaling, serverless, key-value, millisecond latency, no schema
- **RDS/Aurora:** SQL, vertical scaling, relational, complex queries, transactions across tables

**Capacity Planning:**
- **Provisioned:** Predictable traffic, cost optimization, auto-scaling
- **On-Demand:** Unpredictable, new apps, pay-per-use, no planning

**Global Applications:**
- Global Tables for multi-region active-active
- DAX for read performance
- DynamoDB Streams for change tracking

**Data Access:**
- Query for efficient access (requires key)
- Scan for full table (expensive, avoid)
- GSI for alternate access patterns

## 4. Amazon ElastiCache

### Core Concepts

**What is ElastiCache:**
- Fully managed in-memory data store
- Sub-millisecond latency
- Two engines: Redis and Memcached
- Reduce database load (caching)
- Session storage
- Real-time analytics

### Redis vs Memcached

**Redis:**
- Advanced data structures (strings, lists, sets, sorted sets, hashes, bitmaps, hyperloglogs)
- Persistence (RDB snapshots, AOF logs)
- Pub/Sub messaging
- Replication (primary/replica)
- Multi-AZ with automatic failover
- Backup and restore
- Sorted sets for leaderboards
- Geospatial data
- Lua scripting
- Transactions
- Cluster mode for partitioning

**Memcached:**
- Simple key-value store
- Multi-threaded architecture
- No persistence
- No replication
- Auto Discovery for node changes
- Multi-node partitioning (sharding)
- Simpler, faster for simple caching

**Choose Redis if you need:**
- Complex data types
- Persistence/durability
- Replication/high availability
- Pub/Sub
- Sorted sets, geospatial
- Backup/restore
- Transactions

**Choose Memcached if you need:**
- Simple caching
- Multi-threaded performance
- Horizontal scaling (sharding)
- Minimal complexity
- No persistence needed

### Redis Architecture

**Cluster Mode Disabled (Classic Redis):**
- Single shard (node group)
- 1 primary + up to 5 read replicas
- Asynchronous replication
- Read scaling with replicas
- Automatic failover with Multi-AZ
- Max 5 replicas per shard

**Cluster Mode Enabled:**
- Multiple shards (1-500)
- Data partitioned across shards
- Each shard: 1 primary + 0-5 replicas
- Horizontal scaling (add/remove shards)
- Up to 500 shards (90 nodes per cluster max for r6g.large and smaller, 500 nodes for larger types)
- Read and write scaling

**Replication:**
- Asynchronous replication primary → replica
- Multi-AZ for automatic failover
- Failover typically < 30 seconds
- Promotes replica to primary

### Redis High Availability

**Multi-AZ with Auto-Failover:**
- Primary and replicas in different AZs
- Automatic failover on primary failure
- Promotes replica to primary
- Updates DNS endpoint

**Read Replicas:**
- Scale read traffic
- Asynchronous replication
- Can promote manually
- Cross-region replicas (Global Datastore)

### Redis Global Datastore

**Features:**
- Cross-region replication
- 1 primary region (read/write)
- Up to 2 secondary regions (read-only)
- Sub-second replication lag
- Disaster recovery (promote secondary)
- Low-latency reads globally

**Use Cases:**
- Multi-region applications
- Disaster recovery with low RTO
- Read scaling globally

### Redis Persistence

**RDB (Redis Database Backup):**
- Point-in-time snapshots
- Scheduled or manual
- Creates .rdb file
- Minimal impact on performance
- Slower recovery than AOF

**AOF (Append Only File):**
- Logs every write operation
- Replay log to recover
- More durable than RDB
- Larger file size
- Can slow down restarts

**No Persistence:**
- Pure caching use case
- Fastest performance
- Data lost on restart

### Memcached Architecture

**Features:**
- Distributed hash table
- Partitions data across multiple nodes
- No replication (data lost if node fails)
- Multi-threaded
- Auto Discovery (client automatically finds nodes)

**Scaling:**
- Add/remove nodes
- Data automatically redistributed
- Consistent hashing minimizes redistribution

### Caching Strategies

**Lazy Loading (Cache-Aside):**
- Application checks cache first
- If miss, query database and populate cache
- Pros: Only requested data cached, node failures not fatal
- Cons: Cache miss penalty, stale data possible

**Write-Through:**
- Write to cache and database simultaneously
- Pros: Data never stale, write penalty only once
- Cons: Missing data until written, cache churn (data written but never read)

**TTL (Time To Live):**
- Set expiration on cached data
- Combine with lazy loading or write-through
- Prevents infinite staleness

**Best Practice:**
- Lazy loading + TTL for most use cases
- Write-through for critical data consistency

### Security

**Encryption:**
- At rest: KMS encryption (Redis only)
- In transit: TLS (Redis 6+, Memcached)
- Enable at creation

**Access Control:**
- VPC for network isolation
- Security groups
- Redis AUTH: Password protection
- Redis RBAC: Role-based access control (Redis 6+)
- IAM authentication (Redis only)

**Compliance:**
- PCI DSS, HIPAA eligible
- Redis 6+ required for compliance

### Scaling

**Redis - Vertical Scaling:**
- Change node type
- Cluster mode disabled: Requires failover
- Cluster mode enabled: Online scaling (no downtime)

**Redis - Horizontal Scaling:**
- Cluster mode disabled: Add read replicas (read scaling only)
- Cluster mode enabled: Add/remove shards (read and write scaling)

**Memcached - Horizontal Scaling:**
- Add/remove nodes
- Auto Discovery updates clients

### Monitoring

**CloudWatch Metrics:**
- CPUUtilization
- SwapUsage
- Evictions (cache full, removing old data)
- CurrConnections
- CacheHits, CacheMisses
- ReplicationLag (Redis)

**Alarms:**
- High CPU
- High evictions (cache too small)
- Low cache hit rate

### Backup and Restore (Redis Only)

**Automatic Backups:**
- Daily snapshots
- Retention: 0-35 days
- No performance impact (from replica)

**Manual Snapshots:**
- On-demand backups
- Retained until deleted

**Restore:**
- Create new cluster from snapshot
- Can change node type, parameter group

**Final Snapshot:**
- Create snapshot when deleting cluster

### Parameter
Groups

**Redis:**
- maxmemory-policy: Eviction policy (allkeys-lru recommended)
- timeout: Client connection timeout
- cluster-enabled: Enable cluster mode

**Memcached:**
- chunk_size, max_item_size
- binding_protocol

### Exam-Focused Scenarios

**When to use ElastiCache:**
- Reduce database load with caching
- Session storage (distributed applications)
- Leaderboards (Redis sorted sets)
- Pub/Sub messaging (Redis)
- Real-time analytics
- Rate limiting

**Redis vs Memcached:**
- **Redis:** Complex needs, persistence, replication, HA, Pub/Sub, advanced data types
- **Memcached:** Simple caching, multi-threaded, horizontal scaling, no HA needed

**Redis Cluster Mode:**
- **Disabled:** Simple, < 500 GB data, read scaling only
- **Enabled:** Large data, horizontal scaling, read and write scaling

**Caching Strategies:**
- **Lazy Loading:** Most common, cache misses acceptable
- **Write-Through:** Data consistency critical
- **TTL:** Always use to prevent stale data

**High Availability:**
- Multi-AZ with auto-failover (Redis)
- Replicas for read scaling and failover
- Global Datastore for cross-region DR

**Security:**
- Redis in VPC with security groups
- Redis AUTH or IAM authentication
- Encryption at rest and in transit (Redis 6+)
- Memcached: VPC, security groups only

## 5. Amazon Neptune

### Core Concepts

**What is Neptune:**
- Fully managed graph database
- Query and traverse highly connected datasets
- Supports two graph models: Property Graph and RDF
- Query languages: Gremlin (Apache TinkerPop), SPARQL (W3C standard)
- ACID transactions
- Read replicas
- Point-in-time recovery
- Multi-AZ high availability

**Graph Database Use Cases:**
- Social networks
- Recommendation engines
- Fraud detection
- Knowledge graphs
- Network/IT operations
- Life sciences (drug discovery, protein interactions)
- Identity and access management

### Architecture

**Storage:**
- Shared, distributed, fault-tolerant storage
- 6 copies across 3 AZs
- Storage auto-scales from 10 GB to 128 TB
- Continuous backup to S3

**Compute:**
- Primary instance (read/write)
- Up to 15 read replicas
- Replicas can be in different AZs
- Automatic failover (< 30 seconds)

**Endpoints:**
- Cluster endpoint (primary)
- Reader endpoint (load balanced across replicas)
- Instance endpoints (specific instance)

### Query Languages

**Gremlin (Property Graph):**
- Apache TinkerPop graph traversal language
- Query vertices, edges, properties
- Pattern matching, path traversal
- Optimized for transactional graph queries

**SPARQL (RDF):**
- W3C standard query language
- Query RDF triples
- Knowledge graphs, semantic web
- Inferencing with RDFS and OWL

**openCypher:**
- Additional query language for property graphs
- SQL-like syntax for graphs
- Easier for SQL users

### Graph Models

**Property Graph:**
- Vertices (nodes) with properties
- Edges (relationships) with properties and direction
- Labels for categorizing vertices and edges

**RDF (Resource Description Framework):**
- Triples: subject-predicate-object
- URIs for identifying resources
- Support for ontologies and reasoning

### High Availability

**Multi-AZ:**
- Replicas across AZs
- Automatic failover
- Shared storage across AZs

**Read Replicas:**
- Up to 15 replicas
- Sub-10ms replication lag
- Read scaling
- Priority-based failover

### Backup and Recovery

**Automated Backups:**
- Continuous incremental backups to S3
- Retention: 1-35 days
- Point-in-time recovery

**Manual Snapshots:**
- On-demand snapshots
- Retained until deleted

**Restore:**
- Creates new cluster
- Can restore to any point within retention

### Security

**Encryption:**
- At rest: KMS
- In transit: TLS
- Must enable at creation

**Network:**
- VPC deployment only
- Security groups
- VPC endpoints available

**Access Control:**
- IAM authentication
- IAM policies for API access
- No fine-grained access within graph

### Monitoring

**CloudWatch Metrics:**
- Gremlin/SPARQLRequestsPerSec
- CPUUtilization, Memory
- GremlinRequestsPerSec
- MainRequestQueuePendingRequests

**Neptune Streams:**
- Change log for graph data
- Consume with Lambda, Kinesis
- Build downstream applications

**Audit Logging:**
- CloudWatch Logs
- Log queries and operations

### Scaling

**Vertical Scaling:**
- Change instance type
- Some downtime for primary
- Read replicas updated without downtime

**Horizontal Scaling (Reads):**
- Add read replicas
- Up to 15 replicas

**Storage:**
- Automatic, no configuration
- Scales transparently

### Loading Data

**Neptune Bulk Loader:**
- Load data from S3
- Gremlin or RDF format
- Parallel loading
- Fast initial data load

**API-Based Loading:**
- Use Gremlin or SPARQL
- For smaller, ongoing updates
- Transactional consistency

### Exam-Focused Scenarios

**When to use Neptune:**
- Highly connected data
- Relationship queries (friends, recommendations)
- Fraud detection (pattern matching)
- Knowledge graphs
- Network analysis
- NOT suitable for: Simple relational data (use RDS), key-value (use DynamoDB)

**Neptune vs Other Databases:**
- **Neptune:** Graph data, relationships, traversals
- **RDS/Aurora:** Relational, SQL, joins (less efficient for deep relationships)
- **DynamoDB:** Key-value, NoSQL, simple access patterns

**High Availability:**
- Multi-AZ deployment
- Read replicas for redundancy
- Automated backups

**Security:**
- VPC deployment
- IAM authentication
- Encryption at rest and in transit

## 6. Amazon DocumentDB

### Core Concepts

**What is DocumentDB:**
- Managed document database
- MongoDB-compatible
- JSON document storage
- Scales compute and storage independently
- 6 copies across 3 AZs
- Continuous backup
- MongoDB 3.6, 4.0, 5.0 compatible

**Use Cases:**
- Content management
- Catalogs
- User profiles
- Mobile applications
- Gaming applications
- Migration from MongoDB

### Architecture

**Storage:**
- Shared, distributed storage
- Auto-scales from 10 GB to 128 TB
- 6 copies across 3 AZs

**Compute:**
- Primary instance (read/write)
- Up to 15 read replicas
- Automatic failover

**Endpoints:**
- Cluster endpoint (primary writes)
- Reader endpoint (read load balancing)
- Instance endpoints

### MongoDB Compatibility

**Compatible Features:**
- Document data model (JSON)
- Query language
- Indexes (single field, compound, TTL)
- Aggregation framework
- Change streams
- Transactions (4.0+)

**Limitations:**
- Some MongoDB features not supported
- Use AWS Database Migration Service (DMS) for migration
- Test compatibility before migration

### High Availability

**Multi-AZ:**
- Replicas in multiple AZs
- Automatic failover (30-60 seconds)
- Storage replicated 6 times

**Read Replicas:**
- Up to 15 replicas
- Scale read traffic
- Priority-based failover

### Backup and Recovery

**Automated Backups:**
- Continuous, incremental
- Retention: 1-35 days
- Point-in-time recovery

**Manual Snapshots:**
- User-initiated
- Retained until deleted

### Scaling

**Vertical Scaling:**
- Change instance size
- Some downtime for primary

**Horizontal Scaling (Reads):**
- Add read replicas
- Up to 15 replicas

**Storage:**
- Automatic scaling
- Transparent to application

### Security

**Encryption:**
- At rest: KMS (at creation)
- In transit: TLS

**Network:**
- VPC deployment
- Security groups

**Access Control:**
- Username/password authentication
- Role-based access control (MongoDB RBAC)
- IAM authentication (limited)

### Monitoring

**CloudWatch Metrics:**
- CPUUtilization
- DatabaseConnections
- ReadLatency, WriteLatency
- VolumeBytesUsed

**Profiler:**
- Log slow queries
- Analyze query performance

### Exam-Focused Scenarios

**When to use DocumentDB:**
- Need MongoDB-compatible database
- Managed service (reduce ops overhead)
- Document/JSON data model
- Migrate from MongoDB
- Flexible schema

**DocumentDB vs DynamoDB:**
- **DocumentDB:** MongoDB compatible, complex queries, ad-hoc queries, flexible schemas
- **DynamoDB:** AWS-native, key-value, predictable access patterns, serverless

**DocumentDB vs MongoDB on EC2:**
- **DocumentDB:** Fully managed, automatic scaling, backups, HA
- **MongoDB on EC2:** Full MongoDB features, more control, operational overhead

## 7. Amazon Redshift

### Core Concepts

**What is Redshift:**
- Fully managed data warehouse
- Petabyte-scale analytics
- Columnar storage
- Massively Parallel Processing (MPP)
- SQL-based (PostgreSQL compatible)
- Integrates with BI tools (Tableau, QuickSight, etc.)
- Designed for OLAP (Online Analytical Processing)

**Use Cases:**
- Business intelligence
- Analytics and reporting
- Data warehousing
- Log analysis
- Large-scale data aggregation

### Architecture

**Cluster:**
- Leader node: Query planning, aggregation
- Compute nodes: Execute queries, store data
- Node types: Dense Compute (DC2), Dense Storage (DS2), RA3 (managed storage)

**Node Types:**

**RA3 (Recommended):**
- Managed storage on S3
- Scales compute and storage independently
- Cache frequently accessed data on local SSD
- Pay for compute and S3 storage separately
- ra3.xlplus, ra3.4xlarge, ra3.16xlarge

**DC2 (Dense Compute):**
- Fast local SSD
- Compute-intensive workloads
- Fixed storage per node
- dc2.large, dc2.8xlarge

**DS2 (Dense Storage - previous gen):**
- Large HDD storage
- Storage-heavy workloads
- Lower cost per GB
- ds2.xlarge, ds2.8xlarge

### Storage

**Columnar Storage:**
- Stores data by column (not row)
- Better compression
- Faster query on specific columns
- Ideal for analytics (aggregations, filters)

**Data Distribution Styles:**

**AUTO:** Redshift chooses distribution (recommended)

**EVEN:** Round-robin distribution across nodes (default)

**KEY:** Distributes based on column value (co-locate related data)

**ALL:** Full copy on every node (small tables)

**Sort Keys:**
- Determines data storage order
- Single column or compound
- Improves query performance (range filters, joins)

**Compression:**
- Automatic or manual compression encoding
- Reduces storage and improves performance
- COPY command analyzes and applies compression

### Loading Data

**COPY Command:**
- Fastest way to load data
- From S3, DynamoDB, EMR, remote host (SSH)
- Parallel loading
- Supports JSON, CSV, Avro, Parquet, ORC
- Encryption and compression support

**AWS Data Pipeline:**
- Orchestrate data movement
- Schedule and automate

**AWS Glue:**
- ETL jobs to transform and load

**Redshift Spectrum:**
- Query data in S3 without loading
- Extend queries beyond cluster
- Join Redshift tables with S3 data

**Kinesis Data Firehose:**
- Stream data to Redshift
- Near real-time

### Querying

**SQL Interface:**
- PostgreSQL 8.x compatible
- Standard SQL queries
- JDBC/ODBC drivers

**Query Performance:**
- MPP architecture (parallel execution)
- Columnar storage
- Result caching
- Materialized views
- Query optimization (EXPLAIN, ANALYZE)

**Redshift Spectrum:**
- Query exabytes of data in S3
- No loading required
- Serverless scaling for Spectrum queries
- Pay per TB scanned
- Supports: Parquet, ORC, JSON, CSV, Avro

### Concurrency and Workload Management

**Workload Management (WLM):**
- Queues for different workloads
- Auto WLM (recommended): Automatic resource allocation
- Manual WLM: Define query queues, memory, concurrency

**Concurrency Scaling:**
- Automatically adds capacity for read queries
- Handles bursts of user activity
- Free for 1 hour/day
- Additional usage billed per second

**Short Query Acceleration (SQA):**
- Prioritizes short-running queries
- Separate queue for short queries
- Improves dashboard and interactive query performance

### Backup and Restore

**Automated Snapshots:**
- Every 8 hours or 5 GB of changes
- Retention: 1-35 days (default 1 day)
- Stored in S3 (AWS managed)

**Manual Snapshots:**
- User-initiated
- Retained until deleted

**Cross-Region Snapshots:**
- Automated copy to another region
- Disaster recovery
- Configure source and destination regions

**Restore:**
- Creates new cluster
- Can change node type
- Snapshots are incremental

### High Availability and Disaster Recovery

**Multi-AZ:**
- NOT available (single-AZ cluster)
- RA3 nodes store data in S3 (replicated across AZs)

**Snapshots for DR:**
- Cross-region snapshot copy
- Automated snapshots with retention
- Restore in different region

**Node Failure:**
- Redshift automatically detects and replaces
- Data replicated on multiple nodes (RA3)
- Cluster unavailable during replacement (single node clusters)

### Redshift Serverless

**Features:**
- Automatically provisions and scales capacity
- Pay for actual usage (RPU-hours: Redshift Processing Units)
- No cluster management
- Starts in seconds
- Use for: Infrequent analytics, dev/test, unpredictable workloads

**vs Provisioned:**
- Serverless: Variable workloads, no capacity planning, pay per use
- Provisioned: Predictable workloads, Reserved Instances, more control

### Security

**Encryption:**
- At rest: KMS or HSM
- In transit: SSL
- Enable at creation (cannot change)

**Network:**
- VPC deployment
- Security groups
- Enhanced VPC routing (force through VPC)

**Access Control:**
- IAM for API access
- Database users and groups
- GRANT/REVOKE permissions

**Auditing:**
- CloudWatch Logs
- CloudTrail for API calls
- Audit logging to S3

### Monitoring

**CloudWatch Metrics:**
- CPUUtilization
- DatabaseConnections
- HealthStatus
- PercentageDiskSpaceUsed

**Query Monitoring Rules (QMR):**
- Set thresholds for query execution
- Automatic actions: Log, hop, abort

**Performance Insights:**
- Monitor query performance
- Identify bottlenecks

### Advanced Features

**Materialized Views:**
- Precomputed query results
- Faster queries
- Auto-refresh or manual refresh

**Federated Query:**
- Query data in RDS/Aurora directly
- No data movement
- Join Redshift and RDS tables

**Data Sharing:**
- Share live data between Redshift clusters
- No data copies
- Cross-account and cross-region
- Consumers query shared data directly

**Machine Learning:**
- Use SageMaker models in SQL queries
- CREATE MODEL statement
- Predictions in SQL queries

### Maintenance

**Maintenance Windows:**
- Weekly window for patches, upgrades
- Can defer (limited time)

**Version Upgrades:**
- Major version: Manual
- Minor version: Can be automatic

**Resize:**
- Classic resize: Cluster read-only during resize
- Elastic resize: Faster, minutes (recommended)
- Snapshot, restore, resize: Manual process

### Cost Optimization

**Reserved Instances:**
- 1 or 3 years
- Up to 75% discount
- Pay all upfront, partial upfront, or no upfront

**Concurrency Scaling:**
- Free for 1 hour/day

**Pause/Resume:**
- Pause cluster when not in use (Redshift Serverless auto-pauses)
- No compute charges when paused

**RA3 Nodes:**
- Pay for compute and storage separately
- Delete unused data
- Use Redshift Spectrum for cold data

### Exam-Focused Scenarios

**When to use Redshift:**
- Data warehousing
- Complex analytics and aggregations
- BI and reporting
- Historical data analysis
- OLAP workloads
- Petabyte-scale data
- NOT for: OLTP (use RDS/Aurora), key-value (use DynamoDB), real-time (use Kinesis)

**Redshift vs RDS/Aurora:**
- **Redshift:** Analytics, aggregations, OLAP, columnar, petabyte-scale
- **RDS/Aurora:** Transactional, OLTP, row-based, normalized

**Redshift vs Athena:**
- **Redshift:** Frequent, complex queries, predictable workloads, provisioned
- **Athena:** Ad-hoc queries, infrequent, query S3 directly, serverless, pay per query

**Loading Data:**
- **COPY from S3:** Fastest, most efficient
- **Spectrum:** Query without loading
- **Firehose:** Near real-time streaming

**Performance Optimization:**
- Distribution and sort keys
- Compression encoding
- Vacuum and analyze tables
- Materialized views
- WLM for query prioritization

**Cost Optimization:**
- Reserved Instances for predictable workloads
- Redshift Serverless for variable workloads
- RA3 nodes for independent scaling
- Spectrum for cold data in S3

## 8. Amazon QLDB (Quantum Ledger Database)

### Core Concepts

**What is QLDB:**
- Fully managed ledger database
- Immutable, transparent, cryptographically verifiable transaction log
- Central authority (owned by one party, not decentralized)
- Document-oriented data model (PartiQL - SQL-compatible)
- Serverless (auto-scales)
- 2-3x faster than blockchain frameworks

**Use Cases:**
- Financial transactions
- Supply chain tracking
- Systems of record
- Regulatory compliance
- Claims history
- HR/payroll audit
- NOT for: Decentralized trust (use Amazon Managed Blockchain)

### Architecture

**Journal:**
- Immutable, append-only ledger
- Sequence of blocks
- Each block cryptographically chained to previous
- Cannot be deleted or modified

**Tables:**
- Document tables (JSON)
- Query with PartiQL (SQL on JSON)
- Indexes for query performance

**History:**
- Complete history of all changes
- Query historical values
- Document revisions maintained forever

### Cryptographic Verification

**Features:**
- SHA-256 hash chaining
- Journal blocks signed with AWS-managed key
- Verification API to prove data integrity
- Digest: Cryptographic snapshot of journal at point in time
- Can verify document revisions against digest

**Verification:**
- Verify that document hasn't been altered
- Prove data integrity to auditors
- Verify using digest and verification proof

### Querying

**PartiQL:**
- SQL-compatible query language for JSON
- SELECT, INSERT, UPDATE, DELETE
- Supports document structures

**Indexes:**
- Improve query performance
- Automatically created on document ID
- Create additional indexes on any field

**History Queries:**
- Query document history
- See all revisions over time
- Audit trail built-in

### Ledger Streams

**Features:**
- Stream journal data changes to Kinesis Data Streams
- Near real-time delivery
- Can aggregate, transform, analyze ledger data
- Send to S3, Redshift, Elasticsearch

**Use Cases:**
- Real-time analytics
- Event-driven architectures
- Replicate to data warehouse
- Search and indexing

### Backup and Export

**Export to S3:**
- Export journal blocks to S3
- JSON or ION format
- Point-in-time export
- Verify integrity of exported data

**No Traditional Backups:**
- Journal is immutable and durable
- No need for backups (data can't be lost/changed)

### Security

**Encryption:**
- At rest: AWS-managed keys (KMS)
- In transit: TLS

**Access Control:**
- IAM for API access
- Fine-grained permissions on ledger operations

**Deletion Protection:**
- Optional flag to prevent ledger deletion
- Extra protection for critical ledgers

### Performance and Scaling

**Serverless:**
- Automatic scaling
- No capacity planning

**Performance:**
- Document reads: Single-digit milliseconds
- 2-3x faster than common blockchain frameworks
- No mining/consensus overhead (centralized)

### Exam-Focused Scenarios

**When to use QLDB:**
- Need complete, immutable history
- Centralized trusted authority
- Regulatory compliance (audit trail)
- Verify data integrity cryptographically
- Financial systems, supply chain, HR
- NOT for: Decentralized systems (use Managed Blockchain)

**QLDB vs Managed Blockchain:**
- **QLDB:** Centralized, owned by one party, faster, immutable journal, cryptographic verification
- **Managed Blockchain:** Decentralized, multiple parties, consensus mechanisms, Hyperledger Fabric or Ethereum

**QLDB vs RDS:**
- **QLDB:** Immutable history, cryptographic verification, compliance/audit
- **RDS:** Mutable data, transactions, updates/deletes

**QLDB vs DynamoDB:**
- **QLDB:** Immutable, audit history, SQL-like queries, compliance
- **DynamoDB:** Mutable, high-scale key-value, no built-in history

---

## Database Services Comparison Summary

| Service | Type | Use Case | Scaling | Transactions |
|---------|------|----------|---------|--------------|
| **RDS** | Relational (managed) | Standard RDBMS, OLTP | Vertical, read replicas | Yes (ACID) |
| **Aurora** | Relational (cloud-native) | High-performance MySQL/PostgreSQL | Storage auto-scales, 15 replicas | Yes (ACID) |
| **DynamoDB** | NoSQL (key-value/document) | Serverless, massive scale, key-value | Horizontal (automatic) | Yes (ACID) |
| **ElastiCache** | In-memory | Caching, session storage, real-time | Horizontal (Redis/Memcached) | Yes (Redis) |
| **Neptune** | Graph | Relationships, social networks, fraud | Vertical, read replicas | Yes (ACID) |
| **DocumentDB** | NoSQL (document) | MongoDB-compatible, JSON | Vertical, read replicas | Yes (ACID) |
| **Redshift** | Data warehouse | Analytics, BI, OLAP | Horizontal (nodes) | Yes (for loads) |
| **QLDB** | Ledger | Immutable history, audit, compliance | Serverless (automatic) | Yes (ACID) |

## Key Exam Tips for Database Services

1. **Match database to use case:**
   - OLTP → RDS/Aurora
   - OLAP → Redshift
   - NoSQL key-value → DynamoDB
   - Caching → ElastiCache
   - Graph → Neptune
   - Document → DocumentDB
   - Ledger → QLDB

2. **Understand scaling patterns:**
   - Vertical: RDS, Aurora, Neptune, DocumentDB
   - Horizontal: DynamoDB, ElastiCache, Redshift
   - Serverless: DynamoDB, Aurora Serverless, QLDB, Redshift Serverless

3. **High availability strategies:**
   - Multi-AZ: RDS, Aurora, ElastiCache Redis, Redshift (via snapshots)
   - Read replicas: RDS, Aurora, Neptune, DocumentDB
   - Global tables: DynamoDB, Aurora Global Database

4. **Security fundamentals:**
   - Encryption at rest and in transit
   - VPC deployment
   - IAM authentication where available
   - Security groups and NACLs

5. **Cost optimization:**
   - Reserved Instances: RDS, Aurora, Redshift, ElastiCache
   - Serverless options for variable workloads
   - Right-sizing instances
   - Automated snapshots retention management

6. **Backup and recovery:**
   - Automated backups with retention
   - Manual snapshots
   - Point-in-time recovery
   - Cross-region backup copies

7. **Performance optimization:**
   - Appropriate instance types
   - Read replicas for read scaling
   - Caching layers (ElastiCache, DAX)
   - Query optimization
   - Indexes and data distribution

This comprehensive guide covers all AWS database services from the SAP-C02 exam perspective, focusing on architectural patterns, trade-offs, and best practices for each service.