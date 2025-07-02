---
title: "(Day 35) Task : Kubernetes Persistent Volumes & Liveness Probes"
datePublished: Wed Jul 02 2025 09:02:14 GMT+0000 (Coordinated Universal Time)
cuid: cmclqa7w3000d02joa31xbjto
slug: day-35-task-kubernetes-persistent-volumes-and-liveness-probes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1751446817656/bca03ed7-3026-40a9-8cc5-5c206641c672.png
tags: 2articles1week, pvc, volume-mount, kubernetes-livenessprobe, persistent-volume-claim, persistentvolumes

---

In traditional IT environments, storage systems are managed by dedicated storage administrators, and application developers are just informed *how* to use that storage — not *how it’s managed*. However, when we move to Kubernetes and containerized systems, the scenario changes significantly. Managing storage in a dynamic, distributed, and ephemeral system becomes a challenge.

Thankfully, Kubernetes offers a powerful solution — the Persistent Volume (PV) subsystem — to decouple storage provisioning from application usage.

## What is a Persistent Volume (PV)?

A Persistent Volume is a cluster-wide storage resource in Kubernetes. It is provisioned by an administrator or dynamically created by Kubernetes using StorageClasses.

Unlike ephemeral volumes like `emptyDir`, which are tied to the lifecycle of a pod, PVs persist beyond the pod’s lifetime, making them ideal for storing application data like databases, logs, or user uploads.

### Key Features:

* Not tied to a single pod
    
* Cluster-scoped resource
    
* Survives pod restarts and node failures
    
* Can be provisioned statically (pre-created) or dynamically (on-demand)
    

## Why Do We Need Persistent Volumes?

Let’s take a scenario:

A pod is scheduled on Node A and writes data to its local volume. Later, it gets rescheduled on Node B.  
Result? All that data is lost — because local node storage is not shared!

To solve this, network-based volumes like AWS EBS, NFS, or GlusterFS are used. These can be attached and detached across nodes — but Kubernetes needs a way to abstract and manage them. That’s where PV & PVC come in.

## How Persistent Volumes Work :-

### 1\. **Persistent Volume (PV) :**

Admin creates a volume resource in the cluster. For example, an AWS EBS Volume.

### 2\. **Persistent Volume Claim (PVC) :**

User/app claims the volume using a PVC, specifying size, access mode, and storage class.

### 3\. **Binding :**

Kubernetes binds the PVC to a matching PV. After this, the pod can mount it.

### 4\. **Mounting :**

The volume is then mounted into the pod via its spec.

### 5\. **Releasing :**

Once the pod is deleted or the PVC is deleted, the volume is released (but not deleted unless explicitly configured).

## Example: Using AWS EBS as Persistent Volume :

### What is EBS?

Elastic Block Store (EBS) is block storage provided by AWS, ideal for storing data like databases or application logs.

### Restrictions:

1. EBS volumes can only be attached to EC2 instances
    
2. The EBS volume and EC2 must be in the same region and availability zone
    
3. Single-Attach: An EBS volume can be attached to only one EC2 instance at a time
    

1. ### Sample PV YAML (EBS):
    

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: myebsvol
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  awsElasticBlockStore:
    volumeID: <your-ebs-volume-id>
    fsType: ext4
```

2. ### PVC YAML:
    

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myebsvolclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

3. ### deploy-pv.yml
    

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pvdeploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pvapp
  template:
    metadata:
      labels:
        app: pvapp
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: myvolume
      volumes:
      - name: myvolume
        persistentVolumeClaim:
          claimName: myebsvolclaim
```

### Apply in Order:

```bash
kubectl apply -f pv.yml
kubectl apply -f pvc.yml
kubectl apply -f deploy-pv.yml
```

### Verify Resources

```bash
kubectl get pv
kubectl get pvc
kubectl get deploy
kubectl get pods
```

### Test the Volume

You can exec into the pod and write a file to test persistence:

```bash
kubectl exec -it <pod-name> -- /bin/bash
echo "Hello from PV" > /usr/share/nginx/html/index.html
```

## Liveness Probes — Keep Containers Healthy

In a distributed, containerized setup, failure detection is crucial. Kubernetes provides Liveness Probes to ensure that containers are still working correctly.

### What is a Liveness Probe?

A Liveness Probe tells Kubernetes whether a container is still running as expected. If the check fails repeatedly, Kubernetes will automatically restart the container — a huge boost for fault tolerance.

### Types of Liveness Probes:

| Type | Description |
| --- | --- |
| **HTTP GET** | Sends an HTTP request to a path (e.g., `/health`) |
| **Exec** | Runs a command inside the container |
| **TCP Socket** | Tries to open a TCP connection on a port |

### Sample YAML:

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
  failureThreshold: 3
```

## Real World Use Case

Let’s say you’re running a MySQL database inside a pod. You store its data in `/var/lib/mysql`. If the pod restarts, you don’t want to lose data — so you use a Persistent Volume.

At the same time, you want to monitor its health, e.g., by checking if the MySQL port is responsive. For that, you use a Liveness Probe.

This is the power combo of persistent storage and self-healing infrastructure!