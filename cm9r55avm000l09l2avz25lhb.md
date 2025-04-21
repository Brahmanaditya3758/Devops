---
title: "(Day 11) Task : Docker Architecture & Docker Installation :-"
datePublished: Mon Apr 21 2025 13:58:03 GMT+0000 (Coordinated Universal Time)
cuid: cm9r55avm000l09l2avz25lhb
slug: day-11-task-docker-architecture-and-docker-installation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745243558513/1606ca7e-3303-44a3-a2b4-6e5145a0ce96.webp
tags: docker, devops, docker-images, tws, dockerfile-dockerimage-containerization-devops-containerizationtechnology-dockerbuild-dockerrun-dockercontainer-dockercompose-dockerhub-containerdeployment-cloudnative-containerizationbestpractices-devopstools-cicd-feel-free-to-select-the-relevant-hashtags-based-on-the-specific-focus-of-your-blog-post, twsbashblazechallenge-trainwithshubham, 90-days-of-devops-challenge, twscommunity, 11-day-of-90-days-of-devops

---

## ***Docker Architecture :-***

Docker has a client-server architecture, which means it has two main parts:

* The client that you use to type Docker commands.
    
* The server (Docker engine also called the Docker Daemon) that does all the work behind the scenes.
    

![Docker Architecture ](https://media.geeksforgeeks.org/wp-content/uploads/20221205115118/Architecture-of-Docker.png align="left")

### ***The main parts of Docker architecture :-***

***1\. Docker Client :***

* The interface we (the user) interact with.
    
* Sends commands to the Docker Daemon.
    
* Acts like a remote control.
    
    * **Examples of commands:**
        
        ```bash
        docker search <image-name>
        docker pull <image-name>
        ```
        

***2\. Docker Daemon (Docker Engine) :***

* The brain behind Docker.
    
* Listens to Docker Client requests.
    
* Builds, runs, and manages containers.
    
* Runs on the host OS.
    
* Can communicate with other Docker Daemons.
    

***3\. Docker Images :***

* Read-only templates used to create containers.
    
* it’s a single file which contains all dependencies needed to run any program(code, libraries).
    
* We can think of it as a recipe.
    
    * ***Ways to create a Docker image :***
        
        * **Using a Dockerfile :**
            
            * You write a Dockerfile with instructions to build an image.
                
            * Then run `docker build` to create the image.
                

***4\. Docker Containers :***

* The running instances of Docker images.
    
* Lightweight and portable.
    
* Each container runs in isolation.
    
* Think of it like a "live" app created from an image.
    
* ***Note :-*** Image becomes container when run on Docker Engine.
    

***5\. Dockerfile :***

* A text file with a list of instructions to create a Docker Image.
    
* Like a blueprint or recipe to make an image.
    
* Example line: `FROM ubuntu`, `COPY . /app`, `RUN npm install`.
    

***6\. Docker Registry (Docker Hub):***

* A storage for Docker Images just like GitHub.
    
* Public example: Docker Hub.
    
* You can push images to it or pull images from it.
    
    * ***Two types Of Registries :-***
        
        * **<mark>Public Registry :</mark>**
            
            * Open to everyone; anyone can pull or push images (depending on access).
                
            * Most popular example: Docker Hub.
                
            * Other public registries: GitHub Container Registry, Quay.io, etc.
                
        * **<mark>Private Registry :</mark>**
            
            * Restricted access; only authorized users can push/pull images.
                
            * Often used by companies for internal or sensitive images.
                
            * Can be self-hosted or cloud-based.
                

## ***Docker Workflow :-***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745241970039/3d2c43de-5945-484b-af5d-2b58a007fda1.png align="center")

* **Create a Dockerfile**: Developer writes a `Dockerfile` .
    
    `Dockerfile` is like a recipe for building a Docker image. It contains all dependencies & Required Softwares.
    
* Docker file Executes on Docker Engine/Daemon and gives image and these images are stored in Docker Hub or Registry.
    
* Execution of image makes Container .
    

## ***Installing Docker :-***

```bash
sudo yum install docker -y
```

***To See all Images Present in Your Local machine :***

* ```bash
    docker images
    ```
    

***To Find Out Images in Docker Hub :***

* ```bash
    docker search <image_name>
    ```
    

***To Find Image from Docker hub to Local Machine :***

* ```bash
    docker pull <image_name> 
    ```
    

***To Create and Start a container & we’ll reach inside Container :***

* ```bash
    docker run -it --name <name-u-wanna-give> <image-name>
    ```
    

***To Check service is Start or Not :***

* ```bash
    service docker status
    # Or 
    docker info
    ```
    

***To Start a Container & go inside Container :***

* ```bash
    docker start <container_name>             # To Start Container
    docker attach <container_name>            # To Go inside Container
    ```
    

***To See all containers & running Containers:***

* ```bash
    docker ps -a                         # ps --> process Status While -a is all containers
    docker ps
    ```
    

***To Stop Container :***

* ```bash
    docker stop <container_name>
    ```
    

***To Delete Container :***

* ```bash
    docker rm <container_name>
    ```
    

***To Stop & exit Container :***

* ```bash
    exit
    ```