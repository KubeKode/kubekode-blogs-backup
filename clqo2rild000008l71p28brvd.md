---
title: "Day 27: AWS IAM"
datePublished: Wed Dec 27 2023 17:53:38 GMT+0000 (Coordinated Universal Time)
cuid: clqo2rild000008l71p28brvd
slug: day-27-aws-iam
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703697411627/b4ce7dce-2e29-4464-b567-95a41122d6b6.png
tags: linux, aws, devops, aws-iam

---

**✨️*Introduction***

IAM stands for Identity and Access Management. It is like a central manager that helps you control who can enter your cloud, access AWS resources and what they can do once inside.

* **Identity:** IAM helps you manage the identities (like usernames or service accounts) that can interact with your AWS resources.
    
* **Access:** It determines what actions (like reading, writing, or deleting data) each identity is allowed to perform on AWS services.
    

---

### 🔸**Features of IAM:**

1. **Control Who Does What:** IAM lets you decide who can access AWS resources and what they can do with them.
    
2. **Extra Security with MFA:** You can add an extra layer of security by requiring users to use a second form of verification, like a smartphone app, along with their password.
    
3. **Simplify Access for Services:** IAM allows AWS services to access other services securely, reducing the need for secret keys.
    
4. **Fine-Tune Permissions:** You can specify exactly what each user or group can access, ensuring they only have the permissions they need.
    
5. **Access Advisor Insights:** IAM provides insights into permissions, helping you see what users and services are using and adjust permissions accordingly.
    

---

### 🔸**IAM Identities are categorized into three types:**

1. IAM users
    
2. IAM Groups
    
3. IAM Roles
    

### 🔹IAM Users*:*

**IAM users** are individual entities within your AWS account. They are human users, applications, or services that need to interact with your AWS resources. Here's a breakdown of IAM users:

* **Identification**: Each IAM user has a unique username, which is used for authentication. When creating an IAM user, you can choose a username that helps you identify the user, such as "John" or "MarketingApp."
    
* **Access Credentials**: IAM users can have access credentials, including a password or access keys (Access Key ID and Secret Access Key) for programmatic access. Passwords are used for the AWS Management Console, while access keys are for programmatic access through APIs or AWS CLI.
    
* **Permissions**: Users can have policies attached to them that define what actions they can perform and on which AWS resources. These policies control their level of access to AWS services.
    
* **Multi-Factor Authentication (MFA)**: You can enable MFA for IAM users, which adds an extra layer of security. It requires users to provide a second authentication factor, typically from a hardware token or a smartphone app, in addition to their password.
    

### 🔹IAM Groups:

**IAM groups** are collections of IAM users. They are a way to simplify the management of permissions for multiple users who require the same level of access. Here's how IAM groups work:

* **Group Membership**: Users can be added to one or more groups. This is useful when you have a set of users who share similar job roles or responsibilities, as they can be added to a group that defines their common permissions.
    
* **Policy Attachment**: Instead of attaching policies to individual users, you attach policies to IAM groups. All users in the group inherit the permissions defined in the group's policies. This makes it easier to ensure that users with similar roles have consistent access.
    
* **Scalability and Efficiency**: IAM groups make it more efficient to manage permissions. If you need to change permissions for multiple users at once, you can update the group's policies, and all group members receive the changes.
    

### 🔹IAM Roles:

**IAM roles** are a bit different from users and groups. They are meant for scenarios where permissions need to be granted to AWS services, temporary access for trusted entities, or cross-account access. Here's how IAM roles work:

* **Trust Relationships**: Roles are defined with trust relationships, specifying which entities or services can assume the role. For example, you can create a role that allows an EC2 instance to access an S3 bucket.
    
* **Temporary Permissions**: Roles provide temporary access. When a user or service assumes a role, they receive temporary security credentials, which are valid for a limited time. This adds a layer of security, and you can set specific permissions for the duration of the role's use.
    
* **Cross-Account Access**: Roles are used when you want to give another AWS account access to your resources. The other account assumes the role to access your resources securely, without having to share access keys.
    

In summary, IAM users are individual entities with their own credentials and permissions. IAM groups help manage users with similar permissions collectively. IAM roles are used to grant temporary, controlled access to AWS services or entities and can be utilized for cross-account access. Together, these IAM components allow you to efficiently manage and secure access to your AWS resources.

---

### 🔸**Important Terms associated with IAM Roles:**

D**elegation**: is a process of granting permission to the user to allow to the AWS resources that we control.

Delegation is **set up by the trusted account(which owns the AWS resources)** and **the trusting account (which needs access to AWS resources).**

Trusting & Trusted account is categorized into three:

1. Same account.
    
2. Two different accounts under the same organization.
    
3. Two different accounts which are owned by different organizations.
    

To delegate permission to access the resources, the IAM role is created in the trusting account by two policies.

*In AWS Identity and Access Management (IAM), "Permission Policy" and "Trust Policy" are two important policy types associated with IAM roles. Let's explore each of these policies:*

1. **Permission Policy (Attached to IAM Role)**:
    
    * **What it is**: A permission policy, often referred to simply as a "role policy" or "IAM policy," is a document that specifies the actions (permissions) a role is allowed or denied when it's assumed by an entity (such as an AWS service or a federated user).
        
    * **Where it's used**: Permission policies are attached to IAM roles. When an entity assumes the role, it inherits the permissions defined in the attached permission policies.
        
    * **What it contains**: A permission policy consists of one or more permissions statements, each of which includes an "Effect" (Allow or Deny), an "Action" (the specific AWS actions that are allowed or denied), and a "Resource" (the AWS resource to which the permissions apply).
        
    * **Example**: A permission policy might include a statement that allows the `s3:GetObject` action on all objects within a specific Amazon S3 bucket resource.
        
2. **Trust Policy (Attached to IAM Role)**:
    
    * **What it is**: A trust policy is a document that defines which AWS entities or accounts are allowed to assume a specific IAM role. It establishes trust relationships between the role and the trusted entities.
        
    * **Where it's used**: Trust policies are attached to IAM roles to specify who is authorized to assume that role.
        
    * **What it contains**: A trust policy typically includes a "Principal" element that specifies the trusted entity or entities (such as another AWS account, an AWS service, or a federated user) and an "Action" element that defines the action used for assuming the role (usually "sts: AssumeRole").
        

a. **Federation**: Process of creating the trust relationship between external service provider and AWS.

b. **Trust Policy: (JSON Format)** to define who is **allowed to use this role**. (written in IAM Policy language)

c. **Permission Policy: (JSON Format)** to define the actions and resources **that the role can use.** (written in IAM Policy language)

d. **Permissions Boundary**: (an advanced feature of AWS) which limits the maximum permission that a role can have, which is applied to both IAM User & role.

e. **Principal**: can be IAM User, Role or AWS account root user. Here Permission is granted in two ways.

1. Attach a Permission Policy to the role.
    
2. services that support resource-based policies, can identify the principal in the principal element of policy attached to the resource.
    

f. **Cross Account access : (Roles Vs Resource-Based Policies) —** allows you to grant access to the resource in one account to the trusted principal in another account.

---

### 🔸IAM Roles and Use Cases:

Two ways to use the roles:

1. **IAM Console:** When IAM users work in the IAM console and want to use the role, then they can access the permission of the role temporarily by giving up their original permission of the role, when the user exits the role's original permission is restored.
    
2. **Programmatic Access**: An AWS Service such as an EC2 Instance can use role by requesting temporarily security credentials using the programmatic requests to AWS.
    

An IAM Role can be used in the following ways:

1. IAM User
    
2. Applications and services
    
3. Federated Users.
    

Following are the cases of Roles:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698238073/ad803a42-7385-4d43-93c9-708c6d613c94.png align="center")

1. Switch to a role as an IAM user in one AWS account to access another account that you own.
    
2. Providing access to an AWS Service.
    
3. Providing access to external authenticated users. (sometimes users have identities outside of AWS such as corporate directory, if such users want to work with AWS resources then they should have security credentials. In such a situation role is to specify the permission for third-party identity providers\[IDP\]).
    

* **Web-Identity federation —** Users do not require any custom sign-in or user identities. Users can use any external identity provider (Facebook, amazon, google..), after login users get **an authentication token**, and they can **exchange an authentication token for receiving temporary security credentials**.
    
* **SAML—**based Federation **(Security Assertion Markup language)** is an open framework that many identity providers use, which provides a single sign-on to access the AWS Management console.
    

4\. Providing access to third parties: (when third party want to access the AWS resources then they can use roles to delegate access to them without sharing security credentials).

💎**Creating IAM Roles:**

Step 1:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698365046/1503d8e8-9038-474e-a4ac-5820950fd65e.png align="center")

Step 2:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698436145/006ae2d8-1a22-4d03-a535-5fecb076fd3e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698463658/6a911b54-a256-476b-b08c-c71d2d6cc6cb.png align="center")

Attach policies if required (JSON format Trust , Permission Policy)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698556594/b74630c7-7360-40de-8986-5907b15c1db9.png align="center")

Step : 3: Add Role Name , Description … and create role will create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698594722/d4102890-c6bb-4f8b-b640-c049d0ea696f.png align="center")

Step 4: Creating IAM roles for an IAM User. (sample where you can do same)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703698636028/4fbe843d-268d-401d-ab37-a11d08a0070e.png align="center")

---

***Thanks for reading to the end; I hope you gained some knowledge.❤️🙌***

[Linkedln](https://www.linkedin.com/in/vishesh-ghule/)

[Twitter](https://twitter.com/VisheshGhule)

[Github](https://github.com/VisheshGhule)