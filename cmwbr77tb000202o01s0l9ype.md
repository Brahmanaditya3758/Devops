---
title: "(Day 56) Task : Implementing Monitoring and Observability with Amazon CloudWatch"
datePublished: 2026-11-11T06:44:19.127Z
cuid: cmwbr77tb000202o01s0l9ype
slug: day-56-task-implementing-monitoring-and-observability-with-amazon-cloudwatch
cover: https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/79a348e6-d903-4aaa-9c02-384e5df36d4c.png
tags: lambda, aws, api, dynamodb, aws-lambda, apigateway, aws-cloudwatch

---

## **Introduction**

In the previous phases of my StudentHub project, I worked with various AWS services, including VPC, IAM, EC2, S3, CloudFront, ECR, ECS, Route 53, AWS Lambda, API Gateway, and DynamoDB.

After successfully building and integrating a serverless backend, I wanted to understand something important:

How do we monitor an application after deploying it to the cloud?

Building and deploying an application is only one part of the process. We also need to monitor its performance, identify errors, analyze logs, and understand how different AWS services behave.

For this phase, I'm exploring Amazon CloudWatch, AWS's monitoring and observability service.

## What is Amazon CloudWatch?

Amazon CloudWatch is an AWS service that helps monitor applications, infrastructure, and AWS resources.

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/05e85ed6-c607-427b-a5a7-b71fddb12145.png align="center")

It collects and provides access to:

- Metrics
- Logs
- Alarms
- Dashboards

CloudWatch helps developers and DevOps engineers understand application performance, troubleshoot failures, and monitor resource utilization.

## Why Do We Need CloudWatch in StudentHub?

Our StudentHub application now follows a serverless architecture.

The frontend communicates with API Gateway, which invokes Lambda. Lambda retrieves student information from DynamoDB.

Although the application works successfully, we need visibility into what happens behind the scenes.

Some questions we want to answer:

- Is our Lambda function executing successfully?
- How many requests is our API receiving?
- Are there any 4XX or 5XX errors?
- How long does Lambda take to execute?
- How much memory does Lambda consume?
- Is DynamoDB experiencing throttling?

CloudWatch helps us investigate these questions.

## Understanding StudentHub Architecture

Our current application architecture:

Frontend:

- Amazon S3 – Stores HTML, CSS, and JavaScript files.
- Amazon CloudFront – Delivers the frontend through a CDN.

Backend:

- Amazon API Gateway – Receives HTTP requests.
- AWS Lambda – Processes backend requests.
- Amazon DynamoDB – Stores student information.

Monitoring:

- Amazon CloudWatch – Monitors logs and metrics across our backend services.

Architecture flow:

User → CloudFront → S3

User clicks View Students:

Frontend → API Gateway → Lambda → DynamoDB

CloudWatch monitors the backend services.

## **Understanding CloudWatch Logs**

The first practical task was exploring Lambda execution logs.

I opened:

CloudWatch → Logs → Log Groups

Then located the Lambda log group:

`/aws/lambda/studenthub-get-students`

Inside the log group, I explored the latest execution log streams.

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/81f24ea0-cb9c-47c9-a5cd-8a53a85b987e.png align="center")

### Important Lambda log entries

**START**

Indicates that Lambda execution has started.

**END**

Indicates that Lambda execution has completed.

**REPORT**

Provides execution statistics, including duration and memory consumption.

**INIT_START**

Provides information about Lambda runtime initialization.

These entries help us understand how our function executes.

## **Monitoring API Gateway**

After exploring Lambda logs, I moved to API Gateway monitoring.

I navigated to:

CloudWatch → Metrics → Classic metrics → ApiGateway

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/218b6313-016d-429d-b14c-58742abb167a.png align="center")

I explored the available API Gateway metrics.

Important metrics include:

| Metric | Purpose |
| --- | --- |
| Count | Total number of API requests |
| 4XXError | Client-side errors |
| 5XXError | Server-side errors |
| Latency | Total API response time |
| IntegrationLatency | Backend integration response time |
| DataProcessed | Amount of data processed |

For StudentHub, these metrics help us understand API traffic and backend performance.

I also generated test traffic by accessing the StudentHub frontend and clicking the View Students button.

## **Monitoring DynamoDB**

Our next monitoring task is exploring DynamoDB metrics.

Our database table:

`StudentHubStudents`

It stores student records used by the StudentHub backend.

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/cb83296d-dd45-4968-813c-6749e81a7d8a.png align="center")

Some important DynamoDB metrics we want to understand include:

- ConsumedReadCapacityUnits
- ConsumedWriteCapacityUnits
- ReadThrottleEvents
- WriteThrottleEvents
- SuccessfulRequestLatency

These metrics help us understand database activity, capacity consumption, and throttling.

Since our table uses On-demand capacity mode, monitoring actual database activity is particularly useful.

![](https://cdn.hashnode.com/uploads/covers/67e296ceba50f8341dadb994/964e0c5a-1acf-4ddf-b537-008ae1ce21d1.png align="center")

## **Important CloudWatch Concepts**

During this phase, I'm learning the following concepts:

**Metrics:** Numerical measurements used to monitor application and resource performance.

**Logs:** Records containing execution details, application messages, and errors.

**Log Groups:** Collections of log streams.

**Log Streams:** Sequences of log events from a particular source.

**Alarms:** Monitor metrics against configured thresholds and help identify abnormal conditions.

**Dashboards:** Provide a centralized view of important monitoring metrics.

## CloudWatch vs CloudTrail

These two services are often confused.

| Amazon CloudWatch | AWS CloudTrail |
| --- | --- |
| Monitoring and observability | Auditing and governance |
| Collects logs and metrics | Records AWS account activity |
| Helps identify performance issues | Helps investigate account actions |
| Useful for application troubleshooting | Useful for security investigations |

## What I'm Learning from This Phase

This phase is helping me understand that deploying an application is not enough.

As a Cloud/DevOps engineer, I also need to understand:

- Application monitoring
- Centralized logging
- Performance analysis
- Error identification
- Database monitoring
- Troubleshooting
- Observability

These concepts are important for maintaining reliable cloud applications.

## Conclusion

With StudentHub Phase 8, I'm moving beyond building and deploying AWS infrastructure towards understanding how to monitor and maintain applications in the cloud.

Exploring Amazon CloudWatch is giving me practical experience with logs, metrics, performance monitoring, and troubleshooting.

**Building is important, but knowing how to monitor and troubleshoot what you've built is equally important.**

More updates coming as I continue developing StudentHub!

#AWS #AmazonCloudWatch #CloudComputing #DevOps #AWSLambda #APIGateway #DynamoDB #Monitoring #Observability #90DaysOfDevOps