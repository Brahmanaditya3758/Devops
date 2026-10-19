---
title: "(Day 47) Task : Building the Network Foundation for My StudentHub AWS Cloud Project 🚀"
datePublished: 2026-10-19T14:48:42.636Z
cuid: cmvfddk06000502qa4shma1e9
slug: day-47-task-building-the-network-foundation-for-my-studenthub-aws-cloud-project
cover: https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/06395f16-1865-4fd6-b2bc-74a448a8b27b.png
tags: aws, vpc, subnets, publicsubnet, nacl, security-groups, private-subnet, igw

---

As part of my ongoing AWS and DevOps learning journey, I worked on **Phase 1 of my StudentHub Cloud Platform project**, focusing on AWS networking.

Before deploying applications or launching EC2 instances, I wanted to understand how to build a proper network environment in AWS. This phase helped me learn how different networking components work together to provide connectivity, isolation, and security.

## What I Worked On :-

In this phase, I configured the foundational networking infrastructure using **Amazon Virtual Private Cloud (Amazon VPC)**.

Here are the main components I worked with:

*   **VPC:** Created `studenthub-vpc` to provide an isolated network for my project resources.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/850f8120-473f-49af-93fa-14050ca1d561.png align="center")

*   **Public Subnet:** Configured `public-1` for resources that may require internet connectivity.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/25c3334a-5cba-4108-8ba1-a02f48320fe3.png align="center")

*   **Private Subnet:** Created `private-1` for resources that should not be directly accessible from the internet.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/d615a71c-8221-4b1a-87b6-12fbd421dcca.png align="center")

*   **Internet Gateway:** Created and attached `studenthub-igw` to enable internet connectivity for resources in the public subnet when the necessary routing and public IP configuration are in place.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/764d4f3a-81e1-4098-a3d7-98d92faba04e.png align="center")

*   **Route Table:** Configured `public-rt` with a default route (`0.0.0.0/0`) pointing to the Internet Gateway.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/03985799-dbab-4c20-8253-cea5fcfe78d8.png align="center")

*   **Subnet Association:** Associated the public subnet with the public route table while keeping the private subnet separate.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/0bcbf7f4-92c5-411d-ba90-b2335e3b8c70.png align="center")

*   **Security Group:** Reviewed inbound and outbound rules to control traffic to and from supported resources.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/fe2c61ca-12e1-4e39-9787-2369a6f06691.png align="center")

*   **Network ACL (NACL):** Examined inbound and outbound rules to understand subnet-level traffic filtering.
    

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/4426bf0e-cd4c-4ed2-aeb0-b1ed2cc96235.png align="center")

## Understanding the Network Architecture :-

The main objective was to create a network foundation that I can use in the upcoming phases of StudentHub.

A VPC provides an isolated networking environment. Subnets divide that network into smaller IP address ranges, while route tables determine where network traffic should go.

The Internet Gateway provides a path to the internet, but a subnet is not automatically internet-accessible just because it is associated with an Internet Gateway. The appropriate route, public IP addressing, and security rules are also important.

I also learned the difference between Security Groups and Network ACLs:

*   **Security Groups** act as stateful virtual firewalls associated with supported resources, such as EC2 instances.
    
*   **Network ACLs** act as stateless filters at the subnet level, with separate inbound and outbound rules.
    

Understanding these differences is important when designing secure cloud infrastructure.

## What I Learned :-

Working on this phase helped me strengthen my understanding of:

*   Creating and organizing an AWS VPC.
    
*   Understanding the purpose of public and private subnets.
    
*   Configuring an Internet Gateway and route tables.
    
*   Associating subnets with the appropriate route tables.
    
*   Understanding inbound and outbound traffic rules.
    
*   Differentiating between Security Groups and Network ACLs.
    
*   Planning a network foundation for future cloud deployments.
    

Rather than focusing only on individual AWS services, I wanted to understand how these components work together in a real cloud environment.

## What's Next?

With the network foundation in place, I will continue building StudentHub by working through the next phases, including IAM configuration, AWS CLI access, and EC2 deployment.

My goal is to gradually turn this project into a practical cloud infrastructure project while improving my AWS, Linux, and DevOps skills.

I'm continuing to learn by building, documenting my work, and understanding the concepts behind every step.

**One phase at a time. One concept at a time.**

## Frequently Asked Questions (FAQs)

### 1\. What is Amazon VPC?

Amazon Virtual Private Cloud (VPC) allows you to create an isolated virtual network in AWS where you can launch and manage cloud resources.

### 2\. What is the difference between a public subnet and a private subnet?

A public subnet has a route to an Internet Gateway, allowing eligible resources to communicate with the internet when correctly configured. A private subnet does not have a direct route to an Internet Gateway for internet access.

### 3\. What is an Internet Gateway in AWS?

An Internet Gateway enables communication between a VPC and the internet when the required routing, IP addressing, and security configurations are in place. Like a security guard at any society.

### 4\. What is a route table?

A route table defines the path of network or traffic.

### 5\. What is the difference between a Security Group and a Network ACL?

A Security Group is a stateful firewall associated with supported resources, such as EC2 instances. A Network ACL is a stateless firewall that controls traffic at the subnet level.

### 6\. Why do we need both public and private subnets?

Public subnets can host resources that require direct internet connectivity, while private subnets help isolate resources that should not be directly reachable from the internet. This separation supports a more secure cloud architecture.

### 7\. What does `0.0.0.0/0` mean in a route table?

`0.0.0.0/0` represents all IPv4 destinations. When configured as a route through an Internet Gateway, it provides a default internet route, subject to the other required network configurations.

### 8\. Why did I create the VPC before launching EC2 instances?

The VPC establishes the networking environment in which cloud resources can operate. Creating it first helps organize IP addresses, subnet placement, routing, and network security before deploying instances.

### 9\. What did I learn from Phase 1 of StudentHub?

I learned how VPCs, public and private subnets, Internet Gateways, route tables, Security Groups, and Network ACLs work together to form the foundation of AWS networking.

### 10\. What is the next step in the StudentHub project?

The next phase focuses on IAM and AWS CLI configuration, building on the network foundation before progressing to EC2 and other AWS services.

**Final takeaway:** Understanding cloud networking is an important step toward building secure, reliable, and scalable infrastructure on AWS.

#AWS #CloudComputing #DevOps #90DaysOfDevOps #AmazonVPC #CloudEngineering #LearningInPublic #StudentHub