# Load Balancers in AWS

Load Balancers are essential for distributing incoming traffic across multiple targets, such as EC2 instances, containers, and IP addresses, to ensure high availability and reliability.


## **Why Use a Load Balancer?**

- **Distribute Traffic**: Spread traffic evenly across multiple servers.
- **Single Access Point**: Simplifies client access with one DNS endpoint.
- **High Availability**: Operates across multiple availability zones.
- **Health Checks**: Monitors the health of backend instances and redirects traffic accordingly.
- **SSL Termination**: Handles HTTPS encryption/decryption at the load balancer.
- **Stickiness**: Ensures session persistence with cookies.

## **Types of Load Balancers in AWS**

AWS provides four types of managed Load Balancers:

| **Feature**                     | **Classic Load Balancer (CLB)** | **Application Load Balancer (ALB)** | **Network Load Balancer (NLB)** | **Gateway Load Balancer (GWLB)** |
|----------------------------------|---------------------------------|-------------------------------------|---------------------------------|-----------------------------------|
| **Protocol**                     | HTTP, HTTPS, TCP, SSL           | HTTP, HTTPS, WebSocket              | TCP, TLS, UDP                   | IP Protocol                       |
| **OSI Layer**                    | Layer 4, Layer 7                | Layer 7                             | Layer 4                         | Layer 3                           |
| **Use Case**                     | Legacy Applications             | Microservices, Containers           | High Performance, Low Latency   | Network Virtual Appliances        |


## **Routing Rules**

Routing rules define how traffic is directed to the backend targets:

### **Application Load Balancer (ALB)**

| **Routing Type**        | **Description**                                | **Example**                                  |
|--------------------------|-----------------------------------------------|----------------------------------------------|
| **Path-based Routing**   | Routes based on URL path.                     | `/users` → Users Target Group               |
| **Host-based Routing**   | Routes based on hostname.                     | `api.yourapp.com` → API Target Group        |
| **Query String Routing** | Routes based on query string in the URL.      | `?platform=mobile` → Mobile Target Group    |
| **Header-based Routing** | Routes based on HTTP headers.                 | `Header: Browser=Chrome` → Chrome Group     |

### **Network Load Balancer (NLB)**

- Primarily routes based on **port numbers**.
- Best for TCP/UDP traffic with low latency.

## **Features of Elastic Load Balancers**

### **1. Application Load Balancer (ALB)**

- Operates at Layer 7 (HTTP/HTTPS).
- Advanced routing rules based on:
  - Path (`/users`, `/posts`)
  - Hostname (`api.yourapp.com`, `web.yourapp.com`)
  - Query string and headers.
- Best for microservices and containerized applications.

### **2. Network Load Balancer (NLB)**

- Operates at Layer 4 (TCP/UDP).
- Ultra-low latency.
- Static IP for each availability zone.
- Best for high-performance applications.

### **3. Gateway Load Balancer (GWLB)**

- Operates at Layer 3 (IP Protocol).
- Used for deploying network appliances like firewalls.
- Handles traffic inspection and routing.

### **4. Classic Load Balancer (CLB)**

- Legacy option for basic Layer 4 and Layer 7 traffic.
- Limited features compared to ALB and NLB.


## **Sticky Sessions**

Sticky sessions (Session Affinity) ensure that a user is consistently routed to the same backend server.

| **Type of Cookie**      | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| **Application Cookie**   | Managed by your application, with custom attributes.                           |
| **Duration-based Cookie**| Managed by the load balancer; expires after a specified time.                   |


## **Cross-Zone Load Balancing**

Cross-Zone Load Balancing is a feature in AWS Elastic Load Balancers that controls how traffic is distributed across backend instances located in multiple availability zones (AZs). 

---

### **How Cross-Zone Load Balancing Works**

1. **With Cross-Zone Load Balancing Enabled**:
   - The load balancer **distributes traffic evenly** across all registered backend instances, regardless of the AZ they are in.
   - Example: If a load balancer has 10 instances spread across 2 AZs (5 in each AZ), the load balancer will divide the traffic equally across all 10 instances.

2. **Without Cross-Zone Load Balancing Enabled**:
   - The load balancer routes traffic **only to the instances in the same AZ** as the load balancer node.
   - Example: If one AZ receives 70% of traffic but has fewer instances, those instances will experience higher load compared to instances in less-trafficked AZs.

---

### **Key Benefits of Cross-Zone Load Balancing**

- **Even Distribution of Traffic**:
  - Ensures all backend instances handle an equal share of the incoming requests, reducing the chance of overloading specific instances.
  
- **Optimized Resource Utilization**:
  - Balances the workload across the entire fleet of instances, leading to better performance and efficiency.

- **Improved Fault Tolerance**:
  - Ensures that traffic is still distributed evenly, even if one AZ has fewer instances or experiences issues.

---

### **Impact of Cross-Zone Load Balancing**

| **Scenario**                     | **With Cross-Zone Load Balancing**                 | **Without Cross-Zone Load Balancing**          |
|-----------------------------------|---------------------------------------------------|-----------------------------------------------|
| **Traffic Distribution**          | Evenly distributed across all AZs                | Limited to instances in the same AZ as the LB |
| **Fault Tolerance**               | Higher fault tolerance due to balanced traffic   | Lower fault tolerance; some AZs might be overloaded |
| **Inter-AZ Communication**        | Generates inter-AZ traffic for load distribution | No inter-AZ communication, traffic stays local |

---

### **Costs Associated with Cross-Zone Load Balancing**

1. **Application Load Balancer (ALB)**:
   - **Enabled by default**, and **no additional cost** for inter-AZ data transfer.

2. **Network Load Balancer (NLB)** and **Gateway Load Balancer (GWLB)**:
   - **Disabled by default**.
   - Enabling cross-zone load balancing incurs **inter-AZ data transfer charges**.

3. **Classic Load Balancer (CLB)**:
   - **Disabled by default**, but enabling it does **not incur inter-AZ data transfer charges**.

---

### **Example Scenarios**

#### **Scenario 1: Cross-Zone Enabled**

- **Setup**: A load balancer with 10 backend instances (5 in AZ1 and 5 in AZ2).
- **Traffic Distribution**: 
  - 50% of the requests go to AZ1.
  - 50% of the requests go to AZ2.
  - Each instance (regardless of AZ) gets an equal share of traffic (10%).

#### **Scenario 2: Cross-Zone Disabled**

- **Setup**: A load balancer with 10 backend instances (5 in AZ1 and 5 in AZ2).
- **Traffic Distribution**:
  - If 70% of requests are routed to AZ1, only the 5 instances in AZ1 handle the 70% load.
  - Instances in AZ2 handle only 30% of the traffic, leading to uneven resource utilization.

---

### **Best Practices**

1. **Enable Cross-Zone Load Balancing**:
   - Recommended for applications with uneven traffic patterns or varying instance counts across AZs.
   
2. **Monitor Costs for NLB/GWLB**:
   - Be mindful of inter-AZ data transfer charges if enabling cross-zone load balancing for NLB or GWLB.

3. **Scale Instances Across AZs**:
   - Maintain similar instance counts in each AZ to reduce the need for inter-AZ traffic, even when cross-zone load balancing is enabled.

4. **Use ALB for Automatic Configuration**:
   - ALB comes with cross-zone load balancing enabled by default, making it easier to use without additional setup.

---

### **Behavior Comparison**

| **Load Balancer**                | **Cross-Zone Enabled (Default)** | **Cross-Zone Disabled**           |
|----------------------------------|----------------------------------|------------------------------------|
| **Application Load Balancer (ALB)** | Enabled                         | Can be disabled at target group level |
| **Network Load Balancer (NLB)**   | Disabled                        | No inter-AZ traffic unless enabled |
| **Gateway Load Balancer (GWLB)**  | Disabled                        | No inter-AZ traffic unless enabled |
| **Classic Load Balancer (CLB)**   | Disabled                        | Can be manually enabled            |

---

### **Diagram: Cross-Zone Load Balancing**

#### **With Cross-Zone Enabled**
```plaintext
      +---------------------------------------+
      |           Elastic Load Balancer       |
      +---------------------------------------+
       |                   |                   |
   (Traffic evenly      (Traffic evenly      (Traffic evenly
    distributed)         distributed)         distributed)
       |                   |                   |
+-------------+    +-------------+    +-------------+
| Instance 1  |    | Instance 2  |    | Instance 3  |
| (AZ1)       |    | (AZ2)       |    | (AZ1)       |
+-------------+    +-------------+    +-------------+
```

#### **With Cross-Zone not Enabled**
```plaintext
      +---------------------------------------+
      |           Elastic Load Balancer       |
      +---------------------------------------+
       |                   |
   (Traffic stays in AZ1)  (Traffic stays in AZ2)
       |                   |
+-------------+    +-------------+
| Instance 1  |    | Instance 2  |
| (AZ1 only)  |    | (AZ2 only)  |
+-------------+    +-------------+
```


## **SSL/TLS Basics**

- SSL certificates encrypt traffic between clients and the load balancer.
- AWS Certificate Manager (ACM) can manage SSL certificates for your load balancer.

| **Load Balancer**         | **SSL Support**                 | **SNI Support**         |
|---------------------------|---------------------------------|--------------------------|
| **Classic Load Balancer** | Single certificate              | No                       |
| **Application LB**        | Multiple certificates           | Yes                      |
| **Network LB**            | Multiple certificates           | Yes                      |


## **Connection Draining**

Connection Draining (for **Classic Load Balancers**, CLB) or Deregistration Delay (for **Application Load Balancers**, ALB, and **Network Load Balancers**, NLB) ensures in-flight requests are completed before an instance is removed from the load balancer.

### **How Connection Draining / Deregistration Delay Works**

1. **Instance Deregistration**:
   - When an instance is marked for removal (due to manual action, scaling events, or health check failures), the load balancer stops routing new traffic to that instance.

2. **Handling In-Flight Requests**:
   - Any ongoing requests already routed to the instance are allowed to complete within the configured **deregistration delay period**.

3. **Timeout or Completion**:
   - If the instance processes the requests within the delay period, it is gracefully deregistered.
   - If the delay period expires before the instance finishes processing requests, the connections are forcibly terminated.


### **Default and Configurable Delay**

- **Delay Range**:
  - Configurable from **1 second to 3600 seconds**.
  - Default value is **300 seconds** (5 minutes).

- **Recommended Settings**:
  - For applications with **short-lived requests**, use a smaller delay (e.g., 30-60 seconds).
  - For applications with **long-running requests** (e.g., file uploads, batch jobs), configure a longer delay to prevent disruptions.


### **Why Connection Draining is Important**

1. **Improved User Experience**:
   - Prevents abrupt termination of user sessions or requests, avoiding timeouts and errors.

2. **Application Stability**:
   - Ensures backend instances complete their processing before being taken offline.

3. **Smooth Scaling**:
   - During Auto Scaling events, ensures old instances complete their work gracefully while new ones take over.

4. **Load Balancer Flexibility**:
   - Maintains consistent behavior across different types of load balancers (CLB, ALB, NLB).


### **When is Connection Draining Triggered?**

Connection draining occurs during these events:
- **Instance Deregistration**:
  - An instance is manually deregistered from the target group or load balancer.
- **Auto Scaling Down**:
  - Instances are terminated due to a scale-in event.
- **Instance Unhealthy**:
  - Instances fail health checks and are marked as unhealthy.
- **Instance Termination**:
  - Instances are shut down or terminated by an admin or automation script.


### **Behavior for Different Load Balancers**

| **Feature**               | **Classic Load Balancer (CLB)**         | **Application/Network Load Balancer (ALB/NLB)** |
|---------------------------|-----------------------------------------|------------------------------------------------|
| **Feature Name**           | Connection Draining                   | Deregistration Delay                          |
| **Default Timeout**        | 300 seconds                           | 300 seconds                                   |
| **Configurable Timeout**   | 1–3600 seconds                        | 1–3600 seconds                                |
| **Stops New Requests**     | Yes                                   | Yes                                          |
| **Allows In-Flight Requests** | Yes                                   | Yes                                          |
| **Force Termination on Timeout** | Yes                                   | Yes                                          |


### **Example Scenario**

#### Without Connection Draining:
1. A user uploads a file to your application.
2. During the upload, the Auto Scaling Group terminates the instance to scale down.
3. The user gets a timeout or error because the request is abruptly terminated.

#### With Connection Draining:
1. The load balancer stops sending new requests to the instance but allows the file upload request to complete.
2. The instance processes the request, completes the upload, and then deregisters itself.
3. The user experiences no interruption.


### **Best Practices**

1. **Adjust the Delay Period**:
   - Match the delay period to your application’s typical request duration to minimize unnecessary wait times.
   - For example:
     - Web applications: 30-60 seconds.
     - Long-running jobs or uploads: 300-600 seconds.

2. **Monitor with Metrics**:
   - Use **CloudWatch** to monitor connection draining and deregistration delays for insights into the time requests take to complete.

3. **Test in Staging**:
   - Simulate scaling events in a non-production environment to verify connection draining behavior.

4. **Combine with Graceful Shutdowns**:
   - Ensure your application can handle termination signals (e.g., `SIGTERM`) for a seamless shutdown process.


By using Connection Draining or Deregistration Delay, you can ensure a smoother, more reliable user experience during instance updates, scaling, or failure recovery.



## **Summary Table**

| **Feature**               | **CLB**          | **ALB**             | **NLB**               | **GWLB**             |
|---------------------------|------------------|---------------------|-----------------------|----------------------|
| **Layer**                 | Layer 4, Layer 7 | Layer 7             | Layer 4               | Layer 3              |
| **Protocol**              | TCP, HTTP, HTTPS | HTTP, HTTPS         | TCP, TLS, UDP         | IP Protocol          |
| **Advanced Routing Rules**| Limited          | Yes                 | No                    | No                   |
| **Cross-Zone Balancing**  | Disabled (Default)| Enabled (Default)   | Disabled (Default)    | Disabled (Default)   |


## **Diagrams**

### Example ALB Routing

```plaintext
Client Request -> ALB -> Path: /users -> Users Target Group
                              Path: /orders -> Orders Target Group
```
