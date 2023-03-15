---
title: "Deploy to Kubernetes on GKE"
datePublished: Sat Dec 24 2022 19:41:36 GMT+0000 (Coordinated Universal Time)
cuid: clc2ckvxt000208mf174o0lp9
slug: deploy-to-kubernetes-on-gke
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1671910826669/3cad715a-c533-4b9f-81e9-a28df57a756e.png
tags: docker, aws, kubernetes, devops, gke

---

### Tech used:

* Node.js
    
* Docker
    
* Kubernetes
    
* GKE(Google Kubernetes Engine)
    
* GCR(Google Container Registry)
    

# Steps

* Create kubernetes cluster on GKE.
    
* Setup Connection to created GKE cluster in with your local machine or cloud shell.
    
    ```sh
    gcloud container clusters get-credentials <CLUSTER_NAME> --zone <ZONE> --project <PROJECT_ID>
    ```
    
* Create a simple nodejs/express application.
    
* Write Dockerfile for the application
    
    ```plaintext
    FROM --platform=linux/amd64 node:14
    WORKDIR /usr/app
    COPY package.json .
    RUN npm install
    COPY . .
    EXPOSE 80
    CMD ["node","app.js"]
    ```
    
* Build the Docker image
    
    ```sh
    docker build -t us.gcr.io/<PROJECT_ID>/imagename:tag .
    ```
    
* Push docker image to GCR(Google Container Registry)
    
    ```sh
    docker push us.gcr.io/<PROJECT_ID>/imagename:tag
    ```
    
* Test the application using docker.
    
    ```sh
    docker run -d -p 3000:80 us.gcr.io/<PROJECT_ID>/imagename:tag
    ```
    
* Write kubernetes manifest file for deployment. `deploy.yml`
    
    ```yml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name:  nodeappdeployment
      labels:
        type: backend
        app: nodeapp
    spec:
      replicas: 1
      selector:
        matchLabels:
          type: backend
          app: nodeapp
      template:
        metadata:
          name: nodeapppod
          labels:
            type: backend
            app: nodeapp
        spec:
          containers:
            - name: nodecontainer
              image: us.gcr.io/<PROJECT_ID>/imagename:tag
              ports:
                - containerPort: 80
    ```
    
* Write kubernetes manifest file for service. `service.yml`
    
    ```yml
    kind: Service
    apiVersion: v1
    metadata:
      name: nodeapp-load-service
    spec:
      ports:
        - port: 80 
          targetPort: 80
      selector:
        type: backend
        app: nodeapp  
      type: LoadBalancer
    ```
    
* Apply manifest file to create deployment.
    
    ```sh
    kubectl apply -f deploy.yml
    ```
    
* Check status of the deployment.
    
    ```sh
    kubectl get deploy
    ```
    
* Apply manifest file to create load balancer service.
    
    ```sh
    kubectl apply -f service.yml
    ```
    
* Check status of service.
    
    ```sh
    kubectl get svc
    ```
    
* Check the external IP of the service in the browser.