---
title: "(Day 14) Task : Docker Port Expose :-"
datePublished: Sat May 10 2025 05:25:50 GMT+0000 (Coordinated Universal Time)
cuid: cmahs7s2e001e09juh2ixgsir
slug: day-14-task-docker-port-expose
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746852219876/d1f7650c-20eb-45a7-a932-41a3f52be77b.png
tags: docker, devops, devops-articles, devops-journey, docker-port, tws, devopscommunity, twsbashblazechallenge, twsbashblazechallenge-trainwithshubham, twscommunity, docker-port-mapping

---

* Exposing and publishing ports in Docker is essential for enabling communication between your containerized applications and the external world. This guide breaks down the concepts and provides simple, step-by-step instructions to help you manage Docker ports effectively.
    
* In this session we’ll learn how will the user access the content of a container through internet.
    
* Container doesn’t have any ip and to access anything using internet ip is required.
    
* Internet Access Port : 80.
    
* Since , there is a mapping b/w ec2 instance & container when a user runs that instance using public ip in its device remotely then it’ll go to the ec2 instance & since mapping is there user will be able to see content of container that may be website or anything.
    

## Understanding Docker Ports

* **EXPOSE**: A Dockerfile instruction that documents which ports the container listens on. It doesn't publish the port to the host machine.
    
* **\-p / --publish**: A Docker run command option that maps a container's port to a port on the host, making it accessible externally.
    
* **\-P / --publish-all**: Automatically maps all exposed ports to random ports on the host.
    

## How to Expose and Publish Ports

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853391608/9962e16b-22a9-4e41-8f45-a8df9ece19e3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853408065/024318d2-5dd6-462c-8135-87d5046f4db7.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853431320/5b0e3253-4915-42a2-8a7b-1c26d353ddfe.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853473966/8c9f189c-3f8d-4827-87fd-073b4fc77fb2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853486411/dc4f064e-1f23-488a-898e-f19190b8c44d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853503730/2bb85fa8-f9b5-409b-93b3-86e1d02bcb22.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746853652876/c3378bbf-4226-4d6b-b9d5-91768f340ad7.png align="center")

## ***Difference between docker attach and docker exec :-***

## `Docker attach` **:**

* **Functionality**: Attaches your terminal to the container's main process, allowing you to interact as if you were directly connected to it.
    
* **Behavior**:
    
    * Input and output are shared; what you type is sent to the container, and its output is displayed in your terminal.
        
    * If the container was started with `-i` (interactive), your inputs are directed to its stdin.
        
    * Exiting the attach session (e.g., via `CTRL+C`) can stop the container if the main process terminates as a result.
        

### `Docker exec` **:**

* **Functionality**: Executes a new command in a running container, creating a separate process from the main application.
    
* **Behavior**:
    
    * Each exec session is independent, allowing multiple concurrent sessions.
        
    * Does not interfere with the main process; exiting the exec session leaves the container running.
        
    * Supports running commands as different users within the container.
        

## ***Difference between Expose and Publish(-p) :-***

Basically we have Four Options:

1. ### Neither Specify Expose nor publish(-p) :
    
    The service in the container will only be accessible from inside container only.
    
2. ### Only Specify Expose :
    
    In this , Service won’t be accessible from outside Docker , but from inside other Docker Containers Can.
    
3. ### Specify Expose and publish(-p):
    
    The Service can be accessible from anywhere , even outside Docker.
    
4. ### Only Specify publish(-p) :
    
    Docker does an implicit expose. This is because , if a port is open to public , it’s also open to other docker containers hence , -p includes expose.