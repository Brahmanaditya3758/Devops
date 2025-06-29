---
title: "(Day 32) Task : Understanding Kubernetes Networking: 4 Critical Concepts Explained :-"
datePublished: Sun Jun 29 2025 04:08:19 GMT+0000 (Coordinated Universal Time)
cuid: cmch5go9a001k02lb83xj4xeu
slug: day-32-task-understanding-kubernetes-networking-4-critical-concepts-explained
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751169964325/3d51a476-9c53-48ee-a6f9-3c7dba7f6cd6.png
tags: jenkins, k8s, 2articles1week, twsbashblazechallenge-trainwithshubham, devops-kubernetes-containerization-continuousintegration-continuousdelivery-microservices-docker-containerorchestration-automation-cicd-devopsculture-k8s-infrastructureascode-gitops-cloudnative-containermanagement-kubernetescluster-devopstools-containersecurity-kubernetesdeployment-devopsengineer-k8sadmin-kubernetesnetworking-devsecops-scalability-monitoring-gitopspipeline-cloudcomputing-devopsbestpractices-kubernetesoperators, day32of90daysofdevops

---

**Author**: Aditya Sharma  
**Series**: 90 Days of DevOps – Day 32  
**Topic**: Kubernetes Networking Internals – Explained with Real-World Use Cases

## A Quick Note Before We Begin

Before diving into today’s article on Kubernetes Services and networking, I want to share a brief personal update. I’ve been away from writing for the past five days due to a back injury that left me feeling quite weak and unable to focus properly. As someone who has been consistent on this 90 Days of DevOps journey, it was frustrating to pause — but health always comes first.

Now that I’m feeling a bit better, I’m excited to continue where I left off — and today, we’ll explore some of the most important networking concepts in Kubernetes.

## **Introduction :**

Kubernetes is a standard for container orchestration, and networking is its backbone. For applications to scale and interact reliably across containers, Pods, and clusters, Kubernetes provides a robust networking model.

This blog addresses four major networking concerns:

1. How containers inside a Pod communicate
    
2. How Pods in a cluster communicate
    
3. How applications are exposed to the outside world
    
4. How services can be made available internally only
    

## 1\. Containers Inside a Pod Use Networking via Loopback ([localhost](http://localhost)) :

In Kubernetes, a Pod is the smallest deployable unit. It can contain one or more containers that are tightly coupled. All containers in a Pod share:

* The same network namespace
    
* The same IP address
    
* The same <mark>localhost/loopback</mark> interface
    

### How It Works:

* Imagine a Pod with two containers: `c00` and `c01`.
    
* If the app-container exposes an HTTP server on port 8080, the sidecar can reach it using [`localhost:8080`](http://localhost:8080).
    

## **Example: pod1.yaml :**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: testpod
  labels:
    app: ctoc
spec:
  containers:
  - name: c00
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo hello Aditya ; sleep 5; done"]
  - name: c01
    image: httpd
    ports:
      - containerPort: 80
```

### **Run and Test:**

```bash
kubectl apply -f pod1.yaml
kubectl exec testpod -it -c c00 -- /bin/bash
curl localhost:80
```

## 2\. Cluster Networking Allows Communication Between Pods :

Kubernetes requires that all Pods can communicate with each other directly, regardless of which node they're on.

Each Pod gets a unique IP address, and communication happens without NAT — this is ensured by CNI (Container Network Interface) plugins like Calico, Flannel, or Cilium.

### Key Points:

* All Pods in the cluster are part of a flat, routable network.
    
* No need to map ports like in Docker; just use Pod IPs.
    
* DNS is handled by CoreDNS, so you can resolve services and pods by name.
    

### Test Pod-to-Pod Communication:

1. **Deploy Pods :**
    
    ## `testpod1.yaml` (Pod running NGINX)
    
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: testpod1
      labels:
        app: nginx-app
    spec:
      containers:
      - name: c00
        image: nginx
        ports:
        - containerPort: 80
    ```
    
    ## `testpod2.yaml` (Pod running Apache HTTPD)
    
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: testpod2
      labels:
        app: httpd-app
    spec:
      containers:
      - name: c03
        image: httpd
        ports:
        - containerPort: 80
    ```
    
    ### How to Apply Both:
    
    ```bash
    kubectl apply -f testpod1.yaml
    kubectl apply -f testpod2.yaml
    ```
    
    ## Check Pod IPs (for direct communication)
    
    ```bash
    kubectl get pods -o wide
    ```
    
    You’ll get something like:
    
    ```bash
    NAME        READY   STATUS    IP           NODE
    testpod1    1/1     Running   10.244.0.10  minikube
    testpod2    1/1     Running   10.244.0.11  minikube
    ```
    
    ## Exec into One Pod and Curl the Other by IP
    
    Let’s exec into `testpod1` and access the HTTPD server in `testpod2`.
    
    ```bash
    kubectl exec -it testpod1 -- apt update
    kubectl exec -it testpod1 -- apt install curl -y
    kubectl exec -it testpod1 -- curl http://10.244.0.11
    ```
    
    If HTTPD is running on port 80 in `testpod2`, you should see its default Apache welcome page in raw HTML.
    

### Coming Up Tomorrow…

In the next article, I will be covering two important topics related to Kubernetes Services:

* How you can expose an application running in a Pod to the outside world using Services
    
* How to make an internal-only Service for communication limited to within the cluster
    

These are crucial concepts that will help you fully understand how Kubernetes handles service discovery and traffic management.

Due to my ongoing recovery from a back injury, I’m taking it slow and will publish the detailed explanations for these topics tomorrow. Thanks for your patience and continued support on this journey. 🙏

Stay tuned and take care!  
— Aditya Sharma