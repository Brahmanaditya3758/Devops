---
title: "(Day 39) Task : Understanding Basics Of Helm & Helm Charts:"
datePublished: Mon Jul 14 2025 02:45:28 GMT+0000 (Coordinated Universal Time)
cuid: cmd2i3x6r000n02johwbnht9u
slug: day-39-task-understanding-basics-of-helm-and-helm-charts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752461084052/4cce29c0-b380-4ac9-a32a-67d870046f7d.png
tags: kubernetes, helm, orchestration, 2articles1week, k8scluster

---

## What Do You Want to Talk About?

Day 39 of my #90DaysOfDevOps journey: Today, I explored Helm, often called the package manager for Kubernetes. Helm simplifies the deployment and management of complex Kubernetes applications through a concept called Helm Charts. Inspired by a powerful explanation in a video by Technical Guftgu, I’ve created this blog to summarize and explain the key points with examples and practical use cases for beginners and intermediate learners alike.

### What is Helm?

Helm is a package manager for Kubernetes, similar to how apt works for Ubuntu or yum for RHEL-based systems. Helm lets you define, install, and upgrade even the most complex Kubernetes applications using a structured format called Charts.

Think of Helm as a way to package your Kubernetes YAMLs in a reusable and shareable format.

### Why Do We Need Helm?

Manually managing multiple YAML files for applications like WordPress or MongoDB can become messy, especially when you need to:

Set different values for dev, staging, and production environments

Re-deploy apps with new config values

Roll back to a previous release

Track app versioning

Helm handles all this for you!

### What is a Helm Chart?

A Helm Chart is a collection of files that describe a related set of Kubernetes resources.

Structure of a Helm Chart:

```pgsql
mychart/
```

Chart.yaml – Metadata about the Helm chart (name, version, dependencies)

values.yaml – Default configuration settings

templates/ – Folder containing Go-template-based Kubernetes YAML files

### Real-World Analogy :

Think of Helm as Docker Compose for Kubernetes.

You define your app’s entire stack in a chart, with configuration values abstracted into values.yaml, making it easy to reuse and deploy across environments.

### Basic Helm Commands :-

Here are a few must-know commands:

```bash
# Install Helm (on Linux)
```