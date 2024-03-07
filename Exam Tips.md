## Introduction
#### Pass Mark
- Your result on the exam is a score from **100-1000**.
- The minimum passing mark is **720**.
- There are **65 questions**, and you have **130 minutes** to complete the exam.

#### Testing Tips
- Don't spend too much time on **hard questions** at first -- **flag them** and **review later**!
- Watch for **keyword indicators** in certain scenarios.
- Remember the **Well-Architected Framework** pillars.

---

## AWS Fundamentals
#### AWS Building Blocks
- **A Region** is a physical location in the world that consists of three or more Availability Zones (AZs)
- **An AZ** is one or more discrete data centers - each with redundant power, networking, and connectivity, housed in separate facilities.
- Edge Locations are endpoints for AWS that are used for caching content.  Typically this consists of CloudFront, Amazon's CDN

#### Can you do this yourself in the AWS Management Console?

- If yes you are likely responsible.
	Security groups, IAM users, patching EC2 operating systems, patching databases running on EC2, etc.
- If not, AWS is likely responsible.
	Management of data centers, security cameras, cabling, patching RDS operating systems, etc.
- Encryption is a shared responsibility.

#### Key Services to Know for the Exam

- Compute
	- EC2
	- Lambda
	- Elastic Beanstalk
- Storage
	- S3
	- EBS
	- EFS
	- FSx
	- Storage Gateway
- Databases
	- RDS
	- DynamoDB
	- Redshift
- Networking
	- VPCs
	- Direct Connect,
	- Route 53,
	- API Gateway
	- AWS Global Accelerator
#### Most important white paper for the exam is the **[[./resources/wellarchitected-framework.pdf|Well-Architected Framework]]**.

---
## Identity and Access Management (IAM)
#### 4 Steps to Secure Your AWS Root Account
- Enable multi-factor authentication on the root account
- Create an admin group for your administrators, and assign the appropriate permissions to this group.
- Create user accounts for your administrators
- Add your uses to the admin group.

#### Assign Permissions Using IAM Policy Documents Consisting of JSON

Example:
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Actions": "*",
			"Resource" "*"
		}
	]
}
```

#### Identity Access Management (IAM)
- IAM is Universal: It does not apply to regions at this time.
- The Root Account: The account created when you first set up your AWS account which has complete admin access.  Secure it as soon as possible and **do not** use it to log in day to day.
- New Users: No permissions when first created.
- Access key ID andd secret access keys are not the same as usernames and passwords.
- You only get to view these once.  If you lose them, you have to regenerate them.  So, save them in a secure location.
- Always set up password rotations.  You can create and customize your own password rotation policies.
- IAM Federation: You can combine your existing user account with AWS.  For example, when you log on to your PC (usually using Microsoft Active Directory), you can use the same credentials to log into AWS if you set up federation.
- Identity Federation: Uses the SAML standard, which is essentially Active Directory

---
## Simple Storage Service (S3)

#### S3 Overview

- **Object Based**
	  Object-based storage allows you to upload files.
- **Files up to 5 TB** 
	  Files can be from 0 bytes to 5TB.
- **Not OS or DB Storage**
	  Not suitable to install an operating system or run a database on.
- **Unlimited Storage**
	  The total volume of data and number of objects you can store is unlimited.
- **Files are stored in Buckets**
- S3 is a universal namespace
- Successful CLI or API upload will generate an HTTP 200 status code.

#### S3 Object Tips

- **Key**
	  The name of the object (e.g. Ralphie.jpg)
- **Value**
	  The data itself, which is made up of a sequence of bytes
- **Version ID**
	  Important for storing multiple versions of the same object
- **Metadata**
	  Data about the data you are storing (e.g. content-type, last-modified, etc.)

#### Securing your bucket with S3 Block Public Access

- **Buckets are private by default**
	  When you create an S3 bucket, it is private by default (including all objects within it).  You have to allow public access on both the **bucket** and **its objects** in order to make the bucket public.
- **Object ACLs**
	  You can make **individual objects** public using ACLs.
- **Bucket Policies**
	  You can make **entire buckets** public using bucket policies.
- **HTTP status code**
	  When you upload an object to S3 and it's successful, you will receive an HTTP 200 code.


#### Hosting a Static Website Using S3

- **Bucket Policies:** Make entire buckets public using bucket policies.
- **Static Content:** Use S3 to host static content only (not dynamic).
- **Automatic Scaling:** S3 scales automatically with demand.

#### Versioning Objects

- **All Versions**
	  All versions of an S3 object are stored in S3.  This includes all writes and even if you delete an object
- **Backup**
	  Can be a great backup tool.
- **Cannot Be Disabled**
	  Once enabled, versioning cannot be disabled -- only suspended
- **Lifecycle Rules**
	  Can be integrated with lifecycle rules
- **Supports MFA**
	  Can support multi-factor authentication.
- Previous versions are not publicly accessible
- Restore a deleted object by deleting the `Delete Marker`

### S3 Storage Classes

| Storage Class | Availability and Durability | AZ(s) | Use Case |
| --- | --- | --- | --- |
| S3 Standard | 99.99% Availability.  11 9's Durability | >= 3 | Suitable for most workloads (e.g. websites, content distribution, mobile and gaming applications, and big data analytics) |
| S3 Standard Infrequent Access | 99.9% Availability.  11 9's Durability | >= 3 | Long-term, infrequently accessed critical data (e.g. backups, data store for disaster recovery files, etc.) |
| S3 One Zone Infrequent Access | 99.5% Availability.  11 9's Durability | 1 | Long-term infrequently accessed, non-critical data |
| S3 Intelligent Tiering | 99.9% Availability.  11 9's Durability | >= 3 | Unknown or unpredictable access patterns |
| S3 Glacier Instant Retrieval | 99.99% Availability.  11 9's Durability | >= 3 | Provides **long-term data archiving** with instant retrieval time for your data |
| S3 Glacier Flexible Retrieval | 99.99% Availability.  11 9's Durability | >= 3 | Ideal storage class for archive data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost, such as backup or disaster recovery use cases.  Can be minutes or up to 12 hours. |
| S3 Glacier Deep Archive | 99.99% Availability.  11 9's Durability | >= 3 | Cheapest storage class and designed for customers that retain data sets for 7-10 years or longer to meet customer needs and regulatory compliance requirements.  The standard retrieval time is 12 hours, and the bulk retrieval time is 48 hours. | 

#### Lifecycle Management
- Automates moving objects between different storage tiers.
- Can be used in conjunction with versioning.
- Can be applied to current versions and previous versions.

#### Object Lock Tips
- Use **S3 Object Lock** to store objects using a write once, read many (WORM) model.
- Object Lock can be on **individual objects** or applied **across the bucket** as a whole.
- Object Lock comes in two modes: **governance mode** and **compliance mode**

#### Compliance Mode
With **compliance mode**, a protected object version can't be overwritten or deleted by any user, including the root user in your AWS account.

#### Governance Mode
With **governance mode**, user's can't overwrite or delete an object version or alter its lock settings unless they have special permissions.

#### S3 Glacier Vault Lock

- S3 Glacier Vault Lock allows you to **easily deploy** an **enforce compliance controls** for individual S3 Glacier vaults with a vault lock policy.
- You can **specify controls, such as WORM, in a vault lock policy and lock the policy from future edits**.  Once locked, the policy can no longer be changed.

#### Encrypting S3 Objects

- **Encryption in Transit**
	- SSL/TLS
	- HTTPS
- **Encryption at Rest: Server-Side Encryption**
	- **SSE-S3**: S3-managed keys, using AES 256-bit encryption
	- **SSE-KMS**: AWS Key Management Service-managed keys
	- **SSE-C**: Customer-provided keys
- **Encryption at Rest: Client-Side Encryption**
	  You encrypt the files yourself before you upload them to S3
- Enforcing Encryption with a Bucket Policy
	  A bucket policy can deny all **PUT** requests that don't include the `x-amz-server-side-encryption` parameter in the request header.

#### Optimizing S3 Performance

**Prefixes**
- `mybucketname/folder1/subfolder1/myfile.jpg` > **/folder1/subfolder1**
- You can also achieve a high number of requests: **3,500 PUT/COPY/POST/DELETE** and **5.500 GET/HEAD** requests per second per prefix.
- You can get better performance by spreading your reads across **different prefixes**.  For example, if you are using **2 prefixes**, you can achieve **11,000 requests per second**.

**If you are using SSE-KMS to encrypt your objects in S3, you must keep in mind the KMS limits**
- Uploading/downloading will count toward the **KMS quota**
- Region-specific, however it's either **5,500, 10,000,** or **30,000** requests per second.
- Currently, you **cannot** request a quota increase for KMS.

**Upload/Download Performance**

- Use **multipart uploads** to increase performance when **uploading files** to S3.
- Should be used for any files **over 100MB** and must be used for any file over **5GB**.
- Use **S3 byte-range fetches** to increase performance when **downloading files** to S3.

#### S3 Replication

- You can **replicate objects** from one bucket to another,
- Objects in an existing bucket are **not replicated automatically**
- Delete markets are **not replicated by default.**

---

## Elastic Cloud Compute (EC2)

#### EC2 Overview

**EC2 is like a VM, hosted in AWS instead of your own data center.**
- Select the capacity you need **right now.**
- Grow and shrink when you need.
- Pay for what you use.
- Wait minutes, not months.

**EC2 Instance Pricing Options**
- **On-Demand**
	  Pay by the hour or the second, depending on the type of instance you run.
	  Great for flexibility.
- **Reserved**
	  Reserved capacity for 1 or 3 year.s. Up to 72% discount on the hourly charge.
	  Great if you have known, fixed requirements.
- **Spot**
	  Purchase unused capacity at a discount of up to 90%.  Prices fluctuate with supply and demand.
	  Great for applications with flexible start and end times.
- **Dedicated**
	  A physical EC2 server dedicated for your use.  The most expensive option.
	  Great if you have server-bound licenses to reuse or compliance requirements.

#### AWS Command Line

##### Least Privilege
Always give your users the **minimum amount** of access required to do their job.

##### Use Groups
**Create IAM groups** and assign your users to groups.  Group permissions are assigned using IAM policy documents.
Your users will **automatically inherit** the permissions of the group.

##### AWS CLI

- **Secret Access Key**
	  You will only see this once!  If you lose it, you can delete the access key ID and secret access key and regenerate them.  You will need to run `aws configure` again.
- **Don't Share Key Pairs**
	  Each developer should have their own access key ID and secret access key.  Just like passwords, they should not be shared.
- **Support Linux, Window, MacOS**
	  You can install the CLI on your Mac, Linux, or Windows PC.  You can also use it on EC2 instances

#### Using Roles

- **The Preferred Option**
	  Roles are preferred from a security perspective.
- **Avoid Hard-Coding Your Credentials**
	  Roles allow you to provide access without the use of access keys IDs and secret access keys.
- **Policies**
	  Policies control a role's permissions.
- **Updates**
	  You can update a policy attached to a role, and it will take immediate effect.
- **Attaching and Detaching**
	  You can attach and detach roles to running EC2 instances without having to stop or terminate those instances.

#### Security Groups
- Changes to security groups take effect immediately.
- You can have any number of EC2 instances within a security group.
- You can have multiple security groups attached to EC2 instances.
- All inbound traffic is blocked by default.
- All outbound traffic is allowed.

#### Bootstrap Scripts
A bootstrap script is **a script that runs when the instance first runs.**  It passes user data to the EC2 instance and can be used to install applications (like web servers and databases), as well as do updates and more.

#### User Data vs. Metadata
- User data is simply bootstrap scripts.
- Metadata is data about your EC2 instances.
- You can use bootstrap scripts (user data) to access metadata.

#### Networking with EC2

**For different scenarios on the exam, choose the correct networking device.**

**ENI**
For basic networking.  Perhaps you need a separate management network from your production network or a separate logging network, and you need to do this at a low cost.  In this scenario, use multiple ENIs for each network.

**EN (Enhanced Networking)**
For when you need speeds between 10 Gbps and 100 Gbps.  Anywhere you need reliable, high throughput.

**EFA**
For when you need to accelerate High Performance Computing (HPC) an machine learning applications or if you need to do an OS-bypass.  If you see a scenario question mentioning HPC or ML and asking what networking adapter you want, choose EFA.

#### Optimizing with EC2 Placement Groups

##### 3 Types of Placement Groups

**Cluster Placement Groups**
Low network latency, high network throughput.  Same AZ.

**Spread Placement Groups**
Individual critical EC2 instances.

**Partition Placement Groups**
Multiple EC2 instances; HDFS, HBase, and Cassandra

**Placement Group Notes**
- A **cluster placement group** can't span multiple Availability Zones, whereas a spread placement group and a partition placement group can.
- Only **certain types of instances** can be launched in a placement groups (compute optimized, GPU, memory optimized, storage optimized).
- **AWS recommends homogenous instances** within cluster placement groups.
- **You can't merge placement groups.**
- You can **move an existing instance into a placement group.** Before you move the instance, the instance must be in the stopped state.  You can move or remove an instance using the AWS CLI or an AWS SDK, but you can't do it via the console yet.

#### Solving Licensing Issues with Dedicated Hosts

**Any question that talks about special licensing requirements.**

An **Amazon EC2 Dedicated Host** is a **physical server** with EC2 instance capacity fully dedicated to your use.  Hosts allow you to **use your existing** per-socket, per-core, or per-VM software **licenses**, including Windows Server, Microsoft SQL Server, and SUSE Linux Enterprise Server.

#### Timing Workloads With Spot Instances and Spot Fleets

 - Spot Instances save up to 90% of the cost of On-Demand instances.
- Useful for any type of computing where you don't need persistent storage.
- A Spot Fleet is a collection of Spot Instances and (optionally) On-Demand instances.

#### Deploying vCenter in AWS with VMware Cloud on AWS

**You can deploy vCenter on AWS Cloud using VMware.**

Perfect solution for extending your private VMware Cloud into the AWS public cloud.

#### Extending AWS Beyond the Cloud with AWS Outposts

**Scenario about extending AWS to your data center?**
**Think AWS Outposts.**

AWS Outposts racks for **large deployments**
AWS Outposts server for **smaller deployments**

---

## Elastic Block Storage (EBS) and Elastic File System (EFS)

#### EBS Overview

**EBS: SSD Volumes**
Highly available and scalable storage volumes **you can attach to an EC2 instance.**

##### gp2
General Purpose SSD
- Suitable for boot disks and general applications
- Up to 16,000 IOPS per volume
- Up to 99.9% durability

##### gp3
General Purpose SSD
- Suitable for high performance applications
- Predictable 3,000 IOPS baseline performance and 125 MiB/s regardless of volume size
- Up to 99.9% durability

##### io1
Provisioned IOPS SSD
- Suitable for OLTP and latency-sensitive applications
- 50 IOPS/GiB
- Up to 64,000 IOPS per volume
- High performance and most expensive
- Up to 99.9% durability

##### io2
Provisioned IOPS SSD
- Suitable for OLTP and latency sensitive applications
- 500 IOPS/GiB
- Up to 64,000 IOPS per volume
- 99.999% durability
- Latest generation Provisioned IOPS volume


**EBS: HDD Volumes**
Highly available and scalable storage volumes **you can attach to an EC2 instance.**

**st1**
Throughput Optimized HDD
- Suitable for big data, data warehouses, and ETL
- Max throughput is 500 MB/s per volume
- Cannot be a boot volume
- Up to 99.9% durability

**sc1**
Cold HDD
- Max throughput of 250 MB/s per volume
- Less frequently accessed data
- Cannot be a boot volume
- Lowest cost
- Up to 99.9% durability

#### Volumes and Snapshots

-  Volumes exist on EBS, whereas snapshots exist on S3.
- Snapshots are point-in-time photographs of volumes and are incremental in nature.
- The first snapshot will take some time to create.  For consistent snapshots, stop the instance and detach the volume.
- You can share snapshots between AWS accounts as well as between regions, but first you need to copy that snapshot to the target region.
- You can resize EBS volumes on the fly as well as changing the volume types.

#### Protecting EBS Volumes with Encryption

**Encrypted Volumes**

- Data at rest is encrypted inside the volume.
- All data in flight moving between the instance and the volume is encrypted.
- All snapshots are encrypted.
- All volumes created from the snapshot are encrypted.

**How to Encrypt Existing Volumes**

1. Create a snapshot of the unencrypted root device volume.
2. Create a copy of the snapshot and select the encrypt option.
3. Create an AMI from the encrypted snapshot.
4. Use that AMI to launch new encrypted instances.

#### EC2 Hibernation

- **EC2 hibernation** preserves the in-memory RAM on persistent strage (EBS)
- **Much faster to boot up** because you **do not need to reload the operating system.**
- **Instance RAM** must be less than 150GB
- **Instance families include instances in** General Purpose, Compute, Memory and Storage Optimized groups.
- **Available for** Windows, Amazon Linux 2 AMI, and Ubuntu.
- **Instances. can't be hibernated** for more than **60 days**
- **Available for** On-Demand instances and Reserved instances.

#### EFS Overview

- Supports Network File System version 4 (NFSv4) protocol
- Only pay for the storage you use (no pre-provisioning required)
- Can scale up to petabytes
- Can support thousands of concurrent NFS connections
- Data is stored across AZs within a region
- Read-after-write consistency.

**If you have a scenario-based question around highly scalable shared storage using NFS, think EFS.**

#### FSx Overview

**In the exam, you'll be given different scenarios and asked to choose whether you should use EFS, FSx for Windows, or FSx for Lustre**

- **EFS:** When you need distributed, highly resilient storage for Linux instances and Linux-based applications.
- **Amazon FSx for Windows:** When you need centralized storage for Windows-based applications, such as SharePoint, Microsoft SQL Server, Workspaces, IIS Web Server, or any other native Microsoft application.
- **Amazon FSx for Lustre:** When you need high-speed, high-capacity distributed storage.  This will be fore applications that do high performance computing (HPC), financial modeling, etc.  Remember that FSx for Lustre can store data directly on S3.

#### Amazon Machine Images: EBS vs. Instance Store

- Instance store volumes are sometimes called ephemeral storage.
- Instance store volumes **cannot be stopped**.  If the underlying host fails, you will lose your data.
- EBS-backed instances **can be stopped.** You will not lose the data on this instance if it is stopped.
- You can reboot both EBS and instance store volumes and you will not lose your data.
- By default, both root volumes will be deleted on termination.  However, with EBS volumes, you can tell AWS to keep the root device volume.

#### AWS Backup

- **Consolidation:** Use AWS Backup to back up AWS services, such as EC2, EBS, EFS, Amazon FSx for Lustre, Amazon FSx for Windows File Server, and AWS Storage Gateway.
- **Organizations:** You can use AWS Organizations in conjunction with AWS Backup to back up your different AWS services across multiple AWS accounts.
- **Benefits:** Backup give you centralized control, letting you automate your backup and define lifecycle policies for your data.  You get better compliance, as you can enforce your backup policies, ensure your backups are encrypted, and audit them once complete.

#### Storage Option Use Cases

- **S3**
	  Used for serverless object storage
- **Glacier**
	  Used for archiving objects
- **EFS**
	  Network File System (NFS) for Linux instances.  Centralized storage solution across multiple AZs
- **FSx For Lustre**
	  File storage for high performance computing Linux file systems
- **EBS Volumes**
	  Persistent storage for EC2 instances
- **Instance Store**
	  Ephemeral storage for EC2 Instances
- **FSx for Windows**
	  File storage for Windows Instances. Centralized storage solution across multiple AZs

---
## Databases

#### Relational Database Service (RDS) Overview

- **RDS Database Types**
	  SQL Server, Oracle, MySQL, PostgreSQL, MariaDB, and Amazon Aurora
- **RDS is for OLTP Workloads**
	  Great for processing lots of small transactions, like customer orders, banking transactions, payments, and booking systems.
- **Not Suitable for OLAP Workloads**
	  Use Redshift for data warehousing and OLAP tasks, like analyzing large amounts of data, reporting, and sales forecasting

#### Read Replicas

**Multi-AZ** vs **Read Replica**

**Multi-AZ**
- An exact copy of your production database in another Availability Zone.
- Used for disaster recovery.
- In the event of a failure, RDS will automatically fail over to the standby instance.

**Read Replica**
- A read-only copy of your primary database in the same AZ, cross-AZ, or cross-region.
- Used to increase or scale read performance.
- Great for read-heavy workloads and takes the load off your primary database for read-only workloads (e.g. Business Intelligence reporting jobs).

#### Amazon Aurora

- 2 copies of your data contained in each Availability Zone, with a minimum of 3 Availability Zones.  6 copies of your data.
- You can share Aurora snapshots with other AWS accounts.
- 3 types of replicas available: Aurora Replicas, MySQL Replicas, and PostgreSQL Replicas.  Automated failover is only available with Aurora replicas.
- Aurora has automated backups turned on by default.  You can also take snapshots with Aurora.  You can share these snapshots with other AWS accounts.
- Use Aurora Serverless if you want a simple, cost-effective option for infrequent, intermittent, or unpredictable workloads.

#### DynamoDB

- Stored on SSD storage
- Spread across 3 geographically distinct data centers
- Eventually consistent reads (default)
- Configurable for strongly consistent reads 

**Read Consistency**

What's the difference between **eventually consistent reads** and **strongly consistent reads?**

**Eventually**
Consistent across all **copies of data is usually reached within a second.**  Repeating a read after a short time should return the updated data.  Best read performance.

**Strongly**
A strongly consistent read **returns a result that reflects all writes** that received a successful response prior to the read.

#### DynamoDB Transactions

- If you see any scenario that mentions ACID requirements, think **DynamoDB Transactions**
- DynamoDB transactions provide developers **atomicity, consistency, isolation,** and **durability** (ACID) across 1 or more tables within a single AWS account and region.
- **All-or-nothing** transactions.

#### DynamoDB Backups

**On-Demand Backup and Restore**

- Full backups at any time
- Zero impact on table performance or availability
- Consistent within seconds and **retained until deleted**
- Operates within the same region as the source table

**Point-in-Time Recovery (PITR)**

- Protects against accidental writes or deletes
- Restore to any point in the last 35 days
- Incremental backups
- Not enabled by default
- Latest restorable time is **5 minutes** in the past

#### DynamoDB Global Tables

**Managed Multi-Master, Multi-Region Replication**
- Great for globally distributed applications
- Based on DynamoDB streams (required)
- Multi-region redundancy for disaster recovery or high availability
- No application rewrites
- Replication latency under **1 second**

#### Amazon DocumentDB (MongoDB)

A typical question might be about moving your MongoDB database to AWS.

1. MongoDB On-Premises
2. AWS Database Migration Service
3. Amazon DocumentDB

For scenarios where they talk about migrating MongoDB from on-premises to AWS, **think Amazon DocumentDB**.

#### Amazon Keyspaces (Cassandra)

**Scenario Question about Cassandra?**

If you see a scenario question about migrating a big data Cassandra cluster to AWS.. 
**Think of Amazon Keyspaces!**

#### Amazon Neptune (Graph Databases)

**Neptune is often used as a distractor.**

If the scenario is **not** talking about graph databases, **do not** select Neptune as an answer.  You only need to know what Neptune does at a very high level.

#### Amazon Quantum Ledger Database (Amazon QLDB)

**QLDB is often used as a distractor.**

If the scenario is not talking about immutable databases, do not select Amazon QLDB as an answer.  You need to know what QLDB does only at a very high level.

#### Amazon Timestream (Time-Series Database)

**Scenario Question about Time-Series Data?**

If you see a scenario question about where to store a large amount of time-series data for analysis...
**Think of Timestream.**

---
## Virtual Private Cloud (VPC) Networking

#### VPC Overview

- Think of a VPC as a logical data center in AWS
- Consists of internet gateways (or virtual private gateways), route tables, network access control lists, subnets, and security groups.
- 1 subnet is always in 1 Availability Zone

#### Using NAT Gateways for Internet Access

**NAT Gateway Tips**
- Redundant inside the Availability Zone
- Starts at 5Gbps and scales currently to 45 Gbps
- No need to patch
- Not associated with security groups
- Automatically assigned a public IP address

**High Availability with NAT Gateway**
If you have resources in multiple Availability Zones and they share a NAT gateway, in the event the NAT gateway's Availability Zone is down, resource in other Availability Zones lose internet access.

To create an Availability Zone-independent architecture, create a NAT gateway in each Availability Zone a d configure your routing to ensure resources use the NAT gateway in the same Availability Zone.

#### Protecting Your Resources with Security Groups

Security groups are stateful - if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules.

Responses to allowed inbound traffic are allow to flow out, regardless of outbound rules.

#### Network ACLs

- **Default Network ACLs**
	  Your VPC automatically comes with a default network ACL, and by default it allows all outbound and inbound traffic.
- **Custom Network ACLs**
	  You can create custom network ACLs. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
- **Subnet Associations**
	  Each subnet in your VPC must be associated with a network ACL.  If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
- **Block IP Addresses**
	  Block IP addresses using network ACLs, not security groups.

- You can associate a network ACL with multiple subnets; however a subnet can be associated with **only 1 network ACL** at a time.  When you associate a network ACL with a subnet, the previous association is **removed.**
- Network ACLs contain a **numbered list of rules** that are evaluated in order, starting with the **lowest** numbered rule.
- Network ACLs have **separate** inbound and outbound rules, and each rule can either **allow or deny traffic**.
- Network ACLs are **stateless**; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).

#### VPC Endpoints

- **Use Case:** When you want to connect AWS services without leaving the Amazon internal network
- **2 Types of VPC Endpoints:** Interface endpoints and gateway endpoints
- **Gateway Endpoints:** Support S3 and DynamoDB
#### VPC Peering

- **Allows you to connect** 1 VPC to another via a direct network route using private IP addresses
- **Transitive peering is not supported.**  This must always be in a hub-and-spoke model.
- **You can peer between regions and accounts.**
- **No overlapping CIDR address ranges**

#### AWS PrivateLink

- If you see a question about peering VPCs to tens, hundreds, or thousands of customers VPCS, think of AWS PrivateLink.
- Doesn't require VPC peering; no route tables, NAT gateways, internet gateways, etc.
- Requires a Network Load Balancer on the service VPC and an ENI (Elastic Network Interface) on the customer VPC
#### AWS VPN CloudHub

If you have multiple sites, each with its own VPN connection, you can use AWS VPN CloudHub to connect those sites together

- Hub-and-spoke model
- Low cost and easy to manage
- It operates over the public internet, but all traffic between the customer gateway and the AWS VPN CloudHub is encrypted

#### Direct Connect

- Direct Connect directly connects your data center to AWS
- Useful for high-throughput workloads (e.g. lots of network traffic)
- Helpful when you need a stable and reliable secure connection

#### Transit Gateway

- You can use route tables to limit how VPCs talk to one another.
- Works with Direct Connect as well as VPN connections.
- Supports IP multicast (not supported by any other AWS service).

#### AWS Wavelength

**Scenario Question about Mobile Edge Computing**
If you see a scenario question about 5G increasing application speeds at edge using mobile networks...

**Think of AWS Wavelength!**

---
## Route 53

#### Route 53 Overview

- Understand the difference between an alias record and a CNAME
- Given the choice, always choose an alias record over a CNAME

**Common DNS Record Types**
- **A** - IPv4
- **CNAME** - Forwards to another domain
- **NS** - Stores the name server for a DNS entry.
- **SOA** -  Start of Authority.  Stores admin information about a domain

###### 7 Routing Policies Available with Route 53

1. Simple Routing
2. Weighted Routing
3. Latency-Based Routing
4. Failover Routing
5. Geolocation Routing
6. Geoproximity Routing (Traffic Flow Only)
7. Multivalue Answer Routing

**Health Checks**
- You can set health checks on individual record sets.
- If a record set **fails** a health check, it will be removed from Route 53 until is **passes** the health check.
- You can set SNS notifications to **alert** you about failed health checks.

#### Simple Routing Policy
If you choose the simple routing policy, you can only have **one record with multiple IP addresses**. 
If you specify **multiple values in a record**, Route 53 returns **all values** to the user in a **random order.**

#### Weighted Routing Policy

Allows you to split your traffic based on different weights assigned.

For example, you can set 10% of your traffic to go to **us-east-1** and 90% to go to **eu-west-1**.
#### Failover Routing Policy

Failover routing policies are used when you want to create an active/passive set up.

> For example, you may want your primary site to be in EU-WEST-2 and your secondary DR Site in AP-SOUTHEAST-2
> 
> Route 53 will monitor the health of your primary site using a health check.

#### Geolocation Routing Policy

Geolocation routing lets you **choose where your traffic will be sent** based on the **geographic location of your users** (i.e. the location from which DNS queries originate).

#### Geoproximity Routing Policy

- Geoproximity routing lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources.
- You can also optionally choose to route more traffic or less to a given resource by specifying a value known as a **bias.**
- To use **geoproximity routing**, you must use Route 53 **traffic flow**.

#### Latency Routing Policy

Allows you to route your traffic based on the **lowest network latency for your end user** (i.e., which region will give them the fastest response time).

#### Multivalue Answer Routing Policy

Is essentially Simple Routing with Healthchecks

--- 
## Elastic Load Balancer (ELB)

#### ELB Overview

**The Different ELB Types**
- Application Load Balancers (Layer 7)
- Network Load Balancers (Layer 4)
- Gateway Load Balancers (Layer 4)
- Classic Load Balancers (Layer 4/7)

**Health Checks**
- You can use **health checks** to route your traffic to instances or targets that are healthy.

#### Application Load Balancers

- **Listeners:** A listener checks for connection requests from clients, using the protocol and port you configure.
- **Rules:** Determine how the load balancer routes requests to its registered targets.  Each rule consists of a priority, one more actions, and one or more conditions.
- **Target Groups:** Each target group routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number you specify.
- **Limitations:** Applications Load Balancers only support HTTP and HTTPS
- **HTTPS:** To use an HTTPS listener, you must deploy at least one SSL/TLS server certificate on your load balancer.  The load balancer uses a server certificate to terminate the frontend connection and then decrypt requests from clients before sending them to the targets.

#### Network Load Balancers

- Network Load Balancers operate at Layer 4
- Use where you need extreme performance.
- Other use cases are where you need protocols not supported by Application Load Balancers
- Network Load Balancers can decrypt traffic, but you will need to install the certificate on the load balancer.

#### Gateway Load Balancers
Watch for any questions about load balancers **operating at Layer 3 for inline virtual appliances.**

#### Classic Load Balancer

- **A 504 error means the gateway has timed out.**
	  This means the application is not responding within the idle timeout period
- **Troubleshoot the application.**
	  Is it the web server or the database.
- Need the IPv4 address of your end user?
	  Look for the **X-Forwarded-For** header.

#### Sticky Sessions
- Sticky Sessions enable your users to stick to the same EC2 instance.  Can be useful if you are storing information locally to that instance.
- You may see a scenario-based question where you remove an EC2 instance from a pool, but the load balancer continues to direct traffic to that EC2 instance.
- To solve a scenario like this, disable sticky sessions.

**Application Load Balancers**
You can enable sticky sessions for Application Load Balancers as well, but the traffic will be sent at the target group level.

#### Deregistration Delay
- **Enable deregistration delay:** Keep existing connections open if the EX2 instance becomes unhealthy.
- **Disable deregistration delay:** Do this if you want your load balancer to immediately close connections to the instances that are de-registering or have become unhealthy.

---
## Monitoring

#### CloudWatch Overview
- **Go to Tool**
	  Anytime monitoring comes up, think **CloudWatch**
- **Alarms**
	  There are **no default alarms.** Anything you want to hear about must be created.
- **Default vs. Custom**
	  AWS can't see past the hypervisor level for EC2 instances.
- **Managed Services**
	  The more managed a service is, the more checks you get out of the box.
- **Basic vs. Detailed**
	  Basic is 5-minute intervals, whereas detailed is 1-minute.

#### Application Monitoring with CloudWatch Logs

**Logs Should Go to CloudWatch Logs**
Except for situations where we don't need to process them. Then, they should go straight to S3.

**CloudWatch Logs**
- **Go-To Tool**
	  Generally, favor CloudWatch Logs, unless the exam asks for a real-time solution.
- **Alarms**
	  CloudWatch alarms can be used to alert if your filter patterns are found
- **Agent Based**
	  The CloudWatch Agent must be installed and configured.  It's not automatic.
- **SQL**
	  If the exam mentions SQL, think CloudWatch Logs Insights.

#### Amazon Managed Service for Prometheus and Amazon Managed Grafana

**What is Amazon Managed Grafana?**
Fully managed AWS service allowing secure data visualizations for instantly **querying, correlating, and visualizing your operational metrics, logs, and traces** from different sources.

**What is Amazon Managed Service for Prometheus?**
**Serverless, Prometheus-compatible service** used for securely monitoring container metrics at scale.

**Security and Scaling**
Both services let AWS handle the **high availability** and **automatic scaling** of infrastructure.  Leverage **VPC endpoints** for **secure VPC access**.

**Grafana Data Sources**
Several built-in data sources, including **Amazon CloudWatch**,  **Amazon Managed Service for Prometheus**, and **AWS X-Ray**.

**Amazon Managed Grafana Uses**
**Container metric visualizations** (EKS, ECS, or self-hosted Kubernetes clusters), **IoT edge service data**, and **operational teams**.

#### 4 Questions to Ask in the Exam
1. What is the best tool to monitor with?
2. Is that metric available by default?
3. Where can I find those logs?
4. Do I need to adjust my alarm threshold?

#### What to Know for the Exam?
- **CloudWatch is the main tool** for anything alarm related.
- **Not everything should go through CloudWatch.**. We'll discuss some other tools in future sections, but AWS standards should be watched by AWS Config.
- **Know your intervals.** The standard metric is delivered every five minutes, while detailed monitoring delivers data every one minute.
- **CloudWatch Logs is the place for logs.**  It's important to know EC2, premises, RDS, Lambda, and CloudTrail can all integrate with this service.
- **SQL? Think CloudWatch Logs Insights.**. You don't have to know it in depth, but it's a good idea to have a high-level understanding of this service.
- **Real time means Kinesis.** We'll cover this service in our big data section, so avoid picking CloudWatch Logs if it asks for a real-time logging service.
- **Grafana might be best for visualization of container metrics.**  If you need an AWS-managed service for correlation and visualization of container of IoT metrics, this may be the best option.
- **Monitoring container metrics at scale?  Think Amazon Managed Service for Prometheus.**. Leverage this service for any Kubernetes-based metrics monitoring at scaled.  It can be an Amazon EKS cluster or your own self-managed cluster.
- **They are both managed services.** AWS handles scaling and high availability for you.

---
## High Availability and Scaling

#### Horizontal vs Vertical Scaling

**Vertical Scaling**
Vertical scaling is adding more resources to a single instance

**Horizontal Scaling**
Increase number of instances rather than increasing the size of one instance.  This also accomplishes high availability.

**The 3 W's of Scaling**
- **What do we scale?**
	  We have to decide what sort of resource we're going to scale.  How do we define the template?
- **Where do we scale?**
	  When applying the model, where does it go?  Should we scale out database?  Or Webservers?
- **When do we scale?**
	  How do we know we need more?  CloudWatch alarms can tell us when it's time to add more resources.

#### Launch Templates and Launch Configurations

**What makes a Template?**
You need to know it includes the AMI, EC2 instance size, security groups, and potentially networking information

- **Launch Templates** are the most up-to-date and flexible way to create a template.
- **Launch configurations** are the older version.  It's not "wrong" to use them, but, if possible, use templates.
- **User Data** is included in the template or configuration
- **Change** can be versioned for templates.  Configurations are Immutable
- **Networking** is not included in Launch Configurations.  It is optional for Launch Templates.

#### Scaling EC2 Instances with Autoscaling

**Auto Scaling and High Availability**

It's important to remember Auto Scaling is vital to creating a highly available application.

Remember to select answers that spread resources out over multiple Availability Zones and utilize load balancers.

**Auto Scaling groups**
- Auto Scaling groups will contain the location of where your instances will live.
- It's vital to select a load balancer for the instances to live behind.
- Min, max, and desired are the 3 most important settings.
- SNS can act as a notification tool
- Auto Scaling will balance your EX2 instances across the Availability Zones.

#### Deep Dive into Auto Scaling Policies

**It Costs How Much?**

You'll need a good understanding of how Auto Scaling handles minimum, maximum, and desired capacity for the exam.

You'll be given scenarios where you'll need to know the cost implications and reasons why you might or might not want to change one of those numbers.

**Auto Scaling Policies**

- **Scale Out Aggressively**
	  Get ahead of the workload if you need to.
- **Scale In Conservatively**
	  Once the instances are up, slowly roll them back when not needed.
- **Provisioning**
	  Keep an eye on provisioning times. Bake those AMIs to minimize it.
- **Costs**
	  Use EC2 RIs for minimum count of EC2 instances to save money.
- **CloudWatch**
	  Your go-to tool for alerting Auto Scaling that you need more or less instances.

#### Scaling Relational Databases

**Scaling vs. Refactoring**

You'll be given scenarios and, unless otherwise specified, refactoring and changing to DynamoDB is a viable scaling choice.

It won't work that easily in the real world, but in the exam, switching database types can solve the problem.

**Relational Database Scaling**
- **Read Replicas**
	  Ready-heavy workload = Read Replicas
- **Careful with Storage**
	  RDS storage only scales up - it won't scale back down.
- **Vertical Scaling**
	  Don't shy away from this scaling type.
- **Multi-AZ**
	  Unless it's a dev environment, turn this on.  Remember that standby instances cannot be directly used.
- **Aurora Everything**
	  Whenever possible, try to use Aurora if the situation calls for a relational database.

#### Scaling Non-Relational Databases

**Pinch Those Pennies!**

Predictable workload?  Pick provisioned capacity.
Sporadic? Pick on-demand.

- **Access Patterns**
	  Know if your access patterns are predictable or unpredictable.
- **Design Matters**
	  Avoid hot keys will also lead to better performance
- **Switching**
	  You can switch, but only twice per 24 hours per table
- **Cost**
	  Keep cost in mind when considering which type of table to pick.

 **4 Questions to Ask in the Exam**

- Is it highly available?
- Which is appropriate for the situation: horizontal or vertical?
- Is it cost effective?
- Would switching databases fix the problem?

**What to Know for the Exam**

- **Auto Scaling is only for EC2. ** No other service can be scaled using Auto Scaling.  Other service might have a built-in option, but they aren't included in Auto Scaling Groups.
- **Get ahead of the workload.** Whenever possible, favor situations that are predictive rather than reactive.
- **Bake AMIs to reduce build times.** You can avoid long provisioning times by putting everything in an AMI.  This is better than using user data whenever possible.
- **Spread out.** Make sure you're spreading your Auto Scaling groups over multiple Availability Zones.
- **Steady state groups** allow us to create a situation where the failure of a legacy codebase or resource that can't be scaled can automatically recover from failure.
- **ELBs are essential.** Make sure you enable health checks from load balancers - otherwise, instances won't be terminated and replaced when they fail health checks.

**Scaling Databases**
- RDS has the most database scaling options
- Horizontal scaling is usually preferred over vertical
- Read replicas are your friend
- DynamoDB scaling comes down to access patterns.

#### Overview of Disaster Recovery Strategies

**RPO**
At what point in time do you want your environment recovered to?

**RTO**
How quickly do you need your environment recovered (i.e., the downtime the business can handle)?

**4 Different Disaster Recovery Strategies**
- **Backup and Restore**
	  Cheapest, but usually the slowest option.
- **Pilot Light**
	  Faster than backup and restore, but some downtime.
- **Warm Standby**
	  Quicker recovery time than pilot light, but slightly more expensive.
- **Active/Active Failover**
	  Most expensive option, but no downtime and lowest RTO and RPO.

---
## Decoupling Workflows

#### Decoupling Workflows Overview

**Always Loosely Couple**
Even when not specifically asked, loose coupling is the answer.

**Never Tightly Couple**
Don't select answers that include instance-to-instance communication

**Internal and External**
Every level of an application should be loosely coupled.

**One Size**
Doesn't fit all.  There's no one single way to decouple

#### Messaging with SQS

**SQS is Important!**
SQS is going to be featured very heavily on the exam.

It's important to be able to explain every single setting we've talked about.  Know what changing these settings would do and why you'd make that change.

- **"What Does That Do?"**
	  It's imperative that you know what all the settings do.
- **Nothing Lasts Forever**
	  Messages can only live up to 14 days.
- **Troubleshooting**
	  You'll need to pinpoint the issues.
- **Polling**
	  It's important to know the difference between long and short polling.  
- **Size**
	  Messages are 256KB of text in any format.

#### Dead Letter Queues

**DLQs Are the Best Sideline**
If the scenario laid out mentions problems with a message in SQS, immediately think about DLQs and visibility timeout.

It's important to set up a CloudWatch alarm to monitor queue depth.

- **Monitor, Monitor, Monitor**
	  Make sure you set up an alarm and alert on queue depth.
- **It's Not Special**
	  DLQs are just SQS queues that are set to receive the reject messages.
- **FIFO Requirements**
	  DLQs for FIFO SQS queues must also be FIFO
- **Same Retention Window**
	  Messages will be held up to 14 days
- **Usable with SNS**
	  You can create SQS DLQ for SNS topics.

#### Ordered Messages with SQS FIFO

**FIFO Is the Only Option**
If you ever see a scenario talking about message ordering, scan for answers that include FIFO.

While there are other ways to enforce message ordering, FIFO is going to be the best option for the exam.

**FIFO Queues**
- **Performance**
	  FIFO queues do not have the same level of performance as standard queues
- **Not the Only Way**
	  You can order messages with SQS standard, but it's on you to do it.
- **Message deduplication ID**
	  Token used for deduplication of sent messages.  Matching deduplication IDs are not delivered during a deduplication interval.
- **Message Group ID**
	  Helps ensure messages are processed one by one in a strict order based on the group.
- **Cost**
	  It costs more since AWS must spend compute power to deduplicate messages.

#### Delivering Messages with SNS

- For scenarios involving alerting, think Amazon SNS
- SNS only supports custom retry policies for HTTP(S) endpoints
- Push-based message application
- FIFO topics support ordering and deduplication
- Be familiar with the different subscriber options
- Know the fanout architectures

#### AWS API Gateway

- APIs are the front doors to your applications and services
- You should favor answers that include API Gateway over those that hardcode access and secret keys
- Any time the exam talks about creating or managing an API, think API Gateway
- API Gateway supports versioning of your API
- You can front API Gateway with a web application firewall (WAF)
- Remember the different endpoint types as well as the security options.

#### AWS Batch

- **Long-Running Workloads**
	  Perfect for long-running and event-driven workloads.  Anything requiring more than 15 minutes.
- **Managed Service**
	  Leverage AWS to handle infrastructure creations and configurations.
- **Jobs and Job Definitions**
	  Jobs: Unites of work submitted to AWS Batch.
	  Job definitions: Blueprints within the job.
	  All jobs get submitted to queues.
- **Batch or Lambda**
	  AWS Lambda has several limitations.  Consider runtimes, execution time limits, and file storage sizes.
- **What Type of Compute?**
	  Use cases determine when to use Fargate or ECS EC2 instances.  Consider the number of jobs and specific resource requirements (e.g., memory and GPU)
- **Managed or Unmanaged?**
	  Managed compute environments leverage AWS for managing capacity and compute.  They are the most common.
	  Unmanaged allows you to manage EVERYTHING.  They are used for specific scenarios.

#### Amazon MQ

- Managed broker service for easily migrating message broker systems to the AWS cloud.
- Allows you to leverage both Apache ActiveMQ or RabbitMQ engine types.
- Consider Amazon MQ for anything specific to JMS or messaging protocols like AMQP 0-9-1,  AMQP 1.0, MQTT, OpenWire, and STOMP
- New applications should try and leverage SNS with SQS
- Amazon MQ restricts access to private networking.  Must have VPC connectivity (e.g., Direct Connect or Site-to-Site VPN)
- Amazon MQ offers HA architectures that are dependent on the engine type.

#### AWS Step Functions

**What is AWS Step Functions**
Serverless orchestration service meant for event-driven task executions using AWS services.  Comes with a graphical interface.

**Execution Types**
Standard workflows are good for long-running, auditable executions.
Express workflows are good for high event rate executions.

**Amazon State Language**
All state machines are written in the Amazon States Language format.

**States**
States are elements within your state machines.  These are things and actions that happen with workflows.

**Many Integrations**
So many AWS service integrations are available! Common ones are AWS Lambda, API Gateway, and Amazon EventBridge.

**State Types**
There are currently eight different state types: Pass, Task, Choice, Wait, Succeed, Fail, Parallel, and Map.

#### Amazon AppFlow

**What is AppFlow?**
Fully managed integration service for transferring data to and from SaaS vendors and applications.

**Bi-Directional**
Flows can be bi-directional between AWS services and SaaS applications.
Ensure your configuration is supported!

**Exam Scenarios**
Watch for solutions needing easy (managed) and fast transfer of SaaS or third party vendor data into AWS services.

Example: An application needs to reference large amounts of SaaS data regularly, and the data needs to be accessed from within S3.

---
## Big Data

#### Amazon Redshift

- Make sure you don't use Redshift in place of RDS
- Redshift is a relational database based on PostgreSQL
- Support a LOT of data; up to 16 PB
- You can deploy a Multi-AZ cluster for high availability
- Use Redshift Spectrum to efficiently and quickly process data in S3
- Leverage Snapshots for point-in-time recovery or restoration to other Regions

#### Elastic MapReduce (EMR)

**What is EMR?**
AWS-managed big data platform that allows you to process vast amounts of data.

**EC2 Purchasing Rules Apply**
remember, you can leverage On-Demand, Reserved, and Spot Instances.

**Toolsets to Keep in Mind**
Built-in support for Spark, Hive, HBase, Flink, Hudi, and Presto.

**Big Data Frameworks**
Keep an eye out for Apache Hadoop and Apache Spark.

**Storage Type Indicators**
Keyword indicators can include Hadoop Distributed File Sstem (HDFS) and EMR File System (EMRFS).

**Use Cases and Scenarios**
Large dataset processing (ETL), web indexing, machine learning training, and large-sale genomics.

#### Kinesis

**Real Time?  Think Kinesis!**
There are very few scenarios in AWS that offer "real-time" processing.  Kinesis is one of the few services that offers this speed.

If you're faced with a question that asks for real-time processing or streaming of data, look for an answer that includes Kinesis.

- **Kinesis vs SQS**
	  Both are message brokers, but only Kinesis is real time.
- **Scaling**
	  Data Streams does not automatically scale.  Data Firehose does.
- **Transforming Data**
	  Data Analytics is the easiest way to process data going through Kinesis using SQL.

#### Amazon Athena and AWS Glue

**Serverless SQL**
If you're ever faced with a scenario that's looking for a serverless SQL solution, Athena is your best choice.  It's the only service that allows you to directly query your data that's stored in S3.

This is a common big data scenario.

**Athena and Glue**
- **Serverless**
	  Both solutions are fully managed serverless services.
- **Overview**
	  Knowing the 3,000 foot view of these services is good enough for this exam.
- **Better Together**
	  While Athena can work by itself, Glue can design a schema for your data.

#### Amazon QuickSight

- Need to look at your data?  Use QuickSight to do it
- If the phrase "Business Intelligence" comes up, look for QuickSight.
- Integrates with Amazon RDS, amazon Aurora, Athena, S3, and many more.
- Per-session and per-user pricing
- Enterprise edition allows for more advanced capabilities
- Be familiar with SPICE

#### AWS Data Pipeline

- **Managed ETL**
	  Managed AWS service for ETL workflows that automates movement and transformations of your data.
- **Data Driven**
	  Use data-driven workflows to create dependencies between tasks and activities.
- **Storage Integrations**
	  There are several integrations, including DynamoDB, RDS, Redshift, an S3.
- **Compute Integrations**
	  Easily integrate it with EC2 and EMR for managed compute needs.
- **Amazon SNS**
	  Leverage SNS for any failure notifications.  Use it for successes and other event-driven workloads as well!
- **Keywords**
	  Anything related to managed ETL services, and automatic retries for data-drive workflows.

#### Amazon Managed Streaming for Apache Kafka (Amazon MSK)

**Apache Kafka**
Fully managed AWS service for running and building Apache Kafka data streaming applications

**Control Plane**
Service handles control-plane operations: creation, updating, and deletion of clusters.

**Data Plane**
Leverage the same Apache Kafka data-plane operations for producing and consuming data.

**Automatic Recoveries**
Service detects and automatically mitigates most of the common failures.

**Encryption**
Integrates with Amazon KMS for SSE needs.  Uses TLS 1.2 for in-transit communications.

**Logging**
Push broker logs to CloudWatch, S3, or Kinesis Data Firehose.  API calls are logged to CloudTrail.

#### Analyzing Data with Amazon OpenSearch Service

**Amazon OpenSearch Service ❤️ Logs**

If you're given a scenario on the exam that talks about creating a logging solution involving visualization of log file analytics or BI reports, there's a good chance OpenSearch will be included.

Know that it is used as a managed analytics and visualization service.

AWS may still reference its predecessor, Elasticsearch.  The same concepts apply!

#### 4 Questions to Ask in the Exam

- What kind of database works?
- How much data do you have?
- Is serverless a requirement?
- How do we optimize costs?

---
## Serverless Architecture

#### Computing with Lambda
- Scenarios involving automating operations tasks will likely include Lambda
- Know the limitations and general use cases for the service.
- Be familiar with the different triggers that can invoke your Lambda functions
- Lambda excels in running small, short, and lightweight functions
- Lambda plays a major role in the AWS ecosystem.  It's easily integrated with many services
- Lambda can run inside or outside a VPC (default)

#### Containers
- **High Level**
	  You won't have to dive into the specifics of a Dockerfile.
- **Ease of Use**
	  Containers help you easily migrate from on-premises to AWS.
- **Dev vs. Prod**
	  Dev is prod, and prod is dev (but it's a good thing with containers!).

#### Running Containers in ECS or EKS