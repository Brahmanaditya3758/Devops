---
title: "(Day 25) Task : Setting Up Kubernetes Cluster on AWS :-"
datePublished: Thu Jun 05 2025 02:37:29 GMT+0000 (Coordinated Universal Time)
cuid: cmbirnf6s000a09i96im79cls
slug: day-25-task-setting-up-kubernetes-cluster-on-aws
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749091021834/f0fd806b-85ed-4d20-9691-b2e56363e752.jpeg
tags: aws, kubernetes, devops, 2articles1week

---

Kubernetes is a powerful open-source platform for orchestrating containerized applications, enabling seamless deployment, scaling, and management. Setting up a Kubernetes cluster on AWS using Ubuntu is a great way to leverage the cloud’s scalability while learning Kubernetes hands-on. In this detailed guide, we’ll walk through creating a Kubernetes cluster with one master node and two worker nodes on AWS EC2 instances running Ubuntu 24.04 LTS. We’ll cover all commands, including those common to both master and worker nodes, and the bootstrapping process to connect them.

This article is perfect for DevOps enthusiasts, developers, or anyone looking to build a multi-node Kubernetes cluster from scratch. By the end, you’ll have a fully functional cluster ready to deploy containerized applications.

## Prerequisites :

Before we begin, ensure you have the following:

* **AWS Account**: An active AWS account with permissions to create EC2 instances and configure security groups.
    
* **AWS CLI**: Installed and configured on your local machine (optional for manual setup via AWS Console).
    
* **SSH Client**: Tools like OpenSSH, PuTTY, or Git Bash for connecting to EC2 instances.
    
* **Basic Knowledge**: Familiarity with Linux commands, Docker, and Kubernetes concepts.
    
* **Hardware Requirements**:
    
    * **Master Node**: AWS EC2 instance type t2.medium (2 vCPUs, 4 GiB RAM).
        
    * **Worker Nodes**: AWS EC2 instance type t2.micro (1 vCPU, 1 GiB RAM) for each worker node.
        
    * **Operating System**: Ubuntu Server 24.04 LTS for all nodes.
        

## Step 1: Launch EC2 Instances on AWS :

We’ll create three EC2 instances: one for the master node and two for worker nodes.

1. **Log in to AWS Management Console**:
    
    * Navigate to the EC2 Dashboard.
        
    * Click Launch Instance.
        
2. **Configure Instances**:
    
    * **Name and Tags**:
        
        * Master Node: k8s-master
            
        * Worker Node 1: k8s-worker-node1
            
        * Worker Node 2: k8s-worker-node2
            
    * **Amazon Machine Image (AMI)**: Select Ubuntu Server 24.04 LTS (HVM), SSD Volume Type.
        
    * **Instance Type**:
        
        * Master: t2.medium.
            
        * Workers: t2.micro.
            
        * ***Note :*** *In our Case we have used all instances as medium to save us from extra work.*
            
    * **Key Pair**: Create or select an existing key pair for SSH access (e.g., k8s-key.pem).
        
    * **Network Settings**:
        
        * Create a new security group (e.g., k8s-security-group) with the following inbound rules:
            
            * **SSH (port 22)**: Allow from your IP or 0.0.0.0/0 (for learning purposes; restrict in production).
                
            * **All Traffic**: Allow within the security group for inter-node communication (critical for Kubernetes).
                
            * **Port 6443**: Kubernetes API server (master node).
                
            * **Ports 30000–32767**: NodePort services (worker nodes).
                
    * **Storage**: Use default 16 GiB gp3 volume for all nodes.
        
    * **Launch**: Review and launch the instances & then connect to each instance.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020225848/d2680d40-326d-4f46-9dba-3dac61983451.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020501076/0cea3d4d-b4a2-45c9-8004-c679a4ed37bd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020509004/03ed7e7f-7ddc-4f30-980e-bc06d787b2b1.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749020551574/b2b07e7f-3cb9-45b1-a0fe-b4599fe95bab.png align="center")

## ***Step 2: Prepare All Nodes :-***

The following steps are common to both the master and worker nodes. Execute these commands on all three instances to prepare the environment for Kubernetes.Each command is explained to ensure clarity.

**Follow the below commands to establish a connection between the master node and worker node with the help of kubeadm:**

### **Kubeadm Installation:**

* **Both Master & Worker Node**
    

```bash
sudo su
apt update -y
apt install docker.io -y

systemctl start docker
systemctl enable docker

curl -fsSL "https://packages.cloud.google.com/apt/doc/apt-key.gpg" | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/kubernetes-archive-keyring.gpg
echo 'deb https://packages.cloud.google.com/apt kubernetes-xenial main' > /etc/apt/sources.list.d/kubernetes.list

apt update -y
apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y

##To connect with cluster execute below commands on master node and worker node respectively
```

* **Master Node**
    

```bash
sudo su
kubeadm init

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

kubeadm token create --print-join-command
```

* **Worker Node**
    

```bash
sudo su
kubeadm reset pre-flight checks
-----> Paste the Join command on worker node and append `--v=5` at end

#To verify cluster connection
```

* **On Master Node**
    

```bash
kubectl get nodes 

# worker
# kubeadm join 172.31.84.66:6443 --token n4tfb4.grmew1s1unug0get     --discovery-token-ca-cert-hash sha256:c3fda2eaf5960bed4320d8175dc6a73b1556795b1b7f5aadc07642ed85c51069 --v=5
# kubeadm reset pre-flight checks
# kubeadm token create --print-join-command
# kubectl label node ip-172-31-20-246 node-role.kubernetes.io/worker=worker
# kubectl label nodes ip-172-31-92-99 kubernetes.io/role=worker
# kubectl config set-context $(kubectl config current-context) --namespace=dev
```