---
title: "Getting started with Gcloud"
datePublished: Tue Jan 03 2023 23:00:29 GMT+0000 (Coordinated Universal Time)
cuid: clcgu36ky000108jseedec0kz
slug: getting-started-with-gcloud
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1672786793141/6651d887-f9bf-43ca-acfb-2c73c3f25d81.jpeg
tags: aws, kubernetes, devops, google-cloud-platform, gcp

---

* Command line interface to interact with Google Cloud Resources.
    
* Most GCP services can be managed from CLI using GCloud.
    
    * Compute Engine Virtual Machines
        
    * Managed Instance Groups
        
    * Databases
        
    * and .... many more
        
* We can create/delete/update/read existing resources and perform actions like deployments as well.
    
* (REMEMBER) Some GCP services have specific CLI tools:
    
    * Cloud Storage- gsutil
        
    * Cloud BigQuery- bq
        
    * Cloud BigTable- cbt
        
    * Kubernetes- kubectl(in addition to GCloud which is used to manage clusters)
        

## Installation

* GCloud is a part of Google Cloud SDK.
    
    * Cloud SDK requires python
        
    * Instructions to install Cloud SDK(and GCloud): [cloud.google.com/sdk/docs/install](http://cloud.google.com/sdk/docs/install)
        
* We can also use Gcloud on Cloud Shell
    

```plaintext
gcloud --version
```

* Initialize Gcloud
    
    * Initialize or reinitialize gcloud
        
    * Authorize gcloud to use your user account credentials
        
    * Setup configuration
        

```plaintext
gcloud init
```

* List all properties of the active configuration
    
    * region
        
    * zone
        
    * \[core\]
        
        * account
            
        * project
            

```plaintext
gcloud config list
```

## Understanding command structure in Gcloud to play with services

```plaintext
gcloud GROUP SUBGROUP ACTION ...
```

* GROUP:
    
    * config or compute or container or dataflow or functions or iam or....
        
        * which service group are you playing with.
            
* SUBGROUP:
    
    * Instances or images or instance templates or machine-types or regions or zones.
        
        * Which subgroup of the service do you want to play with?
            
* ACTION:
    
    * Create or list or start or stop or describe or...
        
        * What do you want to do?
            

### Example:

* List all GCP Compute Engine instances
    
    ```plaintext
    gcloud compute instances list
    ```
    
    * GROUP: compute
        
    * SUBGROUP: instances
        
    * ACTION: list
        
* Create a new instance
    
    ```plaintext
    gcloud compute instances create my-first-instance 
    ```
    
* Describe a specific instance\[all details around a compute instance\]
    
    ```plaintext
    gcloud compute instances describe my-first-instance
    ```
    
* Delete a instance
    
    ```plaintext
    gcloud compute instances delete my-first-instance
    ```
    
    ### Other examples
    
    ```plaintext
    gcloud compute regions list
    gcloud compute zones list
    gcloud compute machine-types list
    gcloud compute machine-types list --filter="zone:us-central1-b"
    ```
    
    ---
    
    ## Important things to remember
    
    * Cloud Shell is backed by a VM instance(automatically provisioned by Google Cloud when you launch Cloud Shell)
        
        * 5 GB of free persistent disk storage is provided as your $HOME directory
            
        * Prepackaged with the latest version of Cloud SDK, docker, etc.
            
        * (Remember) Files in your home directory persist between sessions(scripts, user configuration files like .bashrc and .vimrc, etc)
            
        * The instance is terminated if you are inactive for more than 20 minutes.
            
            * Any modifications that you made to it outside your $HOME will be lost.
                
        * (REMEMBER) After 120 days of inactivity, even your $HOME directory is deleted.
            
    * Cloud Shell can be used to SSH into virtual machines using their private IP addresses.