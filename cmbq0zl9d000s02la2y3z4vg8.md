---
title: "(Day 27) Task : Mastering Kubernetes: Writing YAML Manifests and Managing Pods :-"
datePublished: Tue Jun 10 2025 04:33:16 GMT+0000 (Coordinated Universal Time)
cuid: cmbq0zl9d000s02la2y3z4vg8
slug: day-27-task-mastering-kubernetes-writing-yaml-manifests-and-managing-pods
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749529870987/5cf5e909-a8ca-4956-9c27-60d21b8b6f72.png
tags: kubernetes, k8s, 2articles1week, k8scluster, k8s-commands, tws, k8s-multi-container-pods

---

## What is a Kubernetes YAML Manifest?

A Kubernetes YAML manifest is a configuration file that specifies the desired state of a Kubernetes resource, such as a pod or service, in a human-readable format. YAML (YAML Ain’t Markup Language) is indentation-sensitive (use 2 spaces, not tabs) and enables declarative resource management.

A YAML manifest typically includes four key fields:

* **apiVersion**: The Kubernetes API version (e.g., v1 for pods).
    
* **kind**: The resource type (e.g., Pod, Deployment).
    
* **metadata**: Information like name, namespace, and labels.
    
* **spec**: The desired configuration, such as containers or volumes.
    

## Prerequisites

Ensure you have:

* A Kubernetes cluster (e.g., Minikube, Kind, or a cloud provider like GKE, EKS, or AKS).
    
* kubectl installed and configured to connect to your cluster.
    
* Basic familiarity with containers (e.g., Docker).
    

Install kubectl on Linux (adjust for your OS):

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Verify installation:

```bash
kubectl version --client
```

## Writing a Kubernetes YAML Manifest for a Pod :-

A pod is the smallest unit in Kubernetes, typically running one or more containers. Let’s create a YAML manifest for a pod named testpod running a Python HTTP server using the python:3.9-slim image.

### Example: Python HTTP Server Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: testpod
  labels:
    app: python-http
spec:
  containers:
  - name: python-http-container
    image: python:3.9-slim
    command: ["python", "-m", "http.server", "8000"]
    ports:
    - containerPort: 8000
```

### Explanation of the YAML

* **apiVersion: v1**: Uses the core Kubernetes API for pods.
    
* **kind: Pod**: Specifies we’re creating a pod.
    
* **metadata**: Sets the pod’s name (testpod) and a label (app: python-http) for identification.
    
* **spec**: Defines the pod’s configuration.
    
    * **containers**: Lists the containers in the pod.
        
        * **name**: The container’s name (python-http-container).
            
        * **image**: The Docker image (python:3.9-slim).
            
        * **command**: Runs Python’s built-in HTTP server on port 8000.
            
        * **ports**: Exposes port 8000 for the container.
            

Save this YAML as testpod.yaml.

## Creating a Pod :-

**To create the pod, run:**

```bash
kubectl apply -f testpod.yaml
```

This command instructs Kubernetes to create or update the pod based on the YAML. Expected output:

```bash
pod/testpod created
```

## Running and Verifying the Pod :-

**Check the pod’s status:**

```bash
kubectl get pods
```

Output:

```bash
NAME     READY   STATUS    RESTARTS   AGE
testpod  1/1     Running   0          12s
```

* **READY**: 1/1 means all containers are ready.
    
* **STATUS**: Running indicates the pod is operational.
    
* **RESTARTS**: Should be 0 for a healthy pod.
    
* **AGE**: Time since creation.
    

## Viewing Pod Details :-

**For detailed information:**

```bash
kubectl describe pod testpod
```

This shows:

* Metadata (name, namespace, labels).
    
* Container details (image, command, ports).
    
* Events (e.g., pod scheduling, container startup).
    

## Identifying the Node :-

**To see which node hosts the pod:**

```bash
kubectl get pods -o wide
```

Output:

```bash
NAME     READY   STATUS    RESTARTS   AGE   IP           NODE
testpod  1/1     Running   0          20s   10.244.0.3   minikube
```

The NODE column indicates the node (e.g., minikube).

## Inspecting Pod Events and Logs :-

### Viewing Pod Events :

Use kubectl describe pod testpod to see the “Events” section, which logs actions like:

* Pod scheduled to a node.
    
* Container image pulled.
    
* Container started or failed.
    

Example:

```bash
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  25s   default-scheduler  Successfully assigned default/testpod to minikube
  Normal  Pulled     24s   kubelet            Container image "python:3.9-slim" already present
  Normal  Created    24s   kubelet            Created container python-http-container
  Normal  Started    24s   kubelet            Started container python-http-container
```

### Viewing Container Logs

Check the container’s logs:

```bash
kubectl logs testpod
```

This shows the Python HTTP server’s output, such as access logs:

```bash
10.244.0.1 - - [10/Jun/2025 09:50:10] "GET / HTTP/1.1" 200 -
```

## Getting Container Information

Inspect container details with:

```bash
kubectl describe pod testpod
```

The Containers section lists:

* **Name**: python-http-container.
    
* **Image**: python:3.9-slim.
    
* **Command**: python -m http.server 8000.
    
* **Ports**: 8000/TCP.
    
* **State**: E.g., Running.
    
* **Environment Variables**: None in this case.
    

For a structured view:

```bash
kubectl get pod testpod -o json
```

This outputs a JSON object with pod and container details.

## Deleting a Pod :-

**To delete the pod:**

```bash
kubectl delete pod testpod
```

Output:

```bash
pod "testpod" deleted
```

Or delete using the YAML file:

```bash
kubectl delete -f testpod.yaml
```

If the pod is managed by a controller (e.g., Deployment), it may be recreated. Delete the controller to prevent this.