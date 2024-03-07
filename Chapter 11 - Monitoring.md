# Monitoring
## CloudWatch Overview

CloudWatch is a **monitoring** and **observability** platform that was designed to give us insight into our AWS architecture.  It allows us to monitor multiple levels of our applications and **identify** potential issues.

### CloudWatch Features
- **System Metrics**
	  These are metrics that you get out of the box.  The more managed the service is, the more you get.
- **Application Metrics**
	  By installing the CloudWatch agent, you can get information from inside your EC2 instances.
- **Alarms**
	  Alarms can alert you when something goes wrong.

### Metrics
##### Default
These metrics are provided out of the box and do not require any additional work on your part to configure
- CPU Utilization
- Network Throughput
##### Custom
These metrics will need to be provided by using the CloudWatch agent installed on the host.
- EC2 Memory Utilization
- EBS Storage Capacity

---
## Application Monitoring with CloudWatch Logs

### CloudWatch Logs
CloudWatch Logs is a tool that allows you to **monitor**, store**,** and **access log files** from a variety of sources.  It gives you the ability to query your logs to look for **potential issues** or data that is relevant to you.

For custom logs, you use the **Amazon CloudWatch Agent**.  The agent enables users to collect and monitor system and application metrics on their AWS instances and on-premises servers.

### CloudWatch Logs Terms

**Log Event**
This is the record of what happened. It contains a timestamp and the data.

**Log Stream**
A collection of **log events** from the same source creates a **log stream**.  Think of one continuous set of logs from a single instance.

**Log Group**
This is a collection of **log streams**.  For example, you would group all your Apache web server logs across hosts together.

### Features

##### Filter Patterns
You can look for specific terms in your logs.  Think 400 errors in your web server logs.

##### CloudWatch Logs Insights
This allows you to query all your logs using a SQL-like interactive solution.

##### Alarms
Once you've identified your trends or patters, it's time to set up alerts for them.

### Exam Tip

**Logs Should Go to CloudWatch Logs**
Except for situations where we don't need to process them. Then, they should go straight to S3.

---
## Amazon Managed Service for Prometheus and Amazon Managed Grafana

### What is Amazon Managed Grafana
Fully managed AWS service allowing secure data visualizations for instantly **querying, correlating, and visualizing your operational metrics, logs, and traces** from different sources.

### Amazon Managed Grafana Overview
- **Grafana Made Easy**
	  Easily deploy, operate, and scale **Grafana** within your AWS accounts.
- **Logical Seperation**
	  **Workspaces** (logical Grafana servers) allow for seperation of data visualizations and querying.
- **AWS Managed**
	  AWS handles scaling, setup, and maintenance of all workspaces.  **You only worry about tasks!**
- **Secure**
	  Built-in security features help you meet corporate **governance and compliance requirements.**
- **Pricing**
	  Pricing is based per active user in a workspace.
- **Data Sources**
	  Integrate it with several sources including **Amazon CloudWatch, Amazon Managed Service for Prometheus, Amazon OpenSearch Service,** and **Amazon Timestream**.

### Amazon Managed Grafana Use Cases

- **Container Metric Visualizations**
	  Connect to data sources like **Prometheus** for visualizing **EKS**, **ECS**, or **your own Kubernetes cluster** metrics.
- **Internet of Things (IoT)**
	  **Vast data plugins** make the service a perfect fit for **monitoring IoT and edge device data.**
- **Troubleshooting**
	  Centralizing dashboards allows for **more efficient** operational issues **troubleshooting**.

### What is Amazon Managed Service for Prometheus

**Serverless, Prometheus-compatible service** used for securely monitoring container metrics at scale.

### Amazon Managed Service for Prometheus Overview

- **Open-Source Prometheus**
	  Leverage the open-source Prometheus data model with AWS-managed scaling and availability.
- **Automatic Scaling**
	  Let AWS manage automatic scaling based on ingestion, storage, and querying of metrics.
- **Designed for High Availability**
	  AWS replicates data across three Availability Zones (AZs) in the same Region.  Designed for availability.
- **Choose your Kubernetes!**
	  Works with clusters running on Amazon EKS or self-managed Kubernetes clusters.
- **PromQL**
	  Leverage the open-source PromQL query language for exploring and extracting data.
- **Data Retention**
	  Data is stored in workspaces for 150 days (automatically deleted afterward).

---
