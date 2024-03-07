# Serverless Architecture

## Serverless Overview

### History of Computing

![[./resources/img/Serverless-history-of-computing.png]]

### Serverless Benefits

**Benefits of Serverless**
Leaving all that management behind.

- **Ease of Use**
	  There isn't much for us to do besides bringing our code.  AWS handles almost everything for us.
- **Event Based**
	  Serverless compute resources can be brought online in response to an event happening
- **Billing Model**
	  "Pay as you go" in it's purest form.  You only pay for your provisioned resources and the length of runtime.

### What's Coming Up

- **Lambda**
	  Write your code, build your function, and that's it!
- **Fargate**
	  Let's run containers in fargate.  No servers, no patching, no mess!

### Exam Tip

**Let's Go Serverless!**

On the. exam, focus on answers that move away from unmanaged architecture like EC2.

It's almost always better to select an answer on the test that uses Lambda or containers rather than a traditional operating system.

---

## Computing with Lambda

### What is AWS Lambda?

Serverless compute service that lets you run code without provisioning or managing underlying servers.

Say goodbye to overhead and focus on your code, not the servers!

Run your code on demand with automated scaling.

### Lambda Features

- **Pricing**
	  Free tier of 1,000,000 requests and 400,000 GBs of compute per month.  After that, pay per request.
- **Integrations**
	  Integrates with numerous AWS services, including S3, DynamoDB, EventBridge, SQS/SNS, and Kinesis
- **Built-in Monitoring**
	  Logging and monitoring can be easily accomplished using Amazon CloudWatch!
- **Easy Configuration**
	  Easily set memory requirements as needed.  Currently able to use up to 10,240MB!  CPU scales with memory.
- **Execution Length**
	  Used for short-term executions.  Time limit of 900 seconds (15 minutes).  For anything longer use ECS, Batch, or EC2.
- **Runtime Support**
	  Leverage industry-dominant languages including Python, Golang, Java, Node.js, and others!

### Building a Function

#### Configuration

- **Runtime:** You'll need to pick from an available runtime or bring your own.  This is the environment your code will run in.
- **Permissions:** If your Lambda function needs to make an AWS API call, you'll need to attach a role.
- **Networking:** Optionally define the VPC, subnet, and security groups your functions are a part of.
- **Resources:** Define the amount of available memory allocated to your function.
- **Trigger:** What's going to alert your Lambda function to start?  Defining a trigger will kick Lambda off if that event occurs.

### Quotas
#### Compute and Storage
- 1,000 concurrent executions
- 512 MB - 10 GB disk storage (/tmp)
- Integration with EFS if needed
- 4 KB for all environment variables
- 128 MB - 10 GB memory allocation
- Can run for up to 900 seconds (15 minutes)

#### Deployments and Configuration
- Compressed deployment package (.zip) size must be <= 50 MB
- Uncompressed deployment package (unzipped) must be <= 250 MB
- Request and response payload sizes up to 6 MB
- Streamed responses up to 20 MB

### Popular Architecture Examples

Processing a file from S3 triggered by `PutObject`

![[./resources/img/Serverless-Lambda-Architecture-01.png]]

Leverage EventBridge to run a Lambda on a cron or at a Rate

![[./resources/img/Serverless-Lambda-Architecture-02.png]]

---
## Leveraging the AWS Serverless Application Repository

### What is the AWS Serverless Application Repository?

- **Serverless Apps**
	  Allows users to easily find, deploy, or even publish their own serverless applications.
- **Sharing is Caring**
	  Ability to privately share applications within orgs or publicly for the world!
- **Manifest**
	  Upload your application code and a manifest file.   known as the AWS SAM template.
- **Integrations**
	  Deeply integrated with the AWS Lambda service.  Appears within the console!

### Publish and Deploy

There are currently two options to choose from.

### Publish
- Publishing apps makes them available for others to find and deploy.
- Define apps with the AWS SAM templates.
- Set to private by default.
- Must explicitly share if desired.

### Deploy
- Find and deploy published applications.
- Browse public apps without needing an AWS account.
- Browse within the AWS Lambda console.
- Be careful of trusting all applications!

### Exam Tips

- **Templates**
	  Define whole applications via AWS SAM templates.  Private by default.
- **Publish or Deploy**
	  Choice of publishing your own applications or deploying publicly available ones.
- **AWS Lambda**
	  Heavily integrated with AWS Lambda!

---

## Container Overview

### What is a Container

A container is a standard unit of software that packages up code and all its dependencies, so the application runs quickly and reliably from one computing environment to another.

![[./resources/img/Serverless-Containers-Overview.png]]

### Container Terminology

- **Dockerfile**
	  Text document that contains all the commands or instructions that will be used to build an image.
- **Image**
	  Immutable file that contains the code, libraries, dependencies, and configuration files needed to run an application
- **Registry**
	  Stores Docker images for distribution.  They can be both public and private.
- **Container**
	  A running copy of the image that has been created.

### Console Demo

#### Creating the Dockerfile
```dockerfile
FROM centos:7

# Install dependencies
RUN yum update -y
RUN yum install httpd -y

# Install app1
RUN rm -rf /var/www/html/*
ADD code /var/www/html
RUN ln -sf /dev/stdout /var/log/httpd/access_log
RUN ln -sf /dev/stderr /var/log/httpd/error_log

EXPOSE 80

CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

#### Building the Image
```shell
sudo docker build .
```

#### Run the image as a container
```shell
sudo docker run -d -p 80:80 <image-id>
```

### Exam Tips

##### Containers Are More Flexible
On the exam, containers are generally seen as more flexible.

They're easier to run on-site and move around to different environments. If you see a scenario that talks about any of these situations, pick an answer that includes containers.

---

## Running Containers in ECS or EKS

