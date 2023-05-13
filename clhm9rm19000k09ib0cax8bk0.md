---
title: "Getting started with ArgoCD"
datePublished: Sat May 13 2023 17:36:46 GMT+0000 (Coordinated Universal Time)
cuid: clhm9rm19000k09ib0cax8bk0
slug: getting-started-with-argocd
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683997096293/108a4174-1fd2-47c7-be08-9b66cf06144d.avif
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1683999394465/fdc84a8f-9c58-4772-a3f5-10e78548d7e2.avif
tags: docker, kubernetes, git, devops, argocd

---

## What Is Argo CD?

* Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.
    
* It follows gitOps principles to deploy your application.
    
* It works like an agent installed on your Kubernetes cluster.
    
* It works on the pull deployment model.
    

## Push Deployment vs Push Deployment

* Push deployment is what you do in your CI/CD pipelines that you define using Jenkins/Github Actions/Gitlab CICD/Circle CI.
    
* When you define your CICD pipeline using push deployment what you do is, you add steps to build, test your application and deploy it to Kubernetes.
    
* You add your CD steps also in your CICD pipeline and you need to configure secrets and credentials for your k8s cluster, aws/GCP in your pipeline.
    
* But if you use pull deployment, you don't need to add your CD steps in your pipeline instead you can use ArgoCD.
    
* You just need to install argocd on your kubernetes cluster and create an application using argocd and map it to your github repository(Where you have your kubernetes configuration files).
    
* ArgoCD works like an agent installed on your k8s cluster that works on pull deployment, checks your configuration files from your git repo and always match your desired state with actual state.
    
* If it finds any difference, it makes changes required to match your desired state with actual state.
    
* It provides an amazing UI where you can create, monitor your k8s deployments and configurations.
    

## Why ArgoCD?

Application definitions, configurations, and environments should be declarative and version controlled. Application deployment and lifecycle management should be automated, auditable, and easy to understand.

## Steps to get started with ArgoCD

### 1\. Install argocd on your kubernetes cluster.

* For cluster, you can use minikube cluster for getting started.
    
* ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```
    
* Now if you check services in argocd namespace, you will see a service (ClusterIP) named argocd-server.
    
* To access argocd-server you port-forward your clusterIP service to 8080 port
    
* ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
    
* Now you access your argocd server on localhost:8080
    
* To login into your argocd server you can use credentials from your kubernetes namespace.
    
    * Username: admin
        
    * Password: You can get from a secret in your k8s cluster.
        
    * ```bash
        kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
        ```
        

### 2\. Create application on argocd and map to Git repo

* Create a application yaml file for argocd application
    
* ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
      name: myapp-argo-application
      namespace: argocd
    spec:
      destination:
        server: https://kubernetes.default.svc
        namespace: myapp
      source:
        repoURL: https://github.com/KubeKode/youtube
        targetRevision: HEAD
        path: k8s
      project: default
      syncPolicy:
        syncOptions:
          - CreateNamespace=true
        automated:
          selfHeal: true
          prune: true
    ```
    
* Apply the config file using kubectl
    
* ```yaml
    kubectl apply -f application.yml
    ```
    

### 3\. Check application deployment

* Now just access your argocd server and you can see your application and deployments.
    

---