# AWS Fundamentals
## AWS Global Infrastructure

31 Regions, 99 Availability Zones (changes frequently)
### Availability Zones
- Availability Zone is a Data Center or Multiple Data Centers close together.
- Availability Zones within a Region are within 100km or 60 miles of each other but are separated by multiple km of distance
### Region
- Region is a Geographical area consisting of **3 (or more) Availability Zones** 
- ex. US-East (Northern Virginia)

### Edge Locations
- endpoints for AWS that are used for caching content
- Typically, this consists of **CloudFront**, Amazon's CDN.
- There are many more edge locations than Regions
- currently there are over 215 edge locations.

### Service Types we need to know for this course
- Compute
- Storage
- Databases
- Migrations and Transfer
- Network and Content Delivery
- Management and Governanace
- Analytics
- Security, Identity & Compliance
- Application Integration
- AWS Cost Management
- Containers

---
## Who Owns the Cloud

**The Shared Responsibility Model**

**Customer** - Responsible for Security **IN** the Cloud.
- Customer Data
- Platform, Applications, Identity & Access Management
- Operating System, Network & Firewall Configuration
- Client-Side Data Encryption & Data Integrity Authentication
- Server-Side Encryption (File System and/or Data)
- Network Traffic Protection (Encryption, Integrity, Identity)

**AWS** - Responsible for Security **OF** the Cloud.
- Regions
- Availability Zones
- Edge Locations
- Physical Security of Data Centers
- Hardware 
- Software that runs the stack
- Hypervisor
- Compute
- Storage
- Database
- Networking

**Can you do this yourself in the AWS Management Console?**

If yes you are likely responsible.
	Security groups, IAM users, patching EC2 operating systems, patching databases running on EC2, etc.
If not, AWS is likely responsible.
	Management of data centers, security cameras, cabling, patching RDS operating systems, etc.

**Encryption is a shared responsibility.**

---
## Compute, Storage, Database, and Networking

### Compute
- EC2 - Virtual Machines
- Lambda - Serverless
- Elastic Beanstalk - Provisioning Engine

### Storage
- S3 (Simple Storage Service)
- EBS (Elastic Block Store)
- EFS (Elastic File Service)
- FSx
- Storage Gateway

### Databases
- RDS (Relational Database Service)
- DynamoDB (Non relational Data Store)
- Redshift (Data Warehousing)

### Networking
A way for compute, storage, and databases to communicate and even a place for them to live.
- VPC (Virtual Datacenters in the cloud)
- Direct Connect - direct connect on prem to AWS
- Route 53 - DNS management
- API Gateway - Serverless replace webserver
- AWS Global Accelerator - Accelerating audiences towards applications in aws

---
## Well Architected Framework

### Six Pillars of the Well-Architected Framework
- Operational Excellence
	  Running and monitoring systems to deliver business value, and continually impriving processes and procedures
- Performance Efficiency
	  Using IT and computing resources efficiently
- Security
	  Protecting information and systems
- Cost Optimization
	  Avoiding unnecessary costs
- Reliability
	  Ensuring a workload performs it's intended function correctly and consistently when it's expected to.
- Sustainability
	  Minimizing the environmental impacts of running cloud workloads.