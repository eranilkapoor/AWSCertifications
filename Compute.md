# Complete AWS Compute Services Knowledge for SAP-C02 Exam

## 1. Amazon EC2 (Elastic Compute Cloud)

### Core Concepts

**Instance Types & Families:**
- **General Purpose (T, M, A series):** Balanced CPU, memory, and networking. T instances use burstable performance with CPU credits
- **Compute Optimized (C series):** High-performance processors for compute-intensive workloads
- **Memory Optimized (R, X, z series):** Fast performance for memory-intensive workloads, large datasets
- **Storage Optimized (I, D, H series):** High sequential read/write to local storage
- **Accelerated Computing (P, G, F, Inf series):** GPU, FPGA for ML, graphics, scientific computing

**Purchasing Options:**

*On-Demand:* Pay per second, no commitment, highest cost per hour

*Reserved Instances (RI):*
- Standard RI: Up to 72% discount, 1 or 3 years, specific instance type/region
- Convertible RI: Up to 54% discount, can change instance family/OS
- Scheduled RI: Launch within reserved time windows
- Can sell unused RIs on Reserved Instance Marketplace

*Savings Plans:*
- Compute Savings Plans: Up to 66% discount, flexible across instance family, size, OS, tenancy, region
- EC2 Instance Savings Plans: Up to 72% discount, committed to instance family in specific region
- 1 or 3-year commitment based on $/hour spend

*Spot Instances:*
- Up to 90% discount compared to On-Demand
- Can be interrupted with 2-minute warning
- Best for fault-tolerant, flexible workloads
- Spot Fleet: Collection of Spot and optionally On-Demand instances
- Spot Block: Uninterrupted for 1-6 hours (deprecated but may appear in questions)

*Dedicated Hosts:*
- Physical server dedicated to your use
- Socket/core visibility for licensing (SQL Server, Oracle)
- Most expensive option
- Can be reserved for cost savings

*Dedicated Instances:*
- Instances run on hardware dedicated to single customer
- May share hardware with other instances from same account
- No visibility into sockets/cores

**Placement Groups:**

*Cluster:* Packs instances close together in single AZ for low-latency, high-throughput networking (10 Gbps+). Use for HPC applications. Single point of failure.

*Spread:* Spreads instances across distinct underlying hardware (max 7 instances per AZ per group). Reduces correlated failures. Use for critical applications.

*Partition:* Divides instances into logical partitions on different racks. Each partition has own network and power. Up to 7 partitions per AZ. Supports hundreds of instances. Use for distributed systems like Hadoop, Cassandra, Kafka.

**Networking:**

*Enhanced Networking:*
- Single Root I/O Virtualization (SR-IOV) provides higher bandwidth, lower latency
- Elastic Network Adapter (ENA): Up to 100 Gbps
- Intel 82599 VF: Up to 10 Gbps (legacy)

*Elastic Fabric Adapter (EFA):*
- Network device for HPC and ML applications
- OS-bypass for ultra-low latency
- Only for Linux, works with MPI applications

*Elastic Network Interface (ENI):*
- Virtual network card
- Can have multiple ENIs per instance
- ENIs can be moved between instances (useful for failover)
- Each ENI has primary private IP, optional secondary IPs, Elastic IP, security groups

*IP Addressing:*
- Private IP: Retained during stop/start
- Public IP: Changes on stop/start unless using Elastic IP
- Elastic IP: Static public IPv4, charged when not associated
- IPv6: All addresses are public

**Storage:**

*Instance Store:*
- Ephemeral storage physically attached to host
- Data lost on stop/termination
- Very high IOPS (millions)
- No additional cost
- Cannot be detached/attached to different instance

*EBS (Elastic Block Store):*
- Network-attached persistent storage
- Automatically replicated within AZ
- Can snapshot to S3 (incremental backups)
- Can change volume type and size while in use
- Volume types: gp3, gp2, io2, io1, st1, sc1
- Can only attach to instances in same AZ
- Can be encrypted (affects snapshots, volumes created from snapshots)

**Auto Scaling Concepts:**

*Launch Templates vs Launch Configurations:*
- Launch Templates: Newer, versioned, supports mixed instance types, Spot, can be used with EC2 directly
- Launch Configurations: Legacy, immutable, only for Auto Scaling groups

*Scaling Policies:*
- Target Tracking: Maintain metric at target value (e.g., CPU at 50%)
- Step Scaling: Scale based on CloudWatch alarm thresholds
- Simple Scaling: Legacy, waits for cooldown
- Scheduled Scaling: Time-based scaling
- Predictive Scaling: ML-based forecasting

*Lifecycle Hooks:*
- Pause instances during launch/terminate for custom actions
- Instances in wait state (Pending:Wait or Terminating:Wait)
- Use with EventBridge, SNS, SQS for automation

**Advanced Features:**

*Hibernation:*
- Saves RAM contents to EBS root volume
- Faster boot than fresh start
- Limited to 150 GB RAM, 60 days max
- Root volume must be encrypted EBS

*User Data:*
- Bootstrap scripts run at instance launch
- Base64 encoded, max 16 KB
- Runs as root user
- Can configure instances dynamically

*Instance Metadata Service (IMDS):*
- IMDSv1: http://169.254.169.254/latest/meta-data/ (request/response)
- IMDSv2: Token-based, more secure, requires session token
- Access instance info, user data, temporary credentials

*EC2 Image Builder:*
- Automated pipeline to create, maintain, validate AMIs
- Can test images before distribution
- Can distribute to multiple regions/accounts
- Integrates with Systems Manager for patch management

### Exam-Focused Scenarios

**When to use EC2:**
- Need full control over OS and applications
- Legacy applications requiring specific configurations
- Long-running applications
- Applications requiring GPU/specialized hardware
- Licensing requires physical server details (use Dedicated Hosts)

**Cost Optimization:**
- Use Savings Plans for predictable workloads across multiple instance types
- Use RIs for stable, predictable workloads with specific instance requirements
- Spot for fault-tolerant, flexible workloads (batch processing, data analysis)
- Right-sizing using Cost Explorer and Compute Optimizer
- Schedule instances to stop during non-business hours

**High Availability:**
- Multi-AZ deployment with Auto Scaling
- ELB health checks to replace unhealthy instances
- Spread placement groups for critical instances
- Cross-region AMI copies for DR

## 2. AWS Lambda

### Core Concepts

**Function Basics:**
- Serverless compute, pay per request and duration
- Supports multiple runtimes: Python, Node.js, Java, .NET, Go, Ruby, custom runtimes
- Max execution time: 15 minutes (900 seconds)
- Max memory: 10,240 MB (10 GB), CPU scales proportionally
- Max deployment package: 50 MB zipped, 250 MB unzipped
- Ephemeral storage (/tmp): 512 MB to 10 GB
- Environment variables: 4 KB max
- Concurrent executions: 1000 per region (soft limit, can be increased)

**Invocation Types:**

*Synchronous:* Client waits for response (API Gateway, ALB, SDK). Error handling is client's responsibility.

*Asynchronous:* Event queued, Lambda retries on error (2 times by default). Can configure dead letter queue (SQS/SNS). Use for S3, SNS, EventBridge, SES.

*Event Source Mapping (Stream/Poll-based):* Lambda polls source for records. For Kinesis, DynamoDB Streams, SQS, Kafka. Lambda manages retries and error handling.

**Configuration:**

*Versions:*
- Immutable, numbered versions ($LATEST is mutable)
- Each version has unique ARN
- Can reference specific version in applications

*Aliases:*
- Pointers to function versions (dev, staging, prod)
- Can point to two versions for weighted traffic splitting
- Enable blue/green deployments

*Layers:*
- Package dependencies, custom runtimes, libraries separately
- Max 5 layers per function
- Max 250 MB total (unzipped)
- Share code across multiple functions
- Reduces deployment package size

**Networking:**

*Default (No VPC):*
- Can access public internet and AWS services
- Cannot access resources in VPC
- Faster cold starts

*VPC-Enabled:*
- Lambda creates ENIs in specified subnets
- Can access VPC resources (RDS, ElastiCache)
- Need NAT Gateway for internet/AWS service access (or VPC endpoints)
- Slower cold starts (now improved with Hyperplane ENIs - shared across functions)

**Performance Optimization:**

*Cold Starts:*
- First invocation or after period of inactivity
- Initialization time for runtime and function code
- Minimize: Keep functions warm with CloudWatch Events, use Provisioned Concurrency, reduce deployment package size, avoid VPC if not needed

*Provisioned Concurrency:*
- Pre-initialized execution environments always ready
- Eliminates cold starts for critical functions
- Charged per GB-second while enabled
- Can auto-scale based on schedule or utilization

*Memory Allocation:*
- CPU power scales with memory
- More memory = faster execution but higher cost
- Use AWS Lambda Power Tuning tool to find optimal configuration

**Event Sources:**

*Push Model:* Source invokes Lambda directly (S3, SNS, API Gateway, ALB, EventBridge, IoT)

*Pull Model:* Lambda polls source (SQS, Kinesis, DynamoDB Streams, Kafka)

*Common Patterns:*
- S3 events for file processing
- DynamoDB Streams for data processing
- EventBridge for scheduled/event-driven tasks
- SQS for decoupling, buffering
- API Gateway for REST APIs

**Error Handling:**

*Synchronous:*
- Returns error to caller
- Client responsible for retries

*Asynchronous:*
- Retries twice automatically
- Configure on-failure destination (SQS, SNS, EventBridge, Lambda)
- Configure max event age and retry attempts

*Stream Processing:*
- Retries until success or data expires
- Can configure: batch size, batch window, max retry attempts, max record age
- Failed records can go to destination (SQS, SNS)
- Supports bisect on error (splits batch and retries)

**Security:**

*Execution Role:* IAM role Lambda assumes to access AWS services

*Resource Policy:* Controls who can invoke the function (cross-account access, service permissions)

*Environment Variables:* Can encrypt with KMS

*VPC Security:* Security groups and NACLs apply to Lambda ENIs

### Advanced Features

**Lambda@Edge:**
- Run Lambda at CloudFront edge locations
- Modify CloudFront requests/responses
- Use cases: URL rewriting, authentication, A/B testing, dynamic content generation
- Limitations: 128 MB memory for viewer functions, different limits than standard Lambda

**Asynchronous Processing:**
- Lambda Destinations: Route function results to SQS, SNS, EventBridge, another Lambda
- Better than DLQ, provides success and failure destinations

**Reserved Concurrency:**
- Guarantees number of concurrent executions for function
- Also sets max limit (throttles beyond this)
- Ensures critical functions have capacity

**Lambda Extensions:**
- External processes that run alongside function
- Use for monitoring, observability, security tools
- Internal extensions (in-process) and external extensions (separate processes)

### Exam-Focused Scenarios

**When to use Lambda:**
- Event-driven, short-duration workloads
- Unpredictable traffic with long idle periods
- Microservices architectures
- Real-time file/stream processing
- Scheduled tasks (replacing cron)
- Backend for mobile/web applications

**Cost Optimization:**
- Right-size memory allocation
- Use Provisioned Concurrency only when needed
- Avoid unnecessary invocations
- Use Step Functions for complex workflows (avoid Lambda-to-Lambda calls)

**Integration Patterns:**
- API Gateway + Lambda for serverless REST APIs
- S3 + Lambda for ETL/data processing
- DynamoDB Streams + Lambda for change data capture
- EventBridge + Lambda for event-driven architectures
- SQS + Lambda for asynchronous processing with buffering

**Limitations to Remember:**
- 15-minute max execution (use Step Functions for longer workflows)
- Regional service (need per-region deployment for multi-region)
- Cold starts (use Provisioned Concurrency for latency-sensitive apps)
- No native persistent storage (use EFS for shared storage)

## 3. Amazon ECS (Elastic Container Service)

### Core Concepts

**Launch Types:**

*EC2 Launch Type:*
- You manage EC2 instances (cluster)
- More control over infrastructure
- Can use Reserved Instances, Savings Plans for cost optimization
- Access to instance for troubleshooting
- Must handle cluster scaling, patching, capacity management

*Fargate Launch Type:*
- Serverless, AWS manages infrastructure
- Pay per task resources (vCPU/memory)
- No instance management
- Better for unpredictable workloads
- Easier to use but less control

**Core Components:**

*Task Definition:*
- JSON blueprint for application (like Dockerfile)
- Defines: container images, CPU/memory, networking mode, IAM roles, logging, environment variables
- Immutable, versioned
- Can define multiple containers per task

*Task:*
- Running instance of task definition
- Can run standalone or as part of service

*Service:*
- Maintains desired count of tasks
- Integrates with ELB for load balancing
- Auto-scaling based on metrics
- Rolling updates with deployment configuration

*Cluster:*
- Logical grouping of tasks/services
- Can mix EC2 and Fargate launch types
- Can span multiple AZs

**Networking Modes (EC2 Launch Type):**

*bridge:* Default Docker virtual network, port mapping required

*host:* Uses host's network directly, no port mapping, no isolation

*awsvpc:* Each task gets own ENI with private IP and security groups. Required for Fargate. Best for isolation.

*none:* No network connectivity

**IAM Roles:**

*Task Role:* Permissions for application (access to S3, DynamoDB, etc.). Defined in task definition.

*Task Execution Role:* Permissions for ECS agent to pull images from ECR, send logs to CloudWatch. Required for Fargate.

*Instance Role (EC2 Launch Type):* Permissions for EC2 instances in cluster to communicate with ECS service.

**Service Discovery:**

*AWS Cloud Map Integration:*
- Automatic DNS registration for tasks
- Health checking
- Route 53 Auto Naming API
- Enables service-to-service communication

*Service Connect:*
- Built-in service mesh capabilities
- Traffic management between services
- No sidecar containers needed (unlike App Mesh)

**Load Balancing:**

*Application Load Balancer (ALB):*
- Layer 7, HTTP/HTTPS
- Dynamic port mapping (required for multiple tasks on same instance)
- Path-based and host-based routing
- Best for microservices

*Network Load Balancer (NLB):*
- Layer 4, TCP/UDP
- High performance, millions of requests per second
- Static IP support
- Preserve source IP

**Auto Scaling:**

*Target Tracking:* Scale based on metric target (CPU, memory, ALB request count)

*Step Scaling:* Scale based on CloudWatch alarms

*Scheduled Scaling:* Time-based scaling

*Cluster Auto Scaling (CAS):* For EC2 launch type, scales EC2 instances based on capacity providers

**Capacity Providers:**
- Define infrastructure for tasks (Fargate, Fargate Spot, EC2 Auto Scaling groups)
- Capacity Provider Strategy: Distribute tasks across providers
- Fargate Spot: Up to 70% discount, can be interrupted
- Managed scaling for EC2 capacity providers

### Advanced Features

**ECS Anywhere:**
- Run ECS on on-premises servers or other clouds
- Hybrid deployments
- Same ECS APIs and tooling
- Use SSM agent for communication

**Task Placement (EC2 Launch Type):**

*Strategies:*
- binpack: Minimize number of instances (cost optimization)
- random: Random placement
- spread: Spread across specified values (AZ, instance ID)

*Constraints:*
- distinctInstance: Each task on different instance
- memberOf: Cluster query language for custom rules

**Secrets Management:**
- Retrieve secrets from Secrets Manager or Parameter Store
- Inject into containers as environment variables
- Encrypted in transit and at rest

**Volumes:**

*Bind Mounts:* Share data between containers in same task

*Docker Volumes:* Managed by Docker volume drivers

*EFS Integration:* Persistent shared storage across tasks and AZs. Works with both Fargate and EC2.

**Deployment Configurations:**

*Rolling Update:* Default, gradually replace tasks. Configure minimum healthy percent and maximum percent.

*Blue/Green:* Uses CodeDeploy, traffic shifted to new task set. All-at-once, linear, or canary deployment.

*Circuit Breaker:* Automatically roll back failed deployments

### Exam-Focused Scenarios

**When to use ECS:**
- Docker containers without Kubernetes complexity
- AWS-native container orchestration
- Integration with AWS services (ELB, CloudWatch, IAM)
- Cost-effective container management

**Fargate vs EC2 Launch Type:**
- Fargate: Unpredictable workloads, minimal operations, rapid scaling, no Reserved Instance benefits
- EC2: Consistent workloads, cost optimization with RIs/Savings Plans, need instance access, compliance requirements

**High Availability:**
- Multi-AZ task placement
- Service auto-scaling
- Health checks with ELB
- Task placement spread strategy

**Security Best Practices:**
- Use awsvpc network mode for isolation
- Task-level IAM roles (least privilege)
- Secrets Manager for sensitive data
- Private subnets with NAT for outbound access
- ECR image scanning

## 4. Amazon EKS (Elastic Kubernetes Service)

### Core Concepts

**Architecture:**

*Control Plane:*
- Managed by AWS (Kubernetes masters)
- Multi-AZ deployment automatically
- Automatic version updates and patching
- Integrated with AWS services

*Data Plane (Worker Nodes):*
- Managed Node Groups: AWS manages EC2 instances, automatic updates
- Self-Managed Nodes: You manage EC2 instances completely
- Fargate: Serverless pods, no node management

**Node Types:**

*Managed Node Groups:*
- Auto Scaling group of EC2 instances
- Automated updates and patching
- Launch template support
- Can use Spot instances

*Self-Managed Nodes:*
- Full control over EC2 instances
- Use when need specific AMIs or instance configurations
- More operational overhead

*Fargate Pods:*
- Serverless, no nodes to manage
- Each pod runs in isolated environment
- Pay per pod resources
- Good for batch jobs, bursty workloads

**Networking:**

*VPC CNI Plugin:*
- AWS native networking for pods
- Each pod gets IP from VPC CIDR
- Security groups for pods
- Can exhaust IP addresses (plan CIDR carefully)

*Secondary CIDR:*
- Add additional CIDR blocks to VPC for more pod IPs
- Prevents IP exhaustion

*Security Groups for Pods:*
- Apply SGs directly to pods
- More granular network control
- Requires VPC CNI plugin

**Storage:**

*EBS CSI Driver:* Persistent volumes using EBS, single AZ

*EFS CSI Driver:* Shared persistent volumes, multi-AZ, ReadWriteMany

*FSx for Lustre CSI Driver:* High-performance computing workloads

*Persistent Volumes (PV) and Claims (PVC):* Standard Kubernetes storage abstraction

**IAM Integration:**

*IAM Roles for Service Accounts (IRSA):*
- Map Kubernetes service accounts to IAM roles
- Fine-grained permissions for pods
- Uses OIDC identity provider
- Better than node-level IAM roles

*Cluster Roles:*
- Cluster Creator: Has admin access via IAM
- Additional IAM users/roles: Add via aws-auth ConfigMap

**Add-ons:**

*Amazon VPC CNI:* Networking plugin

*CoreDNS:* DNS for service discovery

*kube-proxy:* Network proxy on each node

*Amazon EBS CSI Driver:* EBS volumes

*AWS Load Balancer Controller:* Provisions ALB/NLB for Kubernetes services

### Advanced Features

**Auto Scaling:**

*Cluster Autoscaler:*
- Adjusts number of nodes in cluster
- Watches for unschedulable pods
- Integrates with Auto Scaling groups

*Horizontal Pod Autoscaler (HPA):*
- Scales pods based on CPU/memory or custom metrics
- Built-in Kubernetes feature

*Vertical Pod Autoscaler (VPA):*
- Adjusts pod resource requests/limits
- Helps right-size applications

*Karpenter:*
- AWS open-source autoscaler
- More efficient than Cluster Autoscaler
- Provisions right-sized nodes just-in-time
- Consolidates nodes to reduce costs

**Security:**

*Secrets Management:*
- Kubernetes Secrets (base64 encoded)
- AWS Secrets Manager integration via CSI driver
- Parameter Store integration

*Pod Security:*
- Pod Security Standards (PSS)
- Network Policies for pod-to-pod communication
- Security groups for pods

*Image Security:*
- ECR image scanning
- Admission controllers to enforce policies
- Image pull secrets for private registries

**Observability:**

*Container Insights:*
- CloudWatch integration for metrics
- Performance monitoring dashboard
- Log aggregation

*Prometheus:*
- Metrics collection and alerting
- Amazon Managed Service for Prometheus

*Fluent Bit:*
- Log forwarding to CloudWatch Logs

**Multi-Tenancy:**

*Namespaces:* Logical cluster isolation

*Resource Quotas:* Limit resources per namespace

*Network Policies:* Control traffic between namespaces

*RBAC:* Role-based access control

### EKS Anywhere

- Run Kubernetes on-premises
- Same EKS APIs and tooling
- Enterprise support from AWS
- Hybrid cloud deployments

### Exam-Focused Scenarios

**When to use EKS:**
- Already using Kubernetes
- Multi-cloud strategy (portability)
- Complex orchestration requirements
- Large-scale microservices
- Need Kubernetes ecosystem tools

**ECS vs EKS:**
- ECS: AWS-native, simpler, less overhead, good for AWS-only workloads
- EKS: Standard Kubernetes, portable, steeper learning curve, ecosystem

**Cost Optimization:**
- Use Fargate for unpredictable workloads
- Managed Node Groups with Spot instances
- Karpenter for efficient node provisioning
- Horizontal Pod Autoscaler to scale pods

**High Availability:**
- Multi-AZ node groups
- EFS for shared persistent storage across AZs
- Pod Disruption Budgets
- Health checks and readiness probes

## 5. AWS Fargate

### Core Concepts

**What is Fargate:**
- Serverless compute engine for containers
- Works with both ECS and EKS
- No servers to manage
- Pay per task/pod resources (vCPU and memory)

**Task/Pod Resources:**

*CPU and Memory Combinations:*
- 0.25 vCPU: 512 MB to 2 GB memory
- 0.5 vCPU: 1 GB to 4 GB memory
- 1 vCPU: 2 GB to 8 GB memory
- 2 vCPU: 4 GB to 16 GB memory
- 4 vCPU: 8 GB to 30 GB memory
- 8 vCPU: 16 GB to 60 GB memory
- 16 vCPU: 32 GB to 120 GB memory

*Ephemeral Storage:*
- Default: 20 GB
- Configurable: 20 GB to 200 GB
- Charged for storage over 20 GB

**Fargate Networking:**

*awsvpc Mode (Required):*
- Each task gets ENI with private IP
- Security groups attached to task (not instance)
- Task-level network isolation
- Requires subnets and security groups

*Public IP Assignment:*
- Can assign public IP for internet access
- Or use NAT Gateway in private subnet

**Platform Versions:**
- Fargate releases platform versions with updates
- Can specify version in task definition
- LATEST always points to newest recommended version
- Can pin to specific version for stability

### Fargate with ECS

**Task Definitions:**
- Must use awsvpc network mode
- Requires task execution role for ECR and CloudWatch
- Can use task role for application permissions
- Support for EFS volumes

**Service Configuration:**
- Auto-scaling based on CloudWatch metrics
- Integration with ALB/NLB
- Service discovery with Cloud Map
- Rolling deployments

### Fargate with EKS

**Fargate Profiles:**
- Define which pods run on Fargate
- Based on namespace and labels
- One profile per cluster namespace combination

**Pod Execution Role:**
- IAM role for kubelet to pull images, write logs
- Automatically created or specify custom role

**Limitations:**
- No DaemonSets
- No privileged containers
- No HostNetwork or HostPort
- No GPU support

### Advanced Features

**Fargate Spot:**

*ECS Fargate Spot:*
- Up to 70% discount
- Tasks can be interrupted with 2-minute notice
- Use for fault-tolerant workloads
- Configure via capacity providers

*EKS Fargate Spot:*
- Currently not available (only EC2 Spot nodes)

**Graviton Support:**
- Run ARM-based workloads
- Better price-performance
- Requires ARM-compatible images

**EFS Integration:**
- Persistent storage for stateful applications
- Shared across tasks/pods
- Multi-AZ access

**Observability:**
- Logs to CloudWatch Logs
- Container Insights for metrics
- AWS X-Ray for tracing
- Firelens for log routing

### Exam-Focused Scenarios

**When to use Fargate:**
- Don't want to manage infrastructure
- Unpredictable or bursty traffic
- Microservices with varying resource needs
- Faster time to market
- Development/test environments

**Fargate vs EC2:**
- Fargate: Serverless, elastic, less operational overhead, pay per task
- EC2: More control, Reserved Instances for cost savings, consistent workloads, special hardware needs

**Cost Considerations:**
- More expensive per hour than EC2 for sustained workloads
- No upfront costs or long-term commitments
- Right-size task CPU/memory to avoid over-provisioning
- Use Fargate Spot for fault-tolerant workloads

**Networking Best Practices:**
- Use private subnets with NAT Gateway
- Security groups at task level for isolation
- VPC endpoints for AWS services to avoid NAT costs
- Multiple AZs for high availability

## 6. AWS Batch

### Core Concepts

**What is AWS Batch:**
- Fully managed batch processing service
- Automatically provisions compute resources
- Runs batch jobs at any scale
- No batch software to install/manage

**Core Components:**

*Jobs:*
- Unit of work submitted to AWS Batch
- Shell script, executable, or Docker container
- Single job or job array (thousands of jobs)

*Job Definitions:*
- Blueprint for jobs (like task definition)
- Specifies: container image, vCPU, memory, IAM role, environment variables, volumes
- Versioned and immutable

*Job Queues:*
- Jobs wait here before running
- Can have multiple queues with priorities
- Associated with one or more compute environments

*Compute Environments:*
- Managed or Unmanaged
- Defines compute resources (instance types, vCPUs, Spot/On-Demand)

**Compute Environment Types:**

*Managed:*
- AWS provisions and manages instances
- Can use Fargate or EC2
- Auto-scaling based on job queue
- Specify instance types, vCPU limits, Spot/On-Demand

*Unmanaged:*
- You manage ECS cluster
- More control but more operational overhead

**Fargate vs EC2 in Batch:**

*Fargate:*
- Serverless, no instance management
- Suitable for smaller jobs (16 vCPU, 120 GB memory max)
- Faster job start times
- No persistent storage

*EC2:*
- More instance type options
- GPU instances available
- Can use Spot for cost savings
- Better for large, long-running jobs
- Can use EFS, FSx

### Advanced Features

**Job Dependencies:**
- Sequential dependencies: Job B runs after Job A completes
- N-to-N dependencies: Jobs run after multiple parent jobs
- Helps orchestrate complex workflows

**Array Jobs:**
- Submit single job that spawns child jobs
- Index-based parameterization
- Great for parameter sweep workloads
- Can have dependencies between array indices

**Multi-Node Parallel Jobs:**
- Tightly-coupled workloads across multiple nodes
- MPI (Message Passing Interface) support
- High-performance computing (HPC)
- Only supported on EC2, not Fargate

**Retry Strategies:**
- Automatic retry on failure
- Configure number of attempts
- Can use different strategies for different exit codes

**Scheduling:**
- Job priorities within queue
- Fair share scheduling across users/groups
- Can use EventBridge for time-based scheduling

**Resource Management:**

*Allocation Strategy:*
- BEST_FIT: Minimizes cost, uses smallest instance that fits
- BEST_FIT_PROGRESSIVE: Similar to BEST_FIT but scales up gradually
- SPOT_CAPACITY_OPTIMIZED: For Spot, selects instances least likely to be interrupted

*Resource Requirements:*
- vCPU, memory requirements
- GPU requirements
- Can specify instance types or families

### Integration with Other Services

**Amazon ECS:**
- Batch leverages ECS for container orchestration
- Jobs run as ECS tasks

**CloudWatch:**
- Job state changes trigger CloudWatch Events
- Logs from jobs go to CloudWatch Logs
- Metrics for monitoring

**Step Functions:**
- Orchestrate complex workflows
- Combine Batch with Lambda, ECS tasks
- Error handling and retry logic

**S3:**
- Input data from S3
- Output results to S3
- Trigger Batch jobs with S3 events

### Exam-Focused Scenarios

**When to use AWS Batch:**
- Large-scale batch processing
- Data analysis, ETL jobs
- High-performance computing
- Financial modeling, risk analysis
- Media transcoding at scale
- Scientific simulations

**Cost Optimization:**
- Use Spot instances (up to 90% savings)
- BEST_FIT allocation strategy
- Right-size job resource requirements
- Use Fargate for smaller, shorter jobs

**High Availability:**
- Multi-AZ compute environments
- Job retry strategies
- CloudWatch monitoring and alarms
- Automatic instance replacement

**Performance Optimization:**
- Use instance types matched to workload
- Array jobs for parallelization
- Multi-node jobs for HPC workloads
- EFS for shared data across jobs

## 7. AWS Elastic Beanstalk

### Core Concepts

**What is Elastic Beanstalk:**
- Platform as a Service (PaaS)
- Deploy and manage applications without infrastructure management
- Supports: Go, Java, .NET, Node.js, PHP, Python, Ruby, Docker
- Free service (pay only for resources)

**Architecture Components:**

*Application:* Logical container for environment, versions, configurations

*Application Version:* Specific labeled iteration of deployable code

*Environment:* Collection of AWS resources running application version (EC2, ELB, Auto Scaling, RDS)

*Environment Tier:*
- Web Server Tier: Handles HTTP requests, uses ELB
- Worker Tier: Processes background tasks, uses SQS

**Environment Types:**

*Single Instance:* One EC2 instance, Elastic IP. Good for development.

*Load Balanced:* Auto Scaling, ELB, multi-AZ. Good for production.

**Platforms:**

*Preconfigured:* Managed platform for specific language/framework

*Custom Platform:* Create custom AMI with Packer

*Docker:* Single or Multi-container Docker

### Deployment Options

**Deployment Policies:**

*All at Once:*
- Fastest deployment
- Service interruption during deployment
- No additional cost
- Use for dev/test environments

*Rolling:*
- Deploys in batches
- Reduced capacity during deployment
- No additional cost
- Configurable batch size

*Rolling with Additional Batch:*
- Deploys in batches but maintains full capacity
- Additional instances launched first
- Small additional cost during deployment
- Zero downtime

*Immutable:*
- New Auto Scaling group with fresh instances
- Full capacity maintained
- Quickly rollback by terminating new ASG
- Highest cost during deployment
- Zero downtime, safest for production

*Traffic Splitting:*
- Canary testing
- Forward percentage of traffic to new version
- Monitor and roll back if needed
- Based on Application Load Balancer

*Blue/Green:*
- Not built into Beanstalk
- Manual: Create new environment, swap URLs
- Zero downtime
- Full environment separation

### Configuration

**.ebextensions:**
- Configuration files in .ebextensions/ folder
- YAML or JSON format
- Customize resources, packages, services
- Add custom resources (DynamoDB, S3, etc.)

**Environment Properties:**
- Environment variables for application
- Can reference from code
- Useful for configuration without code changes

**Managed Updates:**
- Automatic platform updates
- Scheduled maintenance windows
- Managed platform version updates

### Advanced Features

**RDS Integration:**

*Coupled:* RDS created with environment, deleted when environment terminates. Good for dev/test.

*Decoupled:* RDS created separately, connection info via environment properties. Good for production.

**HTTPS:**
- Load balancer SSL termination
- Upload certificate to IAM or use ACM
- Configure in Beanstalk console

**Custom Domain:**
- Create CNAME in Route 53 to Beanstalk ELB
- Or use A record with Alias

**Worker Environments:**
- SQS queue automatically created
- Cron jobs for periodic tasks (cron.yaml)
- Processes messages from queue
- Dead letter queue for failed messages

**Docker Support:**

*Single Container:* Dockerfile or Dockerrun.aws.json

*Multi-Container:* Uses ECS, Dockerrun.aws.json v2 with task definition

**.ebignore:**
- Similar to .gitignore
- Exclude files from deployment bundle

**Lifecycle Policies:**
- Automatically delete old application versions
- Based on count or age
- Prevents hitting version quota

**Monitoring:**
- CloudWatch metrics for environment
- Health monitoring and dashboard
- Enhanced health reporting
- Logs available in S3 or CloudWatch

### Exam-Focused Scenarios

**When to use Elastic Beanstalk:**
- Rapid application deployment
- Developer wants to focus on code, not infrastructure
- Standard web applications
- Quick prototyping and testing
- Reduced operational complexity

**Migration Strategies:**
- Migrate from on-premises to AWS quickly
- Replicate existing application architecture
- Use .ebextensions to customize environment

**High Availability:**
- Load balanced environment with multi-AZ
- Decouple RDS for persistence
- Use Immutable or Blue/Green deployments
- Health-based monitoring and auto-recovery

**Cost Optimization:**
- Right-size instance types
- Use Reserved Instances for predictable workloads
- Auto Scaling based on metrics
- Delete unused environments and versions

**Limitations:**
- Less control than managing infrastructure directly
- Limited customization compared to EC2
- Specific platform versions
- Not suitable for all application types (complex, highly customized)

## 8. AWS Outposts

### Core Concepts

**What is AWS Outposts:**
- Fully managed service extending AWS infrastructure to on-premises
- AWS-designed hardware installed at customer location
- Runs native AWS services locally
- Hybrid cloud solution

**Outposts Form Factors:**

*Outposts Rack:*
- 42U rack
- Starts at 1 rack, can scale to 96 racks
- Full range of AWS services
- For large deployments (datacenters, co-location facilities)

*Outposts Server:*
- 1U or 2U server
- Subset of AWS services
- For smaller locations (retail stores, branch offices, hospitals)
- More cost-effective for edge use cases

**Available Services:**
- **Compute:** EC2, ECS, EKS
- **Storage:** EBS, S3 on Outposts
- **Database:** RDS (MySQL, PostgreSQL)
- **Analytics:** EMR
- **Container:** ECR (local image caching)
- **Networking:** VPC, ALB, NLB

### Architecture

**Connectivity:**

*Service Link:* VPN connection to AWS region for management

*Local Gateway (LGW):* Routes traffic between Outposts and on-premises network

*VPC Extension:* Subnets span from region to Outposts

**Networking:**

*Outposts Subnet:* Subnet created in Outposts, part of VPC in region

*Local Gateway Route Table:* Controls routing from Outposts to on-premises

*Customer-Owned IP (CoIP):* Use on-premises IP addresses for resources

### Use Cases

**Data Residency:**
- Keep data locally for compliance
- Process sensitive data on-premises
- Meet regulatory requirements

**Low Latency:**
- Applications requiring single-digit millisecond latency
- Real-time processing at the edge
- Industrial IoT, manufacturing

**Local Data Processing:**
- Process data before sending to cloud
- Reduce bandwidth costs
- Edge computing scenarios

**Migration:**
- Hybrid cloud architecture during migration
- Gradual transition to cloud
- Maintain on-premises workloads

### Management

**Fully Managed:**
- AWS installs, manages, patches hardware
- Monitoring and maintenance by AWS
- Same APIs and tools as AWS regions

**Integration with AWS Region:**
- Managed from AWS region
- CloudFormation, Systems Manager support
- CloudWatch for monitoring
- IAM for access control

### Exam-Focused Scenarios

**When to use Outposts:**
- Need AWS services with data residency requirements
- Low-latency requirements for on-premises applications
- Hybrid cloud strategy
- Gradual cloud migration
- Disconnected or remote locations (Outposts Server)

**Outposts vs Other Hybrid Solutions:**
- **Outposts:** Full AWS services on-premises, managed by AWS
- **Local Zones:** AWS infrastructure in metro areas, managed by AWS
- **Wavelength:** 5G edge computing, managed by AWS
- **Snow Family:** Data transfer and edge computing, portable
- **Direct Connect:** Network connection, doesn't provide compute

**Cost Considerations:**
- Significant upfront investment
- 3-year commitment typically
- Includes hardware, delivery, installation, maintenance
- Pay for EC2 instances, EBS volumes similar to region

**High Availability:**
- Outposts can fail (single point of failure for rack)
- Design for failure: critical workloads in region
- Backup to region using S3, snapshot to region EBS
- Application resilience across Outposts and region

---

## Summary Comparison Table

| Service | Use Case | Management | Pricing | Scaling |
|---------|----------|------------|---------|---------|
| **EC2** | Full control, any workload | Self-managed | Per second, multiple options | Manual/Auto Scaling |
| **Lambda** | Event-driven, short tasks | Fully managed | Per request + duration | Automatic |
| **ECS** | Docker containers, AWS-native | Managed orchestration | EC2 or Fargate pricing | Auto Scaling |
| **EKS** | Kubernetes workloads | Managed control plane | Control plane + worker costs | Kubernetes autoscalers |
| **Fargate** | Serverless containers | Fully managed | Per task resources | Automatic |
| **Batch** | Large-scale batch jobs | Fully managed | Underlying compute costs | Automatic |
| **Beanstalk** | Quick web app deployment | Managed platform | Underlying resource costs | Auto Scaling |
| **Outposts** | On-premises AWS services | Fully managed by AWS | 3-year commitment | Manual scaling |

## Key Exam Tips

1. **Know the tradeoffs:** Control vs. management, cost vs. convenience, flexibility vs. simplicity

2. **Right tool for the job:** EC2 for control, Lambda for events, ECS/EKS for containers, Batch for batch processing, Beanstalk for quick deployment, Outposts for on-premises

3. **Cost optimization:** RIs/Savings Plans for EC2, Spot for fault-tolerant workloads, right-sizing, auto-scaling

4. **High availability:** Multi-AZ, auto-scaling, health checks, failover strategies

5. **Security:** IAM roles, security groups, encryption, least privilege, network isolation

6. **Integration patterns:** How services work together (Lambda + API Gateway, ECS + ALB, Batch + S3)

7. **Limitations:** Know the limits (Lambda 15 min, Fargate resource caps, EKS Fargate restrictions)

This comprehensive guide covers all compute services from an SAP-C02 exam perspective. Focus on understanding when to use each service, their integration patterns, and architectural best practices.