# Elastic Block Storage (EBS) and Elastic File System (EFS)
## EBS Overview

**What are EBS Volumes?**
The different types of EBS volumes available. 
Use cases for each type.

### Elastic Block Store
Storage volumes you can attach to your EC2 instances.
Essentially a virtual hard disk attached to your EC2 instance.

Use them the same way you would use any system disk.
- Create a filesystem
- Run a database
- Run an operating system
- Store data
- Install applications

### Mission Critical

- **Production Workloads**
	  Designed for mission-critical workloads.
- **Highly Available**
	  Automatically replicated within a single Availability Zone to protect against hardware failures.
- **Scalable**
	  Dynamically increase capacity and change the volume type with no downtime or performance impact to your live systems.

### EBS Volume Types

#### Solid State Disk
- **General Purpose SSD (gp2)**
	- 3 IOPS per GiB, **up to a maximum of 16,000 IOPS per volume**
	- gp2 volumes **smaller than 1TB** can burst up to 3,000 IOPS
	- **Good for boot volumes** or development and test applications that are not latency sensitive
- **General Purpose SSD (gp3)**
	- New generation of gp
	- Predictable **3,000 IOPS baseline performance** and 125 MiB/s regardless of volume size.
	- Ideal for applications that **require high performance at a low cost**, such as MySQL, Cassandra, virtual desktops, and Hadoop analytics.
	- Customers looking for higher performance **can scale up** to 16,000 IOPS and 1,000 BiB/s for an additional fee.
	- The top performance of **gp3** is **4 times faster than max** throughput of gp2 volumes.
- **Provisioned IOPS SSD (io1)**
	- The high-performance option and the most expensive
	- Up to 64,000 IOPS per volume.  50 IOPS per GiB.
	- Use iff you need more than 16,000 IOPS.
	- Designed for I/O-intensive applications, large databases, and latency-sensitive workloads.
- **Provisioned IOPS SSD (io2)**
	- Latest generation.
	- Higher durability and more IOPS.
	- Same price as **io1**
	- 500 IOPS per GiB **Up to 64,000 IOPS**
	- 99.999% durability **instead of 99.9% durability**
	- I/O-intensive apps, large databases, and latency-sensitive workloads.  **Applications that need high levels of durability.**

#### Hard Disk Drive (MB/s-Intensive)
- **Throughput Optimized HDD (st1)**
	- Low-cost HDD volume
	- Baseline throughput of 40 MB/s per TB
	- Ability to burst up to 250 MB/s per TB
	- Maximum throughput of 500 MB/s per volume
	- Frequently access, throughput-intensive workloads
	- Big data, data warehouses, ETL, and log processing
	- A cost-effective way to store mountains of data
	- Cannot be a boot volume
- **Cold HDD (sc1)**
	- Lowest Cost Option
	- Baseline throughput of 12 MB/s per TB
	- Ability to burst up to 80 MB/s per TB
	- Max throughput of 250 MB/s per volume
	- A good choice for colder data requiring fewer scans per day
	- Good for applications that need the lowest cost and performance is not a factor
	- Cannot be a boot volume

### IOPS vs. Throughput

**IOPS**
- Measures the number of read and write operations per second
- Important metric for quick transactions, low-latency apps, transactional workloads
- The ability to action reads and writes very quickly
- If you have a transactional database, choose Provisioned IOPS (io1 or io2)

**Throughput**
- Measure the number of bits read or written per second (MB/s)
- Important metric for large datasets, large I/O sizes, complex queries
- The ability to deal with large datasets
- If dealing with Big Data, data warehouse, ETL choose Throughput Optimized (st1)

### Exam Tips

**EBS: SSD Volumes**
Highly available and scalable storage volumes **you can attach to an EC2 instance.**

#### gp2
General Purpose SSD
- Suitable for boot disks and general applications
- Up to 16,000 IOPS per volume
- Up to 99.9% durability

#### gp3
General Purpose SSD
- Suitable for high performance applications
- Predictable 3,000 IOPS baseline performance and 125 MiB/s regardless of volume size
- Up to 99.9% durability

#### io1
Provisioned IOPS SSD
- Suitable for OLTP and latency-sensitive applications
- 50 IOPS/GiB
- Up to 64,000 IOPS per volume
- High performance and most expensive
- Up to 99.9% durability

#### io2
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

---
## Volumes and Snapshots

### What Are Volumes?

**Volumes exist on EBS**
Volumes are simply virtual hard disks.  You need a minimum of 1 volume per EC2 instance. **This is called the root device volume.**

### What Are Snapshots?

- **Snapshots exist on S3**
	  Think of snapshots as a photograph of the virtual disk/volume.
- **Snapshots are point in time**
	  When you take a snapshot, it is a point-in-time copy of a volume.
- **Snapshots are incremental**
	  This means only the data that has been changed since your last snapshot are moved to S3.  This saves dramatically on space and the time it takes to take the snapshot.
- **The first snapshot**
	  If it is your first snapshot, it may take some time to create as there is no previous point-in-time copy.

### 3 Tips for Snapshots

**Consistent Snapshots**
Snapshots only capture data that has been written to your Amazon EBS volume, which might exclude any data that has been locally cached by your application or OS.  For a consistent snapshot, it is recommended you stop the instance and take a snap.

**Encrypted Snapshots**
If you take a snapshot of an encrypted EBS volume, the snapshot will be encrypted automatically.

**Sharing Snapshots**
You can share snapshots, but only in the region in which they were created.  To share to other regions, you will need to copy them to the destination region first.

### What to Know about EBS Volumes

- **Location**
	  **EBS volumes will always be in the same AZ as EC2**
	  Your EBS volumes will always be in the same AZ as the EC2 instance to which it is attached.
- **Resizing**
	  **Resize on the fly**
	  You can resize EBS volumes on the fly.  You do not need to stop or restart the instance.  However, you will need to extend the filesystem in the OS so the OS can see the resized volume.
- **Volume Type**
	  **Switch volume types.**
	  You can change volume types on the fly (e.g. go from gp2 to io2).  You do not need to stop or restart the instance.

### Exam Tips

**EBS Volumes and Snapshots**

-  Volumes exist on EBS, whereas snapshots exist on S3.
- Snapshots are point-in-time photographs of volumes and are incremental in nature.
- The first snapshot will take some time to create.  For consistent snapshots, stop the instance and detach the volume.
- You can share snapshots between AWS accounts as well as between regions, but first you need to copy that snapshot to the target region.
- You can resize EBS volumes on the fly as well as changing the volume types.

---

## Protecting EBS Volumes with Encryption

### EBS Encryption

EBS encrypts your volume with a data key using the industry-standard AES-256 algorithm.  Amazon EBS encryption uses AWS Key Management Service (AWS KMS) customer master keys (CMK) when creating encrypted volumes and snapshots.

### What happens when you encrypt

- Data at rest is encrypted inside the volume.
- All data in flight moving between the instance and the volume is encrypted.
- All snapshots are encrypted.
- All volumes created from the snapshot are encrypted.

### Encryption Explored

- **Handled Transparently**
	  Encryption and decryption are handled transparently (you don't need to do anything).
- **Latency**
	  Encryption has a minimal impact on latency.
- **Copying**
	  Copying an unencrypted snapshot allows encryption.
- **Snapshots**
	  Snapshots of encrypted volumes are encrypted.
- **Root Device Volumes**
	  You can now encrypt root device volumes upon creation.

### How to Encrypt Existing Volumes

1. Create a snapshot of the unencrypted root device volume.
2. Create a copy of the snapshot and select the encrypt option.
3. Create an AMI from the encrypted snapshot.
4. Use that AMI to launch new encrypted instances.

---
## EC2 Hibernation

### What is EC2 Hibernation?

When you hibernate an EC2 instance, the operating system is told to perform hibernation (suspend-to-disk).  Hibernation **saves the contents** from the instance memory (RAM) to your EBS root volume.  We persist the instance's Amazon root volume and any attached Amazon EBS data volumes.

### EC2 Hibernation in Action

**When you start your instance out of hibernation**

- The **Amazon EBS** root volume is restored to it's previous state.
- The **RAM** contents are reloaded.
- The processes that were previously running on the instance are resumed.
- Previously attached data volumes are **reattached and the instance retains its instance ID.**

With EC2 hibernation, **the instance boots much faster**.  The operating system does not need to reboot because the in-memory state (RAM) is preserved.  This is useful for:
- Long-running processes
- Services that time time to initialize

### Exam Tips

- **EC2 hibernation** preserves the in-memory RAM on persistent strage (EBS)
- **Much faster to boot up** because you **do not need to reload the operating system.**
- **Instance RAM** must be less than 150GB
- **Instance families include instances in** General Purpose, Compute, Memory and Storage Optimized groups.
- **Available for** Windows, Amazon Linux 2 AMI, and Ubuntu.
- **Instances. can't be hibernated** for more than **60 days**
- **Available for** On-Demand instances and Reserved instances.

---
## EFS Overview

### What is EFS?

**Amazon Elastic File System**
- Managed NFS (network file system) that can be mounted on many EC2 instances.
- EFS works with EC2 instances in multiple Availability Zones.
- Highly available and scalable; however, it is expensive.
### Use Cases

**Content Management**
Great fit for content management systems, as you can easily share content between EC2 instances.

**Web Servers**
Also a great fit for web servers.  Have just a single folder structure for your website.
### Overview

- Uses NFSv4 Protocol
- Compatible with Linux-based AMI (Windows not supported a this time)
- Encryption at rest using KMS
- File system scales automatically; no capacity planning required
- Pay per use
### Performance

- **1000s of Concurrent Connections**
	  EFS can support thousands of concurrent connections (EC2 instances).
- **10 Gbps Throughput**
	  EFS can handle up to 10 Gbps in throughput.
- **Petabytes Scaling**
	  Scale your storage to petabytes.
### Controlling Performance

When creating an EFS file system, you can set what performance characteristics you want.

| General Purpose | Max I/O |
| --- | ---|
| Used for things like web servers, CMS, etc | Used for big data, media processing, etc. |

### Storage Tiers

EFS comes with storage tiers and lifecycle management, allowing you to move your data from one tier to another after X number of days.

- **Standard**
	  For frequently access files.
- **Infrequently Accessed**
	  For files not frequently accessed.

### Exam tips

- Supports Network File System version 4 (NFSv4) protocol
- Only pay for the storage you use (no pre-provisioning required)
- Can scale up to petabytes
- Can support thousands of concurrent NFS connections
- Data is stored across AZs within a region
- Read-after-write consistency.

**If you have a scenario-based question around highly scalable shared storage using NFS, think EFS.**

---
## FSx Overview

### FSx for Windows

Amazon FSx for Windows File Server provides a **fully managed native Microsoft Windows file system** so you can **easily move** your Windows-based applications that require file storage to AWS.

Amazon FSx is built on windows server.
### FSx for Windows vs. EFS

How is FSx for Windwos different from EFS?

**FSx for Windows**
- A managed Windows Server that **runs Windows Server message Block** (SMB)-based file services.
- **Designed for Windows** and Windows applications.
- **Supports** AD users, access control lists, groups, and security policies , along with Distributed File System (DFS) namespaces and replication.

**EFS**
- A managed NAS filer for **EC2 instances based on Network File System** (NFS) version 4
- One of the **first network file sharing protocols** native to Unix and Linux.

### FSx for Lustre

A fully managed file system that is optimized for compute-intensive workloads

- High Performance Computing
- Machine Learning
- Media Data Processing Workflows
- Electronic Design Automation

### FSx for Lustre Performance

With Amazon FSx, you can **launch and run a Lustre file system that can process massive datasets at up to hundreds of gigabytes per second** of throughput, millions of IOPS, and sub-millisecond latencies.

### Exam Tips

**In the exam, you'll be given different scenarios and asked to choose whether you should use EFS, FSx for Windows, or FSx for Lustre**

- **EFS:** When you need distributed, highly resilient storage for Linux instances and Linux-based applications.
- **Amazon FSx for Windows:** When you need centralized storage for Windows-based applications, such as SharePoint, Microsoft SQL Server, Workspaces, IIS Web Server, or any other native Microsoft application.
- **Amazon FSx for Lustre:** When you need high-speed, high-capacity distributed storage.  This will be fore applications that do high performance computing (HPC), financial modeling, etc.  Remember that FSx for Lustre can store data directly on S3.

---
## Amazon Machine Images: EBS vs. Instance Store

### What is an AMI?

An Amazon Machine Image (AMI) provides the information required to launch an instance.

**You must specify an AMI when you launch an instance.**

### 5 Things You Can Base Your AMI On

- Region
- Operating System
- Architecture (32-bit or 64-bit)
- Launch permissions
- Storage for the root device (root device volume)

### EBS vs. Instance Store

**All AMIs are categorized as either being backed by:**

- **Amazon EBS**
	  The root device for an instance launched from the AMI is an Amazon EBS volume created from an EBS snapshot.
- **Instance Store**
	  The root device for an instance launched from the AMI is an instance store volume created from a template stored in Amazon S3.

### Instance Store Volumes

Instance store volumes are sometimes called ephemeral storage.  Instance store volumes **cannot be stopped.**  If the underlying host fails, you will lose your data.  You can, however, reboot the instance without losing your data.

If you delete the instance, you will lost the instance store volume.

### EBS volumes

EBS-backed instances **can be stopped.**  You will not lose the data on this instance if it is stopped.  You can also reboot an EBS volume and not lose your data.

By default, the root device volume will be deleted on termination.  However, you can tell AWS to keep the root device volume with EBS volumes.

### Exam Tips

- Instance store volumes are sometimes called ephemeral storage.
- Instance store volumes **cannot be stopped**.  If the underlying host fails, you will lose your data.
- EBS-backed instances **can be stopped.** You will not lose the data on this instance if it is stopped.
- You can reboot both EBS and instance store volumes and you will not lose your data.
- By default, both root volumes will be deleted on termination.  However, with EBS volumes, you can tell AWS to keep the root device volume.

---
## AWS Backup

### What is AWS Backup?

Backup allows you to consolidate your backups across multiple AWS services, such as EC2, EBS, EFS, Amazon FSx for Lustre, Amazon FSx for Windows File Server, and AWS Storage Gateway.

**It can include other services, such as database technologies like RDS and DynamoDB**

### AWS Backup with Organizations

Backup can be used with AWS Organizations to back up multiple AWS accounts in your organization.

**It gives you centralized control accross all AWS services, in multiple AWS accounts across the entire AWS Organization.**

### Benefits of AWS Backup
- **Central Management**
	  Use a single central backup console, allowing you to centralize your backups across multiple AWS services and multiple AWS accounts.
- **Automation**
	  You can create automated backup schedules and retention policies.  You can also create lifecycle policies, allowing you to expire unnecessary backups after a period of time.
- **Improved Compliance**
	  Backup policies can be enforced while backups can be encrypted both at rest and in transit, allowing alignment to regulatory compliance.  Auditing is made easy due to a consolidated view of backups across many AWS services.


### Exam Tips

- **Consolidation:** Use AWS Backup to back up AWS services, such as EC2, EBS, EFS, Amazon FSx for Lustre, Amazon FSx for Windows File Server, and AWS Storage Gateway.
- **Organizations:** You can use AWS Organizations in conjunction with AWS Backup to back up your different AWS services across multiple AWS accounts.
- **Benefits:** Backup give you centralized control, letting you automate your backup and define lifecycle policies for your data.  You get better compliance, as you can enforce your backup policies, ensure your backups are encrypted, and audit them once complete.



