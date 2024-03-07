# Identity and Access Management (IAM)

## Securing the root account

### What is IAM? 
IAM allows you to manage users and their level of access to the AWS Console
- Create users and grant permissions to those users.
- Create groups and roles.
- Control access to AWS resources.
- IAM works at a global level, not a regional level

**What is the root account?**
The **root account** is the email address you used to sign up for AWS.  The root account has **full administrative access** to AWS.  For this reason, it is important to secure this account.

User -> Security Credentials -> Add an MFA device
### Controlling Users' Actions with IAM Policy Documents
### Permissions with IAM
How do we control permissions in IAM?
We assign permissions using policy documents in JSON.

### Example of a Policy Document
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "*",
			"Resource": "*"
		}
	]
}
```
This gives full administrative access to the AWS console.

**IAM Policy Documents** can be assigned to
- Groups
- Users
- Roles

---
## Permanent IAM Credentials
### Building blocks of IAM
- Users
	  A physical person
- Groups
	  Functions, such as administrator, developer etc.
	  Contains users.
  - Roles
	    Internal usage within AWS

### Best Practices

- It is best practice for users to **inherit permissions** from groups rather than individual users
- Always work on the principle that one user **equals** one physical person.
	- Never share user accounts across multiple people.
- The Principle of Least Privilege
	- Only assign a user the **minimum** amount of privileges they need to do their job.

---
## Lab for IAM Notes

- **ARN** = Amazon Resource Name (Unique Identifier for all Amazon Resources, ex. Users)
- Using `Deny` performs an **explicit deny** which will always override any `Allow`
- Get the sign in url by going to the **IAM Dashboard** and looking under the **AWS Account** section on the right side of the screen
- Users can "assume" a role through the trust relationship with a role.
- Users assume a role via "Switch Role" and typing the name of the role to assume.