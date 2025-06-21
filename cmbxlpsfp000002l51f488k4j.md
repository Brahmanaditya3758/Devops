---
title: "(Day 28) Task : Kubernetes Labels and Selectors:-"
datePublished: Sun Jun 15 2025 11:47:54 GMT+0000 (Coordinated Universal Time)
cuid: cmbxlpsfp000002l51f488k4j
slug: day-29-task-kubernetes-labels-and-selectors
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749988016860/27786e7f-cd5a-4417-9b53-d9f6c7f2cdf2.png
tags: kubernetes, devops, k8s, 2articles1week, k8scluster, technicalguftgu, labels-and-selectors-kubernetes

---

Labels and selectors are the hidden heroes behind managing, organizing, and operating workloads in Kubernetes clusters. Whether you're working with a single Pod or deploying hundreds of microservices, labels and selectors provide a powerful mechanism for targeting specific sets of resources in your Kubernetes environment.

In today's blog post, I’ll walk you through:

* What labels and selectors are.
    
* How to assign and filter them.
    
* Common issues and best practices.
    
* My hands-on experiments with labels.
    

## ***What Are Labels?***

* Labels in Kubernetes are key-value pairs attached to objects like Pods, Nodes, Services, and Deployments. They’re used to identify, group, and filter resources.
    
* Labels are the mechanism used to organize Kubernetes objects.
    
* A label is a key-value pair without any predefined meaning that can be attached to the object.
    
* Labels are similar to tags in AWS or Git, where we use a name to quick reference.
    
* Multiple labels can be added to a single object. Labels will always reside inside metadata.
    

### ***Syntax:***

```yaml
labels:
  key: value
```

### ***Example:***

```yaml
labels:
  env: development
  app: nginx
  region: asia
```

## ***What Are Label Selectors?***

* Label selectors are expressions or queries used to filter Kubernetes objects based on their labels. They are used in commands like `kubectl get`, and also in controller specs (like Deployments or Services).
    
* Unlike Name/UIDs, Label does not provide any uniqueness. We can think of many objects to carry same label.
    
* Once all object has labels, then LabelSelector helps to narrow down.
    
* The API currently supports two types of Selector, EqualityBase and SetBase.
    

### ***Types of Selectors:***

* **Equality-based selectors**
    
    * `env=production`
        
    * `env!=development`
        
* **Set-based selectors**
    
    * `env in (development, testing)`
        
    * `env notin (staging, qa)`
        

## ***<mark>Hands-On Commands:</mark>***

### **Creating a Pod with Labels :**

I created a simple pod named `delhipod` with these labels:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: delhipod
  labels:
    env: development
    class: pods
spec:
  containers:
     - name: c00
       image: ubuntu
       command: ["/bin/bash", "-c", "while true; do echo Hello-Aditya; sleep 5 ; done"]
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749986533034/c76142cf-a58c-4869-ae35-33a95f125432.png align="center")

### **Applied using :**

```yaml
kubectl apply -f pod2.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749986810713/9456f267-d833-4098-80bb-47c05cff77ad.png align="center")

### **Verifying the Pod :**

```yaml
kubectl get pods --show-labels
```

**Output:**

```yaml
NAME       READY   STATUS    RESTARTS   AGE     LABELS
delhipod   1/1     Running   0          2m1s      class=pods,env=development
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749986963877/c7cc8123-ee11-4552-bb3e-7ff2d26dd6ff.png align="left")

### **Adding a Label Post-Creation :**

```yaml
kubectl label pod delhipod myname=Aditya  # Object = pod , Objectname = delhipod , myname=Aditya --> Key : Value
```

**Now the pod has:**

```yaml
LABELS
class=pods,env=development,myname=Aditya
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749987615350/e56782de-465d-4116-bcb6-5141c31c0ead.png align="center")

### Filtering Pods Using Label Selectors :-

1. **Now listing pods matching same labels :(Equality based)**
    

```yaml
kubectl get pods -l env=development
```

2. **Now list where development label not present :(Equality based)**
    

```yaml
kubectl get pods -l env!=development
```

3. **Set-Based Filter :**
    

```yaml
kubectl get pods -l 'env in (development, testing)'
```

4. **Multi-Label Filter :**
    

```yaml
kubectl get pods -l class=pods,myname=Aditya
```

Be careful not to add spaces around commas — Kubernetes will throw an error:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749987628847/13ea771f-a20c-495c-9659-6c638c76666a.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749987640183/83d0e67c-0400-4f8e-b37b-4f0cc208d1e6.png align="center")

## Common Errors I Faced (and Solved!) :-

### 1\. **Invalid Pod Name :**

```yaml
The Pod "Adipod" is invalid: metadata.name: Invalid value: "Adipod"
```

**Fix:** Pod names must be lowercase and follow DNS-1123 format:

```plaintext
[a-z0-9]([-a-z0-9]*[a-z0-9])?
```

### 2\. **Incorrect Label Command Syntax :**

```yaml
kubectl label pods delhipod myname = Aditya  
```

**Fix:** No spaces allowed around the equal sign.

## Final Thoughts

Labels and selectors may seem simple, but they’re powerful tools for managing complex Kubernetes environments. From grouping deployments to performing rolling updates and clean deletions, labels give you a declarative way to organize and manipulate cluster objects.

This hands-on session gave me real insight into how developers and DevOps engineers can leverage this system effectively. Onwards to more!

✍️ **Written by Aditya Sharma**  
🎯 *#90DaysOfDevOps* | *Day 28*  
📍 [](https://hashnode.com/@brahmanaditya37)[Follow me on Hashnode](https://hashnode.com/@brahmanaditya37) for more Kubernetes and DevOps insights!

Let me know if you'd like to add a YAML manifest snippet for multiple containers, Services, or Deployments using selectors!