---
title: "(Day 36) Task :  Kubernetes ConfigMaps & Secrets :-"
datePublished: Fri Jul 04 2025 06:17:04 GMT+0000 (Coordinated Universal Time)
cuid: cmcof9ira001602jxb4jraj1y
slug: day-36-task-kubernetes-configmaps-and-secrets
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751609789820/01c0a56d-2901-49d7-aac7-ee6d7d041991.png
tags: secrets, 2articles1week, kubernetes-configmap

---

In a modern DevOps environment, configuration management and secrets handling are essential for secure and scalable application deployments. Kubernetes offers two native mechanisms for managing configuration data: ConfigMaps and Secrets. These are designed to decouple configuration from the application code and Docker images, thus enabling smoother deployments across different environments.

## Why Use ConfigMaps and Secrets?

When deploying applications across different environments (development, QA, production), configuration files and environment-specific values often differ. Traditionally, updating these values meant:

1. Editing source code
    
2. Committing changes
    
3. Rebuilding the Docker image
    
4. Redeploying the application
    

This approach is time-consuming and error-prone. Kubernetes ConfigMaps and Secrets solve this by decoupling configuration from the container image, enabling the same image to be used in different environments by simply injecting different configurations.

## ConfigMap: Managing Non-Sensitive Configuration :

### What is a ConfigMap?

A ConfigMap is a Kubernetes object used to store **non-sensitive, unencrypted** configuration data in key-value pairs or configuration files.

### Use Cases :

* Application environment variables
    
* System properties or feature toggles
    
* Entire configuration files (e.g., `.env`, `.properties`)
    

### How to Use ConfigMaps :-

#### 1\. **As Environment Variables :**

When you use a ConfigMap as environment variables, the key-value pairs inside the ConfigMap are injected into the container's environment, just like how you’d use `export` in a shell.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-env-demo
spec:
  containers:
    - name: app-container
      image: my-app
      envFrom:
        - configMapRef:
            name: app-config
```

#### 2\. **As Volumes (Mounting Config Files) :**

When you mount a ConfigMap as a volume, Kubernetes creates actual files inside the container's filesystem, where each key becomes a file, and the file content is the value.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: config-volume-demo
spec:
  containers:
    - name: app-container
      image: my-app
      volumeMounts:
        - name: config-volume
          mountPath: "/etc/config"
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

### How to Create a ConfigMap :

#### From Literal Key-Value Pairs

```bash
kubectl create configmap app-config --from-literal=MODE=dev --from-literal=LOG_LEVEL=info
```

#### From a File

```bash
kubectl create configmap app-config --from-file=config.properties
```

## Secrets: Safeguarding Sensitive Information

### What is a Secret?

A Secret is a Kubernetes object designed to store sensitive information, such as:

* Database passwords
    
* API keys
    
* Tokens
    

Unlike ConfigMaps, Secrets are base64 encoded and optionally encrypted when stored in `etcd`. They are mounted in-memory using `tmpfs` so that they are not written to disk.

### Properties of Secrets

* Namespace-scoped objects
    
* Accessible via env vars or volumes
    
* Stored in-memory (`tmpfs`) in the pod
    
* Size limit: 1 MB