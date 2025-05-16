---
title: "(Day 15) Task : Push & pull Docker Image to DockerHub :-"
datePublished: Fri May 16 2025 03:57:17 GMT+0000 (Coordinated Universal Time)
cuid: cmaq9p0qy000009js0lds40fy
slug: day-15-task-push-and-pull-docker-image-to-dockerhub
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1747315292748/b1dea2ba-4f54-4562-8587-6c103542b7e1.webp
tags: devops, devops-articles, devops-trends, devops-journey, devopscommunity, devops-docker-containerization-softwaredevelopment-dockerimage-dockercontainer-dockerhub-dockerfile-dockercompose-dockercommands-containerization101-dockertutorial-containerorchestration-dockerworld-softwarecontainers-dockerization-dockerapp-dockerdevelopment-containermanagement-dockerforbeginners-dockerexplained-containertechnology-dockerbestpractices-dockercommunity-devopstools-containerlifecycle-dockerecosystem

---

Pushing a Docker image to Docker Hub is a fundamental skill for developers and DevOps professionals. It enables seamless sharing and deployment of containerized applications. This comprehensive guide will walk you through each step, ensuring you can confidently push your Docker images to Docker Hub.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747315250542/4747417a-6cc9-4ddc-841c-0b8e28b28342.jpeg align="center")

A primary reason for using Docker is to eliminate the "it works on my machine" problem. By encapsulating an application and its environment into a container, Docker ensures that the application behaves the same way, regardless of where it's deployed—be it a developer's laptop, a testing server, or a production environment.

Docker offers several benefits:

* **Simplified Setup**: Developers can set up applications quickly without worrying about environment configurations.
    
* **Efficient Resource Usage**: Containers are lightweight and use system resources more efficiently than traditional virtual machines.
    
* **Scalability**: Applications can be scaled up or down easily by adding or removing containers.
    
* **Rapid Deployment**: With Docker, deploying applications becomes faster and more straightforward.
    

Now as we want to gain a hands on experience of pushing and pulling to and from docker hub we can make two instances let one is from mumbai region and another is from tokyo region.

## **Step-by-Step Guide to Pushing a Docker Image to Docker Hub (From Mumbai region) :-**

### 1\. **Go to AWS & Select amazon linux machine(HVM) having Docker :**

Before you begin, ensure Docker is installed on your system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747364085548/1591adad-4bf3-40ba-9422-66944e7f36d0.png align="center")

### 2\. Connect to SSH and Start instance :

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747364266316/20b9fd62-78a8-4407-a16b-4927b143dc58.png align="center")

### 3\. **Use commands inside instances CLI :**

* ### `sudo su`
    
    **Purpose**: This command elevates your current user session to the superuser (root) level.
    
    **Explanation**: `sudo` allows a permitted user to execute a command as the superuser or another user. `su` stands for "substitute user" and is used to switch to another user account. When combined as `sudo su`, it grants you root access, allowing you to perform administrative tasks without repeatedly prefixing commands with `sudo`.
    
* ### `yum update -y`
    
    **Purpose**: Updates all packages on the system to their latest versions.
    
    **Explanation**: `yum` is the package manager for RPM-based distributions like CentOS and RHEL. The `update` command refreshes the system's package database and upgrades all installed packages. The `-y` flag automatically answers "yes" to any prompts, allowing the update process to proceed without manual intervention.
    
* ### `yum install docker -y`
    
    **Purpose**: Installs Docker on your system.
    
    **Explanation**: This command uses `yum` to download and install the Docker package and its dependencies. The `-y` flag, as before, automates the installation by confirming prompts. It's worth noting that for newer versions or specific distributions, you might need to set up Docker's official repository before installation.
    
* ### `service docker start`
    
    **Purpose**: Starts the Docker daemon (the background service).
    
    **Explanation**: This command initiates the Docker service, enabling you to run Docker containers. On systems using `systemd`, you might use `systemctl start docker` instead.
    
* ### `docker run -it ubuntu /bin/bash`
    
    **Purpose**: Launches a new Docker container based on the Ubuntu image and provides interactive shell access.
    
    **Explanation**:
    
    * `docker run`: Creates and starts a new container.
        
    * `-it`: Combines `-i` (interactive) and `-t` (allocates a pseudo-TTY), allowing you to interact with the container via the terminal.
        
    * `ubuntu`: Specifies the Docker image to use. If not present locally, Docker will pull it from Docker Hub.
        
    * `/bin/bash`: The command executed inside the container, launching the Bash shell.
        
    
* `Create New Files`: This will create them as empty files.
    
    ```bash
    touch file1 file2 file3
    ```
    
* `Create Container`: This is used to create container of an image.
    
    ```bash
    docker commit <container-name> <image-name>
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747365130748/ced82483-4508-439f-84e8-ce162d7c09af.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747365156005/e20c6938-457e-4a2e-a3fd-01501ba71892.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747365223176/5823b940-cae4-4ff3-a36c-a1d02df409f2.png align="center")

### **Now Create Account in** [***Docker-hub.com***](https://hub.docker.com/) ***:-***

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747366849235/dcdc9fd9-3142-4f5e-9ec5-a84994645375.png align="center")

### **Now, Inside CLI :-**

* **Docker Login :**
    
    ```bash
    docker login    # Enables Connection between EC2 & DockerHub.
    username :      
    # It'll ask about your dockerhub usename & Password.
    Password :
    ```
    
* **Give Tag to Your Image :**
    
    ```bash
    docker tag <image-name> dockerid/newimage 
    # dockerid --> Use Your username help a lot in long run.
    # newimage --> Image name you want to save it on docker hub.
    ```
    
* **Push image to docker :**
    
    ```bash
    docker push dockerid/newimage
    
    # Now we'll be able to see this image in docker hub
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747366902073/bca959c6-a9f6-4437-9e6f-f70f9398ad25.png align="center")

## **Step-by-Step Guide to Pulling a Docker Image to Docker Hub ( From Tokyo region) :-**

* ### **Use commands inside instances CLI :**
    
    * ### `sudo su`
        
        **Purpose**: This command elevates your current user session to the superuser (root) level.
        
        **Explanation**: `sudo` allows a permitted user to execute a command as the superuser or another user. `su` stands for "substitute user" and is used to switch to another user account. When combined as `sudo su`, it grants you root access, allowing you to perform administrative tasks without repeatedly prefixing commands with `sudo`.
        
    * ### `yum update -y`
        
        **Purpose**: Updates all packages on the system to their latest versions.
        
        **Explanation**: `yum` is the package manager for RPM-based distributions like CentOS and RHEL. The `update` command refreshes the system's package database and upgrades all installed packages. The `-y` flag automatically answers "yes" to any prompts, allowing the update process to proceed without manual intervention.
        
    * ### `yum install docker -y`
        
        **Purpose**: Installs Docker on your system.
        
        **Explanation**: This command uses `yum` to download and install the Docker package and its dependencies. The `-y` flag, as before, automates the installation by confirming prompts. It's worth noting that for newer versions or specific distributions, you might need to set up Docker's official repository before installation.
        
    * ### `service docker start`
        
        **Purpose**: Starts the Docker daemon (the background service).
        
        **Explanation**: This command initiates the Docker service, enabling you to run Docker containers. On systems using `systemd`, you might use `systemctl start docker` instead.
        
    * ### `docker run -it ubuntu /bin/bash`
        
        **Purpose**: Launches a new Docker container based on the Ubuntu image and provides interactive shell access.
        
        **Explanation**:
        
        * `docker run`: Creates and starts a new container.
            
        * `-it`: Combines `-i` (interactive) and `-t` (allocates a pseudo-TTY), allowing you to interact with the container via the terminal.
            
        * `ubuntu`: Specifies the Docker image to use. If not present locally, Docker will pull it from Docker Hub.
            
        * `/bin/bash`: The command executed inside the container, launching the Bash shell.
            
        
    * `Pull` :
        
        * The `docker pull` command is essential in Docker for downloading container images from registries like Docker Hub to your local system. These images serve as templates for creating containers, enabling consistent application deployment across various environments.
            
            ```bash
            docker pull dockerid/newimage 
            # Helps to pull image from dockerhub.
            
            docker run -it --name <container-name> dockerid/newimage /bin/bash 
            # Container made from pulling image from dockerhub.
            ```
            
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747365234644/f9525067-9de4-43ef-9f4f-7ba45ffc7dd3.png align="center")