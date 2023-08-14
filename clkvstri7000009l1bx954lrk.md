---
title: "Important points to pass your GCP ACE Exam"
datePublished: Thu Aug 03 2023 23:39:22 GMT+0000 (Coordinated Universal Time)
cuid: clkvstri7000009l1bx954lrk
slug: important-points-to-pass-your-gcp-ace-exam
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691105875405/fcfb3a51-0d91-4b6b-9d9d-3c177b3a0467.png
tags: cloud, aws, devops, google-cloud-platform, certification

---

# Important points for ACE Exam

### GCP Management/Basics/Organization etc.

* Always recommended to create a new project, when need a new isolated environment.
    
* all resources that share common IAM policies need to be grouped together
    
    * Group by Folders.
        
    * projects sharing similar IAM policies can be grouped in a folder and then the IAM policies can be applied at the folder level.
        
* Cloud identity is a part of GSuite or Google Workspace.
    

---

### Google Market Place

* When need to deploy something quickly which is not a Google’s service like Jenkins/wordpress
    
    * Google Market Place
        

---

### Compute

* Preemptible VMs
    
    * Short-lived and low cost VMs, not permanent.
        
* Increasing Memory:
    
    * Select increasing memory manually when need to increase memory only of a VM instance not choose changing machine type.
        
* For Windows Instances
    
    * use gcloud compute reset-windows-password to retrieve the login credentials for the VM.
        
* For Login to VM using SSH without a public IP address
    
    * `Cloud Identity Aware Proxy` can be used to enable access to VMs that don’t have external IP address or don’t permit Direct access over internet.
        
* Using customized SSH Keys logging into Instances
    
    * Metadata managed: Upload the public key to project metadata
        
    * OS Login: Upload your publish SSH Key to your OS Login profile.
        
        * To enable: Set `enable-oslogin` to true in metadata.
            
        * `gcloud compute os-login ssh-keys add`
            
        * OS Login API: POST [https://oslogin.googleapis.com/v1/users/ACCOUNT\_EMAIL:importSshPublicKey](https://oslogin.googleapis.com/v1/users/ACCOUNT_EMAIL:importSshPublicKey)
            

---

### Persistent disks & Local SSDs

* If asked: cost-effective & primary storage, high IOPS with low cost - Choose Local SSD.
    
* Create a copy of VM
    
    * Create custom image from a snapshot
        
    * Create a VM from image
        

---

### VPC Related

* Largest CIDR range for a custom VPC?
    
    * 10.0.0.0/8
        
* Allow traffic from all IP?
    
    * 0.0.0.0/0
        
    * Remember:
        
        * 0.0.0.0/0 - represents all IP addresses
            
        * 0.0.0.0/32 - represents 1 IP (0.0.0.0)
            
* The `172.17.0.0/16` range [is reserved in Cloud SQL](https://cloud.google.com/sql/docs/mysql/private-ip#network_issues).
    
* **Shared VPC**
    
    * Communication between 2 GCP project, different VPCs.
        
    * Share the VPC from one project and request that the Compute Engine instances in the other project use this shared VPC.
        
    * resources in different projects to talk to each other
        
* **VPC Peering**
    
    * connect VPC networks across different organizations
        
    * Networks in same project, different projects and across projects in different organizations can be peered.
        
    * using internal IP addresses
        
* **Cloud VPN**
    
    * Connect on-premise network to GCP network.
        
        * Implemented using IPSec VPN Tunnel
            
        * Traffic through internet(public)
            
* **Cloud Interconnect**
    
    * physical connection between on-premise and VPC networks
        
        * Highly available and high throughput\*\*\*\*
            
* Subnets IP range can be expanded: **gcloud compute networks subnets expand-ip-range**
    
* Auto mode can be changed to custom but custom can’t .
    
* use service accounts to control access between the application servers and the database servers.
    

---

### App Engine

* 28 free instance hours per day.
    
* Versions Limit: Free app: 5, Paid App: 105
    

---

### Big Query

* If need to analyze the billing data
    
    * Export billing data to bigquery dataset.
        
* **External Tables**
    
    * Query external data sources without storing data in bigquery
        
        * Cloud Storage, Cloud SQL, BigTable, Google Drive
            
        * Use permanent or temporary external tables.
            
    * Use case: Joining 2 external data sources tables for data analytics like BigTable and Cloud storage.
        
        * create 2 external tables for bigtable and cloud storage
            
        * Join tables through bigquery console and apply appropriate filters.
            
* Data catalog
    
    * If you need to find something across multiple projects or datasets.
        
* Query cost reduction?
    
    * Apply user project-level custom query quota for Bigquery
        
    * Change your bigquery model from on-demand to flat rate.
        
* If data is broken, first step to debug or finding out the issue
    
    * Go to bigquery and review the job to look for errors.
        

---

### Billing

* Billing data can be exported(on schedule) to
    
    * Big Query(for analyze, visualizing)
        
    * Cloud Storage(for archiving)
        
* Cost Details on billing dashboard gives you cost details for a specific invoice.
    
* billing alert gets triggered on the project’s total cost and not just for specific service costs.
    
* If need alerts for specific service costs, follow this approach
    
    * exporting the billing data to bigquery and analyzing the charges incurred by service is the best option.
        

---

### Cloud Storage Related

* Lifecycle Policy rules
    
    ```json
    {
        "lifecycle":{
            "rule": [
                {
                    "action":{"type": "Delete"},
                    "condition":{
                        "age": 30,
                        "isLive": true
                    }
                },
                {
                    "action":{
                        "type": "SetStorageClass",
                        "storageClass": "NEARLINE"
                    },
                    "condition":{
                        "age": 365,
                        "matchesStorageClass":["MULTI_REGIONAL","STANDARD","DURABLE_REDUCED_AVAILABILITY"]
                    }
                }
            ]
        }
    }
    ```
    
* **Parallel composite uploads**
    
    * Faster uploads
        
    * File divided up to 32 chunks and uploaded in parallel.
        
* Cloud Storage Classes
    
    * Standard - Frequently accessed data
        
        * pricing
            
    * Nearline - Once a month
        
        * pricing
            
    * Coldline - Once a quarter
        
        * pricing
            
    * Archive - Once a year
        
        * pricing
            
* Sync files from on-premises to Cloud storage
    
    * Use gsutil CLI to write a script
        
* Optimal for regional outages and also cost effective storage
    
    * Dual regional
        
    * if cost-effective not mentioned: multi-regional
        
* For configuration from on-premises to Cloud storage
    
    * Create a VPN tunnel
        
    * configure DNS to resolve \*.[googleapis.com](http://googleapis.com) as a CNAME to [restricted.googleapis.com](http://restricted.googleapis.com).
        
* To change the storage class:
    
    ```bash
    gsutil rewrite -s[Storage-class] object-url
    ```
    

---

### Deployment Manager

* Use -preview option in deployment manager to observe the state of resource for the preview what is going to be created.
    
* Type Provider
    
    * A type provider exposes all of the resources of a third-party API to Deployment Manager as base types that you can use in your configurations. These types must be directly served by a RESTful API that supports Create, Read, Update, and Delete (CRUD).
        
    * If you want to use an API that is not automatically provided by Google with Deployment Manager, you must add the API as a type [provide](http://provider.Like)r
        
    * Like for creating kubernetes API objects like Daemonsets, pods, deployments etc.
        

---

### IAM

* Service account roles
    
    * `roles/iam.serviceAccountUser`
        
        * only provides the user with the ability to use service accounts, but it does not grant permissions to manage or administer service accounts.
            
    * `roles/iam.serviceAccountAdmin`
        
        * if need to manage all service accounts in a project.
            
        * provides the minimum set of permissions required to manage all service accounts for Google Cloud Projects.
            
* Follow least privileges principle.
    
* Custom Roles:
    
    * Custom roles include a launch stage, which is stored in the stage property for the role. The launch stage is informational; it helps you keep track of whether each role is ready for widespread use.
        
    * ALPHA means the role is still being developed or tested, or it includes permissions for Google Cloud services or features that are not yet public. It is not ready for widespread use.
        
* First priority is: Primitive and Predefined roles not custom roles.
    
* If user is not one, choose groups to provide the permission.
    
* When que is about Compute VM to Cloud Storage Access
    
    * Choose creating service account, provide access to cloud storage and assign custom sa to compute VM.(Even if between 2 projects)
        

**BigQuery**

* Billing Account Roles
    
    * billing.accounts.update” : to update something in billing account
        
* BigQuery Roles
    
    * **BigQuery Job User**
        
        * [`bigquery.jobs`](http://bigquery.jobs)`.create`\*\*\*\*
            
* If question is about an IAM role for bigquery access datasets, not to delete then choose
    
    * `custom role`
        

---

### Identity

* Setup SSO with third-party identity provider in Cloud Identity with Google as a service provider. When using any other idenity accounts like active directory.
    

---

### Logging and Monitoring

* Audit logs:
    
    * **Access Transparency Log**
        
    * **Cloud Audit Logs**
        
        * Admin Activity logs
            
        * data access logs
            
        * System Event logs
            
        * Policy denied logs
            
* Logs can be exported to
    
    * BigQuery
        
    * Cloud Storage
        
    * Pub/Sub(from pub/sub we can send logs anywhere)
        
* How to export logs?
    
    * Create sinks to the destinations using Log Router(filters can be added)
        
* Disable logging if there’s higher cost
    
    * For GKE: Go to the logs ingestion window in logging and disable the log source for the GKE container resource.
        
* If need to check creation time of a resource: use activity logs to view configuration category.
    
* alert notifications for CPU utilization
    
    * create cloud monitoring alert policy to that uses threshold as trigger condition.
        
    * configure email address in notification channel.
        

---

### GKE & Kubernetes Related

* If something needs to be run on each node
    
    * Use daemonsets
        
* *Node auto-upgrades* help you keep the [nodes](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture#nodes) in your [cluster](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture) up-to-date with the cluster control plane version when your control plane is [updated on your behalf](https://cloud.google.com/kubernetes-engine/upgrades#automatic_cp_upgrades).
    
* To Expose application using HTTPS on a public IP address
    
    * Create a Kubernetes Service of type NodePort for your application, and a Kubernetes Ingress to expose this Service via a Cloud Load Balancer.
        
    * Use Load Balancer service of kubernetes
        
* Need different configurations in nodes like GPU?
    
    * Add a new node pool with required configuration.
        
* Auto provisioning Feature in GKE
    
    * Node auto-provisioning is a mechanism of the cluster autoscaler, which scales on a per-node pool basis. With node auto-provisioning enabled, the cluster autoscaler can extend node pools automatically based on the specifications of unschedulable Pods. Using the GKE cluster's node auto-provisioning feature allows for dynamically provisioning nodes with GPUs only when they are needed. This helps optimize cost as resources are only allocated as necessary.
        
* Auto Scaling Cluster
    
    * Creating a GKE cluster with autoscaling enabled on the node pool allows for automated scaling of the cluster itself based on the workload.
        
    * By configuring a minimum and maximum size for the node pool, the cluster can automatically scale up or down the number of nodes based on the demand.
        
* Limits per cluster
    

| Limits | GKE Standard cluster | GKE Autopilot cluster |
| --- | --- | --- |
| Pods per node 2 | 256 Note: For GKE versions earlier than 1.23.5-gke.1300, the limit is 110 Pods. | 32 |
| Pods per cluster 1 | 200,000 2 | 25,000 |
| Containers per cluster | 400,000 | 25,000 |

---

### Load Balancing

* If question is about to replicate a VM from a single zone to multiple zones, for high availability setup a load balancer which balance traffic between multiple zones VM instances.
    
* HTTP(S) Load Balancer
    
    * If application used to server over HTTPS.
        
    * From Internet to VMs
        
    * External and Internal
        
* HTTPS on 443 port : HTTPS Load balancer
    
* SSL Proxy load balancer
    
    * reverse proxy load balancer that distributes SSL traffic coming from the internet to virtual machine (VM) instances in your Google Cloud VPC network.
        
    * ports supported: 25, 43, 110, 143, 195, 443, 465, 587, 700, 993, 995, 1883, 3389, 5222, 5432, 5671, 5672, 5900, 5901, 6379, 8085, 8099, 9092, 9200, and 9300.
        

---

### Managed Instance Groups

* Cool Down Period(Initial Delay)
    
    * Always choose increase the cool down period, if question is related to instances autoscales rapidly or more than expected.
        
* Rolling Update
    
    * Gradual update of instances in an instance group to the new instance template.
        
    * How should the update happen?
        
        * **Maximum surge**: How many instances are added at any point in time?
            
        * **Maximum unavailable**: How many instances can be offline during the update?
            

---

### Cloud Functions

* The default invocation limit is 1000/s.
    

---

### Cloud SQL

* Binary logging
    
* Only support MySQL and PostgreSQL
    

### Cloud Spanner

* ensuring that the primary key does not have monotonically increasing values can help in resolving the read latency-related performance issues.
    

---

### Gcloud CLI

* In Ubuntu, When you install SDK using apt Cloud SDK Component Manager is disabled and you need to install extra packages again using apt.
    
* Make separate configurations for multiple projects
    
    * activate the appropriate configuration for project to use.