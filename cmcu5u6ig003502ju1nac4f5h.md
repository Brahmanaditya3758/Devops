---
title: "(Day 38) Task : Mastering Kubernetes Jobs in the Container and Pod Lifecycle :-"
datePublished: Tue Jul 08 2025 06:39:49 GMT+0000 (Coordinated Universal Time)
cuid: cmcu5u6ig003502ju1nac4f5h
slug: day-38-task-mastering-kubernetes-jobs-in-the-container-and-pod-lifecycle
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751956647494/9379a8af-9211-4458-b7c8-6734f23b21a8.png
tags: k8s, init-container, k8s-job-management, kubernetes-pod-lifecycle

---

## ***Introduction :-***

Kubernetes has revolutionised how modern applications are deployed, scaled, and managed. While most developers and DevOps engineers are familiar with Deployments and StatefulSets to manage long-running services, one of the most important and powerful features of Kubernetes for one-time or batch processing tasks is the Job.

A Kubernetes Job ensures that a pod successfully completes the assigned task, even if it requires multiple attempts. Combined with CronJobs and Init Containers, Jobs become a key part of automating routine tasks, batch processing, and managing the container and pod lifecycle effectively.

In this article, we will go deep into understanding Kubernetes Jobs, their use cases, associated YAML configurations, how they behave in the pod lifecycle, how to schedule them using CronJobs, and how Init Containers can prepare your main containers for execution. We’ll also include real-world examples, YAML configurations, installation instructions using Minikube, and hands-on commands to help you master Jobs in Kubernetes.

## **Setting Up Your Environment :-**

To follow along with the examples, you’ll need a working Kubernetes cluster. We’ll use Minikube for local development and testing.

### Prerequisites:

* *Linux/macOS or EC2 instance.*
    
* *Docker.*
    
* *kubectl.*
    
* *Minikube.*
    

### ***Step 1: Install Docker :***

```bash
sudo apt update
sudo apt install docker.io -y 
sudo systemctl start docker 
sudo systemctl enable docker 
sudo usermod -aG docker $USER 
newgrp docker
```

***Test Docker :***

```bash
docker run hello-world
```

### ***Step 2: Install Minikube :***

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

***Start your cluster :***

```bash
minikube start --driver=docker
```

***Verify :***

```bash
kubectl get nodes
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751956699299/bab3329c-a373-48e8-be45-5478bd45702e.png align="center")

## What is a Kubernetes Job?

A Job creates one or more pods to perform a specific task to completion. Unlike a Deployment, which maintains a certain number of pods running continuously, a Job runs a pod until it completes successfully (exit code 0), and then it finishes.

### Use Cases for Kubernetes Jobs:

* Database migration scripts
    
* Scheduled backups
    
* Email notifications
    
* Temporary data cleanup
    
* Batch processing pipelines
    

### Basic Job YAML Example:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  template:
    spec:
      containers:
        - name: testjob
          image: centos:7
          command: ["/bin/sh", "-c", "echo Technical-Guftgu; sleep5"]
      restartPolicy: Never
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751954443845/7474f231-22c9-4117-99f1-473fc4add080.png align="left")

***Apply :***

```bash
kubectl apply -f job.yaml
```

***Check Job :***

```bash
kubectl get jobs
kubectl get pods
watch kubectl get pods
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751954744892/3f891c7c-006c-4757-a760-ce9cecec044d.png align="left")

## ***Parallel Jobs in Kubernetes :-***

Kubernetes Jobs also support parallelism, which allows you to run multiple pods at the same time. This is useful when you have batch tasks that can be split and processed concurrently.

### ***YAML Example with Parallelism :***

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  parallelism: 5                 # Run 5 pods in parallel
  activeDeadlineSeconds: 30     # Timeout after 30 seconds
  template:
    metadata:
      name: testjob
    spec:
      containers:
        - name: counter
          image: centos:7
          command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 20"]
      restartPolicy: Never
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955234835/ebebd9de-0c96-44b7-a36e-e5d7db688e12.png align="left")

***Apply it and check :***

```bash
kubectl apply -f job2.yml
kubectl get jobs
kubectl get pods
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955453751/e7dc9395-be8b-4dca-ad22-8a023d51e798.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955523425/fe86f217-b899-4e64-b161-bf8796050512.png align="left")

## ***CronJobs :-***

A CronJob in Kubernetes allows you to run Jobs on a scheduled basis, just like Linux cron. The format is: minute hour day month day-of-week.

### ***CronJob YAML Example :***

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: bhupi
spec:
  schedule: "* * * * *"  # Runs every minute
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: bhupi
              image: ubuntu
              command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
          restartPolicy: Never
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955692123/add59581-bb36-466e-9eda-e3cd5d43e1cc.png align="left")

***Apply and monitor :***

```bash
kubectl apply -f cronjob.yaml
kubectl get cronjobs
watch kubectl get jobs
```

***View logs :***

```bash
kubectl get pods
kubectl logs <pod-name>
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955873778/25b7bc08-c484-49b6-9870-0ac66971de1c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1751955887174/e46bb760-bff4-45a4-a4d5-71c0c76b1ab9.png align="left")

## ***Init Containers in Jobs :-***

Init Containers are special containers that run before the main containers start. They are great for setup tasks like creating files, downloading dependencies, or setting environment variables.

### ***Init Container with Shared Volume Example :***

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-container-demo
spec:
  volumes:
    - name: exchange
      emptyDir: {}
  initContainers:
    - name: c1
      image: ubuntu
      command: ["/bin/bash", "-c", "echo 'Hello Aditya – self3' > /tmp/exchange/testfile"]
      volumeMounts:
        - name: exchange
          mountPath: /tmp/exchange
  containers:
    - name: c2
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo 'Reading shared file:'; cat /tmp/data/testfile; sleep 5; done"]
      volumeMounts:
        - name: exchange
          mountPath: /tmp/data
```

![Uploaded image](https://files.oaiusercontent.com/file-VjCbPnLo3K4BKJP6RjKd4h?se=2025-07-08T06%3A35%3A40Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3DScreenshot%25202025-07-08%2520at%25208.55.03%25E2%2580%25AFAM.png&sig=tRA8rvDnDNYjRdULW1bvhT%2BQW3istb9iwf8HTEdtFs4%3D align="left")

**Apply :**

```bash
kubectl apply -f init-container-demo.yaml
kubectl logs -f init-container-demo
```

## Understanding Pod & Container Lifecycle with Jobs :-

Kubernetes jobs deeply connect with the pod lifecycle:

1. **Pod Creation**: Triggered by the Job controller.
    
2. **Init Containers**: Run in order, must complete first.
    
3. **Main Containers**: Start after init containers finish.
    
4. **Completion**: Pod exits with `0` (success).
    
5. **Job Controller**: Marks job as `Complete`.
    

### Restart Policies:

* `Never`: Job doesn’t retry on failure.
    
* `OnFailure`: Retry on non-zero exit code (default in some controllers).
    

## Best Practices :

| Feature | Best Practice |
| --- | --- |
| Job retries | Set `backoffLimit` to avoid infinite loops |
| Pod cleanup | Manually delete or use TTL controllers |
| Logging | Always use `kubectl logs` for visibility |
| Volume sharing | Use `emptyDir` between init & main containers |
| CronJob limits | Set `successfulJobsHistoryLimit` & `failedJobsHistoryLimit` |