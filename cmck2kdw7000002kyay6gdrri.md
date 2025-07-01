---
title: "(Day 34) Task :  Kubernetes Services – Part 2: NodePort, LoadBalancer & Headless Services & Volume :-"
datePublished: Tue Jul 01 2025 05:10:32 GMT+0000 (Coordinated Universal Time)
cuid: cmck2kdw7000002kyay6gdrri
slug: day-34-task-kubernetes-services-part-2-nodeport-loadbalancer-and-headless-services-and-volume
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751346252357/398e3d7b-66fc-4c12-9c76-fd88fc6c03d9.png
tags: load-balancer, 2articles1week, volume-mount, kubernetes-nodeport, devops-continuousintegration-continuousdelivery-automation-agile-containerization-kubernetes-docker-infrastructureascode-cicd-devsecops-kubernetes-k8s-versioncontrol-releasemanagement-microservices-monitoring-deployment-scalability-orchestration-configurationmanagement-devopsculture-cloudnative-sitereliabilityengineering-gitops-serverless-automationfirst, serialblogger

---

**Day 34 of 90 Days of DevOps**

In the first part of my Kubernetes Services series, we explored ClusterIP and the role of [](https://your-link-to-part1.com)Kubernetes Services in enabling stable communication across pods. Today, we’re going deeper into three more important types of Kubernetes Services:

* NodePort
    
* LoadBalancer
    
* Headless Services
    

These services extend pod communication beyond the cluster boundary, enable load balancing, and offer DNS-level discovery. Let’s get into it

## 1\. NodePort Service

### What is NodePort?

NodePort exposes a pod or set of pods to external traffic by opening a static port on each Node’s IP. This is the simplest way to expose your app outside the cluster without needing any external load balancer.

### YAML Configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - port: 80         # Service Port
      targetPort: 8080 # Container Port
      nodePort: 30036  # External Port on Node
```

`nodePort` must be between **30000-32767**

### How It Works:

```bash
Request → NodeIP:30036 → Service → Pod (targetPort: 8080)
```

### Use Case:

* Quick testing or exposing apps without a cloud load balancer.
    
* Useful in dev or PoC environments.
    

## 2\. LoadBalancer Service

### What is LoadBalancer?

In cloud environments like AWS, GCP, or Azure, `LoadBalancer` automatically provisions an external load balancer and assigns it a public IP.

### YAML Configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 8080
```

### How It Works:

The cloud provider:

* Creates a load balancer.
    
* Maps it to the NodePorts created automatically.
    
* Exposes your service to the internet with a public IP.
    

```bash
Client → LoadBalancer IP → NodePort (Internal) → Pod
```

### Use Case:

* Production-grade applications.
    
* Exposing apps publicly in a cloud-native way.
    
* Automatically handles health checks, scaling, and traffic routing.
    

## 3\. Headless Services

### What is a Headless Service?

A Headless Service (set by `clusterIP: None`) does not assign a Cluster IP. Instead, it returns the DNS records of individual pods behind the service.

This enables direct client-to-pod communication and is heavily used in:

* StatefulSets (like Cassandra, Kafka, etc.)
    
* Service discovery patterns.
    

### YAML Configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-headless-service
spec:
  clusterIP: None
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 8080
```

### How It Works:

With a headless service, a DNS query returns **multiple A-records**, one for each pod IP:

```bash
nslookup my-headless-service.default.svc.cluster.local
```

Each record points to a specific pod, allowing load balancing at the client level.

### Use Case:

* Distributed systems and stateful applications.
    
* DNS-based service discovery.
    
* When you need direct access to pod IPs (no proxy).
    

## Why Do We Need Volumes in Kubernetes?

Let’s begin with the core problem:

Containers are stateless and non-persistent.

If a container crashes, all data stored within it — such as logs, temp files, or database states — is wiped out. Even though Kubernetes can recreate a failed container inside a pod automatically, the new container starts with a fresh filesystem.

However, Kubernetes provides Volumes as a solution to this issue. When a volume is mounted to a container:

* It becomes a persistent storage directory.
    
* The data remains intact even if the container crashes or restarts.
    
* It’s shared across containers in the same pod.
    

## What is a Volume in Kubernetes?

A volume in Kubernetes is essentially a directory that is mounted into a pod’s container(s). This directory is backed by a storage medium — such as memory, disk, cloud disk, or a shared filesystem.

Key properties:

* A volume is attached to a pod, not a container.
    
* All containers in the pod can access and share the volume.
    
* The lifecycle of the volume is tied to the pod, not individual containers.
    
* Volumes ensure data persistence across container restarts, but not across pod restarts.
    
* If a container crashes: volume stays
    
* If a pod crashes and is deleted: the volume is destroyed along with it
    

## Where Are Volumes Used?

Volumes are commonly used for:

* Sharing data between containers in the same pod
    
* Storing logs or temporary files that need to survive container restarts
    
* Hosting configuration or secret files
    
* Integrating with cloud storage or distributed file systems
    

## Kubernetes Volume Types

Kubernetes offers a wide variety of volume types. Each type defines how the volume is created, backed, accessed, and destroyed.

### Categories of Volume Types:

1. **Node-local volumes**
    
    * `emptyDir`, `hostPath`
        
2. **File-sharing volumes**
    
    * `nfs`, `cephfs`
        
3. **Cloud-provider volumes**
    
    * `awsElasticBlockStore`, `azureDisk`, `gcePersistentDisk`
        
4. **Distributed filesystems**
    
    * `glusterfs`, `csi`
        
5. **Special-purpose volumes**
    
    * `secret`, `configMap`, `gitRepo`
        

Each type has different performance, access, and lifecycle properties.

## Deep Dive: `emptyDir` Volume

Let’s start with one of the simplest volume types: `emptyDir`

### What is `emptyDir`?

An `emptyDir` volume is first created when a pod is scheduled to a node. It exists as long as that pod runs on that node.

* Initially, the directory is empty.
    
* All containers in the pod can read/write to it.
    
* It can be mounted at different paths in different containers.
    
* Once the pod is deleted, the volume and its contents are deleted permanently.
    

### Key Characteristics:

| Property | Value |
| --- | --- |
| Scope | Pod |
| Sharing | Between containers in same pod |
| Lifecycle | Exists while pod is running |
| Initial content | Empty |
| Persistence | Across containers , Across pods |

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751346103147/512c0066-01a9-4126-9058-81cadba500e4.png align="center")

### Example YAML: `emptyDir` Volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myvolemptydir
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:                                    # Mount definition inside the container
      - name: xchange
        mountPath: "/tmp/xchange"          
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:
  - name: xchange
    emptyDir: {}
```

### What Happens Here?

* Two containers are in one pod.
    
* `writer` writes to `/data/message`
    
* `reader` reads from the same path
    
* Both share data via `emptyDir` mounted at `/data`