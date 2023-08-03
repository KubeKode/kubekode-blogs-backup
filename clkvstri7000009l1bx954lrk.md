---
title: "Important points to pass your GCP ACE Exam"
datePublished: Thu Aug 03 2023 23:39:22 GMT+0000 (Coordinated Universal Time)
cuid: clkvstri7000009l1bx954lrk
slug: important-points-to-pass-your-gcp-ace-exam
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691105875405/fcfb3a51-0d91-4b6b-9d9d-3c177b3a0467.png
tags: cloud, aws, devops, google-cloud-platform, certification

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
        
* Using customized SSH Keys logging into Instances
    
    * Metadata managed: Upload the public key to project metadata
        
    * OS Login: Upload your publish SSH Key to your OS Login profile.
        
        * To enable: Set `enable-oslogin` to true in metadata.
            
        * `gcloud compute os-login ssh-keys add`
            
        * OS Login API: POST [https://oslogin.googleapis.com/v1/users/ACCOUNT\_EMAIL:importSshPublicKey](https://oslogin.googleapis.com/v1/users/ACCOUNT_EMAIL:importSshPublicKey)
            

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
    
* Auto mode can be changed to custom but custom can’t
    

---

### App Engine

* 28 free instance hours per day.
    
* Versions Limit: Free app: 5, Paid App: 105
    

---

### Big Query Related

* If need to analyze the billing data
    
    * Export billing data to bigquery dataset.
        
* **External Tables**
    
    * Query external data sources without storing data in bigquery
        
        * Cloud Storage, Cloud SQL, BigTable, Google Drive
            
        * Use permanent or temporary external tables.
            
* Data catalog
    
    * If you need to find something across multiple projects or datasets.
        
* Query cost reduction?
    
    * Apply user project-level custom query quota for Bigquery
        
    * Change your bigquery model from on-demand to flat rate.
        

---

### Billing

* Billing data can be exported(on schedule) to
    
    * Big Query(for analyze, visualizing)
        
    * Cloud Storage(for archiving)
        
* Cost Details on billing dashboard gives you cost details for a specific invoice.
    

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
        
    * Nearline - Once a month
        
    * Coldline - Once a quarter
        
    * Archive - Once a year
        
* To change the storage class:
    
    ```json
    gsutil rewrite -s[Storage-class] object-url
    ```
    

---

### Deployment Manager

* Use -preview option in deployment manager to observe the state of resource for the preview what is going to be created.
    
* Type Provider
    
    * A type provider exposes all of the resources of a third-party API to Deployment Manager as base types that you can use in your configurations. These types must be directly served by a RESTful API that supports Create, Read, Update, and Delete (CRUD).
        
    * If you want to use an API that is not automatically provided by Google with Deployment Manager, you must add the API as a type [provide](http://provider.like/)r
        
    * Like for creating kubernetes API objects like Daemonsets, pods, deployments etc.
        

---

### IAM

* Follow least privileges principle.
    
* First priority is: Primitive and Predefined roles not custom roles.
    
* If user is not one, choose groups to provide the permission.
    
* When que is about Compute VM to Cloud Storage Access
    
    * Choose creating service account, provide access to cloud storage and assign custom sa to compute VM.
        
* Billing Account Roles
    
    * billing.accounts.update” : to update something in billing account
        
* BigQuery Roles
    
    * **BigQuery Job User**
        
        * [`bigquery.jobs`](http://bigquery.jobs)`.create`\*\*\*\*
            

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
        
* Limits per cluster
    

| Limits | GKE Standard cluster | GKE Autopilot cluster |
| --- | --- | --- |
| Pods per node 2 | 256 Note: For GKE versions earlier than 1.23.5-gke.1300, the limit is 110 Pods. | 32 |
| Pods per cluster 1 | 200,000 2 | 25,000 |
| Containers per cluster | 400,000 | 25,000 |

---

### Load Balancing

* HTTP(S) Load Balancer
    
    * If application used to server over HTTPS.
        
    * From Internet to VMs
        
    * External and Internal
        

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