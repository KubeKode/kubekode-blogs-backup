---
title: "Getting Started with Docker"
seoTitle: "Docker Tutorial"
seoDescription: "Docker is an advanced version of virtualization.

The main work of docker is containerization.

keywords of docker are: develop, ship, and run anywhere."
datePublished: Wed Nov 16 2022 20:19:42 GMT+0000 (Coordinated Universal Time)
cuid: clak37img000308mtfy9zghqk
slug: getting-started-with-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1668629216703/d0ntPPWu8.jpg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1668629943476/RcPkdIv0Q.jpg
tags: docker, aws, web-development, kubernetes, devops

---

## What is Hypervisor?
A hypervisor is computer software, firmware, or hardware that creates and runs virtual machines. A computer on which a hypervisor runs one or more virtual machines is called a host machine, and each virtual machine is called a guest machine.

## What is a virtual machine?
A Virtual Machine (VM) is a computing resource that uses software instead of a physical 	
computer to run programs and deploy apps. ... Each virtual machine runs its own operating 	
system and functions separately from the other VMs, even when they are all running on the same host.	

## What is docker?
Docker is an advanced version of virtualization.

The main work of docker is containerization.

keywords of docker are: develop, ship, and run anywhere.

We can run multiple containers on one host, Container does not have any operating system.

## Theory about docker

<li>Docker was first released in march 2013, developed by Solomon hykes and Sebastian pahl.
<li>Docker is an open-source centralized platform designed to create, deploy and run applications	
<li>Docker uses a container on the host OS, and runs the application. It allows applications to use the same Linux kernel	
<li>as a system on the host computer, rather than creating a whole virtual OS	
<li>We can install docker-engine on any OS, but the docker engine only runs natively  on Linux distributions	
<li>Docker is written in the Go language	
<li>Docker is a tool that performs OS-level virtualization also called containerization	
<li>Before docker, many users faced problems that a particular code is running in the developer's system but not in user's	
system	
<li>Docker is a set of platforms as a service that uses<b> OS level virtualization </b> whereas VMWARE uses hardware-level virtualization.
<li>Docker provides a type of platform for our application to be executed.</li>


### VMWare virtualization: Hardware Level virtualization
<table>
<tr><td>Virtual Machine</td>
<td>Virtual Machine</td>
<td>Virtual Machine</td>

</tr>
<tr><td>OS</td>
<td>OS</td>
<td>OS</td>

</tr>
<tr><td align="left"></td>
<td>Hypervisor(EXSI)</td>
<td></td>
</tr>
<tr><td></td>
<td>Hardware[HOST]</td>
<td></td>
</tr>
</table>

### Docker virtualization

<table>
<tr><td>Ubuntu Container</td>
<td>Arch Linux</td>
<td>Kali Linux Container</td>

</tr>
<tr><td></td>
<td>Docker Engine</td>
<td></td>

</tr>
<tr><td></td>
<td>OS- Linux, Ubuntu</td>
<td></td>
</tr>
<tr><td></td>
<td>Hardware[HOST]</td>
<td></td>
</tr>
</table>

## Advantages of Docker
<li>No pre-allocation of RAM.
<li>CI efficiency: <br>
Docker enables you to build a container image and use that same image across every steo of the deployment process.
<li>Less Cost
<li>It is light in weight.
<li>It can run on physical hardware/virtual hardware/ or on the cloud. 
<li>You can re-use the image.
<li>It took very less time to create container.</li>


## Disadvantages of Docker
<li>Docker is not a good solution for application that required rich GUI
<li>Difficult to manage large amount of containers
<li>Docker doesn't provide cross-platform compatability means if an application is designed to run in a docker container on windows, then it can't run on linux or vice-versa.
<li>Docker is suitable when the development OS and testing OS are same if the OS is different, we should use VM.
<li>No solution for data recovery and backup.</li>

## Architecture of Docker
<li>Developer creates a Dockerfile - Dependencies & required softwares.
<li>Execute Dockerfile using docker engine, An image will be created using your Dockerfile.
<li>Docker engine will create a container using that image.
<li>If we want to share this image, we can upload it to docker hub(Registry).(just like we store our code on github)
<li>Anyone want to use image can pull the image from docker hub then docker hub will create an image using the image downloaded from docker hub. And then he can use docker to create a container from that image.
<li><b>Layered file system: </b><br>Container does everything in the form of layers.</li>


## Components of Docker
### Docker Ecosystem
<li>Docker client</li>
<li>Docker Hub</li>
<li>Docker Daemon or Docker Server or Docker engine</li>
<li>Docker images</li>
<li>Docker compose</li>


### Docker Daemon
<li>Docker daemon runs on the Host OS.
<li>It is responsible for running containers to manage docker services.
<li>Docker daemon can communicate with other daemons.</li>


### Docker Client
<li>Docker users can interact with docker daemon through a client.</li>
<li>Docker client uses commands and Rest APIs to communicate with the docker daemon.</li>
<li>When a client runs any server command on the docker client terminal, the client terminal sends these commands to the docker daemon.</li>
<li>It is possible for docker client to communicate with more than one daemon.</li>


### Docker host(not a part of ecosystem)
<li>Docker host is used to provide an environment to execute and run applications. It contains the docker daemon, images, containers, networks and storages.</li>


### Docker hub/registry
<li>Docker registry manages and stores the docker images.</li>
<li>Two types of registry in docker</li>
<ol>
<li>Public Registry: Public registry is also called a docker hub.</li>
<li>Private registry: It is used to share images within the enterprise.</li>
</ol>

### Docker images
<li>Docker images are the read-only binary templates used to create docker containers.OR a single file with all dependencies and configurations required to run a program.</li>
<li>Ways to create an Image:</li>
<ol>
<li>Take image from docker hub</li>
<li>Create image from Dockerfile</li>
<li>Create image from existing docker containers</li></ol>

### Docker Container
<li>Container holds the entire packages that are needed to run the application.OR in other words<br>
we can say that the image is a template and the container is a copy of that template.
<li>Container is like a virtual machine.
<li>Images becomes container when they run on the docker engine.</li>


## Basic commands in docker
<li>To see all images present in your local machine</li>

```bash
$ docker images
```

<li>To find out images in docker hub</li>

```bash
$ docker search jenkins
```

<li>To download image from dockerhub to local machine</li>

```bash
$ docker pull jenkins
```

<li>If we want to create a container without running it immediately, we can use the Docker Create command below.

```bash
$ docker container create --name appserver python
```

<li>To give name to container</li>


```bash
$ docker run -it --name tush ubuntu /bin/bash
# -it means- interactive mode to terminal
# run command, creates container and starts it
```

<li>To check service is start or not</li>

```bash
$ Service docker status
# $ docker info
# start docker service
$ service docker start
# stop service
$ service docker stop
```

<li>To start container</li>

```bash
$ docker start tush
```

<li>To go inside container</li>


```bash
$ docker attach tush
```

<li>To see all containers</li>

```bash
$ docker ps -a
```

<li>To see only running containers</li>

```bash
$ docker ps
```

>ps - process status

<li>To stop container</li>

```bash
$ docker stop tush
```

<li>To delete container</li>

```bash
$ docker rm tush
```

# What is Dockerfile and create container from it, Components & diff command

## Create an image from an existing container

<li>Create one container first

```bash
$ docker run -it --name tushexample ubuntu /bin/bash
```

<li>Inside that container do some work, let's create a file inside tmp directory.

```bash
$ cd tmp/

$ touch myfile
```

<li>Exit from this container
<li>Now if we want to see the difference between the base image & changes on it then

```bash
$ docker diff tushexample
```

<li>Create image of this container

```bash
$ docker commit newcontainer updateimage
```

<li>Check our all images

```bash
$ docker images
```

<li>Create container from our updated image

```bash
$ docker run -it --name tushnewcon updateimage /bin/bash
```

<li>Check inside this container our file exists or not.

```bash
$ ls
$ cd tmp/
$ ls
```

<hr>