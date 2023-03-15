---
title: "Labels, Selectors, Replication, and Replicaset in Kubernetes"
seoTitle: "Kubernetes DevOps"
datePublished: Thu Nov 24 2022 19:08:40 GMT+0000 (Coordinated Universal Time)
cuid: clavg6zd9000108jxgha8dmy7
slug: labels-selectors-replication-and-replicaset-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1669316883525/7ReV_6MSw.jpg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1669316889758/FLS7SHmkw.jpg
tags: docker, aws, javascript, kubernetes, devops

---

# Labels

<li> Labels are the mechanism you use to organize Kubernetes Objects.
<li>A label is a key-value pair without any predefined meaning that can be attached to the objects.
<li>Labels are similar to tags in AWS or git where you use a name for quick reference.
<li>So we are free to choose labels as we need them to refer to an environment that is used for dev or testing or production, refer a product group like department A, or department B.
<li>Multiple labels can be added to a single object.

```yml
# pod5.yml
kind: Pod
apiVersion: v1
metadata:
  name: delhipod
  labels:
    env: development
    class: pods
spec:
  containers:
    - name: c00
      image: ubuntu
      command:
        [
          "/bin/bash",
          "-c",
          "while true; do echo Hello DevOps; sleep 5 ; done",
        ]
```

```sh
kubectl apply -f pod5.yml
kubectl get pods --show-labels
```

<li>Now if you want to add a label to an existing pod

```sh
kubectl label pods delhipod myname=tushar
kubectl get pods --show-labels
```

<li>Now, list pods matching a label

```sh
kubectl get pods -l env=development
```

<li>Now, give a list, where 'development' label is not present

```sh
kubectl get pods -l env!=development
```

<li>Now, if you want to delete pod using labels

```sh
kubectl delete pod -l env!=development
kubectl get pods
```

# Label-selector

<li>Unlike name/UIDs, labels do not provide uniqueness, as in general, we can expect many objects to carry the same labels.
<li>Once labels are attached to an object, we would need filters to narrow down and these are called as label selectors.
<li>The API currently supports two types of selectors - Equality based and set based.
<li>A label selector can be made multiple requiremnents which are comma-separated.

#### <li>Equality based:

```sh
(=,!=)
name: tushar
class: nodes
project: development
```

#### <li>Set based:

```sh
(in, notin and exists)

env in (production, dev)
env notin (team1,team2)
```

<li>Kubernetes also supports set-based selectorrs i.e match multiple values.

```sh
kubectl get pods -l 'env in (development, testing)'

kubectl get pods -l 'env notin (development, testing)'

kubectl get pods -l class=pods,myname=tushar
```

# Node-Selector

<li>One use case for selecting labels is to constrain the set of nodes onto which a pod can schedule i.e you can tell a pod can schedule i.e you can tell a pod to only be able to run a particular nodes.
<li>Generally such constraints are unnecessary as the scheduler will automatically do a reasonable placement, but on certain circumstances we might need it.
<li>We can use labels to tag nodes.
<li>You the nodes are tagged, can use the label selectors to specify the pods run only of specific nodes.
<li>first we give label to the node.
<li>Then use node-selector to the pod configuration.

```yml
kind: Pod
apiVersion: v1
metadata:
  name: nodelabels
  labels:
    env: development
spec:
  containers:
    - name: c00
      image: ubuntu
      command:
        [
          "/bin/bash",
          "-c",
          "while true; do echo Hello-Bhupinder; sleep 5 ; done",
        ]
  nodeSelector:
    hardware: t2-medium
```

```sh
kubectl get nodes
# get your nodes and add label to one of them
kubectl label nodes nodename hardware=t2-medium
```

# Scaling and Replication

<li>Kubernetes was designed to orchestrate multiple containers and replication.
<li>Need for multiple containers/replication helps us with these:
<ul>

### <li>Replication:

<ul>
<li>if we add replicas=2 in config file then it will create two pods of same kind on one node.
<li>if one pod fails second pod will continue as a backup pod.
</ul>

### <li>Reliability:

<ul>
<li>By having multiple versions of an application, you prevent problems if one or more fails.
</ul>

### <li>Load Balancing:

<ul>
<li>Having multiple versions of a container enables you to easily send traffic to different instances to prevent overloading of a single instance or node.
</ul>

### <li> Scaling:

<ul>
<li>When load does become too much for the number of existing instances, kubernetes enables you to easily scale up your application, adding additional instances as needed.
</ul>

### <li>Rolling updates:

<ul>
<li>Updates to a service by replacing pods one by one.
</ul>
</ul><br>

# Replication Controller

<li>A replication controller is a object that enables you to easily create multiple pods, then make sure that number of pods always exist.
<li>If a pod created using RC will be automatically replicated if they does crash, failed, or terminated.
<li>RC is recommended if you just want to make sure 1 pod always running, even afte system reboots.
<li>You can run the RC with 1 replica & the RC will make sure the pod is always running.

```yml
apiVersion: v1
kind: ReplicationController # this defines to create the object of replication type
metadata:
  name: nginx
spec:
  replicas: 3 # the element defines the desired number of pods
  selector: # tells the controller which pods to watch/belong to this rc
    app: nginx # this must match the labels
  template: # template element defines a template to launch a new pod
    metadata:
      name: nginx
      labels: # selectors values need to match the labels values specified in the pod template
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
        - name: C00
          image: ubuntu
          command: ["bin/bash", "-c", "echo Hi"]
```

# Replica Set

<li>Replica Set is a next generation of replication controller.
<li>The replication controller only supports equality-based selector whereas the replica set supports set-based selector i.e filtering according to set of values.
<li>Replicaset rather than the replication controller is used by other objects like deployment.

```yml
apiVersion: apps/v1
kind: ReplicaSet # this defines to create the object of replication type
metadata:
  name: nginx
spec:
  replicas: 3 # the element defines the desired number of pods
  selector: # tells the controller which pods to watch/belong to this rc
    matchExpressions:
      - {key: myname,operator: In,values: [Tushar,Tush,Tusha]}
      - {key: env,operator: NotIn,values: [production]}
    app: nginx # this must match the labels
  template: # template element defines a template to launch a new pod
    metadata:
      name: nginx
      labels: # selectors values need to match the labels values specified in the pod template
        app: nginx
        myname: Tush
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
      - name: C00
        image: ubuntu
        command: ["bin/bash","-c","echo Hi"]
```

```sh
kubectl apply -f myrs.yml
kubectl get rs
kubectl scale --replicas=1 rs/myrs # for scaling down
```
<hr>