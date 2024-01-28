---
title: "Day 32: Deploying a application on private subnet"
datePublished: Sun Jan 28 2024 16:17:00 GMT+0000 (Coordinated Universal Time)
cuid: clrxpeid7000109lb1g8ng89x
slug: day-32-deploying-a-application-on-private-subnet
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706435371405/c86aaff1-56fe-4605-b6de-87e08b11549d.jpeg
tags: aws, devops, vpc

---

### 🚀Introduction

In this blog, we will run an HTML file within a private subnet as depicted in the provided screenshots. Follow the instructions exactly as shown in the images.

---

The VPC public and private subnet architecture is shown below, like this we are going to create in this blog.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706439466551/196a05ef-60a9-4424-bf31-2ff62b89fcf5.jpeg align="center")

---

### **🔸**Step1: Create VPC

Select **VPC and more** option it will create & attach everything for you such as subnet, route table, internet gateway, and NAT gateway.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706435709153/c2a71299-6bb3-4bc1-a827-1305e9a70c58.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706435893311/058ed515-2604-407e-ac3a-d7b001c2c22e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706435905646/17edce10-84c5-4635-a998-7564733f7697.png align="center")

Now, you have created a VPC 👆

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436013562/33da7576-c94b-499d-b449-ffc9c776e9a3.png align="center")

This is a resource architecture of your VPC.👆

---

### **🔸**Step2: Launch template & Create autoscaling group

**Launch template**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436096629/31fc941c-dc3b-4881-93b9-b2e14e32c742.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436181983/b668ff5a-0c40-466f-ae97-42135a2ed7a3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436199497/786f5238-9c59-4a6f-aa5d-322972123615.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436245060/d80d94d5-aab2-46e4-a90d-fc1a0227951a.png align="center")

It is same as creating like ec2 instance.👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436283419/a0188457-d118-47c7-9318-777913db9224.png align="center")

**Create auto scaling group👇**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436298916/b6df293e-696a-49d1-941f-1f405dad2381.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436408241/96a36a5d-69b8-443d-9b79-c82bd5297f1e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436434686/666dad7e-bb98-4836-9760-6b46a99a7709.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436445633/3761e432-a5fa-484d-8767-0b67756da567.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436506785/a4b7fcb0-081d-45fd-979b-8af6c24538b8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436523134/b4ab9480-a51b-402a-a4df-459e25fba161.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436534171/8d7ab9e2-af19-4720-8203-ab8cf44846cf.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436551782/0c7a89ab-46aa-4e57-add4-56675279e6ca.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436611513/ccaa2a28-2d1d-42d4-ba87-b52fb861bed4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436623621/86329575-afab-4a9f-918a-42ec2d3ee2bb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436658609/1d0e8131-e939-4339-9640-9406e756fda0.png align="center")

Now you have created auto sacling group👆

---

### **🔸**Step3: Now launch instance

Now, go ahead and check if two private instances have been created. Next, create a public instance to do bastion host.👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436871600/3871e615-ee21-4f76-9987-53caff8b7e0a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436877969/e1fb4c68-cc97-4569-90e4-7d2f755c65ce.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436889651/608e6a79-332c-4e91-be9b-bc3b1b05d61f.png align="center")

**Copy your pem file from your local terminal to public subnet**

use this command to copy

`scp -i /your/pem/file/path /your/pem/file/path/ ubuntu@<your public instance IP>:/home/ubuntu`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706436904323/0503bf60-662f-4aa6-ac28-31a63c6f6997.png align="center")

After copying pem file👆 now do ssh your public instance in your local terminal👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437157097/a929010d-1bc1-45f4-9e59-d1cbfb17b969.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437178255/2a98b0d9-6bd8-4e5b-9918-a49470634f6f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437247843/914f3dd3-cbf7-455f-85d1-0d1f6f66224a.png align="center")

Now do ssh in private instance 👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437461212/7bbf6ba9-e3ba-4fb4-94d5-2c3e475014a8.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437473677/92784724-8cf1-4301-97a9-297984b7a7af.png align="center")

After doing ssh of private instance, create file or download any python project. This is only for example purposes.👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437487755/78f9022e-9f0f-4313-991b-82689706da0b.png align="center")

---

### **🔸**Step4: Now create target group & Load balancer

**Create target group👇**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437732774/3174649f-cbef-479d-9380-15706e068752.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437741639/c00cfc3c-f69a-48c6-8534-9bd13d4ef464.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437753459/2d5439e7-062a-4310-a66a-7911b92c5c45.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437767683/b80e1576-bac6-41cc-bc9e-52f4bf4acb0e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437805595/8052d3a2-b7a0-40cf-b37c-9e30571c4df6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437835600/18b16c41-453b-46ed-96f9-ef11b7627ca3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437846229/58f4fdb9-aa7d-43a8-be23-0ac1c2b9be60.png align="center")

**Create Load balancer👇**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437857732/fb435cc8-4bfa-4730-bbfc-2cd84ea99448.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437941670/9bb6fcfa-dcd3-4a96-8d39-c8f05a91f512.png align="center")

Select application load balancer👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706437955180/ad016fe3-c60c-4306-af7e-a3aa0d2ec812.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438002245/f528bc6a-b498-4de1-bb3b-3a2c87eee14d.png align="center")

Always select public subnet here, otherwise it will show error👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438026329/dea1e96c-8413-4f2a-bb68-b40fa0003c24.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438088665/9d6282f1-76f0-4169-a0c5-a0607c226f2f.png align="center")

keep http 80 as default👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438100740/0fa4601f-b68c-4ba3-8885-7f090fd8addb.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438121959/8f19189a-5000-462c-acc8-902fdcc8a430.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438217724/7fca1586-8938-474a-a51c-fd12b78d1938.png align="center")

It takes some time for the load balancer to become active, so please wait for 2-3 minutes.👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438227652/7c448300-ca45-468e-950b-e80daa79531e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438318659/3b0f0d6a-7b81-455a-8c17-41dca06214cd.png align="center")

Go in listeners and rules, scroll down and you will get error like this, to solve this error edit security group👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438329286/992c36c8-318f-433e-b268-25545788f446.png align="center")

You will see there is an option of security beside the listeners & rules.👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438438758/638ea6bc-e185-4477-a92b-37ff0d1bb185.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438548793/4849f2fa-eccf-4fa6-aa5f-a12fed199e9f.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438557729/5227e8c7-c374-4899-b227-46fe119f2035.png align="center")

Add rule

Type &gt; HTTP

Source &gt; Anywhere IPv4👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438573513/d478c1fc-99a2-4a07-851b-defb01fe5517.png align="center")

Now you can see that the error which was there before is no longer present👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706438959544/e2f9a66f-1c7b-4905-ab74-de5dab35be0f.png align="center")

Go into the private instance where you had SSH and type this command👇

`python3 -m http.server 8000`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706439087872/2b4a0d4a-4822-4cea-8f77-3d00b217e042.png align="center")

Now go in listeners and rule👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706439109678/896502e4-8303-419e-b5cb-074cb8231923.png align="center")

Copy DNS name and paste it on chrome👇

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706439124683/1175a86b-1e6f-4c27-b2ac-fdb6a7a4543c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706439153756/7d6e9ea2-7eb0-43f1-a69f-ef6ed5a22738.png align="center")

---

***Thanks for reading to the end; I hope you gained some knowledge.❤️🙌***

[Linkedln](https://www.linkedin.com/in/vishesh-ghule/)

[Twitter](https://twitter.com/VisheshGhule)

[Github](https://github.com/VisheshGhule)