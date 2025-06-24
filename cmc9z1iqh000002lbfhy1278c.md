---
title: "(Day 31) Task : Understanding Kubernetes Networking: Services Explained with NodePort & Real-Time Examples :-"
datePublished: Tue Jun 24 2025 03:34:11 GMT+0000 (Coordinated Universal Time)
cuid: cmc9z1iqh000002lbfhy1278c
slug: day-31-task-understanding-kubernetes-networking-services-explained-with-nodeport-and-real-time-examples
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1750735997183/45aee635-574b-43f9-bda4-8459bd6388cd.png
tags: kubernetes-kubeadm-devops-cloudcomputing-aws-ec2-containers-docker-k8s-linux-ubuntu-systemadministration-selfhosted-networking-cicd-automation-infrastructureascode-rbac-security-monitoring-logging-techblog-learning-opensource-career

---

## Introduction to Kubernetes Networking

Kubernetes is powerful, but one of the most crucial and often misunderstood components is networking—especially how Pods communicate with each other and with the outside world. That’s where Kubernetes Services come into play.

In this blog, we’ll explore:

* What is a Kubernetes Service?
    
* ClusterIP, NodePort, and LoadBalancer in detail.
    
* NodePort architecture & flow.
    
* Real-time YAML examples and access patterns.
    
* Use cases and best practices.
    

## What is a Kubernetes Service?

A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. Even if a Pod dies and restarts, the Service gives it a stable endpoint.

Without Services:

* Pod IPs are ephemeral and non-static.
    
* There’s no simple way to expose your application to users or other Pods.
    

## Types of Kubernetes Services :-

### 1️⃣ ClusterIP (Default) :

* Internal-only Service.
    
* Exposes the app on a cluster-internal IP.
    
* Use-case: backend services that other services consume.
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: my-backend
  ports:
    - port: 80
      targetPort: 8080
```

Accessed from inside the cluster only using DNS like `backend-service.default.svc.cluster.local`.

### 2️⃣ NodePort

* Exposes the Service on each Node’s IP at a static port.
    
* External users can access using `NodeIP:NodePort`.
    
* Range: `30000–32767`.
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30036
```

**Access Example:** `http://<NodeIP>:30036`

Ideal for:

* Exposing apps in bare-metal clusters.
    
* Quick testing environments.
    
* Local or development purposes.
    

### 3️⃣ LoadBalancer

* Available only in cloud providers (AWS, GCP, Azure).
    
* Exposes the Service externally using a cloud load balancer.
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: lb-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

**Cloud creates**:

* An external IP.
    
* Internal routing to your NodePort.
    
* Load balancing across nodes.
    

Best for production-grade applications.

## NodePort Networking Architecture :-

Here's what happens when you use NodePort:

1. **Client** sends a request to `<NodeIP>:<NodePort>`.
    
2. Kubernetes routes it internally to the correct **Pod IP** using kube-proxy.
    
3. Service load balances if multiple Pods are running under the same selector.
    

**Tip**: Even if a Pod is not on a specific node, the traffic will be forwarded correctly.

## Real-Time Commands & Use Cases

### Expose a Deployment as NodePort

```yaml
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get svc nginx
```

Sample output:

```yaml
NAME    TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
nginx   NodePort   10.96.0.1     <none>        80:30080/TCP   3m
```

Access at: `http://<NodeIP>:30080`