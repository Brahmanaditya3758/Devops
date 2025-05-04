---
title: "(Day 13) Task : Docker Volumes & Sharing :-"
datePublished: Sun May 04 2025 04:39:40 GMT+0000 (Coordinated Universal Time)
cuid: cma95xatq000809laekipfxqy
slug: day-13-task-docker-volumes-and-sharing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746332923895/7da90d22-cc4b-4e9d-9fd4-f21bbc57c10b.png
tags: docker, devops, dockerfile, docker-compose, docker-images, devops-articles, devops-trends, devops-journey, docker-volume, devopscommunity, devops-dockervolume

---

## ***Introduction :-***

Containers are designed to be ephemeral, meaning their file systems are wiped clean when a container stops. But what if we want to persist data or share data between containers? That’s where Docker Volumes come in.

In this note, you’ll learn:

* What Docker Volumes are.
    
* How to create and use them.
    
* How to share volumes between containers.
    
* Volume types and their use cases.
    

## ***What is a Docker Volume?***

* A Docker Volume is a persistent storage mechanism managed by Docker. It stores data outside of the container's file system (outside of the union file system), making it ideal for preserving data between container runs.
    
* Think of volumes as mount points into containers, backed by a specific location on the host machine.
    
* Volume is a directory inside our container Firstly, we have to declare the directory as a volume Even if we Stop Container , Still we can access Container.
    
* Volume will be Created in one Container and then Shared to multiple Containers Anyone From Any Container can Send Data and Everyone can see it.
    
* You can declare a directory as a volume only while creating Container.
    
* You Can’t Create Volume From Existing Container , Volume won’t be included when you update an image.
    

## ***Why Use Docker Volumes?***

* **Data Persistence:** Keeps your data safe across container restarts or rebuilds.
    
* **Decoupling:** Separates data from application code.
    
* **Sharing:** Allows multiple containers to read/write to the same data.
    
* **Performance:** More efficient than using bind mounts for heavy workloads.
    
* **Backups:** Easy to backup and restore.
    

## ***Benefits Of Volume :-***

* Decoupling Container from Storage.
    
* Share Volume among Multiple Containers.
    
* Attach Volume to Container.
    
* On Deleting Container Volume Doesn’t Delete.
    

## ***Volume Mapping or Sharing :-***

### ***i) Container to Container :***

It means two or more containers access the same volume, enabling them to read/write shared data. This is commonly used for:

* Sharing configuration.
    
* Logging between services.
    
* Data exchange without external networks.
    

### ***Create Volume From DockerFile :***

* Create a Docker file using vi editor :
    

```apache
vi Dockerfile
```

* Open vi editor and make a volume inside it :
    

```apache
# After Opening Vi Editor write below codes
FROM ubuntu
VOLUME ["/MY VOLUME1"]
```

* Create Image From DockerFile :
    

```apache
docker build -t myimage .
```

* Create Container From Image and Run it :
    

```apache
docker run -it --name container1 myimage /bin/bash
```

* Share Volume with Another Container :
    

```apache
docker run -it --name container2 --privileged=true --volumes-from container1 ubuntu /bin/bash
```

* Now after creating container2 Myvolume1 is visible, whatever we do in one volume , can see from other volume :
    

```apache
touch filex filey
docker start container1
docker attach container1
cd Myvolume1
ls
exit
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746332782030/47edd77d-0644-41ba-b6f0-83c5f6d1c918.png align="center")

### ***Create Volume By Using Commands :***

* Made Container Along with Volume :
    

```apache
docker run -it --name container3 -v/volume2 ubuntu /bin/bash
ls
cd volume2
touch file1 file2
exit
```

* Now Create one more container and Share Volume 2 :
    

```apache
docker run -it --name container4 --privileged=true --volumes-from Container3 ubuntu /bin/bash
```

* Now after creating container3 volume2 is visible, whatever we do in one volume , can be seen from other volume inside another container :
    

```apache
cd volume2
ls
touch file4 file5
exit
docker start container3
docker attach container3
cd volume2
ls
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1746332823516/b8642773-80eb-496e-a18e-2490bbbe67d6.png align="center")

## ***Basic Commands For Volume Locally :***

## 1\. Docker volume create `<Volumename>.`

**Purpose**: Creates a named Docker volume.

```apache
docker volume create myvol
```

This volume is now available for use by any container.

It will be stored in Docker’s default volume directory:

## 2\. Docker volume ls :

**Purpose**: Lists all volumes managed by Docker.

```apache
docker volume ls
```

## 3\. Docker volume inspect `<Volumename>.`

**Purpose**: Shows detailed metadata about the volume.

```apache
docker volume inspect myvol
```

## 4\. Docker volume rm `<Volumename>.`

**Purpose**: Deletes a specific volume, but only if it’s not in use by a container.

```apache
docker volume rm myvol
```

## 5\. Docker volume prune :

**Purpose**: Deletes all unused volumes.

```apache
docker volume prune
```