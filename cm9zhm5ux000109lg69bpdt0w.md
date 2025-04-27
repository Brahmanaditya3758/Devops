---
title: "(Day 12) Task : Dockerfile Components & different commands :-"
datePublished: Sun Apr 27 2025 10:09:14 GMT+0000 (Coordinated Universal Time)
cuid: cm9zhm5ux000109lg69bpdt0w
slug: day-12-task-dockerfile-components-and-different-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745748435583/756d651a-f6f0-44b9-aa82-f52ab06f6c81.png
tags: docker, devops, docker-images, devops-articles, devops-journey, 90daysofdevops, 90daysofdevops-chanllenge, 90daysofdevops-devops-projectdevelopment-nonitbackground-github-docker-cloudplatforms-ec2-aws-elasticbeanstalk-lambdafunctions-devopspipelines-terraform-jenkins-docker-devsecops-scm-git-gitlab-bitbucket-buildtools-griddle-maven-ant-msbuild-monitoringtools-prometheus-grafana-ansible-ai-chatgpt-valueaddition-realworldproblems, 90daysofdevopschallenge, tws, devopscommunity, twsbashblazechallenge, twsbashblazechallenge-trainwithshubham, twscommunity

---

## What is a Dockerfile?

* A Dockerfile is a simple text file with a set of instructions to build a Docker image.
    
* It defines what goes inside the Docker image — like OS, libraries, dependencies, app code, environment variables, etc.
    

## ***Components of a Dockerfile :-***

Here are the most commonly used instructions:

### 1\. **FROM :**

* **Purpose**: Specifies the base image you want to use.
    
* **Example**:
    
    ```bash
    FROM ubuntu
    ```
    
* **Notes**: Every Dockerfile must start with a `FROM` instruction.
    

### 2\. **MAINTAINER** :

* **Purpose**: Specifies the author , Owner or maintainer of the image.
    
* **Example**:
    
    ```bash
    MAINTAINER ="yourname@example.com"
    ```
    

### 3\. **RUN :**

* **Purpose**: Executes commands inside the container during image building.
    
* **Example**:
    
    ```bash
    RUN echo "Aditya Sharma" >/tmp/testfile
    ```
    
* **Notes**:
    
    * Each `RUN` creates a new image layer.
        
    * Best to combine commands with `&&` to reduce layers.
        

### 4\. **CMD :**

* **Purpose**: Executes Commands but during Container Creation.
    
* **Example**:
    
    ```bash
    CMD ["echo", "Hello World"]
    ```
    
* **Notes**:
    
    * Only the last CMD in the Dockerfile is used.
        
    * Overridable at runtime (`docker run`).
        

### 5\. **ENTRYPOINT :**

* **Purpose**: Similar to `CMD`, but fixed — can't easily be overridden this has higher priority over CMD , first commands will be executed by entry point only.
    
* **Example**:
    
    ```bash
    ENTRYPOINT ["python3", "app.py"]
    ```
    

### 6\. **COPY :**

* **Purpose**: Copy files/folders from local machine into the image we need to provide source & destination.
    
* **Example**:
    
    ```bash
    COPY ./app /usr/src/app
    ```
    

### 7\. **ADD :**

* **Purpose**: Like `COPY`, but also:
    
    * Can unzip archives automatically.
        
    * Can download from URL (but not recommended — better to use `curl`/`wget`).
        
* **Example**:
    
    ```bash
    ADD app.zip /usr/src/app
    ```
    

### 8\. **WORKDIR :**

* **Purpose**: Sets the working directory for a container instead of going to container.
    
* **Example**:
    
    ```bash
    WORKDIR /usr/src/app
    ```
    
* **Notes**:
    
    * No need to use `cd` in `RUN` — just set `WORKDIR`.
        

### 9\. **EXPOSE :**

* **Purpose**: Tells Docker the port the container will listen on.
    
* **Example**:
    
    ```bash
    EXPOSE 8080
    ```
    
    **Notes**:
    
    * This does not publish the port automatically, it’s just documentation.
        

### 10\. **ENV :**

* **Purpose**: Set environment variables inside the container.
    
* **Example**:
    
    ```bash
    ENV=production
    ```
    

## ***Important Docker Commands :-***

Now let's look at important Docker commands you must know:

### ***Working with Containers***

* **Run Container :**
    
    ```bash
    docker run -it --name <container-name> <image-name> /bin/bash
    ```
    
    * `-d` runs in detached (background) mode.
        
    * `-p` maps host port to container port.
        

* **List running Containers :**
    
    ```bash
    docker ps
    ```
    

* **List All Containers (running + stopped) :**
    
    ```bash
    docker ps -a
    ```
    

* **Stop Container :**
    
    ```bash
    docker stop <container-name>
    ```
    

* **Remove Container :**
    
    ```bash
    docker rm <container-name>
    ```
    

* **View Container Logs :**
    
    ```bash
    docker logs <container-name>
    ```
    

* **Execute Command Inside Running Container :**
    
    ```python
    docker exec -it <container-name>
    ```
    
    * `-it` makes it interactive.
        
    * `bash` opens shell inside container.
        

## ***Steps to Create a Container Using a Dockerfile :-***

1. **Create the Dockerfile :**
    

* * Using `touch` command OR using a text editor (like `vim`, `nano`, or Visual Studio Code).
        
        ```bash
        touch Dockerfile
            # OR
        vim Dockerfile
            # OR
        nano Dockerfile
        ```
        

2\. **Add Instructions to Dockerfile :**

* Open the Dockerfile in your editor.
    
* Add basic instructions.
    
    * **For example:**
        
        ```bash
        FROM ubuntu
        RUN echo "Aditya sharma" >/tmp/testfile
        ```
        

3. **Create an Image from a Dockerfile :**
    
    **What Does "Build an Image" Mean?**
    
    * Building an image means converting your Dockerfile into a ready-to-run Docker image.
        
    * Docker reads each instruction in the Dockerfile (like FROM, RUN, COPY) and creates an image layer step-by-step.
        
    * The final result is a custom image you can use to run containers.
        
    
    ### **Use the** `docker build` Command :
    
    * Command Syntax:
        
        ```bash
        docker build -t <image-name> .
        docker ps -a                          # To check all containers status
        ```
        
        * `docker build`: Command to build an image.
            
        * `-t`: Tag the image with a name and optionally a version/tag.
            
        * `.` (dot): Current directory (Docker will search for Dockerfile here).
            
4. **Create and Run a Container from an Image :**
    
    ### **Use the** `docker run` Command
    
    * Command syntax:
        
        ```bash
        docker run -it --name <container-name> <image-name> /bin/bash 
        cat /tmp/testfile
        ```
        
    * **Note :** In same docker file if instructions changes then new images can be formed easily.
        

## **Codes :**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745748034789/2f7d7124-466c-4ce0-ae04-7101f3b53c8e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745748074474/6ce0e036-fbf7-4630-947f-83e9d4dd0adf.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745748099559/7d8f9e46-067b-4093-93e6-a532495add7c.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745748258117/cb4321d1-f1e0-4716-bebd-fc9f21440420.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745748269950/9f402092-ee3e-452b-9906-cc9abfc621c2.png align="center")