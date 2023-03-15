---
title: "Kubernetes Networking Visualized"
seoTitle: "Kubernetes Visualized"
datePublished: Thu Nov 17 2022 22:18:22 GMT+0000 (Coordinated Universal Time)
cuid: clalmvz6t000608medbct6pkp
slug: kubernetes-networking-visualized
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1668723167490/a-aYWzyTb.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1668723484164/DQWU3XT5q.png
tags: docker, aws, kubernetes, devops, docker-devops-kubernetes-aws-web-development

---

### Kubernetes Networking addresses four concerns:
1. Containers within a pod use networking to communicate via loopback.
2. Cluster Networking provides communication between different pods.
3. The service resources let you expose an application running in pods to be reachable from outside of your cluster.
4. You can also use services to publish services only for consumption inside your cluster.


![1_vQQSViWiDkh1X81r3n99Qw.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1668723211036/2NCHvtVLC.webp align="left")

## Container to Container communication on same pod 

![1_pyGYl93pM4b2rJ49DHgJlg.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1668723287108/wthDPgKJZ.webp align="left")
- happens through localhost within the containers.

```yml
kind: Pod
apiVersion: v1
metadata:
  name: testpod
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello; sleep 5; done"]
    - name: c01
      image: httpd
      ports:
        - containerPort: 80
```

```sh
kubectl apply -f pod.yml
```

```sh
kubectl get pods
kubectl exec testpod -it -c c00 -- /bin/bash
```

- inside the container

```sh
apt update
apt install curl
curl localhost:80
```

## Communication between two different Pods within same machine(Node)
- Pod to Pod communication on same worker node through Pod IP.
- By Default Pod's IP will not be acccessible outside the node.

![1_-9yFiujQtPJL1tczRDeavg.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1668723333787/BIXpJ_8pf.webp align="left")

- Create 2 pods

```yml
kind: Pod
apiVersion: v1
metadata:
  name: testpod1
spec:
  containers:
    - name: c00
      image: nginx
      command: ["/bin/bash","-c","while true; do echo Hello; sleep 5; done"]
      ports:
        - containerPort: 80
```

```yml
kind: Pod
apiVersion: v1
metadata:
  name: testpod2
spec:
  containers:
    - name: c03
      image: httpd
      ports:
        - containerPort: 80
```

```sh
kubectl apply -f pod2.yml
kubectl apply -f pod3.yml
```

- Check pods are running, and pods wide description

```sh
kubectl get pods
kubectl get pods -o wide
```


- Inside node, run commands to get request on pods IP addresses

```sh
curl <POD_ID>:80
```


<hr>