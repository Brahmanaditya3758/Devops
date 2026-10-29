---
title: "(Day 51) Task : Containerizing and Deploying with Docker, Amazon ECR, and Amazon ECS."
datePublished: 2026-10-29T15:18:25.652Z
cuid: cmvtouahm000002ir5ige8aoq
slug: day-51-task-containerizing-and-deploying-with-docker-amazon-ecr-and-amazon-ecs
tags: docker, nodejs, expressjs-cilb5apda0066e053g7td7q24, ecs, ecr

---

This is Phase 5 of my StudentHub Cloud Platform project, where I'm learning AWS and DevOps by building things step by step.

As a fresher, my goal is not just to follow tutorials or copy commands. I want to understand what each tool does, why we use it, and how the different parts work together.

In this phase, I started working with Docker, Amazon ECR, and Amazon ECS. I successfully built and tested my API locally, uploaded its Docker image to Amazon ECR, and created an ECS task definition. The ECS service deployment is still in progress because I need to configure the networking correctly.

## What am I building?

StudentHub is a learning project that I'm developing in phases to understand how applications can be deployed on AWS.

For this phase, I created a simple backend API using Node.js and Express. It provides two endpoints:

- `/health` — checks whether the API is running.
- `/api/students` — returns sample student information.

The application listens on port `3000`.

## Step 1: Creating and testing the API

I started by creating a basic API using Express.

The health endpoint returns a response showing that the service is healthy. The students endpoint returns three sample student records.

I tested both endpoints locally using `curl` and confirmed that they returned the expected JSON responses.

This helped me understand that before deploying an application to the cloud, I should first make sure it works in my local environment.

## Step 2: Understanding Docker

After testing the API, I created a Dockerfile to package the application.

Docker helps package an application with the dependencies it needs so that it can run in a consistent environment.

I used a Node.js Alpine base image, installed the production dependencies, copied the application code, and configured the container to start the API.

I then built the Docker image successfully.

One important concept I learned is the difference between an image and a container:

- **Docker image:** The packaged template used to start an application.
- **Docker container:** A running instance created from that image.

I started a container on my Mac and tested the API again. Both endpoints worked as expected.

## Step 3: Uploading the image to Amazon ECR

After testing the Docker container, I moved to Amazon Elastic Container Registry (ECR).

ECR is a service that stores container images. It allows AWS services such as ECS to retrieve the image when they need to run an application.

I created a private ECR repository named `studenthub-api` in the Mumbai AWS region (`ap-south-1`).

Next, I authenticated Docker with ECR, tagged my local image with the repository address, and pushed it to AWS.

My image is available in the repository under the tag `1.0`.

This step helped me understand that building an image locally and storing it in a cloud registry are two different operations.

## Step 4: Creating an ECS task definition

Once the image was available in ECR, I opened Amazon Elastic Container Service (ECS).

ECS is a service for running and managing containerized applications.

I created a task definition named `studenthub-api-task`, revision `1`, using AWS Fargate.

A task definition acts like a blueprint. It tells ECS which container image to use and how the container should run.

I configured the container to use my ECR image and set its container port to `3000`, matching the port used by my Express application.

I also configured the task execution role. This role allows ECS to perform tasks such as retrieving the private image from ECR and sending container logs to CloudWatch.

The task definition was created successfully.

## Step 5: My first ECS deployment issue

After creating the task definition, I started configuring an ECS service to run the application.

However, the deployment failed with the following error:

`SubnetId cannot be empty`

At first, this was confusing because my Docker image and task definition had already been created successfully.

I learned that running a container on AWS requires more than just providing an image. The task also needs network configuration, including a VPC and subnet.

A subnet provides a network location where the Fargate task can run. Without a valid subnet selection, ECS cannot complete the deployment.

I have not completed the ECS deployment yet. My next step is to configure the correct subnet and security group, review networking costs, and retry the deployment.

I see this as part of the learning process. Understanding an error and knowing how to troubleshoot it is just as important as getting a successful result.

## What I learned

During this phase, I gained practical experience with:

- Building and testing a Node.js API.
- Creating a Docker image from a Dockerfile.
- Running a container locally.
- Understanding Docker image tags.
- Authenticating Docker with Amazon ECR.
- Pushing a container image to a private ECR repository.
- Creating an ECS task definition using AWS Fargate.
- Understanding the purpose of task execution roles.
- Learning why subnet and network configuration are necessary for an ECS deployment.

## What's next?

In the next part of this phase, I'll continue with ECS and Fargate.

My next goals are to:

1. Configure the VPC and subnet for the ECS service.
2. Set up the appropriate security group and public IP configuration for testing.
3. Start the Fargate task and inspect its logs.
4. Test the API endpoints after deployment.
5. Review the resources created and avoid unnecessary AWS costs.

I'm building this project to strengthen my practical cloud and DevOps knowledge, one step at a time.

**This phase is still in progress, and I'll update the article after I successfully deploy and test the API on AWS.**