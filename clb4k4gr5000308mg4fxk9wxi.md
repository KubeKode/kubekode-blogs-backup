---
title: "Service Object in Kubernetes"
seoTitle: "Kubernetes, Load balancer service"
datePublished: Thu Dec 01 2022 04:08:37 GMT+0000 (Coordinated Universal Time)
cuid: clb4k4gr5000308mg4fxk9wxi
slug: service-object-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1669867215251/Cn4JvzhW_.jpg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1669867694289/Np2VongcI.jpg
tags: programming-blogs, docker, google-cloud, kubernetes, devops

---

- Each pod gets its own IP address, however, in a deployment, the set of pods running in one moment in time could be different from the set of pods running that application a moment later.

-  This leads to a problem: if some set of Pods(call them 'backends') provides functionality to other pods(call them 'frontend') inside your cluster, how do the frontends find out and keep track of which IP address to connect to, so that the frontend can use the backend part of the workload?

- When using RC(Replication controller), pods are terminated and created during scaling and replication operations.
- When using deployments, while updating the image version the pods are terminated and new pods take the place of other pods.
- Pods are very dynamic i.e they come & go on the k8s cluster and on any of the available nodes & it would be difficult to access the pods as the pod's IP changes once it's recreated.

## What are Services?
- Service Object is a logical bridge between pods and end users, which provides virtual IP(VIP).
- Service allows clients to reliably connect to the containers running in the Pod using the VIP.
- The VIP is not an actual IP connected to a network interface, but its purpose is purely to forward traffic to one or more pods.
- <b>Kube Proxy</b>:
  - is the one that keeps the mapping between the VIP and the pods up to date, which queries the API server to learn about new services in the cluster.

- Although each pod has a unique IP address, those IPs are not exposed outside the cluster.
- Services help to expose the VIP mapped to the pods & allow an application to receive traffic.
- Labels are used to select which are pods to be put under a service.
- Creating a service will create an endpoint to access the pods/application in it.
- Services can be exposed in different ways by specifying a type in the service spec.

### 4 types of services:
<ol>
<li>Cluster IP.(default)
<li>NodePort.
<li>Load Balancer.
<li>Headless.
</ol>

<li> Cluster IP.
<li> NodePort.
<li> Load Balancer.

- Created by cloud providers that will route external traffic to every node on the NodePort(eg. ELB on AWS)
<li>Headless:

- Creates several endpoints that are used to produce DNS records each DNS record is bound to a Pod.

## Important points to remember
<li> By Default Service can run only between ports 30000 - 32,767.
<li> The Set of pods targeted by a service is usually determined by a selector.

<hr>

## Cluster IP
- Exposes VIP only reachable from within the cluster.
- Mainly used to communicate between components of microservices.

<img src="https://tusharrajpoot.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcc778e99-4d30-4005-bdcd-4082f2b5addf%2FUntitled.png?table=block&id=64ac1bfb-6d2f-4671-9b1c-02de1eecd7ce&spaceId=a76e34cd-6f40-4ea9-b2ce-f4521ea829f1&width=2000&userId=&cache=v2">

- Create a deployment object with 1 pod and 1 container
```yml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: mydeployment1
spec:
  replicas: 1
  selector:
    matchLabels:
      name:  deployment1
  template:
    metadata:
      name: testpod1
      labels:
        name:  deployment1
    spec:
      containers:
        - name: c00
          image: httpd
          ports:
            - containerPort: 80
```

```sh
kubectl apply -f mydeploy.yml
```

- Create a service

```yml
kind: Service
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80 # containers port expose
      targetPort: 80  # Pods port
  selector:
    name: deployment # Apply this service to any pods which has the specific label
  type: ClusterIP # Specifies the service type i.e ClusterIP or NodePort.
```

```sh
kubectl apply -f service.yml
```

- Check services

```sh
kubectl get svc 
# or
kubectl get services
```

- Now Delete the running pod.

```sh
kubectl delete pod <POD_NAME>
```

- When we delete the running pod, a new pod will be create automatically.
- Now new pod will have a different IP address, but we can access also using our service VIP.

>Service VIP (type ClusterIP) will only work, inside the cluster(from any node).

<hr>

## NodePort service type

- makes a service accessible from outside the cluster.
- Exposes the service on the same port of each selected node in the cluster using NAT.

<img src="https://tusharrajpoot.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F934e5a4e-11c8-4a20-916e-5e62bf53b0b4%2FUntitled.png?table=block&id=c12f5409-effe-435a-ba2b-3a9a91c680e1&spaceId=a76e34cd-6f40-4ea9-b2ce-f4521ea829f1&width=2000&userId=&cache=v2">


```yml
kind: Service
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80 # containers port expose
      targetPort: 80  # Pods port
  selector:
    name: deployment1 # Apply this service to any pods which has the specific label
  type: NodePort
```

## Load Balancer Service Type in GKE
<img src="https://miro.medium.com/max/700/1*531E99qa7V2nDeIWdiTOxg.png">


```yml
kind: Service
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
    - port: 80 # containers port expose
      targetPort: 80  # Pods port
  selector:
    name: deployment1 # Apply this service to any pods which has the specific label
  type: LoadBalancer
```

after applying it, we get an external IP address and we can access our k8s service through that IP address.
<hr>
