
# Elastic Cloud Computer (EC2)

## EC2 Overview

Secure, resizable compute capacity in the cloud.

- Like a VM, only hosted in AWS instead of your own datacenter.
- Designed to make web-scale cloud computing easier for developers.
- The capacity you want when you need it.
- You are in complete control of your own instances.

Launched in 2006

**Game Changer**
AWS led a big change in the industry by introducing EC2.

**Pay Only for What You Use**
EC2 changed the economics of computing.

**No Wasted Capacity**
Select the capacity you need right now.  Grow and shrink when you need.

### On-Premise Infrasctructure

**Estimate Capacity**
Long-term investment, 3-5 years.
Expectation that the application will "grow into" it.
Wasted Capacity

### EC2 Pricing Options

**On-Demand**
Pay by the hour or the second, depending on the type of instance you run.

**Reserved**
Reserved capacity for 1 or 3 year.s. Up to 72% discount on the hourly charge.

**Spot**
Purchase unused capacity at a discount of up to 90%.  Prices fluctuate with supply and demand.

**Dedicated**
A physical EC2 server dedicated for your use.  The most expensive option.

### On-Demand Instances

**Typical Use Cases**
- **Flexible**
	  Low cost and flexibility of Amazon EC2 without any upfront payment or long-term commitment.
- **Short Term**
	  Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted.
- **Testing the Water**
	  Applications being developed or tested on Amazon EC2 for the first time.

### Reserved Instances

- **Predictable Usage**
	  Applications with steady state or predictable usage.
- **Specific Capacity Requirements**
	  Applications that require reserved capacity.
- **Pay up Front**
	  You can make upfront payments to reduce the total computing costs even further
- **Standard RIs**
	  Up to 72% off the on-demand price.
- **Convertible RIs**
	  Up to 54% off the on-demand price. Has the option to change to a different RI type of equal or greater value.
- **Scheduled RIs**
	  Launch within the time window you define. Match your capacity reservation to a predictable recurring schedule that only requires a fraction of a day, week, or month.

Note: Reserved Instances apply on a **Regional** level.

**Savings Plans with Reserved Instances**
- Save up to 72%
	  All AES compute usage, regardless of instance type or Region.
- Commit to 1 or 3 years
	  Commit to use a specific amount of compute power (measured by the hour) for a 1-year or 3-year period.
- Super Flexible
	  Not only EC2, this also includes serverless technologies like lambda and Fargate.

### Spot Instances

**When to Use Spot Instances**
- Applications that have flexible start and end times.
- Applications that are only feasible at very low compute prices.
- User with an urgent need for large amounts of additional computing capacity.

**Example Use Cases**
- Image Rendering
- Genomic sequencing
- Algorithmic trading engines

### Dedicated Hosts

**Typical Use Cases** 

- **Compliance**
	  Regulatory requirements that may not support multi-tenant virtualization.
- **Licensing**
	  Great for licensing that does not support multi-tenancy or cloud deployments.
- **On-Demand**
	  Can be purchased on-demand (hourly).
- **Reserved**
	  Can be purchased as a reservation for up to 70% off the on-demand price.  The longer the contract and the more you pay up front, the bigger the discount.

### AWS Pricing Calculator

Explore AWS services and pricing using the **AWS Pricing Calculator**
https://calculator.aws

### Exam Tips

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

---

## Launching an EC2 Instance

1. Navigate to the EC2 Dashboard
2. Select Launch Instance
3. Name the instance. *ex. MyWebServer1*
4. Select an Amazon Machine Image (AMI).  *ex. Amazon Linux 64-bit (x86)*
5. Select Instance Type.  *ex. t2.micro*
6. Create a new Key Pair.  RSA/.pem
7. Under Network Settings, create a Security Group
	- Allow SSH Traffic from Anywhere
	- Allow HTTPS traffic from the internet
	- Allow HTTP traffic from the internet
8. Configure Storage
9. Select **Launch Instance**

---
## AWS Command Line

You can **interact with AWS** using the Command Line.

The AWS command line allows you to **interact with AWS simply by typing commands.** In this example we list all our buckets in S3.
```bash
aws s3 ls
```


### Create Access Keys

When using the CLI, if it has not been configured with credentials you may see the message:
```bash
Unable to locate credentials.  You can configure credentials by running "aws configure".
```

In order to generate these access keys, navigate to **IAM** and perform the following:
1. Navigate to **User Groups**
2. Create new User Group
	1. Give it a name (ex. Admin-S3)
	2. Attach permissions policies (ex. AmazonS3FullAccess)
	3. Create Group
3. Create a User
	1. Provide a username (ex. AlexSmith)
	2. Add user to the group created above
	3. Create User
4. Navigate to the user's **Security Credentials**
5. Create **Access Keys**
	1. Use case - Command Line Interface (CLI)
	2. Next -> Create
6. The keys will now be available to copy which you can then use with the `aws configure` command.
### Exam Tips

#### Least Privilege
Always give your users the **minimum amount** of access required to do their job.

#### Use Groups
**Create IAM groups** and assign your users to groups.  Group permissions are assigned using IAM policy documents.
Your users will **automatically inherit** the permissions of the group.

### AWS CLI

- **Secret Access Key**
	  You will only see this once!  If you lose it, you can delete the access key ID and secret access key and regenerate them.  You will need to run `aws configure` again.
- **Don't Share Key Pairs**
	  Each developer should have their own access key ID and secret access key.  Just like passwords, they should not be shared.
- **Support Linux, Window, MacOS**
	  You can install the CLI on your Mac, Linux, or Windows PC.  You can also use it on EC2 instances

---
## Using Roles

### What is an IAM Role

A role is an identity you can create in IAM that has specific permissions.  A role is similar to a user, as it has an AWS identity with permission policies that determine what the identity can and cannot do in AWS.

However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it.

### Roles are Temporary

A role does not have standard long-term credentials the same way passwords or access keys do.  Instead, when you assume a role, it provides you with temporary security credentials for your role session.


### What Else Can Roles Do?

Roles can be assumed by people, AWS architecture, or other system-level accounts.

Roles can allow cross-account access.  This allows one AWS account the ability to interact with resources in other AWS accounts.

### Exam Tips

#### What to Remember When Using Roles
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

---
## Security Groups and Bootstrap Scripts

### How Computers Communicate

| Action | Protocol | Port |
| --- | --- | --- |
| Linux Administration | SSH | 22 |
| Windows Administration | RDP | 3389 |
| Web Browsing | HTTP | 80 |
| Encrypted Web Browsing (SSL) | HTTPS | 443 |

### Security Groups

Security groups are **virtual firewalls for your EC2 instance**.  By default, everything is blocked.

**TO LET EVERYTHING IN: 0.0.0.0/0**

In order to be able to **communicate to your EC2 instances via SSH/RDP/HTTP**, you will need to **open up the correct ports.**

### Bootstrap Scripts

**A script that runs when the instance first runs.**

*Example bootstrap script to install and start apache web server*
```bash
#!/bin/bash
yum install httpd -y
# installs apache
yum service httpd start
# starts apache
```

Adding these tasks at boot time **adds to the amount of time it takes to boot the instance**.  However, it allows you to **automate the installation** of applications.

Bootstrap scripts are entered in the **Advanced -> User Data** section when launching or modifying an EC2 instance.
### Security Groups Exam Tips

- Changes to security groups take effect immediately.
- You can have any number of EC2 instances within a security group.
- You can have multiple security groups attached to EC2 instances.
- All inbound traffic is blocked by default.
- All outbound traffic is allowed.

#### Bootstrap Scripts

A bootstrap script is **a script that runs when the instance first runs.**  It passes user data to the EC2 instance and can be used to install applications (like web servers and databases), as well as do updates and more.

---

## EC2 Metadata and User Data

### What is EC2 Metadata?

**EC2 metadata is simply data about your EC2 instance.**
	This can include information such as private IP address, public IP address, hostname, security groups, etc.

### Retrieving Metadata

Using the `curl` command, we can query metadata about our EC2 instance.

```bash
TOKEN=$(curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
PUBLIC_IP=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/public-ipv4)
```

Items which can be queried
- ami-id
- ami-launch-index
- ami-manifest-path
- block-device-mapping
- events/
- hibernation/
- hostname
- identity-credentials/
- instance-action
- instance-id
- instance-life-cycle
- instance-type
- local-hostname
- local-ipv4
- mac
- managed-ssh-keys/
- metrics/
- network/
- placement/
- profile
- public-hostname
- public-ipv4
- public-keys/
- reservation-id
- security-groups

### Exam Tips

#### User Data vs. Metadata
- User data is simply bootstrap scripts.
- Metadata is data about your EC2 instances.
- You can use bootstrap scripts (user data) to access metadata.

---
## Networking with EC2

You can attach 3 different types of **virtual networking cards** to your EC2 instances.

**ENI - Elastic Network Interface**
	For basic, day-to-day networking

**EN - Enhanced Networking**
	Uses single root I/O virtualization (SR-IOV) to provide high performance

**EFA - Elastic Fabric Adapter**
	Accelerates High Performance Computing (HPC) and machine learning applications.

### ENI - Elastic Network Interface

An **ENI** is simply a virtual network card that allows:
- Private IPv4 Addresses
- Public IPv4 Address
- Many IPv6 Addresses
- Mac Address
- 1 or More Security Groups

**Common ENI Use Cases**
- Create a management network.
- Use network and security appliances in your VPC.
- Create dual-homed instances with workloads/roles on distinct subnets.
- Create a low-budget, high availability solution.

### EN - Enhanced Networking

#### For High-Performance Networking between **10 Gbps - 100 Gbps**

**Single Root I/O Virtualization (SR-IOV)**
SR-IOV provides higher I/O performance and lower CPU utilization

**Performance**
Provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies.

#### Comes in two different types

**Elastic Network Adapter (ENA)**
Supports networking speeds of up to 100 Gbps for supported instance types

**Intel 82599 Virtual Function (VF) Interface**
Supports network speeds of up to 10 Gbps for supported instance types.  Typically used on older instances.

Always choose **ENA** over **VF**

### EFA - Elastic Fabric Adapter

A network device you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications.

Provides lower and more consistent latency and higher throughput than the TCP transport traditionally used in cloud-based HPC systems.

**EFA can use OS-Bypass**

It makes it a lot faster with much lower latency.

IS0bypass enables HPC and machine learning applications to bypass the operating system kernel and communicate directly with the EFA device.  Not currently supported with Windows -- only Linux.

### Exam Tips

**For different scenarios on the exam, choose the correct networking device.**

**ENI**
For basic networking.  Perhaps you need a separate management network from your production network or a separate logging network, and you need to do this at a low cost.  In this scenario, use multiple ENIs for each network.

**EN (Enhanced Networking)**
For when you need speeds between 10 Gbps and 100 Gbps.  Anywhere you need reliable, high throughput.

**EFA**
For when you need to accelerate High Performance Computing (HPC) an machine learning applications or if you need to do an OS-bypass.  If you see a scenario question mentioning HPC or ML and asking what networking adapter you want, choose EFA.

---
## Optimizing with EC2 Placement Groups

### Cluster Placement Groups

**Grouping of instances within a single Availability Zone**. Recommended for applications that need low network latency, high network throughput, or both.

Only certain instance types can be launched into a cluster placement group.

### Spread Placement Groups

A spread placement group is a group of instance that are **each placed on distinct underlying hardware**.

Spread placement groups are recommended for applications that have a small number of critical instances that should be kept separate from each other.

**Used for Individual Instances**

### Partition Placement Groups

Each partition placement group has its own set of racks.  Each rack has its own network and power source.  No two partitions within a placement group share the same racks, allowing you to isolate the impact of hardware failure within your application.

**EC2 divides each group into logical segments called partitions.**

**Used for Multiple Instances**

### Exam Tips

#### 3 Types of Placement Groups

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

---
## Solving Licensing Issues with Dedicated Hosts

### Dedicated Host

A physical EC2 server dedicated for your use.  The most expensive EC2 pricing model.

**Licensing**
Great for licensing that does not support multi-tenancy or cloud deployments

Can be purchased **On-Demand** (hourly) or **Reserved** for up to 70% off the on-demand price.

### Exam Tip

**Any question that talks about special licensing requirements.**

An **Amazon EC2 Dedicated Host** is a **physical server** with EC2 instance capacity fully dedicated to your use.  Hosts allow you to **use your existing** per-socket, per-core, or per-VM software **licenses**, including Windows Server, Microsoft SQL Server, and SUSE Linux Enterprise Server.

---

## Timing Workloads with Spot Instances and Spot Fleets

**Amazon EC2 Spot Instances** let you take advantage of unused EC2 capacity in the AWS Cloud.  Spot instances are available at up to a 90% discount compared to On-Demand prices.

**When to Use Spot Instances**
Stateless, fault-tolerant, or flexible applications

Application such as big data, containerized workloads, CI/CD, high-performance computing (HPC), and other test and development workloads.

### Spot Prices

To use **Spot Instances,** you must first decide on your maximum Spot price.  The instance will be provisioned so long as the Spot price is **BELOW** your maximum Spot price.

- The **hourly Spot price** varies depending on capacity and region.
- If the Spot price goes above your maximum, you have **2 minutes** to choose whether to stop or terminate your instance.

**Spot Instances are useful for the following tasks:**

- Big data and analytics
- Containerized workloads
- CI/CD and testing
- Image and media rendering
- High-performance computing

**Spot Instances are not good for:**
- Persistent workloads
- Critical jobs
- Databases


### Terminating Spot Instances

**Spot Request**
- Maximum price
- Desired number of instances
- Launch specification (AMI etc)
- Request type: one-time | persistent
- Valid from, Valid until

For persistent Spot Requests you need to first cancel the Spot Request before terminating the instances.  Otherwise they will be re-provisioned under the spot request

### Spot Fleets

**A Spot Fleet is a collection of Spot Instances and (optionally) On-Demand Instances.**

The **Spot Fleet** attempts to launch the number of Spot instances and On-Demand instances to meet the target capacity you specified in the Spot Fleet request.  The request for Spot instances is fulfilled if there is available capacity and the **maximum price you specified in the request exceeds the current Spot price.**. The Spot Fleet also attempts to maintain its target capacity fleet if your Spot Instances are interrupted.

**Spot Fleets will try to match the target capacity with your price restraints.**

### Launch Pools

- Set up different launch pools.  Define things like **EC2** instance type, operating system, and Availability Zone.
- You can have **multiple** pools, and the fleet will choose the best way to implement depending on the strategy you define.
- Spot fleets will **stop launching instances** once you reach the price threshold or capacity desire.

### Strategies for Spot Fleets

**You can have the following strategies with Spot Fleets.**

##### `capacityOptimized`
The Spot Instances come from the pool with optimal capacity for the number of instances launching.

##### `diversified`
The Spot Instances are distributed across all pools.

##### `lowestPrice`
The Spot Instances come from the pool with the lowest price.  This is the default strategy.

##### `InstancePoolsToUseCount`
The Spot Instances are distributed across the number of Spot Instance pools you specify.  This parameter is valid only when used in combination with `lowestPrice`.

### Exam Tips

- Spot Instances save up to 90% of the cost of On-Demand instances.
- Useful for any type of computing where you don't need persistent storage.
- A Spot Fleet is a collection of Spot Instances and (optionally) On-Demand instances.
---
## Deploying vCenter in AWS with VMware Cloud on AWS

### Why Use VMware on AWS?

VMWare is used by organizations around the world for **private cloud deployments.**. Some organizations opt for a **hybrid cloud strategy** and would like to leverage AWS services.

### Use Cases for VMware

- **Hybrid Cloud**
	  Connect your on-premises cloud to the AWS public cloud, and manage a hybrid workload.
- **Cloud Migration**
	  Migrate your existing cloud environment to AWS using VMware's built-in tools.
- **Disaster Recovery**
	  VMware is famous for its disaster recovery technology.  Using hybrid cloud, you can have an inexpensive disaster recovery environment on AWS.
- **Leverage AWS**
	  Use over 200 AWS services to update your applications or to create new ones.

### VMware Cloud on AWS
#### How is it deployed?
- It runs on dedicated hardware hosted in AWS using a single AWS account.
- Each host has two sockets with 18 cores per socket, 512 GiB  RAM, and 15.2 TB Raw SSD storage.
- Each host is capable of running multiple VMware instances (up to the hundreds).
- Clusters can start with two hosts up to a maximum of 16 hosts per cluster.

### Exam Tips

**You can deploy vCenter on AWS Cloud using VMware.**

Perfect solution for extending your private VMware Cloud into the AWS public cloud.

---

## Extending AWS Beyond the Cloud with AWS Outposts

### What is Outposts?

Outposts brings the **AWS data center directly to you, on premises.**. Outposts allows you to have the large variety of AWS services in your data center.  You can have Outposts in sizes such as 1U and 2U servers all the way up to 42U racks and multiple-rack deployments.

### Benefits of Outposts

##### Hybrid Cloud
Create a hybrid cloud where you can leverage AWS services inside your own data center.

##### Fully Managed Infrastructure
AWS can manage the infrastructure for you. You do not need a dedicated team to look after your Outposts infrastructure.

##### Consistency
Bring the AWS Management Console, APIs, and SDKs into your data center, allowing uniform consistency in your hybrid environment.


### Family Members

#### Outposts Rack

**Hardware**
Available starting with a single 42U rack and scale up to 96 racks.

**Services**
Provides AWS compute, storage, database, and other services locally.

**Results**
Gives the same AWS infrastructure, services and APIs in your own datacenter.

#### Outposts Server

**Hardware**
Individual servers in 1U or 2U form factor

**Use Cases**
Useful for small space requirements, such as retail stores, branch offices, healthcare provider locations, or factory floors.

**Results**
Provides local compute and networking services.

### Process

1. **Order** 
	 Log in to the AWS Management Console, and order your Outpost configuration.
2. **Install**
	   AWS staff will come on-site to install and deploy the hardware, including power, networking, and connectivity.
3. **Launch**
	   Using the AWS Management Console, you can launch instances on your Outpost on-site.
4. **Build**
	   Start building your on-site AWS environment.

### Exam Tips

**Scenario about extending AWS to your data center?**
**Think AWS Outposts.**

AWS Outposts racks for **large deployments**
AWS Outposts server for **smaller deployments**


---

## EC2 Instance Bootstrapping

```bash
#!/bin/bash

# Update APT
sudo apt-get update && sudo apt-get upgrade -y
# Install Apache and unzip
sudo apt-get install apache2 unzip -y
# Download AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
# Install the AWS CLI
unzip awscliv2.zip
sudo ./aws/install
# Allow editing of the index.html page
sudo chmod 777 /var/www/html/index.html
# Update index.html with metadata
echo '<html><h1>Bootstrap Demo</h1><h3>Availability Zone: ' > /var/www/html/index.html 
curl http://169.254.169.254/latest/meta-data/placement/availability-zone >> /var/www/html/index.html 
echo '</h3> <h3>Instance Id: ' >> /var/www/html/index.html 
curl http://169.254.169.254/latest/meta-data/instance-id >> /var/www/html/index.html 
echo '</h3> <h3>Public IP: ' >> /var/www/html/index.html 
curl http://169.254.169.254/latest/meta-data/public-ipv4 >> /var/www/html/index.html 
echo '</h3> <h3>Local IP: ' >> /var/www/html/index.html 
curl http://169.254.169.254/latest/meta-data/local-ipv4 >> /var/www/html/index.html 
echo '</h3></html> ' >> /var/www/html/index.html
# Install mysql
sudo apt-get install mysql-server -y
sudo systemctl enable mysql
```
