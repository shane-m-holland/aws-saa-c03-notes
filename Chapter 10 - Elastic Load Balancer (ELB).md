# Elastic Load Balancer (ELB)
## ELB Overview

### What is Elastic Load Balancing?

Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances.  This can be done across multiple AZs.

### Types of Load Balancers

**Application Load Balancer**
Best suited for load balancing of HTTP and HTTPS traffic.  They operate at Layer 7 and are application aware. 
*Intelligent Load Balancer.*

**Network Load Balancer**
Operating at the connection level (Layer 4) on the OSI Model.  Net work Load Balancers are capable of handling millions of requests per second, while maintaining ultra-low latencies.
*Performance Load Balancer.*

**Gateway Load Balancer**
Operating at the Network Level on the OSI Model (Layer 3), you should use Gateway Load Balancer when deploying inline virtual appliances where network traffic is not destined for the Gateway Load Balancer itself.
*For Inline Virtual Appliance Load Balancing*

**Classic Load Balancer**
Legacy load balancers.  You can load balance HTTP/HTTPS applications and use Layer 7-specific features, such as X-Forwarded and sticky sessions.
*Classic/Test/Dev Load Balancer*

### Health Checks
All AWS load balancers can be configured with health checks.  Health checks periodically send requests to load balancers' registered instances to test their status.

The status of the instances that are healthy at the time of the health check is **InService**.

The status of any instances that are unhealthy at the time of the health check is **OutOfService**.  The load balancer performs health checks on all registered instances, whether the instance is in a healthy state or an unhealthy state.

The load balancer routes requests only to the healthy instances.  Then the load balancer determines an instance is unhealthy, it stops routing requests to that instance.

The load balancer resumes routing requests to the instance when it has been restored to a healthy state.

---
## Application Load Balancers

### Layer 7 Load Balancing
An Application Load Balancer functions at the Application Layer - the seventh layer of the Open Systems Interconnection (OSI) model.  After the load balancer receives a request, it evaluates the listener rules in priority order to determine which rule to apply, and then selects a target from the target group for the rule action.

### Listeners
A listener checks for connection requests from clients, using the protocol and port you configure.

You define rules that determine how the load balancer routes requests to its registered targets.

Each rule consists of a priority, one or more actions, and one or more conditions.

### Rules
 When the conditions for a rule are met, then its actions are performed.  You must define a default rule for each listener, and you can **optionally define additional rules**.
 
### Target Groups
Each **target group** routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number you specify

### Application Load Balancer Diagram
![ELB - Application Load Balancer Diagram](./resources/img/ELB%20-%20Application%20Load%20Balancer%20Diagram.png)
### Path-Based Routing
**Common Exam Scenario**
Enable Path Patterns to route based on URL path patterns.

##### Limitations of Application Load Balancers
Application Load Balancers only support **HTTP** and **HTTPS**

### HTTPS Load Balancing
To use and HTTPS listener, you must deploy at least one SSL/TLS server certificate on your load balancer.  The load balancer uses a server certificate to terminate the frontend connection and then decrypt requests from clients before sending them to the targets.

---
## Network Load Balancers

### Layer 4 Load Balancing
A Network Load Balancer functions at the fourth layer of the Open Systems Interconnection (OSI) model.  It can handle millions of requests per second.

### Requests Received
After the load balancer receives a connection request, it selects a target from the target group for the default rule.

It attempts to open a TCP connection to the selected target on the port specified in the listener configuration.

### Listeners
A listener checks for connection requests from clients, using the protocol and port you configure.

The listener on a Network Load Balancer then forwards the request to the target group.  There are no rules, unlike with Application Load Balancers.

### Target Groups
Each **target group** routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number you specify

### Protocols and Ports
**Protocols:** TCP, TLS, UDP, TCP_UDP
**Ports:**  1-65535

### Encryption
You can use a TLS listener to offload the work of encryption and decryption  to your load balancer so your applications can focus on their business logic.

If the listener protocol is TLS, you must deploy exactly one SSL server certificate on the listener.

### Use Cases
Network Load Balancers are **best suited for load balancing TCP traffic** where extreme performance is required.  Operating at the connection level (Layer 4), Network Load Balancers are capable of handling millions of requests per second, while maintaining ultra-low latencies.

**Use for Extreme Performance!**

---
## Classic Load Balancer

Classic Load Balancers are the legacy load balancers.  You can load balance HTTP/HTTPS applications and use Layer 7-specific features such as X-Forwarded and sticky sessions.  You can also use strict Layer 4 load balancing for applications that rely purely on the TCP protocol.
### X-Forwarded-For
When traffic is sent from a load balancer, the server access logs contain the IP address of the proxy or load balancer only.

To see the original IP address of the client the **X-Forwarded-For** request header is used.

### Gateway Timeouts
If your application stops responding, the Classic Load Balancer responds with a 504 error.

This means the applications is having issues.  This could be either at the web server layer or the database layer.

---
## Sticky Sessions

### What are Sticky Sessions
**Classic Load Balancers** route each request independently to the registered EC2 instance with the smallest load.

**Sticky sessions allow you to bind a user's session to a specified EC2 instance.**

This ensures all requests from the user during the session are sent to the same instance.

### Scaling Issues
Can cause issues if the instance a user's session is attached to is terminated or goes down.

**Application Load Balancers**
You can enable sticky sessions for Application Load Balancers as well, but the traffic will be sent at the target group level.

---
## Deregistration Delay

### What is Deregistration Delay (Connection Draining)
Deregistration Delay allows Load Balancers to keep existing connections open if the EC2 instances are de-registered or become unhealthy.

This enables the load balancer to complete in-flight requests made to instances that are de-registering or unhealthy.

You can disable deregistration delay if you want your load balancer to immediately close connections to the instances that are de-registering or have become unhealthy.