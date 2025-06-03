---
title: "(Day 24) Task : Kubernetes Architecture Explained :-"
datePublished: Tue Jun 03 2025 05:27:24 GMT+0000 (Coordinated Universal Time)
cuid: cmbg2u8ax000009jn80sc45bw
slug: day-24-task-kubernetes-architecture-explained
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1748928301641/b39876ac-55f2-4a25-a99d-2da5aee56476.png
tags: 90daysofdevops-kubernetes-devops-containerorchestration-cloudnative-techjourney-learninginpublic

---

Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. Its architecture is designed to be highly distributed, scalable, and resilient, enabling it to manage workloads across clusters of machines. This article provides a detailed overview of Kubernetes' architecture, focusing on its core components, the control plane (master), worker nodes, and their interactions.

## Overview of Kubernetes Architecture :-

Kubernetes follows a master-worker architecture, where the control plane (master) manages the cluster's state and orchestrates workloads, while worker nodes execute the containerized applications. The control plane maintains the desired state of the cluster, and worker nodes run the actual workloads. Communication between components is facilitated through the Kubernetes API, ensuring modularity and extensibility.

### Key Components :

* **Control Plane (Master)**: Manages the cluster's state and orchestrates workloads.
    
* **Worker Nodes**: Run containerized applications and report status to the control plane.
    
* **etcd**: A distributed key-value store for persistent cluster state.
    
* **Container Runtime**: Executes containers on worker nodes.
    
* **Networking**: Facilitates communication within and outside the cluster.
    

Below, we dive into each component, their roles, and how they interact within the Kubernetes ecosystem.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748927468923/36baabaa-972a-453e-aa48-eaa532111ecf.jpeg align="center")

## Control Plane (Master) Components :-

The control plane is responsible for maintaining the desired state of the cluster, making decisions about scheduling, scaling, and healing. It consists of several components, typically running on one or more master nodes for high availability.

### 1\. **API Server (kube-apiserver) :**

The kube-apiserver is the central hub of the Kubernetes control plane. It exposes the Kubernetes API, which is the primary interface for interacting with the cluster. All communication, whether from users, CLI tools (like kubectl), or other components, flows through the API server.

* **Functionality**:
    
    * Validates and processes API requests (e.g., creating, updating, or deleting resources like Pods, Services, or Deployments).
        
    * Acts as a gateway between the control plane and worker nodes.
        
    * Authenticates and authorizes requests using mechanisms like RBAC (Role-Based Access Control).
        
    * Stores API objects in etcd after validation.
        
* **Key Features**:
    
    * RESTful API for cluster management.
        
    * Watches for changes in resource states and triggers updates.
        
    * Supports extensibility via custom resources (CRDs).
        
* **Example Interaction**: When you run kubectl apply -f deployment.yaml, the API server processes the YAML, validates it, and stores the Deployment object in etcd, triggering subsequent actions by other components.
    

### 2\. **etcd**

**etcd** is a distributed, consistent key-value store that serves as the cluster's single source of truth for all configuration data and state.

* **Functionality:**
    
    * Stores all Kubernetes objects (e.g., Pods, Services, ConfigMaps) in a hierarchical key-value structure.
        
    * Provides reliable storage with strong consistency guarantees.
        
    * Supports watch operations, allowing components to monitor state changes in real time.
        
* **High Availability:**
    
    * Typically deployed as a cluster (e.g., 3 or 5 nodes) to ensure fault tolerance.
        
    * Uses the Raft consensus algorithm to maintain consistency across nodes.
        
* **Example**: When a Pod is created, the API server writes its specification to etcd. The scheduler and other components watch etcd for updates to make decisions.
    

### 3\. **Controller Manager (kube-controller-manager)**

The kube-controller-manager runs a set of controllers that monitor the cluster's state and take actions to reconcile the actual state with the desired state.

* **Functionality:**
    
    * Runs multiple control loops, each responsible for a specific resource type (e.g., ReplicaSet, Deployment, Node).
        
    * Watches etcd for changes and acts to maintain the desired state.
        
    * Examples of controllers:
        
        * **Node Controller**: Monitors node health and manages node lifecycle.
            
        * **Replication Controller**: Ensures the correct number of Pods for a ReplicaSet.
            
        * **Deployment Controller**: Manages rolling updates and rollbacks for Deployments.
            
* **Key Features:**
    
    * Runs as a single process but manages multiple independent controllers.
        
    * Handles tasks like scaling, self-healing, and resource cleanup.
        
* **Example**: If a Pod in a ReplicaSet crashes, the Replication Controller detects the discrepancy via etcd and creates a new Pod to meet the desired replica count.
    

### 4\. **Scheduler (kube-scheduler) :-**

The kube-scheduler is responsible for assigning Pods to worker nodes based on resource requirements, constraints, and policies.

* **Functionality:**
    
    * Watches for unscheduled Pods in etcd.
        
    * Evaluates nodes based on criteria like resource availability (CPU, memory), affinity/anti-affinity rules, taints, and tolerations.
        
    * Assigns a Pod to a suitable node by updating the Pod's specification in etcd.
        
* **Key Features**:
    
    * Supports custom scheduling policies via scheduler extensions.
        
    * Optimizes for resource utilization, load balancing, and application requirements.
        
* **Example**: A new Pod is created with a resource request of 2 CPU cores. The scheduler evaluates nodes, selects one with sufficient resources, and binds the Pod to that node by updating etcd.
    

## Worker Node Components :-

Worker nodes are the machines (physical or virtual) that run containerized applications. Each worker node hosts several components to manage and execute workloads.

### 1\. **Kubelet :**

The kubelet is an agent running on each worker node, responsible for managing Pods and their containers.

* **Functionality**:
    
    * Communicates with the API server to receive Pod specifications.
        
    * Ensures containers within Pods are running and healthy.
        
    * Interacts with the container runtime to start, stop, or restart containers.
        
    * Reports node and Pod status to the API server.
        
* **Key Features**:
    
    * Manages both Pods created by the control plane and static Pods defined locally.
        
    * Performs health checks (liveness and readiness probes) to ensure container health.
        
* **Example**: When the scheduler assigns a Pod to a node, the kubelet on that node pulls the container images, starts the containers, and monitors their health.
    

### 2\. **Kube-Proxy :**

The kube-proxy runs on each worker node and manages network connectivity for Pods.

* **Functionality**:
    
    * Implements Kubernetes Services by maintaining network rules.
        
    * Supports load balancing across Pods for a Service.
        
    * Operates in modes like iptables, IPVS, or userspace (iptables is the most common).
        
* **Key Features**:
    
    * Enables communication between Pods, Services, and external clients.
        
    * Handles Service discovery and DNS resolution (via CoreDNS).
        
* **Example**: When a Service is created with multiple Pods, kube-proxy sets up iptables rules to distribute incoming traffic across the Pods.
    

### 3\. **Container Runtime :**

The container runtime is the software responsible for running containers on worker nodes.

* **Functionality**:
    
    * Pulls container images from registries (e.g., Docker Hub, quay.io).
        
    * Creates, starts, stops, and deletes containers as instructed by the kubelet.
        
    * Supported runtimes include containerd, CRI-O, and Docker (via containerd shim).
        
* **Key Features**:
    
    * Conforms to the Container Runtime Interface (CRI) for compatibility with Kubernetes.
        
    * Manages container lifecycle and resource isolation.
        
* **Example**: The kubelet instructs containerd to pull a nginx:latest image and start a container, which containerd executes on the node.
    

## Networking in Kubernetes

Kubernetes networking is critical for communication between Pods, Services, and external systems. Key aspects include:

* **Pod-to-Pod Communication**:
    
    * Each Pod gets a unique IP address within the cluster.
        
    * A Container Network Interface (CNI) plugin (e.g., Calico, Flannel, Weave) manages Pod networking.
        
* **Service Networking**:
    
    * Services provide stable virtual IPs (ClusterIPs) for accessing Pods.
        
    * Kube-proxy manages Service traffic routing.
        
* **External Access**:
    
    * Ingress controllers or LoadBalancer Services expose applications to external clients.
        
    * NodePort Services expose Pods on specific node ports.
        

## How Kubernetes Components Work Together

The interaction between control plane and worker node components follows a declarative model, where the desired state is defined, and Kubernetes ensures the actual state matches it. Here’s a typical workflow:

1. **User Interaction**:
    
    * A user submits a Deployment manifest via kubectl apply.
        
    * The API server validates the manifest and stores it in etcd.
        
2. **Controller Action**:
    
    * The Deployment Controller detects the new Deployment in etcd.
        
    * It creates a ReplicaSet, which in turn creates the specified number of Pods.
        
3. **Scheduling**:
    
    * The scheduler watches for unscheduled Pods in etcd.
        
    * It evaluates nodes and assigns Pods to suitable ones, updating etcd.
        
4. **Pod Execution**:
    
    * The kubelet on the assigned node detects the Pod assignment via the API server.
        
    * It instructs the container runtime to pull images and start containers.
        
5. **Networking**:
    
    * Kube-proxy sets up network rules to route traffic to the Pods.
        
    * Services and Ingress ensure proper load balancing and external access.
        
6. **Monitoring and Healing**:
    
    * The kubelet monitors Pod health and reports to the API server.
        
    * The Controller Manager detects failures and recreates Pods as needed.
        
    * etcd maintains the cluster's state for consistency.