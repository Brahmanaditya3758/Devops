---
title: "(Day 33) Task :  Kubernetes Services – Solving the Problem of Dynamic Pod IPs Using Virtual IPs:-"
datePublished: Mon Jun 30 2025 05:53:35 GMT+0000 (Coordinated Universal Time)
cuid: cmcionwag001f02l53hfn8dqz
slug: day-33-task-kubernetes-services-solving-the-problem-of-dynamic-pod-ips-using-virtual-ips
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751262700034/51a8734d-2798-4466-ac5a-bb5b03a943d6.png
tags: kubernetes, 2articles1week, k8scluster, clusterip, k8sservices, trainwithshubham-tws-90daysofdevops-90daysofdevopschallenge-devops-devopscommunity-shubhamlondhe-automation-kubernetes-autoscaling-autohealing-microservices-kubernetescluster-kubelet-kubectl-orchestration-minikube-kubeadm-pod-kubernetesdeployment-kubernetespods-kubernetesservice-namespace-labels-selectors-nodeport-clusterip-loadbalancer-autoscaling-autohealing-configmaps-secrets-persistentvolume-persistentvolumeclaim-helm

---

In Kubernetes, Pods are the basic execution units, but they’re also inherently dynamic and short-lived. When scaling, updating, or restarting, Pods get new IP addresses. So how do you build reliable communication in such a constantly changing system?

That’s where Kubernetes Services come into play.

## The Problem: Pods Have Their Own IPs – But They Change :

Each Pod in Kubernetes gets a unique IP address when it's created. This is great in theory because it allows direct communication. However, there's a big problem:

In a Deployment or ReplicaSet, the set of Pods running at any given time can change due to updates, scaling, failures, or rolling deployments.

For example, imagine a frontend app that needs to talk to a backend. If the backend is running as a Deployment with multiple Pods:

* Which IP does the frontend connect to?
    
* What happens when one of those Pods is deleted or rescheduled and gets a new IP?
    

These questions make it clear: Pod IPs are unreliable for stable communication.

## The Solution: Services and Virtual IPs :

To solve this issue, Kubernetes introduces a Service object, which acts as a stable access point to a dynamic set of Pods.

### Here's how it works:

* The Service is assigned a virtual IP (ClusterIP).
    
* This virtual IP acts as a permanent endpoint, regardless of which Pods are currently running.
    
* The Service uses label selectors to dynamically route traffic to healthy Pods matching those labels.
    
* If a Pod dies and a new one is created, the Service automatically remaps traffic to the new Pod.
    

**In other words:**

Virtual IP stays constant → Kubernetes maintains mapping → You don’t need to track Pod IPs

## Example Scenario: 10 Pods, 1 Frontend :

Let’s say you have:

* 10 backend Pods running in a ReplicaSet
    
* A frontend app that needs to send requests to the backend
    

Without Services:

* The frontend would need to know all 10 IPs
    
* It would have to detect new Pods and remove stale ones
    

With Services:

* The frontend just connects to one virtual IP
    
* Kubernetes handles load balancing and mapping automatically
    

## Why This Is Critical in Deployments :

When you use:

* ReplicationController → Pods are scaled up/down dynamically
    
* Deployment → Updates cause Pods to be deleted and replaced
    
* Rolling updates or restarts → Pod IPs change regularly
    

So, trying to connect to Pod IPs directly would constantly break communication.

That’s why Services are critical components in Kubernetes networking. They bridge the gap between dynamic Pods and stable connectivity.

## What Is a Service in Kubernetes?

A Service is a Kubernetes object that:

* Provides a virtual IP address
    
* Maps to one or more Pods based on labels
    
* Load balances traffic to the matching Pods
    
* Can optionally be exposed outside the cluster
    

## Virtual IP ≠ Physical IP :

This virtual IP is not tied to any physical network. It’s a cluster-level construct, which internally maps to the actual Pod IPs.

The kube-proxy running on every node is responsible for maintaining this mapping using `iptables` or `IPVS`.

* Virtual IP is reachable within the cluster
    
* kube-proxy updates the routing rules dynamically as Pods come and go
    

## Pod IPs Are Private :

Even though each Pod has a unique IP, these IPs are:

* Assigned from an internal subnet (e.g., `10.244.x.x`)
    
* Not routable from the outside world
    
* Short-lived due to the nature of Pods
    

Therefore, to allow your app to receive traffic from other components (or users), you need a Service.

## How a Service Knows Which Pods to Route To?

This is handled using labels and selectors.

Each Pod has metadata labels like:

```yaml
labels:
  app: backend
```

The Service uses a selector:

```yaml
selector:
  app: backend
```

That way, it will forward traffic to any Pod with that label.

## Types of Services in Kubernetes :

There are four types of services, each used in different scenarios:

| Type | Description |
| --- | --- |
| **ClusterIP** | Default; only accessible from inside the cluster |
| **NodePort** | Exposes the service on a static port on each node (30000–32767) |
| **LoadBalancer** | Uses a cloud provider's load balancer to expose the service externally |
| **Headless** | No ClusterIP; gives clients direct access to Pod IPs via DNS |

## **Let’s Talk About ClusterIP – The First and Most Common Type :**

ClusterIP is the default type of Service.

### Features:

* Allocates a virtual IP inside the cluster
    
* Not accessible from outside
    
* Used for communication between microservices
    

### Example YAML:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: demoservice
spec:
  ports:
    - port: 80
      targetPort: 80
  selector:
     name: deployment
  type: ClusterIP
```

## Recap: Why Services Are Important in Kubernetes

* Pods are dynamic – they come and go.
    
* Each Pod has a unique IP, but it’s temporary.
    
* Virtual IPs (via Services) provide a stable endpoint.
    
* kube-proxy keeps mapping between Virtual IP and Pod IP.
    
* Services allow internal and external components to reliably connect to apps running inside Pods.
    

## **Kubernetes ClusterIP Practical – HTTP Server Deployment and Service Communication :-**

In this project, we’ll set up a Deployment running the `httpd` image and expose it via a Service of type ClusterIP. This helps us simulate internal communication within the Kubernetes cluster using a virtual IP.

We'll go step by step:

* Create the Deployment using `deployHttpd.yml`
    
* Create the ClusterIP Service using `service.yml`
    
* Test connectivity using `minikube ssh` and `curl`
    

## File 1: deployHttpd.yml (Deployment) :

This file defines the Deployment that runs the Apache HTTP server (`httpd`) container.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployhttpd
spec:
  replicas: 1
  selector:
    matchLabels:
      name: deployment
  template:
    metadata:
      labels:
        name: deployment
    spec:
      containers:
        - name: httpd-container
          image: httpd
          ports:
            - containerPort: 80
```

### Apply the Deployment:

```bash
kubectl apply -f deployHttpd.yml
```

### Verify Pod:

```bash
kubectl get pods 
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751262177485/faf40194-d196-494d-8c10-a89638917e16.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751262339384/042a2072-81e7-4fa0-a8fe-67c76398019b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751262293612/20e71a1a-f5bf-403a-bf2f-56e42fb04aee.png align="left")

## File 2: service.yml (ClusterIP Service) :

This file creates a **ClusterIP Service** that targets the Pods from the Deployment above using a **label selector**.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: demo-service
spec:
  ports:
    - port: 80
      targetPort: 80
  selector:
    name: deployment
  type: ClusterIP
```

* `port`: The port used inside the cluster.
    
* `targetPort`: The container port defined in the Deployment.
    
* `selector`: Routes traffic to Pods labeled `name: deployment`.
    

### Apply the Service:

```bash
kubectl apply -f service.yml
```

## Check Service and ClusterIP

```bash
kubectl get svc or service
```

Expected output:

```bash
NAME            TYPE        CLUSTER-IP      PORT(S)   AGE
demo-service    ClusterIP   10.97.210.158    80/TCP    5s
```

This `10.96.252.123` is the virtual IP of the Service. It's internal and accessible only within the cluster.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751262460026/99ef1eca-3171-42c7-b2ed-40a1aaff92ae.png align="left")

## Test ClusterIP Communication from Inside the Cluster

Since ClusterIP is not accessible from outside the cluster, we must test it from within:

### Step 1: SSH into Minikube

```bash
minikube ssh
```

### Step 2: Install `curl` inside Minikube (if needed):

```bash
sudo apt update
sudo apt install curl -y
```

### Step 3: Curl the ClusterIP

```bash
curl <CLUSTER-IP> :80
```

Or, using the service name:

```bash
curl http://demo-service
```

You should see the default Apache welcome page HTML content.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751262318817/d135a5d1-9247-4c53-99e1-c0979ed412aa.png align="left")

## **Final Thoughts :**

Kubernetes Services are a foundational component in building robust, scalable, and maintainable applications in Kubernetes.

They:

* Abstract away Pod lifecycle changes
    
* Offer load balancing and discoverability
    
* Bridge communication across services internally and externally
    

Without Services, managing connectivity between Pods in a dynamic, self-healing system like Kubernetes would be nearly impossible.

***In the next article, we’ll explore how to expose a service to the outside world using <mark>NodePort, and later, how to manage traffic with Ingress.</mark>***

**🙏 Thanks for following along on my 90 Days of DevOps Journey. I’m still recovering from my back injury, so I appreciate your support and patience as I go at a steady pace.**

**— Aditya Sharma**