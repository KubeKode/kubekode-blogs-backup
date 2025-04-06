---
title: "Cloud System Design: Choosing Between Kubernetes, Serverless, and VMs"
datePublished: Sun Apr 06 2025 18:34:30 GMT+0000 (Coordinated Universal Time)
cuid: cm95zf1rs000009la6fwo0hgs
slug: cloud-system-design-choosing-between-kubernetes-serverless-and-vms
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1743964406753/bdb05f01-67f9-4d58-8719-ed51fc9faa25.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1743964459342/5673a203-b3de-4342-9904-ccd201355bbd.png

---

Have you ever wondered what’s the best way to deploy and manage your applications in the cloud?

Should you go with the container orchestration power of **Kubernetes**, the simplicity of **Serverless**, or the flexibility of **Virtual Machines (VMs)**?

In this blog, we’ll break down these technologies and go a step further by exploring **real-world scenarios**, **flowcharts**, and **system design principles** to help you make informed choices.

## Why Cloud Technology Matters in System Design?

Modern applications need to scale, recover, and respond to changing workloads efficiently. Choosing the right cloud technology isn't just about cost—it's about aligning with your app’s **architecture**, **traffic patterns**, and **team capabilities**.

## 🛠️ Understanding the Technologies

### 🚢 Kubernetes

Kubernetes is an open-source platform that automates the deployment, scaling, and management of **containerized applications**.

**Key Features:**

* Auto-healing containers
    
* Load balancing
    
* Rolling updates
    
* Strong ecosystem
    

**Real-World Example:** Think of an **e-commerce platform** with microservices like `cart`, `checkout`, and `inventory`. Kubernetes can auto-scale these services during sales events and restart failed containers automatically.

### ⚡ Serverless (e.g., AWS Lambda, Azure Functions)

Serverless computing abstracts away server management. You just write the code, and the cloud provider takes care of provisioning and scaling.

**Key Features:**

* No server management
    
* Pay-as-you-go pricing
    
* Instant scalability
    
* Event-driven
    

**Example:** You upload a file to an **S3 bucket**. This event triggers an AWS **Lambda** function that resizes the image and stores it in another bucket. You only pay for the compute time used.

**Flow:**

1. File uploaded to S3
    
2. Lambda function is triggered
    
3. Image is processed
    
4. Output saved to DynamoDB or sent to SQS
    

### 🖥️ Virtual Machines (VMs)

VMs offer complete control over an operating system. They are ideal for **legacy applications** that can't be containerized.

**Use Case:** Migrating a traditional **ERP system** to the cloud? Use VMs to recreate the environment without changing the application code.

## 🔍 Cloud System Design Principles

### 📌 Event-Driven Architecture

* Decouples producers and consumers
    
* Enables scalability and fault tolerance
    

**Example:** Order service emits an event. Payment and Notification services subscribe and process it independently.

### 🔄 Asynchronous Programming

* Allows non-blocking operations
    
* Ideal for background tasks and queues
    

### 📈 Scalability

* Use horizontal scaling, event queues, and Kubernetes autoscalers
    
* Decouple services to avoid bottlenecks
    

## 🌍 Real-World System Design Scenarios

### 1\. 🛒 E-Commerce with Event-Driven Serverless Architecture

**Scenario:** After a user places an order:

* The **Payment Service** must process it immediately.
    
* The **Notification Service** can be slightly delayed.
    

**Solution:**

* **Synchronous call** to the Payment API
    
* **Asynchronous event** sent to a message queue (like SQS)
    
* The notification service subscribes and processes when ready
    

**Flowchart:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743948741038/08a9caa4-605e-4638-b3de-8177b1144d9f.png align="center")

1. Order placed
    
2. Call Payment API (synchronously)
    
3. On success, publish event to SQS
    
4. Show the success page
    
5. The notification service sends email/SMS
    

**Why This Works:**

* Immediate processing for critical paths
    
* Loose coupling for scalability
    

### 2\. 💬 Real-Time Chat App with Kubernetes

**Scenario:** A real-time chat app with services like `users`, `messages`, and `notifications`.

**Solution Using Kubernetes:**

* Deploy each service as a **microservice**
    
* Use **Ingress Controller** for routing
    
* Use **Horizontal Pod Autoscaler** for scaling `messages` service
    
* Store secrets in **ConfigMaps** and **Secrets**
    
* Monitor with **Prometheus** and **Grafana**
    

**Flowchart:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743948797059/02d9aebc-adb1-499c-a907-35d18809eca9.png align="center")

1. User sends a message
    
2. Routed via Ingress to `messages` service
    
3. `Messages` calls `users` service for validation
    
4. Message saved to DB (e.g., MongoDB)
    
5. `Notifications` service sends real-time alert
    
6. Kubernetes auto-scales as needed
    

### 3\. Healthcare Management System on VMs with Load Balancer

You’re managing a **healthcare management system** used by hospitals to manage patient records, appointments, and billing. The application is a **monolithic Java app** that can't be easily containerized or split into microservices (yet). Still, it needs to scale to handle the load from multiple hospitals accessing it concurrently.

### Solution Using VMs + Load Balancer:

1. Deploy the monolithic Java application on **multiple VMs** (e.g., EC2 instances in AWS).
    
2. Use a **cloud load balancer** (like AWS ELB or GCP Load Balancer) in front of the VMs to:
    
    * Distribute incoming HTTP requests
        
    * Check VM health using health checks
        
    * Automatically reroute traffic in case of a VM failure
        
3. Store data in a **centralized database** (like Amazon RDS or Cloud SQL).
    
4. Use **auto-scaling groups** to spin up or down VMs based on CPU or request load.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743950195657/a40e87ba-76b2-455b-b463-251b1e3f4f45.png align="center")

## The Hybrid Approach: Why Mix Technologies?

### Kubernetes for Microservices

Amazon EKS (Elastic Kubernetes Service) is used for running key microservices:

* **Authentication, Course Management, and User Profiles:** These are critical, stateful components that benefit from Kubernetes' robust scheduling and orchestration.
    
* **Real-Time Communication:** A dedicated chat service using WebSockets can dynamically scale with demand.
    

Kubernetes allows us to define:

* **Deployments:** Manage application updates and scaling.
    
* **Services:** Expose applications internally within the cluster.
    
* **Ingress Controllers:** Handle external access with defined routing rules, ports, and security configurations.
    

### Serverless for Event-Driven Processing

For tasks that are intermittent and require rapid scaling:

* **Video Uploads & Transcoding:** Users upload videos to an S3 bucket. This event triggers AWS Lambda functions that validate files and enqueue processing tasks via Amazon SQS. Workers (Lambda functions or Fargate tasks) then transcode videos using AWS Elemental MediaConvert.
    
* **Cost Efficiency:** With serverless architectures, you pay only when the code executes, making it ideal for unpredictable workloads.
    

### VMs for Legacy and Stateful Workloads

Not every component fits neatly into containers:

* **Legacy Exam Scoring Engine:** A traditional binary application that is challenging to containerize runs on Amazon EC2. Auto Scaling Groups (ASGs) ensure that the workload scales, while an Application Load Balancer (ALB) efficiently distributes traffic.
    
* **Isolation and Control:** VMs allow greater control over the runtime environment and are better suited for applications that require specific OS-level configurations.
    

## The EduPro Architecture: A Detailed Walkthrough

Imagine a scenario where EduPro must support multiple functionalities:

1. **User Interaction & Frontend Delivery:**
    
    * Users access EduPro through their browsers.
        
    * **Amazon CloudFront** serves static assets, ensuring low latency globally.
        
    * **AWS API Gateway** manages incoming API requests, providing security and request validation before routing traffic to the backend.
        
2. **Microservices Layer on Amazon EKS:**
    
    * **Ingress Controller:** Acts as the entry point into the Kubernetes cluster, managing SSL termination and routing based on URL paths.
        
    * **Service Deployments:**
        
        * **Auth Service:** Handles login and token management.
            
        * **Course Service:** Manages course content and metadata.
            
        * **User Profile Service:** Maintains user data and personalization.
            
        * **Chat Service:** Supports real-time messaging using WebSocket protocols.
            
    * **Kubernetes Networking:** Each service is exposed using ClusterIP services with dedicated ports (e.g., 8080 for auth, 8081 for course management), while the Ingress Controller maps external requests (e.g., `/auth`, `/course`) to these services.
        
3. **Video Processing Using Serverless Components:**
    
    * When a user uploads a video, it is stored in an **Amazon S3** bucket.
        
    * An S3 event triggers an **AWS Lambda** function that validates the video and sends a processing request to an **Amazon SQS** queue.
        
    * A transcoding worker (either another Lambda function or a Fargate task) picks up the job, processes the video using **AWS Elemental MediaConvert**, and stores the final version back in S3.
        
4. **Legacy Exam Scoring via EC2:**
    
    * **Legacy Application:** Hosted on EC2 instances within an Auto Scaling Group.
        
    * **Load Balancing:** An ALB routes exam submission requests to the EC2 fleet.
        
    * **Integration:** Results from the legacy exam engine are then passed back to the Kubernetes layer via secure internal APIs for display to the user.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743962125683/414d0e64-499b-4d8e-85df-ac3047383a3d.png align="center")

---

## 🧭 Decision Framework and Trade-Offs

| Feature | Kubernetes | Serverless | VMs |
| --- | --- | --- | --- |
| **Scalability** | High | Automatic | Medium |
| **Cost Efficiency** | Medium | High | Low |
| **Operational Overhead** | High | Low | Medium |
| **Best For** | Microservices | Event-Driven Apps | Legacy Systems |

**Key Questions to Ask:**

* Does your app need **real-time** or can it tolerate **delays**?
    
* Are you optimizing for **cost**, **control**, or **speed to market**?
    
* Do you need **fine-grained scaling** or **hands-off infrastructure**?
    

## Conclusion

We explored **Kubernetes**, **Serverless**, and **VMs** through real-world lenses and design principles.

There’s **no one-size-fits-all**. Each option comes with strengths and trade-offs. Your decision should be based on:

* **Application architecture**
    
* **Traffic patterns**
    
* **Team expertise**
    

## What’s Next?

In the next blog/video, we’ll **design a complete microservices architecture** using Kubernetes, including CI/CD and observability setup.

📣 **Tell us** in the comments:  
*Which one do you prefer for your next project and why?*