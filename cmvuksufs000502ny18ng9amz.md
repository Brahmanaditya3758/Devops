---
title: "(Day 52) Task : Deploying Containers with Amazon ECS and AWS Fargate"
datePublished: 2026-10-30T06:13:05.922Z
cuid: cmvuksufs000502ny18ng9amz
slug: day-52-task-deploying-containers-with-amazon-ecs-and-aws-fargate
cover: https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/08a10d05-748f-4005-87a2-11a3e0cdd799.png
tags: aws, ecs, fargate, eks, aws-cloud, phase5

---

## Introduction

In Part 1 of this phase, I built a REST API using Node.js and Express, containerized it using Docker, and pushed the Docker image to Amazon Elastic Container Registry (ECR).

With the container image ready, the next step was to run the application on AWS.

In this part, I worked with **Amazon Elastic Container Service (ECS)** and **AWS Fargate** to understand how containerized applications are deployed and managed in the cloud.

## 1\. What is Amazon ECS?

Amazon Elastic Container Service (ECS) is a fully managed container orchestration service provided by AWS.

It helps deploy, manage, and scale containerized applications without manually managing the orchestration infrastructure.

For this project, I used:

- **Amazon ECS:** To manage the container deployment.
- **AWS Fargate:** To run containers without managing EC2 instances.
- **Amazon ECR:** To store and provide the Docker image.
- **Amazon CloudWatch:** To collect application logs.
- **Amazon VPC:** To provide the networking environment.

## 2\. Creating the ECS Cluster

I created an ECS cluster named:

`studenthub-cluster`

An ECS cluster is a logical grouping of resources where ECS tasks and services run.

I used AWS Fargate as the compute option, meaning I did not need to provision or manage EC2 instances for the container.

## 3\. Creating the Task Definition

Next, I created a task definition family named:

`studenthub-api-task`

An ECS task definition is a blueprint that tells ECS how to run a container.

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/58b93eef-ac95-45bf-889a-c7fc4329338b.png align="center")

I configured the following:

| Configuration | Value |
| --- | --- |
| Launch type | AWS Fargate |
| Operating system | Linux |
| CPU architecture | X86_64 |
| CPU | 0.25 vCPU |
| Memory | 0.5 GB |
| Container port | 3000 |
| Execution role | ecsTaskExecutionRole |
| Log group | /ecs/studenthub-api-task |

I configured the container to use the Docker image stored in Amazon ECR.

The task definition also included the execution role, which allows ECS to perform required actions such as pulling the image and sending logs to CloudWatch.

## 4\. Creating the ECS Service

After creating the task definition, I created an ECS service named:

`studenthub-api-task-service-v7ru58lm`

An ECS service helps maintain the desired number of running tasks and manages deployments.

I configured the service to use the Fargate launch type and the task definition created earlier.

The application was configured to use port `3000`.

## 5\. Verifying the Deployment

During the first deployment, the ECS task failed to start because of a container image platform mismatch.

After troubleshooting and creating a new task definition revision, the deployment succeeded.

The ECS task eventually reached the:

**RUNNING state**

I also checked the CloudWatch log stream and found the application startup message:

```text
StudentHub API running on port 3000
```

This confirmed that the Node.js application had started inside the ECS container.

## 6\. Troubleshooting: Container Image Platform Mismatch

One of the main issues I encountered was:

```text
CannotPullContainerError:
image Manifest does not contain descriptor matching platform 'linux/amd64'
```

### Why did this happen?

I was developing on an Apple Silicon Mac mini, while the ECS task was configured to run on Linux/X86_64.

During troubleshooting, I compared the image digest referenced by the ECS task definition with the image digest stored in ECR.

I discovered that the first task definition revision referenced an older image digest.

### How did I resolve it?

I built the Docker image specifically for the AMD64 platform:

```bash
docker buildx build \
  --platform linux/amd64 \
  --provenance=false \
  --load \
  -t studenthub-api:1.0 .
```

I then pushed the image to ECR and created a new task definition revision using the current ECR image tag.

The subsequent deployment successfully started the container.

**Key learning:** Container image architecture and the image referenced by the task definition must be compatible with the target runtime.

## 7\. Networking Issue: Accessing the API Externally

Although the ECS task reached the RUNNING state, I could not access the API through its public IP address.

I attempted to access:

```text
http://<public-ip>:3000/health
```

However, the browser returned:

```text
ERR_ADDRESS_UNREACHABLE
```

I checked the security group and confirmed that an inbound TCP rule for port `3000` had been added for my public IP address.

I also verified that the public subnet route table contained a route to the Internet Gateway.

However, external connectivity was still unsuccessful.

This meant that the application process was running inside the container, but successful public access to the API was not established.

Further investigation of the complete networking path, including network ACLs and subnet configuration, would be required.

## 8\. AWS Resource Cleanup

After completing the deployment experiments, I decided to remove the Phase 5 ECS resources to avoid unnecessary ongoing AWS costs.

The cleanup included:

- Scaling the ECS service down to zero tasks.
- Deleting the ECS service and cluster.
- Deleting the ECR repository and its image.
- Deleting the ECS CloudWatch log group.
- Deregistering the task definition revisions.

I also checked for NAT gateways and load balancers in the Mumbai region (`ap-south-1`). No resources were returned.

Finally, I verified that the ECS service list was empty.

The existing StudentHub VPC and Phase 4 S3/CloudFront resources were retained.

## Conclusion

This part helped me understand how a Dockerized application moves from Amazon ECR to Amazon ECS using AWS Fargate.

I gained practical experience creating ECS clusters, configuring task definitions, deploying services, checking container logs, and troubleshooting image architecture compatibility.

Although the container successfully reached the RUNNING state, external API connectivity remained unresolved.

This was an important learning experience because deploying a container successfully is only one part of the process. Networking, security groups, routing, and access configuration are equally important when making an application available to users.

Most importantly, I learned how to investigate deployment failures rather than simply recreating resources, and how to clean up AWS resources after experimentation.

## Frequently Asked Questions (FAQs)

### 1\. What is the difference between Amazon ECS and AWS Fargate?

Amazon ECS is the container orchestration service that manages tasks and services. AWS Fargate is the serverless compute engine that runs containers without requiring users to manage EC2 instances.

### 2\. What is an ECS task definition?

A task definition is a configuration blueprint that describes how an ECS task should run, including its container image, CPU, memory, ports, environment configuration, and IAM roles.

### 3\. What is the difference between an ECS task and an ECS service?

An ECS task is a running instance of a task definition. An ECS service manages the desired number of tasks and helps maintain application availability.

### 4\. Why did I use Fargate instead of EC2?

I used Fargate to focus on container deployment without managing EC2 instances, operating systems, or the underlying container infrastructure.

### 5\. Why did the ECS deployment initially fail?

The initial deployment encountered a container image platform mismatch. The task definition referenced an older image digest that did not match the expected Linux AMD64 platform.

### 6\. How did I verify that the application started?

I checked the ECS task status and CloudWatch logs. The task reached RUNNING, and the logs displayed the message `StudentHub API running on port 3000`.

### 7\. Does an ECS task in RUNNING state mean the API is publicly accessible?

No. RUNNING confirms that ECS considers the task running. It does not guarantee that external clients can reach the application. Networking, routing, security groups, and network ACLs must also be configured correctly.

### 8\. Why couldn't I access the API through the public IP?

External access attempts failed even after adding an inbound security group rule and verifying the public subnet's Internet Gateway route. The exact cause was not confirmed and would require further network troubleshooting.

### 9\. What happens when an ECS task definition is updated?

A new revision can be created. An ECS service can then use the updated revision for a new deployment.

### 10\. Why did I delete the ECS and ECR resources?

I removed the experimental resources after testing to avoid unnecessary ongoing costs. The existing VPC and Phase 4 frontend infrastructure were retained.