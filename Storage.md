# Complete AWS Storage Services Knowledge for SAP-C02 Exam

## 1. Amazon S3 (Simple Storage Service)

### Core Concepts

**Object Storage Fundamentals:**
- Objects stored in buckets (globally unique names)
- Objects identified by key (full path)
- Object size: 0 bytes to 5 TB
- Metadata: system and user-defined
- Version ID (if versioning enabled)
- Flat structure (no real directories, simulated with key prefixes)

**Bucket Properties:**
- Region-specific (data doesn't leave region unless replicated)
- Bucket names: 3-63 characters, lowercase, no uppercase, no underscores
- Can host static websites
- 100 buckets per account (soft limit, can be increased to 1000)

**S3 Storage Classes:**

**S3 Standard:**
- 99.99% availability, 11 9's durability
- Low latency, high throughput
- Stored across ≥3 AZs
- Use for: Frequently accessed data, dynamic websites, content distribution, analytics

**S3 Intelligent-Tiering:**
- Automatic cost optimization by moving objects between tiers
- No retrieval fees
- Small monthly monitoring fee per object
- Tiers: Frequent Access, Infrequent Access, Archive Instant Access, Archive Access (optional), Deep Archive Access (optional)
- Use for: Unknown or changing access patterns

**S3 Standard-IA (Infrequent Access):**
- 99.9% availability
- Lower storage cost than Standard
- Retrieval fee per GB
- Minimum storage duration: 30 days
- Minimum object size: 128 KB
- Use for: Backups, disaster recovery, long-term storage accessed occasionally

**S3 One Zone-IA:**
- 99.5% availability (single AZ)
- 20% lower cost than Standard-IA
- Retrieval fee
- Use for: Secondary backup copies, recreatable data

**S3 Glacier Instant Retrieval:**
- Same instant access as Standard
- 68% lower cost than Standard-IA
- Minimum storage duration: 90 days
- Retrieval fee
- Use for: Archive data needing immediate access (once per quarter)

**S3 Glacier Flexible Retrieval (formerly Glacier):**
- Retrieval times: Expedited (1-5 min), Standard (3-5 hours), Bulk (5-12 hours)
- Minimum storage duration: 90 days
- Use for: Archive with occasional retrieval needs

**S3 Glacier Deep Archive:**
- Lowest cost storage
- Retrieval times: Standard (12 hours), Bulk (48 hours)
- Minimum storage duration: 180 days
- Use for: Long-term archive, regulatory compliance (7-10 years)

**S3 Outposts:**
- Object storage on AWS Outposts
- S3 APIs locally
- Single storage class

### Lifecycle Management

**Transition Actions:**
- Move objects between storage classes automatically
- Can cascade: Standard → IA → Glacier → Deep Archive
- Minimum days before transition (e.g., 30 days for IA)
- Can filter by prefix or tags

**Expiration Actions:**
- Delete objects after specified time
- Delete incomplete multipart uploads
- Delete old versions (if versioning enabled)
- Delete expired delete markers

**Rules:**
- Scope: entire bucket, prefix, or tags
- Multiple rules allowed
- Rules evaluated for all objects daily

### Versioning

**Version States:**
- Unversioned (default)
- Enabled (cannot be disabled, only suspended)
- Suspended

**Features:**
- Protects against unintended deletes
- Each version is billable
- Delete marker added on delete (soft delete)
- Can permanently delete specific version
- MFA Delete: Require MFA for version deletion or suspending versioning

**Use Cases:**
- Compliance and auditing
- Recover from application failures
- Accidental deletion protection

### Replication

**Cross-Region Replication (CRR):**
- Replicate to different region
- Compliance, lower latency, disaster recovery
- Can change storage class during replication
- Can replicate to different AWS account

**Same-Region Replication (SRR):**
- Replicate within same region
- Log aggregation, live replication between prod/test
- Compliance requirements for data copies

**Requirements:**
- Versioning must be enabled on source and destination
- IAM permissions for S3 to replicate
- Asynchronous replication

**Features:**
- Can replicate existing objects (Batch Replication)
- Can replicate delete markers (optional)
- Can replicate encrypted objects (SSE-S3, SSE-KMS, SSE-C)
- No chaining (A→B→C doesn't work)
- Replication Time Control (RTC): 99.99% replicated in 15 minutes (SLA)

### Security

**Encryption at Rest:**

**Server-Side Encryption (SSE):**

*SSE-S3 (AES-256):*
- AWS manages keys
- Header: "x-amz-server-side-encryption": "AES256"
- Enabled by default for new buckets (as of January 2023)

*SSE-KMS:*
- AWS KMS manages keys
- User control over key rotation
- Audit trail in CloudTrail
- Header: "x-amz-server-side-encryption": "aws:kms"
- Can specify KMS key ARN
- Quota limits: KMS API calls (5500, 10000, or 30000 req/s per region)

*SSE-C (Customer-Provided):*
- Customer manages keys
- AWS encrypts/decrypts but doesn't store key
- HTTPS required
- Key must be provided in every request

*Client-Side Encryption:*
- Encrypt before uploading
- Decrypt after downloading
- Client manages encryption process

**Encryption in Transit:**
- SSL/TLS (HTTPS)
- Force with bucket policy: aws:SecureTransport condition

**Access Control:**

**IAM Policies:**
- User-based permissions
- Control which API calls user can make

**Bucket Policies:**
- JSON-based
- Resource-based policy attached to bucket
- Grant cross-account access
- Public access
- Conditions: IP address, VPC endpoint, MFA, time of day

**Access Control Lists (ACLs):**
- Legacy (AWS recommends bucket policies)
- Object-level or bucket-level
- Grant basic read/write permissions

**S3 Block Public Access:**
- Account-level or bucket-level settings
- Four settings: block public ACLs, ignore public ACLs, block public bucket policies, restrict public buckets
- Overrides other policies
- Enabled by default for new buckets

**Pre-Signed URLs:**
- Temporary access to objects
- Inherits permissions of user who created URL
- Expiration time configurable
- Use for: time-limited downloads, temporary upload access

**S3 Access Points:**
- Named network endpoints with dedicated access policies
- Simplify managing access to shared datasets
- Each access point has its own DNS name and policy
- Can restrict to VPC (VPC endpoint required)
- S3 Multi-Region Access Points: Route requests to nearest S3 bucket

### Performance Optimization

**Request Rate Performance:**
- 3,500 PUT/COPY/POST/DELETE requests per second per prefix
- 5,500 GET/HEAD requests per second per prefix
- No limit on number of prefixes
- Spread reads across prefixes for higher throughput

**Transfer Acceleration:**
- Uses CloudFront edge locations
- Upload to edge, then transfer to S3 over AWS network
- Compatible with multipart upload
- Test speed at: s3-accelerate-speedtest.s3-accelerate.amazonaws.com
- Additional cost

**Multipart Upload:**
- Recommended for files > 100 MB
- Required for files > 5 GB
- Parallel uploads
- Pause and resume
- Upload parts independently
- Can list incomplete multipart uploads and abort

**S3 Byte-Range Fetches:**
- Parallel downloads by requesting specific byte ranges
- Resilience: retry smaller ranges on failure
- Can retrieve partial objects (e.g., first 50 bytes for metadata)

**S3 Select & Glacier Select:**
- Server-side filtering using SQL
- Retrieve less data (up to 400% faster, 80% cheaper)
- Filter rows and columns
- Works with CSV, JSON, Parquet

### Advanced Features

**S3 Event Notifications:**
- React to events: ObjectCreated, ObjectRemoved, ObjectRestore, Replication
- Destinations: SNS, SQS, Lambda, EventBridge
- Millisecond delivery
- Filter by prefix/suffix
- EventBridge: all events, more destinations, advanced filtering, multiple destinations

**S3 Object Lock:**
- Write-Once-Read-Many (WORM) model
- Two modes:
  - **Governance Mode:** Special permissions required to delete (can grant override)
  - **Compliance Mode:** Cannot be deleted by anyone, including root, during retention period
- Retention period: Fixed amount of time
- Legal hold: Indefinite retention, no expiry, can be removed with s3:PutObjectLegalHold permission
- Requires versioning
- Can set default retention on bucket or per-object

**S3 Inventory:**
- List objects and metadata
- Outputs: CSV, ORC, Parquet
- Daily or weekly
- Can filter by prefix
- Use for: audit, compliance, analytics

**S3 Batch Operations:**
- Perform operations on billions of objects
- Operations: Copy, Invoke Lambda, Replace tags, Restore from Glacier, Object Lock, etc.
- Uses S3 Inventory or manifest
- Retry and progress tracking
- Completion reports

**S3 Analytics:**
- Storage Class Analysis: Recommendations for lifecycle policies
- Takes 24-48 hours to start
- Export to CSV

**Requester Pays:**
- Bucket owner pays for storage
- Requester pays for data transfer and requests
- Requester must be authenticated (no anonymous access)

**Object Tags:**
- Key-value pairs (up to 10 per object)
- Useful for lifecycle rules, analytics, access control
- Searchable with S3 Batch Operations

**S3 Access Logs:**
- Log all requests to bucket
- Destination bucket must be in same region
- Don't set destination same as monitored bucket (logging loop)
- Can analyze with Athena

**CORS (Cross-Origin Resource Sharing):**
- Allow requests from another origin
- Configure allowed origins, methods, headers
- Required for: Web fonts, client-side web applications

**MFA Delete:**
- Require MFA for: permanent deletion of version, suspend versioning
- Only bucket owner (root account) can enable/disable
- Versioning must be enabled

### S3 Integration Patterns

**Static Website Hosting:**
- Enable on bucket properties
- Index document and error document
- Bucket name must match domain (for Route 53)
- Works with HTTPS using CloudFront

**Cross-Account Access:**
- Bucket policies with cross-account principal
- IAM roles for temporary access
- Resource-based policies

**VPC Endpoints:**
- Gateway endpoint for S3 (no charge)
- Private connection from VPC to S3
- Route table entries
- Bucket policies can require VPC endpoint

**CloudFront Integration:**
- Global content delivery
- Cache S3 content at edge locations
- Origin Access Identity (OAI) or Origin Access Control (OAC) to restrict direct S3 access
- Geo-restriction capabilities

### Exam-Focused Scenarios

**When to use S3:**
- Static content storage and delivery
- Backup and archive
- Data lakes and big data analytics
- Disaster recovery
- Hybrid cloud storage
- Application hosting (static websites)

**Cost Optimization:**
- Lifecycle policies to transition to cheaper storage classes
- Intelligent-Tiering for unknown access patterns
- Delete incomplete multipart uploads
- S3 Analytics for optimization recommendations
- Compress objects before storage
- Use S3 Select to retrieve only needed data

**Data Protection:**
- Versioning for accidental deletion
- MFA Delete for critical data
- Object Lock for compliance
- Replication for disaster recovery
- Cross-region for geographic redundancy
- Backup with S3 Batch Operations

**Performance:**
- Randomize prefixes for high request rates
- Transfer Acceleration for global uploads
- Multipart upload for large files
- Byte-range fetches for parallel downloads
- CloudFront for read-heavy workloads

**Security Best Practices:**
- Block public access unless necessary
- Encryption at rest (SSE-S3, SSE-KMS)
- Encryption in transit (enforce HTTPS)
- Least privilege IAM policies
- VPC endpoints for private access
- Access logging and monitoring
- S3 Access Analyzer for policy review

## 2. Amazon EBS (Elastic Block Store)

### Core Concepts

**What is EBS:**
- Block-level storage volumes for EC2
- Network-attached storage (not physically attached)
- Persist independently from instance life
- Automatically replicated within AZ
- Can only attach to instances in same AZ
- Can attach multiple volumes to single instance
- Can detach and reattach to different instances (except root volumes when running)

**EBS Volume Types:**

### SSD-Based Volumes (For random I/O):

**General Purpose SSD (gp3):**
- Latest generation
- Baseline: 3,000 IOPS, 125 MB/s throughput
- Size: 1 GB - 16 TB
- Can provision IOPS and throughput independently
- Max: 16,000 IOPS, 1,000 MB/s throughput
- Price: Lower than gp2
- Use for: Virtual desktops, dev/test environments, boot volumes, low-latency applications

**General Purpose SSD (gp2):**
- Previous generation
- IOPS scales with volume size (3 IOPS/GB)
- Baseline: 100-16,000 IOPS
- Burst up to 3,000 IOPS (for volumes < 1 TB)
- Size: 1 GB - 16 TB
- Throughput: 128-250 MB/s
- Use for: Boot volumes, legacy applications

**Provisioned IOPS SSD (io2 Block Express):**
- Latest generation
- Max IOPS: 256,000 (1,000 IOPS/GB)
- Max throughput: 4,000 MB/s
- Size: 4 GB - 64 TB
- Sub-millisecond latency
- 99.999% durability
- Use for: Largest, most critical, performance-sensitive workloads

**Provisioned IOPS SSD (io2):**
- Max IOPS: 64,000 (500 IOPS/GB for < 32GB, 1000 IOPS/GB for ≥ 32GB)
- Max throughput: 1,000 MB/s
- Size: 4 GB - 16 TB
- 99.999% durability
- Use for: Databases, critical business applications

**Provisioned IOPS SSD (io1):**
- Previous generation
- Max IOPS: 64,000 (50 IOPS/GB)
- Max throughput: 1,000 MB/s
- Size: 4 GB - 16 TB
- 99.8-99.9% durability
- Use for: Legacy applications requiring high IOPS

### HDD-Based Volumes (For sequential I/O):

**Throughput Optimized HDD (st1):**
- Baseline throughput: 40 MB/s/TB
- Burst throughput: 250 MB/s/TB
- Max throughput: 500 MB/s
- Size: 125 GB - 16 TB
- Cannot be boot volume
- Use for: Big data, data warehouses, log processing, streaming workloads

**Cold HDD (sc1):**
- Lowest cost HDD
- Baseline throughput: 12 MB/s/TB
- Burst throughput: 80 MB/s/TB
- Max throughput: 250 MB/s
- Size: 125 GB - 16 TB
- Cannot be boot volume
- Use for: Infrequently accessed data, lowest cost requirements

### EBS Snapshots

**Snapshot Basics:**
- Point-in-time backups to S3
- Incremental (only changed blocks)
- First snapshot: full copy
- Can create volume from snapshot in any AZ
- Can copy snapshots across regions
- Snapshots billed for actual data stored

**Features:**

**EBS Snapshot Archive:**
- Move snapshots to archive tier
- 75% cheaper than standard
- 24-72 hours to restore
- Use for: Rarely accessed snapshots, long-term retention

**Recycle Bin for EBS Snapshots:**
- Protect against accidental deletion
- Specify retention period (1 day to 1 year)
- Restore from Recycle Bin during retention

**Fast Snapshot Restore (FSR):**
- No latency penalty on first use of volume created from snapshot
- Expensive (charged per AZ per snapshot)
- Use for: Time-critical restores, large snapshots

**Snapshot Lifecycle:**
- Data Lifecycle Manager (DLM) to automate snapshots
- Create, retain, delete based on schedule
- Tag-based policies
- Cross-account snapshot copy

**Snapshot Copy:**
- Copy within region or cross-region
- Can encrypt during copy
- Incremental copy (only unique blocks)

### EBS Encryption

**Encryption at Rest:**
- Uses AWS KMS (AES-256)
- Encrypt volumes, snapshots, volumes created from snapshots
- Minimal impact on latency
- Encryption and decryption handled transparently

**What Gets Encrypted:**
- Data at rest inside volume
- Data in-flight between instance and volume
- Snapshots created from volume
- Volumes created from snapshot

**Encryption Process:**
- Create encrypted volume from unencrypted: snapshot → copy with encryption → create volume
- Encrypt-by-default: All new volumes encrypted automatically
- Can use default CMK or custom CMK

### EBS Multi-Attach

**Features:**
- Attach single io2/io2 BE volume to multiple EC2 instances in same AZ
- Max 16 instances
- All instances have full read/write permissions
- Must use cluster-aware file system (not XFS, EXT4)
- Use for: High-availability clustered applications (Teradata), concurrent writes

### Advanced Features

**EBS-Optimized Instances:**
- Dedicated bandwidth for EBS
- Prevents network contention
- Default on newer instance types
- Can enable on older instance types (additional cost)

**EBS Performance:**
- IOPS: Input/Output operations per second
- Throughput: MB/s of data transfer
- Latency: Time to complete I/O operation
- Queue depth: Number of pending I/O requests

**CloudWatch Metrics:**
- VolumeReadBytes, VolumeWriteBytes
- VolumeReadOps, VolumeWriteOps
- VolumeThroughputPercentage
- VolumeQueueLength
- BurstBalance (gp2, st1, sc1)

**Volume Modifications:**
- Change type, size, IOPS, throughput while in use
- Cannot decrease volume size
- Can only modify once every 6 hours
- Modifications go through optimization phase (can take hours/days for large changes)

**EBS Direct APIs:**
- Read snapshots directly
- Create/write snapshots without EC2 instance
- Use for: Disaster recovery, backup/restore solutions

### Exam-Focused Scenarios

**When to use EBS:**
- Boot volumes for EC2 instances
- Databases (MySQL, PostgreSQL, Oracle, SQL Server)
- File systems requiring block storage
- Applications requiring persistent storage
- Low-latency access to data

**Volume Type Selection:**
- **Boot volumes:** gp3 (cost-effective) or io2 (performance-critical)
- **Databases:** io2/io2 BE for production, gp3 for dev/test
- **Big data/logs:** st1 for throughput
- **Infrequent access:** sc1 for cost savings
- **General purpose:** gp3 (better than gp2 for most use cases)

**High Availability:**
- Snapshots for backup (automated with DLM)
- Copy snapshots to different region for DR
- RAID configurations (RAID 0 for performance, RAID 1 for redundancy)
- Multi-Attach for clustered applications

**Cost Optimization:**
- Delete unattached volumes
- Snapshot archive for old snapshots
- Use gp3 instead of gp2
- Right-size volumes (don't over-provision)
- Delete old snapshots with lifecycle policies

**Performance Optimization:**
- Use io2/io2 BE for consistent high IOPS
- RAID 0 for higher IOPS/throughput (striping multiple volumes)
- Pre-warm volumes created from snapshots (FSR or read all blocks)
- Monitor CloudWatch metrics and adjust

**Data Protection:**
- Regular snapshots (automated with DLM)
- Encryption at rest
- Cross-region snapshot copy
- Recycle Bin for accidental deletion protection

## 3. Amazon EFS (Elastic File System)

### Core Concepts

**What is EFS:**
- Fully managed NFS (Network File System) v4.1
- Shared file storage for Linux instances
- Scales automatically (petabytes)
- Pay for what you use
- Multi-AZ by default (regional service)
- Can mount from thousands of EC2 instances concurrently
- POSIX-compliant file system

**Use Cases:**
- Content management, web serving
- Big data analytics
- Application development and testing
- Media workflows
- Database backups
- Container storage (persistent volumes for ECS/EKS)
- Home directories

### Performance Modes

**General Purpose (default):**
- Lowest latency
- Max 7,000 file operations/second
- Use for: Most workloads (web serving, content management, home directories)

**Max I/O:**
- Higher latency
- Virtually unlimited file operations/second
- Use for: Big data, media processing, large-scale analytics
- Cannot change after creation

### Throughput Modes

**Bursting (default):**
- Throughput scales with storage size
- Baseline: 50 MB/s per TB
- Burst up to 100 MB/s
- Burst credits accumulate when below baseline
- Use for: Unpredictable workloads

**Provisioned:**
- Set throughput independent of storage size
- Pay for provisioned throughput
- Use for: High throughput to storage ratio (small files but high access)

**Elastic (recommended):**
- Automatically scales throughput up/down based on workload
- Up to 3 GB/s read, 1 GB/s write
- Pay only for throughput used
- Use for: Unpredictable or spiky workloads

### Storage Classes

**Standard:**
- Multi-AZ, frequently accessed files
- Highest durability and availability
- 99.99% availability

**Standard-IA (Infrequent Access):**
- Lower storage cost (up to 92% savings)
- Retrieval cost per GB
- Lifecycle management moves files automatically
- Same availability and durability as Standard

**One Zone:**
- Single AZ storage
- 47% lower cost than Standard
- 99.9% availability

**One Zone-IA:**
- Single AZ + infrequent access
- 92% lower cost than Standard
- Use for: Non-critical data, backups

### Lifecycle Management

**Policies:**
- Move files to IA after N days of not being accessed
- Options: 7, 14, 30, 60, 90 days, or disabled
- Metadata operations don't count as access
- Intelligent-Tiering: Automatically moves between Standard and IA

### Security and Access Control

**Network Access:**
- Mount targets in VPC subnets
- Control access with security groups
- One mount target per AZ
- Can access from on-premises via Direct Connect or VPN

**IAM:**
- Control administrative actions (CreateFileSystem, DeleteFileSystem)
- Not for file/directory level access

**File System Policies:**
- Resource-based policies
- Control NFS client access
- Enforce read-only, encryption in transit, root squashing

**POSIX Permissions:**
- Standard Unix file permissions
- User and group IDs
- chmod, chown commands

**Access Points:**
- Application-specific entry points into file system
- Enforce user identity and root directory
- Simplify access for applications
- Can combine with IAM policies

### Encryption

**Encryption at Rest:**
- Uses AWS KMS
- Must be enabled at creation
- Cannot enable/disable after creation

**Encryption in Transit:**
- TLS between client and EFS
- Enable with mount helper
- Can enforce with file system policy

### Performance and Scaling

**Performance:**
- Hundreds of gigabytes per second throughput
- Petabyte-scale storage
- Millions of IOPS
- Single-digit millisecond latencies

**Scaling:**
- Automatically scales capacity up/down
- No provisioning required
- No capacity planning

### EFS Replication

**Cross-Region or Same-Region:**
- Replicate to another EFS file system
- Continuous, automatic replication
- RPO/RTO in minutes
- Use for: Disaster recovery, compliance

### EFS Integration

**AWS Backup:**
- Automatic backups with policies
- Point-in-time recovery
- Cross-region backup copy
- Restore to new file system

**CloudWatch Metrics:**
- ClientConnections
- DataReadIOBytes, DataWriteIOBytes
- MetadataIOBytes
- PercentIOLimit (for General Purpose mode)
- PermittedThroughput, MeteredIOBytes (for Provisioned throughput)

**DataSync:**
- Migrate data to EFS from on-premises or other cloud
- Fast, secure, automated

**Transfer Family:**
- SFTP/FTPS/FTP access to EFS
- Managed file transfer service

**Container Integration:**
- Persistent storage for ECS tasks (Fargate and EC2)
- EKS persistent volumes
- Shared storage across containers

### Exam-Focused Scenarios

**When to use EFS:**
- Shared file storage across multiple EC2 instances
- Linux-based applications
- Content management systems
- Web serving with shared content
- Big data analytics
- Container persistent storage
- Lift-and-shift on-premises applications using NFS

**EFS vs EBS vs S3:**
- **EFS:** Shared file system, multiple instances, Linux, NFS
- **EBS:** Block storage, single instance (except Multi-Attach), can be Windows or Linux
- **S3:** Object storage, unlimited instances, HTTP API, static content

**Cost Optimization:**
- Lifecycle management to move to IA
- One Zone for non-critical data
- Elastic throughput mode (pay for what you use)
- Delete unused file systems
- Monitor with CloudWatch

**High Availability:**
- Multi-AZ by default (Standard storage classes)
- Replication for disaster recovery
- AWS Backup for point-in-time recovery
- Mount targets in multiple AZs

**Performance Optimization:**
- Elastic throughput for most workloads
- Provisioned throughput for consistent high performance
- Max I/O performance mode for large-scale operations
- Monitor PercentIOLimit metric

**Security Best Practices:**
- Encryption at rest and in transit
- VPC security groups for mount targets
- Access points for application isolation
- POSIX permissions for file-level control
- File system policies to enforce security

## 4. Amazon FSx

### Overview

FSx provides fully managed third-party file systems. Four types available:
1. FSx for Windows File Server
2. FSx for Lustre
3. FSx for NetApp ONTAP
4. FSx for OpenZFS

## FSx for Windows File Server

### Core Concepts

**What is FSx for Windows:**
- Fully managed Windows native shared file system
- Built on Windows Server
- SMB protocol (2.0, 3.0, 3.1.1) and NFS (for Linux)
- Supports NTFS, Windows ACLs, AD integration
- Single-AZ or Multi-AZ deployment

**Features:**
- DFS Namespaces for grouping file systems
- Supports up to thousands of concurrent connections
- Automatic backups to S3
- Encryption at rest (KMS) and in transit (SMB 3.0+)
- SSD and HDD storage options
- De-duplication to save storage (HDD only)

### Performance

**Throughput Tiers:**
- 8 MB/s to 2 GB/s
- 100,000s IOPS
- Sub-millisecond latencies

**Storage Options:**
- **SSD:** Latency-sensitive, transactional, I/O intensive (databases, media processing, analytics)
- **HDD:** Throughput-intensive, less latency-sensitive (home directories, CMS, media workflow)

### Deployment Options

**Single-AZ:**
- Single subnet in one AZ
- Automatic daily backups
- Lower cost
- Use for: Dev/test, lower availability requirements

**Multi-AZ:**
- Standby file server in different AZ
- Automatic failover (typically < 30 seconds)
- Higher availability and durability
- Use for: Production, business-critical applications

### Access and Integration

**Access Methods:**
- Windows instances via SMB
- Linux instances via SMB or NFS
- On-premises via Direct Connect or VPN
- VPN from individual workstations

**Active Directory Integration:**
- AWS Managed Microsoft AD
- Self-managed AD (on-premises or in AWS)
- User authentication and access control
- Supports Kerberos authentication

**AWS Services Integration:**
- WorkSpaces for user home directories
- AppStream 2.0 for persistent storage
- AWS Backup for centralized backup

### Data Protection

**Backups:**
- Automatic daily backups (retention 0-90 days)
- User-initiated backups (retention up to infinite)
- Backups stored in S3
- Can copy backups to another region

**Shadow Copies:**
- Windows Previous Versions feature
- Users can restore files from snapshots
- Configurable schedule (default: 7 AM & 12 PM UTC)

### Exam-Focused Scenarios

**When to use FSx for Windows:**
- Windows applications requiring SMB
- Active Directory integration needed
- Windows Server workloads migration
- .NET applications
- SQL Server file shares
- Home directories for Windows users
- SharePoint, Microsoft CRM

## FSx for Lustre

### Core Concepts

**What is FSx for Lustre:**
- High-performance file system for compute-intensive workloads
- Lustre = Linux + cluster
- Scales to hundreds of GB/s throughput
- Millions of IOPS
- Sub-millisecond latencies
- POSIX-compliant

**Use Cases:**
- High Performance Computing (HPC)
- Machine Learning training
- Video processing and transcoding
- Financial modeling
- Electronic Design Automation (EDA)
- Genomics

### Deployment Types

**Scratch File System:**
- Temporary storage
- Data not replicated (lost if file server fails)
- 6x faster than Persistent
- Lowest cost
- Burst up to 200 MB/s/TB
- Use for: Short-term processing, temporary storage, cost-sensitive workloads

**Persistent File System:**
- Long-term storage
- Data replicated within AZ
- 50 MB/s, 100 MB/s, or 200 MB/s per TB throughput
- Use for: Long-term storage, production workloads, sensitive data

### S3 Integration

**Lazy Loading:**
- Link FSx file system to S3 bucket
- Files loaded from S3 on first access
- Subsequent access from local cache

**Data Repository Association (DRA):**
- Import data from S3 to FSx
- Export data from FSx to S3
- Automatic export on file close or on schedule
- Multiple S3 buckets supported

**Export Back to S3:**
- Automatically export new/changed files to S3
- Helps with data protection and long-term archival

### Performance

**Throughput:**
- Scratch: 200 MB/s/TB
- Persistent: 50, 100, or 200 MB/s/TB
- Can provision higher with more storage

**Storage:**
- 1.2 TB to hundreds of petabytes
- SSD storage (in-memory cache for hot data)

### Access and Integration

**Access Methods:**
- Mount on Linux instances via Lustre client
- On-premises via Direct Connect or VPN
- Cannot mount on Windows (Linux-only)

**Compute Integration:**
- EC2 instances
- ECS/EKS containers
- AWS Batch for HPC jobs
- SageMaker for ML training

### Data Protection

**Backups:**
- Automatic daily backups (Persistent only)
- User-initiated backups
- Backups to S3
- Incremental backups

### Exam-Focused Scenarios

**When to use FSx for Lustre:**
- HPC workloads
- Machine learning training with large datasets
- Big data analytics requiring high throughput
- Media processing at scale
- Financial simulations
- Need integration with S3 for data lakes

**Scratch vs Persistent:**
- **Scratch:** Temporary workloads, cost-sensitive, highest performance, no replication
- **Persistent:** Long-term workloads, production data, replication, backups

## FSx for NetApp ONTAP

### Core Concepts

**What is FSx for ONTAP:**
- Fully managed NetApp ONTAP file system
- Supports NFS, SMB, iSCSI protocols
- Works with Linux, Windows, MacOS
- NetApp features: Snapshots, clones, replication, compression, deduplication
- Multi-AZ high availability

**Features:**
- Point-in-time snapshots
- Cloning (instant, space-efficient copies)
- SnapMirror replication (to another FSx ONTAP or on-premises)
- Storage efficiency: Compression, deduplication, compaction
- FlexClone for rapid copy creation

### Storage Tiers

**Primary Storage (SSD):**
- Low latency
- Frequently accessed data

**Capacity Pool (Elastic):**
- Cost-optimized for infrequently accessed data
- Automatically tiers cold data
- Transparent to applications

### Protocol Support

**Multi-Protocol:**
- NFS (v3, v4.0, v4.1, v4.2)
- SMB (2.0, 2.1, 3.0, 3.1.1)
- iSCSI
- All simultaneously

### Use Cases

**Migration:**
- Lift-and-shift on-premises ONTAP to AWS
- Migrate enterprise applications
- NetApp Cloud Volumes ONTAP migration

**Hybrid Cloud:**
- Replicate between on-premises ONTAP and FSx
- Disaster recovery
- Burst to cloud

**Multi-Protocol Access:**
- Applications requiring both NFS and SMB
- Mixed Linux and Windows environments

### Exam-Focused Scenarios

**When to use FSx for ONTAP:**
- Migrating NetApp workloads to AWS
- Need multi-protocol access (NFS + SMB)
- Advanced data management (snapshots, clones, replication)
- Storage efficiency features (dedup, compression)
- iSCSI block storage requirements

## FSx for OpenZFS

### Core Concepts

**What is FSx for OpenZFS:**
- Fully managed OpenZFS file system
- NFS protocol (v3, v4.0, v4.1, v4.2)
- Up to 1 million IOPS
- < 0.5 ms latency
- Point-in-time snapshots
- Data compression

**Features:**
- Snapshots up to 10,000 per file system
- Cloning for rapid copy creation
- Z-Standard compression
- Copy-on-write for data integrity

### Performance

- Up to 12.5 GB/s throughput
- Up to 1 million IOPS
- Low latency (< 0.5 ms)

### Use Cases

- Linux-based applications
- Move ZFS-based workloads to AWS
- Application development requiring snapshots
- Data-intensive applications

### Exam-Focused Scenarios

**When to use FSx for OpenZFS:**
- Migrating OpenZFS/ZFS workloads
- Need high IOPS with low latency
- Linux NFS file systems
- Frequent snapshots and clones
- Data compression requirements

---

## FSx Comparison Summary

| Feature | Windows | Lustre | ONTAP | OpenZFS |
|---------|---------|--------|-------|---------|
| **Protocol** | SMB, NFS | Lustre | NFS, SMB, iSCSI | NFS |
| **OS** | Windows, Linux | Linux | All | Linux |
| **Performance** | Moderate-High | Highest | Moderate | High |
| **Use Case** | Windows apps, AD | HPC, ML | Multi-protocol, NetApp | ZFS migration, Linux |
| **S3 Integration** | No | Yes | No | No |
| **Multi-AZ** | Yes | No | Yes | No (Single-AZ) |
| **Deployment** | Single/Multi-AZ | Scratch/Persistent | Multi-AZ | Single-AZ |

## 5. AWS Storage Gateway

### Core Concepts

**What is Storage Gateway:**
- Hybrid cloud storage service
- Bridge between on-premises and AWS storage
- Virtual appliance (VM) deployed on-premises
- Provides low-latency access to cloud storage
- Three types: File Gateway, Volume Gateway, Tape Gateway

### Hardware Appliance

**Physical Appliance:**
- Pre-configured server from AWS
- For locations without virtualization infrastructure
- 1U rack-mount server
- High-performance NVMe SSD

### File Gateway (Amazon S3 File Gateway)

**Architecture:**
- NFS or SMB interface to S3
- Files stored as objects in S3
- Local cache for frequently accessed data
- S3 bucket versioning, lifecycle, replication

**Features:**
- Protocol: NFS (v3, v4.1), SMB (2, 3)
- Cache: Local disks for low-latency access
- S3 storage classes: Standard, IA, One Zone-IA, Intelligent-Tiering
- Lifecycle policies to Glacier
- Active Directory integration (SMB)
- Bucket sharing across multiple File Gateways

**Use Cases:**
- Backup to S3
- Tiering on-premises storage to cloud
- Disaster recovery
- Cloud data processing with on-premises front-end
- Migrating files to S3

**Monitoring:**
- CloudWatch metrics: CacheHitPercent, CachePercentUsed, ReadBytes, WriteBytes
- Health checks and alerts

### Volume Gateway

Two modes: Stored Volumes and Cached Volumes

**Stored Volumes:**
- Entire dataset stored on-premises
- Asynchronous backup to S3 as EBS snapshots
- Low-latency access to complete dataset
- Volume size: 1 GB - 16 TB
- Max 32 volumes per gateway (512 TB total)
- Use for: Full on-premises dataset with cloud backup

**Cached Volumes:**
- Primary data stored in S3
- Frequently accessed data cached on-premises
- Low-latency access to hot data
- Volume size: 1 GB - 32 TB
- Max 32 volumes per gateway (1 PB total)
- Use for: Primary data in cloud with on-premises cache

**Features:**
- iSCSI protocol
- Point-in-time snapshots to S3 (EBS snapshots)
- Can create EBS volumes from snapshots
- Can clone volumes
- Bandwidth throttling

**Use Cases:**
- Backup and disaster recovery
- Cloud-based storage for iSCSI applications
- Migrating to cloud storage
- Data archival

### Tape Gateway (VTL - Virtual Tape Library)

**Architecture:**
- Virtual tapes stored in S3 and Glacier
- iSCSI interface
- Compatible with existing backup applications (Veeam, Veritas, Commvault, etc.)

**Components:**
- **Virtual Tapes:** Stored in S3 (Virtual Tape Library)
- **Virtual Tape Shelf (VTS):** Archived tapes in Glacier or Deep Archive
- **Media Changer:** Manages tape library
- **Tape Drives:** 10 drives per gateway

**Features:**
- Tape size: 100 GB to 5 TB
- Max 1,500 virtual tapes (1 PB total)
- Compression reduces storage costs
- Encryption at rest and in transit

**Use Cases:**
- Replace physical tape infrastructure
- Long-term backup archival
- Regulatory compliance
- Cloud backup for backup applications

### Gateway Performance and Configuration

**Cache Sizing:**
- File Gateway: 150 GB minimum per file share
- Volume Gateway: Size based on working set
- Monitor CacheHitPercent (should be > 80%)

**Bandwidth Optimization:**
- Bandwidth throttling to control upload
- Schedule for off-peak transfers
- Upload buffer for write staging

**High Availability:**
- VMware HA or Microsoft Failover Clustering
- Multiple gateways for redundancy
- Automated health checks

**Activation:**
- Deployed on-premises (VMware, Hyper-V, Linux KVM, EC2)
- Activated in AWS region
- Connected to AWS via internet, Direct Connect, or VPN

### Security

**Encryption:**
- Data encrypted in transit (SSL/TLS)
- Data encrypted at rest in AWS (S3-SSE, Glacier)
- Optional client-side encryption

**Access Control:**
- IAM for gateway management
- POSIX permissions (File Gateway)
- CHAP authentication (Volume/Tape Gateway)
- Active Directory integration (SMB File Gateway)

**Compliance:**
- HIPAA eligible
- PCI DSS compliant

### Exam-Focused Scenarios

**When to use Storage Gateway:**
- Hybrid cloud storage
- On-premises applications needing cloud storage
- Disaster recovery with cloud backup
- Tiering cold data to cloud
- Cloud migration path
- Replacing physical tape infrastructure

**Gateway Type Selection:**
- **File Gateway:** File-based access, NFS/SMB, backup files to S3
- **Volume Gateway (Stored):** iSCSI, full dataset on-premises, S3 backup
- **Volume Gateway (Cached):** iSCSI, primary data in cloud, on-premises cache
- **Tape Gateway:** Backup applications, tape replacement, Glacier archival

**Cost Optimization:**
- Lifecycle policies to transition to Glacier
- Monitor and right-size cache
- Bandwidth management to avoid overage
- Compress data before upload

**Performance Optimization:**
- Adequate cache sizing (monitor metrics)
- High-speed local disks (SSD)
- Sufficient bandwidth to AWS
- Direct Connect for consistent performance

## 6. AWS DataSync

### Core Concepts

**What is DataSync:**
- Data transfer service for moving large amounts of data
- To/from: On-premises ↔ AWS, AWS ↔ AWS
- Automatic encryption and integrity validation
- Up to 10x faster than open-source tools
- Fully managed (no scripts, no infrastructure)

**Destinations:**
- Amazon S3 (all storage classes)
- Amazon EFS
- Amazon FSx (Windows, Lustre, NetApp ONTAP, OpenZFS)

### Architecture

**Components:**

**DataSync Agent:**
- Virtual machine deployed on-premises (VMware, Hyper-V, KVM) or EC2
- Connects to on-premises storage
- Not needed for AWS-to-AWS transfers

**DataSync Task:**
- Defines source, destination, and transfer settings
- Can schedule or run on-demand
- Includes filtering, bandwidth limiting

**Locations:**
- Source and destination endpoints
- Can be on-premises NFS/SMB or AWS storage

### Transfer Features

**Performance:**
- Up to 10 Gbps per agent
- Parallel transfers
- Automatic retry and recovery
- Network optimization

**Data Integrity:**
- Automatic verification (checksums)
- Ensures source and destination match
- Validates during and after transfer

**Bandwidth Management:**
- Throttle bandwidth usage
- Schedule transfers for off-peak hours
- Avoid network congestion

**Filtering:**
- Include/exclude patterns
- Filter by file modification time
- Transfer only changed data

### Transfer Modes

**Full Copy:**
- Transfer all files
- Initial sync or full refresh

**Incremental:**
- Transfer only changes since last sync
- Detects changes automatically
- Faster subsequent transfers

### Security

**Encryption:**
- In-transit: TLS 1.2
- At-rest: Encrypted in destination (S3-SSE, EFS/FSx KMS)

**Access Control:**
- IAM roles for AWS resource access
- VPC endpoints for private connectivity
- Security groups for network access

**Connectivity:**
- Public internet
- Direct Connect
- VPN
- VPC PrivateLink

### Monitoring and Logging

**CloudWatch:**
- Metrics: BytesTransferred, FilesTransferred, DataIntegrityErrors
- Alarms for task failures
- Logs for troubleshooting

**EventBridge:**
- Task state change events
- Trigger automation on completion

### Use Cases

**Data Migration:**
- Migrate on-premises file servers to EFS or FSx
- Migrate NAS/SAN to S3
- Cloud-to-cloud migration

**Data Processing:**
- Transfer data to AWS for processing (analytics, ML)
- Hybrid workflows (on-premises processing, cloud storage)

**Replication and Sync:**
- Periodic sync between on-premises and cloud
- Disaster recovery data sync
- Active archive to S3

**Data Distribution:**
- Distribute data to multiple AWS regions
- Replicate to multiple destinations

### Exam-Focused Scenarios

**When to use DataSync:**
- One-time large data migration
- Periodic sync between on-premises and AWS
- Fast, secure, automated transfers
- Data integrity validation required
- Transfer to EFS, FSx, or S3

**DataSync vs Other Transfer Methods:**
- **DataSync:** Large datasets, automated, scheduled, integrity checks, fast (10 Gbps)
- **S3 Transfer Acceleration:** Direct S3 uploads, < 1 GB files, global users
- **Snowball:** Offline transfer, 10+ TB, limited bandwidth
- **Direct Connect:** Dedicated network, consistent performance, not a transfer service
- **Storage Gateway:** Hybrid storage, ongoing access, not one-time transfer

**Performance Optimization:**
- Deploy agent close to data source
- Use Direct Connect for large transfers
- Multiple agents for massive scale
- Schedule during off-peak hours

**Cost Optimization:**
- Transfer directly to S3 IA or Glacier
- Use bandwidth throttling
- Monitor data transfer costs
- One-time vs. ongoing sync frequency

## 7. AWS Snow Family

### Overview

Physical devices for data transfer and edge computing:
- **Snowcone:** 8-14 TB, smallest
- **Snowball Edge:** 80-210 TB, compute capable
- **Snowmobile:** Exabyte-scale, shipping container

### AWS Snowcone

**Specifications:**
- **Snowcone:** 8 TB HDD storage, 2 vCPUs, 4 GB RAM
- **Snowcone SSD:** 14 TB SSD storage, 2 vCPUs, 4 GB RAM
- **Dimensions:** 9" x 6" x 3", weighs 4.5 lbs
- Rugged, portable, small

**Features:**
- USB-C power or battery
- DataSync agent pre-installed
- Edge computing with EC2 instances
- Offline data transfer or online with DataSync

**Use Cases:**
- Small-scale data collection
- Edge computing in space-constrained or harsh environments
- Drone data collection, IoT, field data capture
- Transfer 8-24 TB data

**Connectivity:**
- Wi-Fi, wired Ethernet
- DataSync for online transfer
- Ship device to AWS for offline transfer

### AWS Snowball Edge

Two device types: Storage Optimized and Compute Optimized

**Snowball Edge Storage Optimized:**
- 80 TB usable HDD (210 TB with clustering)
- 40 vCPUs, 80 GB RAM
- 1 TB SSD for block volumes
- 40 Gb/100 Gb network
- Use for: Large data migrations, local storage, disaster recovery

**Snowball Edge Compute Optimized:**
- 42 TB usable HDD or 28 TB NVMe
- 52 vCPUs (208 with optional GPU), 208 GB RAM
- Optional NVIDIA V100 GPU
- Use for: Machine learning, video processing, analytics at edge

**Features:**
- Run EC2 instances (AMI available: Amazon Linux 2)
- Run Lambda functions
- NFS, S3-compatible endpoint
- Cluster up to 15 devices for more storage/compute
- Encryption: 256-bit encryption, managed keys in KMS (never stored on device)

**Use Cases:**
- Large-scale data transfer (10 TB - 10 PB)
- Disaster recovery
- Data center migration
- Edge computing and local processing
- Content distribution

**Ordering and Shipping:**
- Order via AWS console
- AWS ships device to you
- You copy data and ship back
- AWS imports data to S3

**Transfer Speed:**
- 10 Gb/40 Gb/100 Gb network
- Typical transfer: 1 week for 80 TB
- Faster than internet for > 2 TB (typically)

### AWS Snowmobile

**Specifications:**
- 100 PB per Snowmobile
- 45-foot shipping container
- Truck-mounted

**Features:**
- Tamper-resistant, waterproof, temperature controlled
- GPS tracking
- 24/7 video surveillance
- Security escort

**Use Cases:**
- Data center migration
- Exabyte-scale transfers
- Multiple petabytes of data
- Typically for > 10 PB

**Process:**
- AWS brings Snowmobile to your site
- Connects to your network
- You copy data (weeks to months)
- AWS transports to AWS facility
- Data loaded to S3

### Snow Family Comparison

| Feature | Snowcone | Snowball Edge | Snowmobile |
|---------|----------|---------------|------------|
| **Capacity** | 8-14 TB | 80-210 TB | 100 PB |
| **Size** | Smallest | Medium | Shipping container |
| **Compute** | 2 vCPUs | 40-52 vCPUs | No |
| **Use Case** | < 24 TB | 10 TB - 10 PB | > 10 PB |
| **Portability** | Most portable | Portable | Stationary at site |
| **Edge Computing** | Yes (limited) | Yes (capable) | No |

### Edge Computing Capabilities

**Snowcone:**
- Run edge applications
- Limited compute (2 vCPUs, 4 GB RAM)
- IoT data processing

**Snowball Edge:**
- EC2 instances (sbe1, sbe-c, sbe-g instance types)
- Lambda functions
- Machine learning inference
- Local S3 storage
- Video processing

**Use Cases:**
- Pre-process data before transfer
- Disconnected environments
- Manufacturing, maritime, oil & gas, mining
- Tactical edge (military, emergency response)

### OpsHub

**AWS OpsHub:**
- GUI application for Snow devices
- Manage devices without CLI
- Monitor metrics, unlock/configure devices
- Launch EC2 instances
- Transfer files (drag and drop)
- Available for Windows, Mac, Linux

### Snow Family Security

**Encryption:**
- 256-bit encryption
- Keys managed in KMS
- Keys never stored on device

**Physical Security:**
- Tamper-resistant enclosures
- Trusted Platform Module (TPM)
- E-ink shipping label (digital, updates automatically)

**Network Security:**
- VPC endpoints
- End-to-end tracking

### Exam-Focused Scenarios

**When to use Snow Family:**
- Large data transfers (TB to PB scale)
- Limited or no network bandwidth
- Cost: Cheaper than network transfer for large data
- Time: Faster than network for multi-TB datasets
- Secure offline transfer
- Edge computing in disconnected locations

**Device Selection:**
- **< 24 TB:** Snowcone (portable, small locations)
- **24 TB - 10 PB:** Snowball Edge (standard migrations, edge compute)
- **> 10 PB:** Snowmobile (data center migration, exabyte scale)

**Snow vs Network Transfer:**
- Rule of thumb: If > 2 TB or limited bandwidth, consider Snow
- Calculate: Data size / bandwidth = transfer time
- Example: 100 TB at 100 Mbps = ~100 days; Snowball = ~1 week

**Cost Considerations:**
- Snow device fees (per day after 10 days)
- Shipping costs (to and from AWS)
- Data transfer in (free to S3)
- Compare with Direct Connect or internet transfer costs

**Edge Computing:**
- Snowball Edge for: ML inference, video processing, local data processing
- Snowcone for: Lightweight processing, IoT, drones

---

## Storage Services Comparison Summary

| Service | Type | Access | Durability | Use Case |
|---------|------|--------|------------|----------|
| **S3** | Object | HTTP API | 11 9's (multi-AZ) | Static content, data lakes, backups |
| **EBS** | Block | Single EC2 (same AZ) | Single AZ | EC2 boot volumes, databases |
| **EFS** | File (NFS) | Multi-EC2, multi-AZ | Multi-AZ | Shared file storage, content management |
| **FSx Windows** | File (SMB) | Multi-instance | Single/Multi-AZ | Windows apps, Active Directory |
| **FSx Lustre** | File (Lustre) | Multi-instance | Single AZ | HPC, ML, video processing |
| **FSx ONTAP** | File (Multi) | Multi-instance | Multi-AZ | NetApp workloads, multi-protocol |
| **FSx OpenZFS** | File (NFS) | Multi-instance | Single AZ | ZFS workloads, Linux |
| **Storage Gateway** | Hybrid | On-prem + Cloud | Varies | Hybrid cloud, cloud migration |
| **DataSync** | Transfer | N/A | N/A | Data migration, sync |
| **Snow Family** | Transfer/Edge | Physical device | N/A | Large-scale transfer, edge compute |

## Key Exam Tips for Storage Services

1. **Know the use cases:** Match service to scenario (object vs. block vs. file, access pattern, performance needs)

2. **Understand cost optimization:**
   - S3 lifecycle policies and storage classes
   - EBS volume types and sizing
   - EFS storage classes and lifecycle
   - Snow Family for large transfers

3. **Security patterns:**
   - Encryption at rest and in transit
   - Access control (IAM, bucket policies, POSIX, AD)
   - VPC endpoints for private access

4. **Performance characteristics:**
   - S3: Throughput, request rates, Transfer Acceleration
   - EBS: IOPS, throughput, latency
   - EFS: Performance and throughput modes
   - FSx: Deployment types and performance tiers

5. **High availability and durability:**
   - S3: Multi-AZ by default, replication
   - EBS: Snapshots, single AZ
   - EFS: Multi-AZ by default
   - FSx: Single vs. Multi-AZ options

6. **Integration patterns:**
   - S3 + Lambda, CloudFront, Athena
   - EBS + EC2, snapshots
   - EFS + ECS/EKS containers
   - FSx Lustre + S3, HPC
   - Storage Gateway + on-premises

7. **Data migration strategies:**
   - DataSync for automated, scheduled transfers
   - Snow Family for offline, large-scale transfers
   - Storage Gateway for hybrid, ongoing access
   - S3 Transfer Acceleration for global uploads

This comprehensive guide covers all AWS storage services from the SAP-C02 exam perspective, focusing on architectural decisions, use cases, and best practices.