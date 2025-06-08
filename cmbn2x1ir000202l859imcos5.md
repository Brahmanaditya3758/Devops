---
title: "(Day 26) Task : Installing Minikube & Understanding Kubernetes Fundamentals :-"
datePublished: Sun Jun 08 2025 03:03:58 GMT+0000 (Coordinated Universal Time)
cuid: cmbn2x1ir000202l859imcos5
slug: day-26-task-installing-minikube-and-understanding-kubernetes-fundamentals
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749351783268/ecad21b2-67d5-490e-933a-6bc84198b0a5.jpeg
tags: aws, kubernetes, k8s, 2articles1week, manifest, k8scluster

---

Kubernetes (often abbreviated as K8s) is the de facto standard for orchestrating containerized applications. Whether you're just getting started with DevOps or preparing for certifications and real-world deployments, mastering Kubernetes and tools like Minikube is essential.

This guide walks you through:

1. Basics of Kubernetes Objects
    
2. Kubernetes Architecture and Relationships
    
3. Kubernetes Object State, Manifests, and Configuration
    
4. Installation and Setup of Minikube
    
5. YAML and Declarative Configurations
    

## 1\. What is Kubernetes?

Kubernetes is an open-source platform developed by Google for automating deployment, scaling, and management of containerized applications. It ensures your app is always running as desired, even as demand changes.

### What are Containerized Applications?

A containerized application runs in a container—an isolated environment with its own OS-level dependencies. Kubernetes manages these containers at scale, deploying and managing them across a cluster of machines.

## 2\. Kubernetes Objects: Core Building Blocks :-

Kubernetes objects represent the desired state of your system. Here are the most important ones:

### Pod :

* A pod is the smallest deployable unit.
    
* It can contain one or more containers.
    
* All containers in a pod share storage and network resources.
    

### Service :

* Abstracts and exposes a set of pods.
    
* Types: ClusterIP (internal), NodePort (external), LoadBalancer (cloud load balancers).
    

### Volume :

* Persistent storage solution for containers.
    
* Can be ephemeral (emptyDir) or persistent (PersistentVolume).
    

### Namespace :

* Logical partitions of a cluster.
    
* Isolate resources between teams or environments (dev, staging, prod).
    

## Relationship Between Objects :-

* Pod is deployed inside a Namespace.
    
* Service selects a group of Pods using labels.
    
* Volumes are mounted into Pods.
    

## State of Kubernetes Objects :-

Every Kubernetes object has 2 states:

* **Desired State:** What you want the system to look like (declared in YAML or via CLI).
    
* **Current State:** What is actually running.
    

Kubernetes continuously reconciles these two states.

## What is a Manifest?

A manifest is a YAML/JSON file that describes a Kubernetes object, including its metadata, specification, and desired state.

**Example YAML (pod):**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
```

## Kubernetes Object Management :-

### Imperative Management :

* Done using `kubectl` commands.
    
* Example:
    

```bash
kubectl run nginx --image=nginx
```

### Declarative Management :

* Define desired state in YAML files (manifest).
    
* Apply using:
    

```bash
kubectl apply -f pod.yaml
```

## Fundamentals of Kubernetes Configuration :-

### All-in-One (Single Node) :

* All components (API server, Scheduler, Controller, etcd, kubelet) run on one node.
    
* Ideal for learning and testing (like Minikube).
    

### Single-master, Multi-worker :

* 1 master node manages the cluster.
    
* Multiple worker nodes run application pods.
    
* etcd may run on the same or different machine as the master.
    

### High-Availability Setup :

* Multiple master and etcd nodes for fault tolerance.
    

## YAML Format Essentials :-

YAML is a human-readable configuration format.

Key rules:

* Use 2-space indentation.
    
* No tabs.
    
* Follow key-value structure.
    

**YAML Manifest Example (Deployment):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx
        ports:
        - containerPort: 80
```

## Installing Minikube (Local Kubernetes Lab) :-

Minikube lets you run Kubernetes locally. It creates a single-node cluster.

### Prerequisites:

* VirtualBox or Docker installed.
    
* `kubectl` CLI tool installed.
    

### Steps to Install:

#### On Linux:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

#### On Windows:

* Download executable from [Minikube releases](https://github.com/kubernetes/minikube/releases).
    
* Add it to system PATH.
    

### Start Minikube:

```bash
minikube start --driver=docker
```

### Verify Cluster:

```bash
kubectl get nodes
```

### Launch Dashboard:

```bash
minikube dashboard
```