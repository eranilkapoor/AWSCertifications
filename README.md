# AWSCertifications
All details related to AWS Certifications

# AWS Certified Developer Associate

The AWS Certified Developer - Associate (DVA-C02) exam has a pass or fail designation. The exam is scored against a minimum standard established by AWS professionals who follow certification industry best practices and guidelines.
Your results for the exam are reported as a scaled score of 100–1,000. The minimum passing score is 720. Your score shows how you performed on the exam as a whole and whether you passed. Scaled scoring models help equate scores across multiple exam forms that might have slightly different difficulty levels.
Your score report could contain a table of classifications of your performance at each section level. The exam uses a compensatory scoring model, which means that you do not need to achieve a passing score in each section. You need to pass only the overall exam.
Each section of the exam has a specific weighting, so some sections have more questions than other sections have. The table of classifications contains general information that highlights your strengths and weaknesses. Use caution when you interpret section-level feedback

* Content outline
    This exam guide includes weightings, content domains, and tasks for the exam. This guide does not provide a comprehensive list of the content on the exam. However, additional context for each task is available to help you prepare for the exam.
    The exam has the following content domains and weightings:
    - Content Domain 1: Development with AWS Services (32% of scored content)
    - Content Domain 2: Security (26% of scored content)
    - Content Domain 3: Deployment (24% of scored content)
    - Content Domain 4: Troubleshooting and Optimization (18% of scored content)

* Content Domain 1: Development with AWS Services
    - Task 1: Develop code for applications hosted on AWS.
        - Knowledge of:
            - Architectural patterns (for example, event-driven, microservices, monolithic, choreography, orchestration, fanout)
            - Idempotency
            - Differences between stateful and stateless concepts
            - Differences between tightly coupled and loosely coupled components
            - Fault-tolerant design patterns (for example, retries with exponential backoff and jitter, dead-letter queues)
            - Differences between synchronous and asynchronous patterns
        - Skills in:
            - Creating fault-tolerant and resilient applications in a programming language (for example, Java, C#, Python, JavaScript, TypeScript, Go)
            - Creating, extending, and maintaining APIs (for example, response/request transformations, enforcing validation rules, overriding status codes)
            - Writing and running unit tests in development environments (for example, using AWS Serverless Application Model [AWS SAM])
            - Writing code to use messaging services
            - Writing code that interacts with AWS services by using APIs and AWS SDKs
            - Handling data streaming by using AWS services
    - Task 2: Develop code for AWS Lambda.
        - Knowledge of:
            - Event source mapping
            - Stateless applications
            - Unit testing
            - Event-driven architecture
            - Scalability
            - The access of private resources in VPCs from Lambda code
        - Skills in:
            - Configuring Lambda functions by defining environment variables and parameters (for example, memory, concurrency, timeout, runtime, handler, layers, extensions, triggers, destinations)
            - Handling the event lifecycle and errors by using code (for example, Lambda Destinations, dead-letter queues)
            - Writing and running test code by using AWS services and tools
            - Integrating Lambda functions with AWS services
            - Tuning Lambda functions for optimal performance
    - Task 3: Use data stores in application development.
        - Knowledge of:
            - Relational and non-relational databases
            - Create, read, update, and delete (CRUD) operations
            - High-cardinality partition keys for balanced partition access
            - Cloud storage options (for example, file, object, databases)
            - Database consistency models (for example, strongly consistent, eventually consistent)
            - Differences between query and scan operations
            - Amazon DynamoDB keys and indexing
            - Caching strategies (for example, write-through, read-through, lazy loading, TTL)
            - Amazon Simple Storage Service (Amazon S3) tiers and lifecycle management
            - Differences between ephemeral and persistent data storage patterns
        - Skills in:
            - Serializing and deserializing data to provide persistence to a data store
            - Using, managing, and maintaining data stores
            - Managing data lifecycles
            - Using data caching services
* Content Domain 2: Security
    - Task 1: Implement authentication and/or authorization for applications and AWS services.
        - Knowledge of:
            - Identity federation (for example, Security Assertion Markup Language [SAML], OpenID Connect [OIDC], Amazon Cognito)
            - Bearer tokens (for example, JSON Web Token [JWT], OAuth, AWS Security Token Service [AWS STS])
            - The comparison of user pools and identity pools in Amazon Cognito
            - Resource-based policies, service policies, and principal policies
            - Role-based access control (RBAC)
            - Application authorization that uses ACLs
            - The principle of least privilege
            - Differences between AWS managed policies and customer-managed policies
            - Identity and access management
        - Skills in:
            - Using an identity provider to implement federated access (for example, Amazon Cognito, AWS Identity and Access Management [IAM])
            - Securing applications by using bearer tokens
            - Configuring programmatic access to AWS
            - Making authenticated calls to AWS services
            - Assuming an IAM role
            - Defining permissions for principals
    - Task 2: Implement encryption by using AWS services.
        - Knowledge of:
            - Encryption at rest and in transit
            - Certificate management (for example, AWS Private Certificate Authority)
            - Key protection (for example, key rotation)
            - Differences between client-side encryption and server-side encryption
            - Differences between AWS managed and customer managed AWS Key Management Service (AWS KMS) keys
        - Skills in:
            - Using encryption keys to encrypt or decrypt data
            - Generating certificates and SSH keys for development purposes
            - Using encryption across account boundaries
            - Enabling and disabling key rotation
    - Task 3: Manage sensitive data in application code.
        - Knowledge of:
            - Data classification (for example, personally identifiable information [PII], protected health information [PHI])
            - Environment variables
            - Secrets management (for example, AWS Secrets Manager, AWS Systems Manager Parameter Store)
            - Secure credential handling
        - Skills in:
            - Encrypting environment variables that contain sensitive data
            - Using secret management services to secure sensitive data
            - Sanitizing sensitive data
* Content Domain 3: Deployment
    - Task 1: Prepare application artifacts to be deployed to AWS.
        - Knowledge of:
            - Ways to access application configuration data (for example, AWS AppConfig, Secrets Manager, Parameter Store)
            - Lambda deployment packaging, layers, and configuration options
            - Git-based version control tools (for example, Git)
            - Container images
        - Skills in:
            - Managing the dependencies of the code module (for example, environment variables, configuration files, container images) within the package
            - Organizing files and a directory structure for application deployment
            - Using code repositories in deployment environments
            - Applying application requirements for resources (for example, memory, cores)
    - Task 2: Test applications in development environments.
        - Knowledge of:
            - Features in AWS services that perform application deployment
            - Integration testing that uses mock endpoints
            - Lambda versions and aliases
        - Skills in:
            - Testing deployed code by using AWS services and tools
            - Performing mock integration for APIs and resolving integration dependencies
            - Testing applications by using development endpoints (for example, configuring stages in Amazon API Gateway)
            - Deploying application stack updates to existing environments (for example, deploying an AWS SAM template to a different staging environment)
    - Task 3: Automate deployment testing.
        - Knowledge of:
            - API Gateway stages
            - Branches and actions in the continuous integration and continuous delivery (CI/CD) workflow
            - Automated software testing (for example, unit testing, mock testing)
        - Skills in:
            - Creating application test events (for example, JSON payloads for testing Lambda, API Gateway, AWS SAM resources)
            - Deploying API resources to various environments
            - Creating application environments that use approved versions for integration testing (for example, Lambda aliases, container image tags, AWS Amplify branches, AWS Copilot environments)
            - Implementing and deploying infrastructure as code (IaC) templates (for example, AWS SAM templates, AWS CloudFormation templates)
            - Managing environments in individual AWS services (for example, differentiating between development, test, and production in API Gateway)
    - Task 4: Deploy code by using AWS CI/CD services.
        - Knowledge of:
            - Git-based version control tools (for example, Git)
            - Manual and automated approvals in AWS CodePipeline
            - Access application configurations from AWS AppConfig and Secrets Manager
            - CI/CD workflows that use AWS services
            - Application deployment that uses AWS services and tools (for example, CloudFormation, AWS Cloud Development Kit [AWS CDK], AWS SAM, AWS CodeArtifact, AWS Copilot, Amplify, Lambda)
            - Lambda deployment packaging options
            - API Gateway stages and custom domains
            - Deployment strategies (for example, canary, blue/green, rolling)
        - Skills in:
            - Updating existing IaC templates (for example, AWS SAM templates, CloudFormation templates)
            - Managing application environments by using AWS services
            - Deploying an application version by using deployment strategies
            - Committing code to a repository to invoke build, test, and deployment actions
            - Using orchestrated workflows to deploy code to different environments
            - Performing application rollbacks by using existing deployment strategies
            - Using labels and branches for version and release management
            - Using existing runtime configurations to create dynamic deployments (for example, using staging variables from API Gateway in Lambda functions)
* Content Domain 4: Troubleshooting and Optimization
    - Task 1: Assist in a root cause analysis.
        - Knowledge of:
            - Logging and monitoring systems
            - Languages for log queries (for example, Amazon CloudWatch Logs Insights)
            - Data visualizations
            - Code analysis tools
            - Common HTTP error codes
            - Common exceptions generated by SDKs
            - Service maps in AWS X-Ray
        - Skills in:
            - Debugging code to identify defects
            - Interpreting application metrics, logs, and traces
            - Querying logs to find relevant data
            - Implementing custom metrics (for example, CloudWatch embedded metric format [EMF])
            - Reviewing application health by using dashboards and insights
            - Troubleshooting deployment failures by using service output logs
    - Task 2: Instrument code for observability.
        - Knowledge of:
            - Distributed tracing
            - Differences between logging, monitoring, and observability
            - Structured logging
            - Application metrics (for example, custom, embedded, built-in)
        - Skills in:
            - Implementing an effective logging strategy to record application behavior and state
            - Implementing code that emits custom metrics
            - Adding annotations for tracing services
            - Implementing notification alerts for specific actions (for example, notifications about quota limits or deployment completions)
            - Implementing tracing by using AWS services and tools
    - Task 3: Optimize applications by using AWS services and features.
        - Knowledge of:
            - Caching
            - Concurrency
            - Messaging services (for example, Amazon Simple Queue Service [Amazon SQS], Amazon Simple Notification Service [Amazon SNS])
        - Skills in:
            - Profiling application performance
            - Determining minimum memory and compute power for an application
            - Using subscription filter policies to optimize messaging
            - Caching content based on request headers

## The following list contains AWS services and features that are in scope for the exam. This list is non-exhaustive and is subject to change. AWS offerings appear in categories that align with the offerings’ primary functions:-

* Analytics:
    - Amazon Athena
    - Amazon Kinesis
    - Amazon OpenSearch Service
* Application Integration:
    - AWS AppSync
    - Amazon EventBridge
    - Amazon Simple Notification Service (Amazon SNS)
    - Amazon Simple Queue Service (Amazon SQS)
    - AWS Step Functions
* Compute:
    - Amazon Elastic Compute Cloud (Amazon EC2)
    - AWS Elastic Beanstalk
    - AWS Lambda
    - AWS Serverless Application Model (AWS SAM)
* Containers:
    - AWS Copilot
    - Amazon Elastic Container Registry (Amazon ECR)
    - Amazon Elastic Container Service (Amazon ECS)
    - Amazon Elastic Kubernetes Service (Amazon EKS)
* Database:
    - Amazon Aurora
    - Amazon DynamoDB
    - Amazon ElastiCache
    - Amazon MemoryDB
    - Amazon Relational Database Service (Amazon RDS)
* Developer Tools:
    - AWS Amplify
    - AWS CloudShell
    - AWS CodeArtifact
    - AWS CodeBuild
    - AWS CodeDeploy
    - Amazon CodeGuru
    - AWS CodePipeline
    - AWS X-Ray
* Management and Governance:
    - AWS AppConfig
    - AWS Cloud Development Kit (AWS CDK)
    - AWS CloudFormation
    - AWS CloudTrail
    - Amazon CloudWatch
    - Amazon CloudWatch Logs
    - AWS Command Line Interface (AWS CLI)
    - AWS Systems Manager
* Networking and Content Delivery:
    - Amazon API Gateway
    - Amazon CloudFront
    - Elastic Load Balancing (ELB)
    - Amazon Route 53
    - Amazon Virtual Private Cloud (Amazon VPC)
* Security, Identity, and Compliance:
    - AWS Certificate Manager (ACM)
    - Amazon Cognito
    - AWS Identity and Access Management (IAM)
    - AWS Key Management Service (AWS KMS)
    - AWS Private Certificate Authority
    - AWS Secrets Manager
    - AWS Security Token Service (AWS STS)
    - AWS WAF
* Storage:
    - Amazon Elastic Block Store (Amazon EBS)
    - Amazon Elastic File System (Amazon EFS)
    - Amazon Simple Storage Service (Amazon S3)
    - Amazon S3 Glacier



=====================================================================================================
# AWS Certified Solutions Architect Professional - Complete 2026 Syllabus & 90-Day Study Plan

## Current Exam Overview (SAP-C02)

The AWS Certified Solutions Architect - Professional exam validates advanced technical skills in designing distributed systems on AWS. The current exam version is SAP-C02.

**Exam Details:**
- Duration: 180 minutes (3 hours)
- Format: 75 questions (multiple choice and multiple response)
- Passing Score: 750/1000
- Cost: $300 USD
- Validity: 3 years
- Prerequisites: Recommended to have Associate-level certification and hands-on experience

## Complete Exam Syllabus

The exam covers four main domains with the following weightage:

**Domain 1: Design Solutions for Organizational Complexity (26%)**
- Cross-account AWS architectures using AWS Organizations, Service Control Policies (SCPs), and resource sharing
- Multi-account strategies with AWS Control Tower and Landing Zone
- Hybrid connectivity solutions including AWS Direct Connect, VPN, Transit Gateway
- Network segmentation and multi-VPC architectures
- Cost optimization and governance across organizations
- Centralized logging and monitoring with CloudWatch, CloudTrail, Config

**Domain 2: Design for New Solutions (29%)**
- Security and compliance requirements implementation
- Designing reliable and resilient architectures with multi-AZ and multi-region strategies
- High-performance computing and storage solutions
- Data migration strategies using AWS DataSync, Database Migration Service, Snow family
- Application modernization patterns including microservices, containers, serverless
- Solution design based on business requirements and SLAs

**Domain 3: Continuous Improvement for Existing Solutions (25%)**
- Performance optimization for compute, storage, database, and networking
- Cost optimization strategies using Reserved Instances, Savings Plans, Spot Instances
- Operational excellence improvements through automation and Infrastructure as Code
- Security posture enhancement with AWS Security Hub, GuardDuty, Macie
- Scalability improvements and auto-scaling configurations
- Disaster recovery improvements and backup strategies

**Domain 4: Accelerate Workload Migration and Modernization (20%)**
- Migration strategies (6 Rs: Rehost, Replatform, Refactor, Repurchase, Retire, Retain)
- Application Migration Service and Server Migration Service
- Database migration patterns and tools
- Large-scale data transfer solutions
- Legacy application modernization approaches
- Container and serverless migration patterns

## Key AWS Services to Master

**Compute:** EC2, Lambda, ECS, EKS, Fargate, Batch, Elastic Beanstalk, Outposts

**Storage:** S3, EBS, EFS, FSx, Storage Gateway, DataSync, Snow Family

**Database:** RDS, Aurora, DynamoDB, ElastiCache, Neptune, DocumentDB, Redshift, QLDB

**Networking:** VPC, Direct Connect, Transit Gateway, Route 53, CloudFront, Global Accelerator, VPN, PrivateLink

**Security:** IAM, KMS, Secrets Manager, Certificate Manager, WAF, Shield, GuardDuty, Security Hub, Macie

**Management:** CloudFormation, Systems Manager, Organizations, Control Tower, Config, CloudTrail, CloudWatch, Trusted Advisor

**Analytics:** Athena, EMR, Kinesis, Glue, Lake Formation, QuickSight

**Integration:** SQS, SNS, EventBridge, Step Functions, API Gateway, AppSync

**Migration:** Migration Hub, Application Migration Service, Database Migration Service, DataSync

## 90-Day Learning Plan

### Weeks 1-3: Foundation and Core Services (Days 1-21)

**Week 1: Core Architecture & Compute**
- Days 1-2: Review AWS Well-Architected Framework (all 6 pillars)
- Days 3-4: Advanced EC2 (placement groups, instance types, purchasing options, networking)
- Days 5-6: Auto Scaling, Load Balancers (ALB, NLB, GWLB), and high availability patterns
- Day 7: Practice labs on EC2 advanced configurations

**Week 2: Networking Deep Dive**
- Days 8-9: VPC advanced concepts (subnetting, routing, NACLs, security groups, flow logs)
- Day 10: VPC peering, Transit Gateway, PrivateLink
- Day 11: Direct Connect, VPN solutions, hybrid architectures
- Days 12-13: Route 53 advanced features, CloudFront distributions
- Day 14: Hands-on labs for multi-VPC architectures

**Week 3: Storage & Databases**
- Days 15-16: S3 advanced features (lifecycle, replication, encryption, storage classes)
- Day 17: EBS, EFS, FSx comparisons and use cases
- Days 18-19: RDS, Aurora advanced features (read replicas, multi-AZ, global databases)
- Day 20: DynamoDB advanced patterns (GSI, LSI, streams, DAX)
- Day 21: Practice exam questions on storage and databases

### Weeks 4-6: Advanced Services & Solutions (Days 22-42)

**Week 4: Security & Identity**
- Days 22-23: IAM advanced (policies, permissions boundaries, SCPs, resource-based policies)
- Day 24: KMS, CloudHSM, Secrets Manager, Parameter Store
- Days 25-26: Security services (GuardDuty, Security Hub, Macie, Inspector, WAF, Shield)
- Day 27: Compliance and governance (Config, Audit Manager, Artifact)
- Day 28: Security hands-on labs

**Week 5: Serverless & Containers**
- Days 29-30: Lambda advanced (layers, versions, aliases, event sources, performance tuning)
- Day 31: API Gateway, AppSync, Step Functions
- Days 32-33: ECS, EKS, Fargate architectures and deployment patterns
- Day 34: Container security and networking
- Day 35: Serverless and container labs

**Week 6: Data Analytics & Migration**
- Days 36-37: Analytics services (Kinesis, Glue, Athena, EMR, Lake Formation)
- Day 38: Migration strategies and AWS Migration Hub
- Day 39: Database Migration Service, DataSync, Transfer Family
- Day 40: Application modernization patterns
- Days 41-42: Practice questions on analytics and migration

### Weeks 7-9: Organizational Complexity & Advanced Patterns (Days 43-63)

**Week 7: Multi-Account & Governance**
- Days 43-44: AWS Organizations, SCPs, consolidated billing
- Day 45: AWS Control Tower, Landing Zone best practices
- Day 46: Resource sharing with AWS RAM
- Days 47-48: Cross-account access patterns and security
- Day 49: Organizational complexity practice scenarios

**Week 8: Monitoring, Logging & Automation**
- Days 50-51: CloudWatch advanced (metrics, alarms, dashboards, Logs Insights)
- Day 52: CloudTrail, Config, Systems Manager
- Days 53-54: Infrastructure as Code (CloudFormation advanced, CDK basics)
- Day 55: CI/CD pipelines with CodePipeline, CodeBuild, CodeDeploy
- Day 56: Automation hands-on labs

**Week 9: Cost Optimization & Advanced Architectures**
- Days 57-58: Cost optimization strategies (Reserved Instances, Savings Plans, Spot, rightsizing)
- Day 59: Disaster recovery patterns (backup, pilot light, warm standby, multi-site)
- Day 60: Global architectures and multi-region deployments
- Days 61-62: Design complex solutions integrating multiple services
- Day 63: Review week 7-9 topics

### Weeks 10-12: Practice & Review (Days 64-84)

**Week 10: Intensive Practice**
- Days 64-67: Take 2-3 full practice exams (analyze all wrong answers)
- Days 68-70: Review weak areas identified from practice exams

**Week 11: Scenario-Based Learning**
- Days 71-73: Work through AWS case studies and whitepapers
- Days 74-76: Design complete solutions for business scenarios
- Day 77: Review AWS service limits and quotas

**Week 12: Final Review**
- Days 78-80: Review all domains systematically
- Days 81-82: Take 1-2 final practice exams
- Day 83: Review exam strategies and time management
- Day 84: Light review and mental preparation

### Days 85-90: Final Preparation & Exam

- Days 85-87: Focus on weak areas only, avoid new topics
- Day 88: Review key architectural patterns and common scenarios
- Day 89: Relax, light review of frequently confused services
- Day 90: Exam day - arrive early, stay calm, trust your preparation

## Daily Study Recommendations

**Study Hours:** 2-3 hours on weekdays, 4-5 hours on weekends (total 20-25 hours/week)

**Study Structure:**
- 40% hands-on labs and practice
- 30% video courses and reading
- 20% practice questions
- 10% documentation and whitepapers

## Recommended Resources

**Training Platforms:**
- A Cloud Guru or Linux Academy AWS SAP-C02 course
- Udemy courses by Stephane Maarek or Neal Davis
- AWS Skill Builder (official AWS training)
- Tutorials Dojo practice exams

**Practice Exams:**
- Tutorials Dojo (highly recommended - 4 practice tests)
- Whizlabs practice tests
- AWS official practice exam

**Essential Reading:**
- AWS Well-Architected Framework whitepaper
- AWS Disaster Recovery whitepaper
- AWS Security Best Practices
- Service-specific FAQs for major services

**Hands-On:**
- Create free-tier AWS account for practice
- Follow AWS workshops and hands-on tutorials
- Build real projects implementing learned concepts

## Exam Day Tips

- Read questions carefully, watch for keywords like "MOST cost-effective," "LEAST operational overhead"
- Eliminate obviously wrong answers first
- Flag difficult questions and return to them
- Manage time: roughly 2.4 minutes per question
- For scenario questions, identify the key requirements first
- Don't overthink - AWS wants practical, well-architected solutions

## Success Metrics

Track your progress weekly by assessing your ability to design complete solutions for real-world scenarios and maintaining practice exam scores above 80% consistently before scheduling your exam.

Good luck with your certification journey! The Professional level is challenging but highly rewarding.