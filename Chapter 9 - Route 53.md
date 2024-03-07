# Route 53
## Route 53 Overview

### What is DNS?

DNS is used to convert human-friendly domain names into an internet protocol (IP) address.

IP addresses are used by computers to identify each other on the network.  IP addresses commonly come in 2 different forms: IPv4 and IPv6

Computers use DNS to convert domain names into IP addresses.
### IPv4 vs. IPv6

IPv4 Addresses are running out.
- The IPv4 space is a **32-bit** field and has over 4 billion different addresses (4,294,967,296 -- to be precise)
- IPv6 was created to solve this depletion issue and  has an address space of **128 bits.**
- In theory there are 340,282,366,920,938,463,463,374,607,431,768,211,456 (or 240 undecillion) addresses

### Top-level Domains

If we look at common domain names (e.g. google.com, bbc.co.uk) you will notice a string of characters separated by dots (periods).

The last word in a domain name represents the top-level domain.  The second to last word in a domain name is known as a second-level domain name (this is optional, though, and depends on the domain name).

**TLD examples**
- .gov
- .gov.uk
- .com.au
- .edu

These top-level domain names are controlled by the Internet Assigned Numbers Authority (IANA) in a root zone database, which is essentially a database of all available top-level-domains.

You can view this database by visiting:

http://www.iana.org/domains/root/db
### Domain Registrars

Because all names in a given domain name must be unique, there needs to be a way to organize this all so that domain names aren't duplicated.

This is where domain registrars come in.

**A registrar is an authority that can assign domain names directly under one or more top-level domains.  These domains are registered with interNIC, a service of ICANN, which enforces the uniqueness of domain names across the internet.**

Each domain name becomes registered in a central database known as the WHOIS database.

**5 Popular Domain Registrars**:
- domain.com
- GoDaddy
- Hover
- AWS
- Namecheap
### Common DNS Record Types

**SOA**
**The SOA record stores information about:**
- The name of the server that supplied the data for the zone
- The administrator of the zone
- The current version of the data file
- The default number of seconds for the time-to-live file on the resource records

**NS stands for name server records**

NS records are used by top-level domain servers to direct traffic to the content DNS server that contains the authoritative DNS records.

**Types of Records**
- **A** - IPv4
- **AAAA** - IPv6
- **CNAME** - Forwards to another domain
- **MX** - Directs mail to an email server
- **TXT** -  Lets an admin store text notes in the record.  (Often used for email security)
- **NS** - Stores the name server for a DNS entry.
- **SOA** - Stores admin information about a domain
- **SRV** - Specifies a port for specific services.
- **PTR** - Provides a domain name in reverse-lookups.
### What is a TTL?

The length that a DNS record is cached on either the resolving server or the user's own local PC is equal to the value of the **time to live (TTL)** in seconds.

The lower the time to live, the faster changes to DNS records take to propagate throughout the internet.

### Alias Records

Alias records are used to map resource record sets in your hosted zone to load balancers, CloudFront distributions, or S3 buckets that are configured as websites.

Alias records work like a CNAME record in that you can map one DNS name to another "target" DNS name

**CNAME** - Cannot be used for naked domain names (zone apex record).   You can't have a CNAME for http://acloudguru.com

**A record/Alias** - Can be used for a naked domain name/zone apex record.

### What is Route 53

**Route 53 is Amazon's DNS service.**
It allows you to register domain names, create hosted zones, and manage and create DNS records.

**Route 53 is named after Route 66 (one of the original highways across the United States) but is called 53 because DNS operates on port 53.**
### Routing Policies

**7 Routing Policies Available with Route 53**
- Simple Routing
- Weighted Routing
- Latency-Based Routing
- Failover Routing
- Geolocation Routing
- Geoproximity Routing (Traffic Flow Only)
- Multivalue Answer Routing

### Exam Tips

- Understand the difference between an alias record and a CNAME
- Given the choice, always choose an alias record over a CNAME

**Common DNS Record Types**
- **A** - IPv4
- **CNAME** - Forwards to another domain
- **NS** - Stores the name server for a DNS entry.
- **SOA** -  Start of Authority.  Stores admin information about a domain
---
## Simple Routing Policy

If you choose the simple routing policy, you can only have **one record with multiple IP addresses**. 
If you specify **multiple values in a record**, Route 53 returns **all values** to the user in a **random order.**

---
## Weighted Routing Policy

Allows you to split your traffic based on different weights assigned.

For example, you can set 10% of your traffic to go to **us-east-1** and 90% to go to **eu-west-1**.

### Health Checks
- You can set health checks on individual record sets.
- If a record set **fails** a health check, it will be removed from Route 53 until is **passes** the health check.
- You can set SNS notifications to **alert** you about failed health checks.

---
## Failover Routing Policy

Failover routing policies are used when you want to create an active/passive set up.

> For example, you may want your primary site to be in EU-WEST-2 and your secondary DR Site in AP-SOUTHEAST-2
> 
> Route 53 will monitor the health of your primary site using a health check.

---
## Geolocation Routing Policy

Geolocation routing lets you **choose where your traffic will be sent** based on the **geographic location of your users** (i.e. the location from which DNS queries originate).

### Use Cases
For example, you might want all queries from Europe to be routed to a fleet of EC2 instances that are specifically configured for your European customers.

**Localization**
These servers may have the local language of your European customers and display all prices in euros.

---
## Geoproximity Routing Policy

### Route 53 Traffic Flow

You can use Route 53 traffic flow to build a routing system that uses a combination of:

- geographic location
- latency
- and availability to route traffic
from your users to your cloud or on-premises endpoints.

You can build your traffic routing policies:
- from scratch
- or you can pick a template from a library and then customize it.
### Geoproximity Routing  (Traffic Flow Only )

- Geoproximity routing lets Amazon Route 53 route traffic to your resources based on the geographic location of your users and your resources.
- You can also optionally choose to route more traffic or less to a given resource by specifying a value known as a **bias.**

**A bias expands or shrinks the size of the geographic rgion from which traffic is routed to a resource.**

---
## Latency Routing Policy

Allows you to route your traffic based on the **lowest network latency for your end user** (i.e., which region will give them the fastest response time).

**Latency Resource Record Set**
To use latency-based routing, you create a latency resource record set for the EC2 (or ELB) resource in each region that hoses your website.

When Route 53 receives a query for your site, it selects the latency resource record set for the region that gives the user the lowest latency.

**Route 53** then responds with the value associated with that resource record set.

---
## Multivalue Answer Routing Policy

Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries.

**Similar to Simple Routing**
You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources.





