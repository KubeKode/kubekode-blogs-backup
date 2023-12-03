---
title: "Day 24: AWS EC2"
datePublished: Sun Dec 03 2023 19:08:54 GMT+0000 (Coordinated Universal Time)
cuid: clppuvv7j000509lcf0z2082i
slug: day-24-aws-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1701626487100/208d8718-1003-4e58-b091-437ea47d3003.webp
tags: ec2, linux, aws, devops

---

**✨️As we all know what is EC2 instance and how can we use it, in this blog we will discuss more on types of EC2 instances, pricing models and security of EC2 instances**

### **🔸Types of EC2 Computing Instances:**

**AWS offers 7 types of instances which are as follows:**

### **1\. General Purpose Instances (T-Series):**

* **Use Cases**: These instances are suitable for a broad range of workloads, including web servers, development environments, and small to medium databases. They are cost-effective for applications with varying workloads.
    
* **Instance Types**:
    
    * `t2.nano`, `t2.micro`, `t2.small`, `t2.medium`, `t2.large`, `t2.xlarge`, `t2.2xlarge`, `t3.nano`, `t3.micro`, `t3.small`, `t3.medium`, `t3.large`, `t3.xlarge`, `t3.2xlarge`, `t4g.nano`, `t4g.micro`, `t4g.small`, `t4g.medium`, `t4g.large`, `t4g.xlarge`, `t4g.2xlarge`
        
* **Key Features**: Burstable CPU performance, suitable for workloads with periodic high CPU demands.
    

---

### **2\. (C-Series): Compute Optimized Instances**

* **Use Cases**: These instances are optimized for compute-intensive workloads like high-performance web servers, batch processing, and scientific modeling.
    
* **Instance Types**:
    
    * `c4.large`, `c4.xlarge`, `c4.2xlarge`, `c4.4xlarge`, `c4.8xlarge`, `c5.large`, `c5.xlarge`, `c5.2xlarge`, `c5.4xlarge`, `c5.9xlarge`, `c5.12xlarge`, `c5.18xlarge`, `c5.24xlarge`, `c6g.nano`, `c6g.micro`, `c6g.small`, `c6g.medium`, `c6g.large`, `c6g.xlarge`, `c6g.2xlarge`
        
* **Key Features**: High CPU performance, ideal for compute-bound applications.
    

---

### **3\. Memory Optimized Instances (R-Series and X-Series):**

* **Use Cases**: Memory-optimized instances are best suited for in-memory databases, big data processing, and applications that require substantial RAM.
    
* **Instance Types**:
    
    * `r3.large`, `r3.xlarge`, `r3.2xlarge`, `r3.4xlarge`, `r3.8xlarge`, `r4.large`, `r4.xlarge`, `r4.2xlarge`, `r4.4xlarge`, `r4.8xlarge`, `r4.16xlarge`, `r5.large`, `r5.xlarge`, `r5.2xlarge`, `r5.4xlarge`, `r5.8xlarge`, `r5.12xlarge`, `r5.16xlarge`, `r5.24xlarge`, `x1.16xlarge`, `x1.32xlarge`, `x1e.xlarge`, `x1e.2xlarge`, `x1e.4xlarge`, `x1e.8xlarge`, `x1e.16xlarge`, `x1e.32xlarge`
        
* **Key Features**: High memory capacity, suitable for data-intensive and memory-bound applications.
    

---

### **4\. Storage Optimized Instances (I-Series and D-Series):**

* **Use Cases**: Storage-optimized instances are designed for applications that require high-speed, low-latency storage, such as NoSQL databases and data warehousing.
    
* **Instance Types**:
    
    * `i3.large`, `i3.xlarge`, `i3.2xlarge`, `i3.4xlarge`, `i3.8xlarge`,`i3.16xlarge`, `i3.metal`, `d2.xlarge`, `d2.2xlarge`, `d2.4xlarge`, `d2.8xlarge`
        
* **Key Features**: High-speed, high-capacity storage for data-intensive applications.
    

---

### **5\. Accelerated Computing Instances (P-Series, G-Series, F-Series, and Inf1):**

* **Use Cases**: These instances are equipped with GPUs or specialized hardware for tasks like machine learning, graphics rendering, and video encoding.
    
* **Instance Types**:
    
    * `p2.xlarge`, `p2.8xlarge`, `p2.16xlarge`, `p3.2xlarge`, `p3.8xlarge`, `p3.16xlarge`, `p4d.24xlarge`, `g3s.xlarge`, `g3.4xlarge`, `g3.8xlarge`, `g3.16xlarge`, `g4dn.xlarge`, `g4dn.2xlarge`, `g4dn.4xlarge`, `g4dn.8xlarge`, `g4dn.12xlarge`, `g4dn.16xlarge`, `g4ad.4xlarge`, `f1.2xlarge`, `f1.4xlarge`, `f1.16xlarge`, `inf1.xlarge`, `inf1.2xlarge`, `inf1.6xlarge`
        
* **Key Features**: Specialized hardware for tasks that require GPU or FPGA acceleration.
    

---

### **6\. Networking Instances (N-Series):**

* **Use Cases**: Instances in this category offer high-speed, low-latency networking capabilities, making them ideal for applications that require substantial network performance.
    
* **Instance Types**:
    
    * `n1-standard-1`, `n1-standard-2`, `n1-standard-4`, `n1-standard-8`, `n1-standard-16`, `n1-highmem-2`, `n1-highmem-4`, `n1-highmem-8`, `n1-highmem-16`, `n1-highcpu-2`, `n1-highcpu-4`, `n1-highcpu-8`, `n1-highcpu-16`
        
* **Key Features**: High-speed networking for applications that rely heavily on network performance.
    

---

### **7\. Arm-Based Instances (A1-Series, M6g-Series, C6g-Series, R6g-Series, T4g-Series):**

* **Use Cases**: These instances use ARM processors and are optimized for cost-effective performance in various workloads, including web servers and containerized applications.
    
* **Instance Types**: Numerous instance types available across different series.
    
* **Key Features**: Cost-effective performance, ideal for ARM-compatible workloads.
    

---

### **🔸Burstable Performance Instances**

*T2 instances* are burstable instances, meaning the CPU performs at a baseline, say 20% of its capability. When your application needs more than 20% of the performance of the CPU, the CPU enters into a burst mode giving the higher performance for a limited amount of time, therefore work happens faster.

* You get these credits when your CPU is idle.
    
* Each CPU credit gives a burst of 1 minute to the CPU.
    
* If your CPU credits are not used they are credited to your account and they stay there for 24 hours.
    
* Based on your credit balance, you can decide whether the t2 instance, should be scaled up or down.
    
* These bursts happen at a cost, every time a burst happens in a CPU, CPU credits are used.
    

Let’s understand this through an example. Suppose in a company they have to follow the following tasks:

***1.Analysis of customer’s data***

Customer’s website activity, etc. should all be monitored in real-time. There will be times when the traffic on the website will be minimal, therefore using a very powerful processor should not be considered, as it will become expensive for the company because it will not be used for every hour of the day. Hence, for this task, we might take **t2 instances** because they give **Burstable CPU performance** i.e. when the traffic is more the CPU performance will be increased accordingly to meet the requirements.

***2\. auto-response emailing system***

It should be quick, therefore we would require systems, where the response time is as short as possible. This could be achieved by using **EBS-optimized instances**, as they offer high IOPS and hence, low latencies.

***3\. The search engine on our website***

It should be able to sort the keywords and return relevant results, therefore we might have 2 servers for this. One is the database and the other server for processing the keywords. Therefore, the communication between these servers should be at the maximum possible rate. To achieve this, we can put them in a placement group and for that, we have to use **Cluster Networking Instances.**

***4\. Some processes in every organization are highly confidential***

Because these processes give us an edge over other companies, no matter how secure the servers, maybe, some policies are still made to be sure. Therefore, we might use **Dedicated Instances** for these kinds of processes.

* ***Auto Scaling***
    

*Auto Scaling* is a service designed by AWS EC2, which automatically launches or terminates EC2’s instances based on user-defined policies, schedules, and health checks.

* **Elastic Load Balancing**
    

*Elastic Load Balancing* (ELB) automatically distributes incoming application traffic across multiple EC2 instances, in multiple Availability Zones.

Availability zones are places where Amazon has set up its servers. Since they have customers from the whole globe, they have set up multiple Availability zones to reduce the latency.

*Elastic IP Addresses* are static IP addresses that are associated with your AWS account, they can be used to mask the failure of an instance by automatically remapping your address to another working instance in your account.

* **Understanding the EC2 Price Model :**
    

There are 3 common types of Billing for EC2

* On-Demand
    
* Reserved
    
* Spot
    

* **On-Demand Instances**: Offer flexibility with no upfront costs but are generally more expensive for continuous, predictable workloads.
    
* **Reserved Instances**: Provide significant cost savings for workloads with a steady or predictable usage pattern but require an upfront commitment.
    
* **Spot Instances**: Offer the most cost-effective option but come with the caveat of potential interruptions. They are suitable for workloads that can handle these interruptions and don't require continuous, predictable usage.
    

---

***Thanks for reading to the end; I hope you gained some knowledge.❤️🙌***

[Linkedln](https://www.linkedin.com/in/vishesh-ghule/)

[Twitter](https://twitter.com/VisheshGhule)

[Github](https://github.com/VisheshGhule)