# Simple Storage Service (S3)
## What is S3?

- Object Storage in the Cloud
- S3 provides secure, durable, highly scalable object storage.
- S3 allows you to store and retrieve any amount of data from anywhere on the web at a very low cost.
- Amazon S3 is easy to use, with a simple web service interface.

### S3 is Object Based Storage
- Manages data as objects rather than in file systems or data blocks.
- Upload any file type you can think of to S3
- Examples include photos, videos, code, documents, and text files.
- Cannot be used to run an operating system or a database

### S3 Basics
- **Unlimited Storage**
	  The total volume of data and the number of objects you can store is unlimited
- **Objects up to 5TB in Size**
	  S3 objects can range in size from a minimum of 0 bytes to a maximum of 5 terabytes
- **S3 Buckets**
	  Store files in buckets (similar to folders).

### Working with S3 Buckets
- **Universal Namespace**
	  All AWS accounts share the S3 namespace.  Each S3 bucket name is globally unique
- **Example S3 URLs**
	  https://bucket-name.s3.Region.amazonaws.com/key-name
	  https://acloudguru.s3.us-east-1.amazonaws.com/Ralphie.jpg
- **Uploading Files**
	  When you upload a file to an S3 bucket, you will receive an HTTP 200 code if the upload was successful
- **Key Value Store**
	- **Key**
		  The name of the object (e.g. Ralphie.jpg)
	- **Value**
		  The data itself, which is made up of a sequence of bytes
	- **Version ID**
		  Important for storing multiple versions of the same object
	- **Metadata**
		  Data about the data you are storing (e.g. content-type, last-modified, etc.)

### S3 is a safe place to store your files

The data is spread across multiple devices and facilities to ensure availability and durability.

### Highly Available and Highly 

Built for Availability
	Built for 99.5% - 99.99% service availability, depending on the S3 tier.
Designed for Durability
	Designed for 99.999999999% (9 decimal places)/(11 nines) durability for data stored in S3.

### S3 Standard

- High Availability and Durability
	- Data is stored redundantly across multiple devices in multiple facilities (>= 3 AZs)
	- 99.99% availability
	- 99.999999999% durability (11 9's)
- Designed for Frequent Access
	- Perfect for frequently accessed data
- Suitable for Most Workloads
	- The default storage class.
	- Use cases include websites, content distribution, mobile and gaming applications, and big data analytics.

### Characteristics

- **Tiered Storage**
	  S3 offers a range of storage classes designed for different use cases.
- **Lifecycle Management**
	  Define rules to automatically transition to a cheaper storage tier or delete objects that are no longer required after a set period of time.
- **Versioning**
	  With versioning, all versions of an object are stored and can be retrieved, including deleted objects.

### Securing your Data

- **Server-Side Encryption**
	  You can set default encryption on a bucket to encrypt all new objects when they are stored in the bucket
- **Access Control Lists (ACLs)**
	  Define which AWS accounts or groups are granted access and the type of access.  You can attach S3 ACLs to individual objects within a bucket.
- **Bucket Policies**
	  S3 bucket policies specify what actions are allowed or denied (e.g. allo user Alice to PUT but not DELETE objects in the bucket).

### Strong Read-After-Write Consistency

- After a successful write of a new object (PUT) or an overwrite of an existing object, any subsequent read request immediately receives the latest version of the object.
- Strong consistency for list operations, so after a write you can immediately perform a listing of the objects in a bucket with all changes reflected.

### What to Know for the Exam
- **Object Based**
	  Object-based storage allows you to upload files.
- **Files up to 5 TB** 
	  Files can be from 0 bytes to 5TB.
- **Not OS or DB Storage**
	  Not suitable to install an operating system or run a database on.
- **Unlimited Storage**
	  The total volume of data and number of objects you can store is unlimited.

---
## Securing Your Bucket with S3 Block Public Access

### Object ACLs vs. Bucket Policies

- **Object ACLS**
	  Object ACLs work on an **individual object** level.
- **Bucket Policy**
	  Bucket policies work on an **entire bucket** level.

### What to Know for the Exam

- **Buckets are private by default**
	  When you create an S3 bucket, it is private by default (including all objects within it).  You have to allow public access on both the **bucket** and **its objects** in order to make the bucket public.
- **Object ACLs**
	  You can make **individual objects** public using ACLs.
- **Bucket Policies**
	  You can make **entire buckets** public using bucket policies.
- **HTTP status code**
	  When you upload an object to S3 and it's successful, you will receive an HTTP 200 code.

---
## Hosting a Static Website Using S3

### You can use S3 to host static websites, such as .html sites

**Dynamic** websites, such as those that require **database connections** cannot be hosted on S3.

### S3 Scales Automatically

**S3 scales automatically to meet demand.**
Many enterprises will put static websites on S3 when they think there is going to be a large number of requests (e.g. for a movie preview).

### To enable static website hosting
Bucket must have `Block all Public Access` turned off.
Navigate to the desired bucket -> Properties -> Static Website Hosting
Enable and specify landing and error page.

Permissions, edit Bucket Policy
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": [
				"s3:GetObject"
			],
			"Resource": [
				"arn:aws:s3:::BUCKET_NAME/*"
			]
		}
	]
}
```

---
## Versioning Objects in S3

### What is Versioning?

You can enable versioning in S3 so you can have **multiple versions of an object within S3**

### Advantages of Versioning

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

---

## S3 Storage Classes

### S3 Standard
- **High Availability and Durability**
	  Data is stored redundantly across multiple devices in multiple facilities (>=3 AZs)
	  99.99% availability
	  99.999999999% durability (11 9's)
- **Designed for Frequent Access**
	  Perfect for frequently accessed data
- **Suitable for Most Workloads**
	  The default storage class
	  Use cases include websites, content distribution, mobile and gaming applications, and big data analytics.

### S3 Standard-Infrequent Access (S3 Standard-IA)

Designed for infrequently accessed data
- **Rapid Access**
	  Used for data that is accessed less frequently but requires rapid access when needed.
- **You Pay to Access the Data**
	  There is a low per-GB storage price and a per-GB retrieval fee.
- **Use cases**
	  Great for long-term storage, backups, and as a data store for disaster recovery files.
- 99.9% Availability
- 99.999999999% (11 9's) Durability

### S3 One Zone-Infrequent Access
**Like S3 Standard-IA, but data is stored redundantly within a single AZ.**

- Costs **20% less** than regular S3 Standard-IA
- Great for long-lived, infrequently accessed, non-critical data
- 99.5% Availability
- 99.999999999% (11 9's) Durability

### S3 Intelligent-Tiering

**2 Tiers - Frequent and Infrequent Access**

Automatically moves you data to the most cost-effective tier based on how frequently you access each object.
- 99.9% Availability
- 99.999999999% (11 9's) Durability
- Good way to optimize cost
- Monthly fee of $0.0025 per 1,000 objects.

### Glacier and Glacier Deep Archive

**3 Glacier Options**
- You pay each time you access your data
- Use only for archiving data
- Glacier is cheap storage
- Optimized for data that is very infrequently accessed
- 99.99 % Availability
- 11 9's Durability

**Glacier Instant Retrieval**
Provides long-term data archiving with instant retrieval time for your data.

**Glacier Flexible Retrieval**
Ideal storage class for archive data that does not require immediate access but needs the flexibility to retrieve large sets of data at no cost, such as backup or disaster recovery use cases.  Can be minutes or up to 12 hours.

**Glacier Deep Archive**
Cheapest storage class and designed for customers that retain data sets for 7-10 years or longer to meet customer needs and regulatory compliance requirements.  The standard retrieval time is 12 hours, and the bulk retrieval time is 48 hours.

![[./resources/img/S3-storage-classes.png]]

![[./resources/img/S3-storage-classes-cost-1.png]]

![[./resources/img/S3-storage-classes-cost-2.png]]

---
## Lifecycle Management with S3

### What is Lifecycle Management

Lifecycle management automates moving your objects between different storage tiers, thereby maximizing cost effectiveness

| S3 Standard | S3 Standard-IA | S3 Glacier |
| --- | --- | --- |
| Keep for 30 days | After 30 Days | After 90 Days |

### Combining Lifecycle Management with Versioning

You can use lifecycle management to **move different versions** of objects to **different storage tiers**.

---
## S3 Object Lock and Glacier Vault Lock

### S3 Object Lock

You can use S3 Object Lock to store objects using a **write once, read many (WORM)** model.  It can help prevent objects from being deleted or modified for a fixed amount of time or indefinitely.

You can use the **S3 Object Lock** to meet regulatory requirements that require WORM storage, or add an extra layer of protection against object changes or deletion.
### Governance Mode

In governance mode, **user's can't overwrite or delete an object version or alter its lock settings** unless they have special permissions.

With governance mode, you protect objects against being deleted by most users, but you can still grant some users **permissions to alter the retention settings** or delete the object if necessary.

### Compliance Mode

In compliance mode, **a protected object version can't be overwritten or deleted by any user**, including the root user in your AWS account.  When an object is locked in compliance mode, its retention mode can't be changed and its retention period can't be shortened.  Compliance mode ensures an object version **can't be overwritten or deleted** for the duration of the retention period

### Retention Periods

A retention period **protects and object version for a fixed amount of time**. When you place a retention period on an an object version, Amazon S3 stores a timestamp in the object version's metadata to indicate when the retention period expires.

After the retention period expires, the object version can be **overwritten or deleted** unless you also place a legal hold on the object version.

### Legal Holds

S3 Object lock that also enables you to place a legal hold on an object version.  Like a retention period, a legal hold **prevents an object version from being overwritten or deleted**. However, a legal hold does not have an associated retention period and remains in effect until removed.  Legal holds can be freely placed and removed by any user who has the `s3:PutObjectLegalHold` permission.

### Glacier Vault Lock

S3 Glacier Vault Lock allows you to **easily deploy and enforce compliance controls for individual S3 Glacier vaults with a vault lock policy**.  You can specify controls, such as WORM, in a vault lock policy and lock the policy from future edits.  Once locked, the policy can no longer be changed.

---
## Encrypting S3 Objects

### Types of Encryption
- **Encryption in Transit**
	- SSL/TLS
	- HTTPS
- **Encryption at Rest: Server-Side Encryption**
	- **SSE-S3**: S3-managed keys, using AES 256-bit encryption
	- **SSE-KMS**: AWS Key Management Service-managed keys
	- **SSE-C**: Customer-provided keys
- **Encryption at Rest: Client-Side Encryption**
	  You encrypt the files yourself before you upload them to S3

### Server-Side Encryption

Enabled by Default
All Amazon S3 buckets have encryption configured by default.  All objects are automatically encrypted by using server-side encryption with Amazon S3 managed keys (SSE-S3).

**This encryption setting applies to all objects in your Amazon S3 buckets.**

### Enforcing Server-Side Encryption

1.  **x-amz-server-side-encryption**
	   If the file is to be encrypted at upload time, the `x-amz-server-side-encryption` parameter will be included in the request header
2. **Two Options** 
   - `x-amz-server-side-encryption: AES256` 
     (SSE-S3 -- Se-managed keys) 
   - `x-amz-server-side-encryption: aws:kms` 
     (SSE-KMSS -- KMS-managed keys)
3. **PUT Request Header**
	   When this parameter is included in the header of the **PUT** request, it tells S3 to encrypt the object at the time of upload, using the specified encryption method.

You can create a bucket policy that denies any S3 **PUT** request that doesn't include the `x-amz-server-side-encryption` parameter in the request header.


## Optimizing S3 Performance

### S3 Prefixes Explained
| Resource Path | S3 Prefix |
| --- | --- |
| `mybucketname/folder1/subfolder1/myfile.jpg` | **/folder1/subfolder1** |
| `mybucketname/folder2/subfolder1/myfile.jpg` |  **/folder2/subfolder1** |
| `mybucketname/folder3/myfile.jpg` | **/folder3** |
| `mybucketname/folder4/subfolder4/myfile.jpg` | **/folder4/subfolder4** |

### S3 Performance

S3 has extremely low latency.  You can get the first byte out of S3 within 100-200 milliseconds.

You can also achieve a high number of requests: **3,500 PUT/COPY/POST/DELETE** and **5,500 GET/HEAD** requests per second per prefix.

1. You can get better performance by spreading your reads across **different prefixes**.  For example, if you are using **2 prefixes**, you can achieve **11,000 requests per second**.
2. If we used all **4 prefixes** in the last example, you would achieve **22,000 requests per second**.

### Limitations with KMS

- If you are using **SSE-KMS** to encrypt your objects in S3, you must keep in mind the **KMS limits**.
- When you **upload** a file, you will call `GenerateDataKey` in the KMS API.
- When you **download** a file, you will call `Decrypt` in the KMS API.

### KMS Request Rates

- Uploading/downloading will count toward the **KMS quota.**
- Currently, you **cannot** request a quota increase for KMS.
- Region-specific, however, it's either **5,500, 10,000,** or **30,000** requests per second.

### S3 Performance: Uploads

**Multipart Uploads**
- Recommended for files **over 100MB**
- Required for files **over 5GB**
- Parallelize uploads (increases **efficiency**)

### S3 Performance: Downloads

**S3 Byte-Range Fetches**
- Parallelize **downloads** by specifying byte ranges.
- If there's a failure in the download, it's only for a specific byte range.

- Can be used to **speed up** downloads
- Can be used to download **partial amounts of the file** (e.g. header information)

## Backing up Data with S3 Replication

### S3 Replication

1. **You can replicate objects from one bucket to another.**
	   Versioning must be enabled on both the source and destination buckets.
2. **Objects in an existing bucket are not replicated automatically.**
	   Once replication is turned on, all subsequent updated objects will be replicated automatically.
3. **Delete markers are not replicated by default.**
	   Deleting individual version or delete markers will not be replicated.
