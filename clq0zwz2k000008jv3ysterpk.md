---
title: "Day 26: Amazon S3 (Simple Storage Service)"
datePublished: Mon Dec 11 2023 14:15:11 GMT+0000 (Coordinated Universal Time)
cuid: clq0zwz2k000008jv3ysterpk
slug: day-26-amazon-s3-simple-storage-service
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702294694189/4959da15-ee43-4429-bd59-789f51e8c5b1.png
tags: aws, devops, aws-s3

---

Amazon S3 (Simple Storage Service) is a foundational AWS service that has revolutionized cloud storage. In this blog, we'll explore the key aspects of Amazon S3, from its basic to advanced features.

S3 uses a flat namespace, where data is stored in buckets, each with a unique name. Inside a bucket, data is organized into objects. These objects are identified by a unique key, and their content can range from documents and images to application data and backups.

**Key Features of S3:**

Amazon S3 is an object storage service designed to store and retrieve any amount of data, anywhere on the web. Key features include scalability, durability, security, and versatility. Whether you're a startup or a global enterprise, S3's capabilities can meet your storage needs.

**Security:**

Amazon takes security very seriously. Being in the Cloud and serving the thousands of organizations using these services means an exceptional level of security is required. AWS provides multiple levels of security, let’s go through them one by one to understand them in detail.

Let’s divide the overall security part into two: Data Access Security and Data Storage Security.

  
**Identity and Access Management (IAM) policies:**

IAM policies apply to specific principles like User, Group, and Role. The policy is a JSON document, which mentions what the principle can or can not do.

An example IAM policy will look like this. Any IAM entity (user, role, group) having the below policy can access the `app gambit-s3access-test` bucket and objects inside that.

```bash
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::appgambit-s3access-test",
                 "arn:aws:s3:::appgambit-s3access-test/*"]
    }
  ]
}
```

**Bucket policies**

Bucket policy uses JSON-based access policy language to manage advanced permissions. If you want to make all the objects inside a bucket publicly accessible, then following simple JSON will do that. Bucket policies are only applicable to S3 buckets.

```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "MakeBucketPublic",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::appgambit-s3access-test/*"
        }
    ]
}
```

Bucket policies are very versatile and have lot of configuration options. Let’s say the bucket is service web assets and it should only serve content to requests originating from a specific domain.

```bash
{
  "Version":"2012-10-17",
  "Id":"Allow Website Access",
  "Statement":[
    {
      "Sid":"Allow Access to only appgambit.com",
      "Effect":"Allow",
      "Principal":"*",
      "Action":"s3:GetObject",
      "Resource":"arn:aws:s3:::appgambit-s3access-test/*",
      "Condition":{
        "StringLike":{
            "aws:Referer":[
                "https://www.appgambit.com/*",
                "https://appgambit.com/*"
            ]
        }
      }
    }
  ]
}
```

**Access Control Lists (ACLs)**

ACL is a legacy access policy option to grant basic read/write permissions to other AWS accounts.

**Query String Authentication**

Imagine you have private content that you want to share with your authenticated users out of your application, like sending the content link via Email which only that user can access.

AWS S3 allows you to create a specialized URL that contains information to access the object. This method is also known as a Pre-Signed URL.  
Data Access Security

By default when you create a new bucket, only you have access to Amazon S3 resources they create. You can use access control mechanisms such as bucket policies and Access Control Lists (ACLs) to selectively grant permissions to users and groups of users. Customers may use four mechanisms for controlling access to Amazon S3 resources.

### Backup & Recovery

By default, the AWS S3 provides the same level of durability and availability across all the regions. But then also the things can go wrong, so most organisations when they use Cloud to host their data, would like to have backup and recovery in their plan.

AWS S3 provides a simple mechanism to create a backup for data, Cross Region Replication. Cross-region Replication enables automatic and asynchronous copying of objects across buckets in different AWS regions. This is useful in case we want to access our data in different regions or create a general backup of the data.

> **Cross-Origin Resource Sharing (CORS):**
> 
> Cross-Origin Resource Sharing (CORS) configuration in Amazon S3 allows you to control which web domains or websites are permitted to access the resources (such as objects) in your S3 bucket from a web browser. This is essential for web applications that need to make cross-origin requests to S3. Cross Region Replication requires Versioning enabled, so this will have an impact on your AWS billing amount as well. The CRR includes the Versioning cost as well as the Data Transfer cost.

* Here's how you can configure CORS in an S3 bucket:
    
    1. **Sign in to AWS Management Console**: Sign in to your AWS account and navigate to the S3 service.
        
    2. **Select Your Bucket**: Click on the name of the S3 bucket for which you want to configure CORS.
        
    3. **Access Permissions Tab**: In the bucket's properties, navigate to the "Permissions" tab.
        
    4. **CORS Configuration**: Scroll down to the "Cross-origin resource sharing (CORS)" section, and click on "Edit" to configure CORS rules.
        
    5. **Create CORS Rules**: You can add one or more CORS rules, which are defined in JSON format. CORS rules typically include the following components:
        
        * **AllowedOrigins (Origins)**: This specifies the list of domains (origins) that are allowed to make cross-origin requests to your S3 bucket. For example:
            

* ```bash
        [
            "http://example.com",
            "https://example.net"
        ]
    ```
    
* **AllowedMethods (HTTP methods)**: You can specify which HTTP methods (e.g., GET, PUT, POST) are allowed in cross-origin requests. For example:
    
* ```bash
        [
            "GET",
            "HEAD",
            "PUT"
        ]
    ```
    
* **AllowedHeaders**: You can specify which HTTP headers can be included in the actual request from the client. For example:
    
* ```bash
        [
            "Authorization",
            "Content-Type"
        ]
    ```
    
* **ExposeHeaders**: You can specify which response headers can be exposed to the client. For example:
    

* 1. * * ```bash
                    [
                        "x-amz-server-side-encryption"
                    ]
                ```
                
            * **MaxAgeSeconds**: This defines how long the preflight request (an initial request sent before the actual request) is cached by the browser, in seconds. For example:
                
            * **Save Your CORS Configuration**: After specifying your desired CORS rules, click "Save" to save your CORS configuration.
                

**Storage Classes of S3:**  
Amazon S3 offers several storage classes, each designed to address different use cases and optimize costs based on your data's access patterns and retrieval requirements. Here's an overview of the various storage classes available in Amazon S3:

1. **S3 Standard**:
    
    * **Use Case**: This is the default storage class, suitable for frequently accessed data that requires low-latency retrieval. It's ideal for data that is actively used and needs high availability.
        
    * **Durability**: 99.999999999% (11 9's)
        
    * **Availability**: 99.99%
        
2. **S3 Intelligent-Tiering**:
    
    * **Use Case**: Designed for data with unknown or changing access patterns. It automatically moves objects between two access tiers (frequent and infrequent) based on changing access patterns, optimizing costs without any performance impact.
        
    * **Durability**: 99.999999999% (11 9's)
        
    * **Availability**: 99.9%
        
3. **S3 Standard-IA (Infrequent Access)**:
    
    * **Use Case**: Suitable for data that is accessed less frequently but still requires low-latency retrieval when needed. It's a cost-effective choice for data that can tolerate slightly higher retrieval times.
        
    * **Durability**: 99.999999999% (11 9's)
        
    * **Availability**: 99.9%
        
4. **S3 One Zone-IA**:
    
    * **Use Case**: Similar to Standard-IA but stores data in a single availability zone, offering lower costs. It's a good choice for data that can be easily recreated if lost, and where durability is less critical.
        
    * **Durability**: 99.999999999% (11 9's) within a single availability zone
        
    * **Availability**: 99.5%
        
5. **S3 Glacier and Glacier Deep Archive**:
    
    * **Use Case**: Designed for long-term archival of data that is accessed infrequently, such as compliance data, backups, and historical records. Data is archived and has longer retrieval times.
        
    * **Durability**: 99.999999999% (11 9's)
        
    * **Availability**: Glacier: Not designed for immediate retrieval; retrieval times vary. Glacier Deep Archive: Longer retrieval times.
        
6. **S3 Glacier Storage Class**:
    
    * **Use Case**: Similar to S3 Glacier, but with configurable retrieval times. You can choose between expedited, standard, or bulk retrieval options based on your needs.
        
7. **S3 Outposts**:
    
    * **Use Case**: Intended for data stored on AWS Outposts, which are AWS-managed, on-premises data centers. It allows data stored on Outposts to be seamlessly integrated with S3.
        
8. **S3 Reduced Redundancy Storage (RRS)** (Deprecated):
    
    * **Use Case**: Previously, RRS was an option for non-critical, reproducible data. However, AWS has deprecated this storage class, and users are encouraged to use other options, like S3 Standard or S3 Standard-IA.
        

---

***Thanks for reading to the end; I hope you gained some knowledge.❤️🙌***

[Linkedln](https://www.linkedin.com/in/vishesh-ghule/)

[Twitter](https://twitter.com/VisheshGhule)

[Github](https://github.com/VisheshGhule)