---
title: "Setting up Github OIDC Authentication with GCP"
datePublished: Wed Mar 22 2023 11:16:12 GMT+0000 (Coordinated Universal Time)
cuid: clfjl9wel000e0aju8urs0x84
slug: setting-up-github-oidc-authentication-with-gcp
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679483082650/e8c527cc-d457-491e-b8ca-737f0ba0f9be.png
tags: programming-blogs, aws, github, devops, gcp

---

<iframe width="560" height="315" src="https://www.youtube.com/embed/-cp7GeQE2H0"></iframe>

If you're looking for a secure and streamlined way to authenticate users in your Google Cloud Platform (GCP) environment, you might want to consider using OpenID Connect (OIDC) authentication with Github. OIDC is an open standard that allows for authentication and authorization between different parties, while Github is a popular web-based platform for code management and collaboration. In this blog, we'll walk you through the steps to set up Github OIDC authentication with GCP.

### Step 1: Create a Github OAuth2 Application

First, you'll need to create a Github OAuth2 application. Go to your Github account settings and click on "Developer settings." Then, click on "OAuth Apps" and "New OAuth App." Fill in the required information, such as the application name, homepage URL, and callback URL. The callback URL should be the URL of the GCP identity provider service that will be used to authenticate users. Once you've filled in the required information, click on "Register Application" to create your Github OAuth2 application.

### Step 2: Create an Identity Provider in GCP

Next, you'll need to create an identity provider in GCP. Go to the GCP Console and navigate to "Identity & Access Management" (IAM). Click on "Identity Providers" and then "Create Identity Provider." Select "OpenID Connect" as the provider type and fill in the provider information, such as the provider ID and issuer URL. The issuer URL should be the URL of the Github OIDC server, which can be found in the Github OAuth2 application settings. Once you've filled in the required information, click on "Create" to create your identity provider.

### Step 3: Create a GCP Service Account

To use Github OIDC authentication with GCP, you'll need to create a GCP service account. Go to the GCP Console and navigate to "IAM & Admin" and then "Service Accounts." Click on "Create Service Account" and fill in the required information, such as the service account name and ID. Once you've filled in the required information, click on "Create" to create your service account.

### Step 4: Add IAM Roles to the Service Account

After you've created the service account, you'll need to add IAM roles to the account. IAM roles determine what actions the service account can perform in GCP. Go to the service account details page and click on "Add Member." Enter the email address of the service account and select the IAM role you want to assign to the account, such as "Cloud Identity-Auditor" or "Cloud Identity-User." Once you've selected the IAM role, click on "Save" to add the role to the service account.

### Step 5: Configure the Identity Provider in GCP

After you've created the identity provider and service account, you'll need to configure the identity provider in GCP. Go to the identity provider details page and click on "Edit." Fill in the required information, such as the client ID and client secret, which can be found in the Github OAuth2 application settings. Once you've filled in the required information, click on "Save" to configure the identity provider.

### Step 6: Test the Authentication

To test the authentication, you'll need to create a test user in Github and then log in to GCP using the test user's credentials. Go to your Github account settings and click on "OAuth Apps." Click on the Github OAuth2 application you created and then click on "Authorize." Enter your test user's credentials to authenticate with Github. Then, go to the GCP Console and click on "Sign In" and select "Continue with Github." Enter your test user's credentials again to authenticate with GCP

* Create a new workload Identity pool
    
    ```bash
    gcloud iam workload-identity-pools create "k8s-pool" \
    --project="${PROJECT_ID}" \
    --location="global" \
    --display-name="k8s Pool"
    ```
    
* Create a oidc identity provider for authenticating with Github
    
    ```bash
    gcloud iam workload-identity-pools providers create-oidc "k8s-provider" \
    --project="${PROJECT_ID}" \
    --location="global" \
    --workload-identity-pool="k8s-pool" \
    --display-name="k8s provider" \
    --attribute-mapping="google.subject=assertion.sub,attribute.actor=assertion.actor,attribute.aud=assertion.aud" \
    --issuer-uri="https://token.actions.githubusercontent.com"
    ```
    
* Create a service account with these permissions
    
    ```bash
    roles/compute.admin
    roles/container.admin
    roles/container.clusterAdmin
    roles/iam.serviceAccountTokenCreator
    roles/iam.serviceAccountUser
    roles/storage.admin
    ```
    
* Add IAM Policy bindings with Github repo, Identity provider and service account.
    
    ```bash
    gcloud iam service-accounts add-iam-policy-binding "${SERVICE_ACCOUNT_EMAIL}" \
    --project="${GCP_PROJECT_ID}" \
    --role="roles/iam.workloadIdentityUser" \
    --member="principalSet://iam.googleapis.com/projects/${GCP_PROJECT_NUMBER}/locations/global/workloadIdentityPools/k8s-pool/attribute.repository/${GITHUB_REPO}"
    ```