---
title: "(Day 16) Task : What is Ansible? A Complete Beginner's Guide :-"
datePublished: Tue May 20 2025 02:46:26 GMT+0000 (Coordinated Universal Time)
cuid: cmavwxbbn000209jufssudifd
slug: day-16-task-what-is-ansible-a-complete-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1747708764047/4c8ac4ce-aafb-4bb0-9428-dee19b6a68dc.png
tags: ansible, devops, devops-articles, devops-journey, ansible-playbook, devopscommunity

---

## Introduction

In today’s world of DevOps and automation, tools like Ansible play a major role in helping system administrators and developers manage and configure multiple systems easily and efficiently.

Ansible is a free, open-source, IT automation tool that helps you automate software provisioning, configuration management, and application deployment.

In simple terms:

Ansible helps you control many computers from one place, without needing to log in to each one separately.

## What Does Ansible Do?

Ansible allows you to:

* Automatically install software on multiple servers.
    
* Update, configure, or remove software.
    
* Start or stop services.
    
* Deploy applications.
    
* Manage servers and their settings.
    

It uses SSH (Secure Shell) to connect to remote systems and doesn’t require a separate agent to be installed on those machines.

## History of Ansible

* **Founded:** Ansible was created by Michael DeHaan in 2012.
    
* **Initial Release:** 2012
    
* **Written in:** Python
    
* **Acquired by:** Red Hat in 2015, which itself was later acquired by IBM in 2019.
    
* Today, Ansible is a part of **Red Hat Ansible Automation Platform**.
    

The idea was to build a tool that is simple, powerful, and doesn’t require a lot of overhead or setup time.

## How Does Ansible Work?

Ansible works by:

1. Connecting to your remote machines via SSH.
    
2. Running commands or scripts written in YAML (called Playbooks).
    
3. Applying tasks in an idempotent way (it won't make unnecessary changes if things are already set correctly).
    
4. It is agentless, which means you don’t need to install any software on the remote systems.
    

## Advantages of Ansible

Here are the major benefits of using Ansible:

### Easy to Learn and Use :

* Uses YAML syntax (simple, human-readable).
    
* Great for beginners and non-programmers.
    

### Agentless Architecture :

* No need to install anything on the target systems.
    
* Uses SSH, which is already available on most systems.
    

### Open Source and Free :

* Anyone can use it without any licensing cost.
    
* Supported by a large open-source community.
    

### Idempotent :

* Running the same playbook multiple times won't break anything.
    
* It only makes changes if needed.
    

### Scalable :

* Can manage dozens, hundreds, or even thousands of machines.
    

### Cross-Platform Support :

* Works on Linux, macOS, and Windows (with limitations).
    

### Large Community and Modules :

* Thousands of pre-built modules are available.
    
* Strong support from Red Hat and the open-source community.
    

## Disadvantages of Ansible

Though Ansible is powerful, it has some limitations:

### Performance for Large Setups :

* Might be slower than agent-based tools (like Chef or Puppet) in very large environments.
    

### Limited Windows Support :

* Ansible works best with Linux; Windows support exists but is more complex.
    

### No GUI (by default) :

* The command-line interface is powerful but not always user-friendly for beginners.
    
* GUI available via **AWX** or **Ansible Tower**, but these may require setup and licensing.
    

### Steep Learning Curve for Complex Tasks :

* Simple tasks are easy.
    
* Complex logic (like error handling, loops, conditionals) requires deeper YAML knowledge.
    

## Use Cases of Ansible

Here’s where Ansible is commonly used:

* Server setup and provisioning
    
* Cloud infrastructure management (AWS, Azure, GCP)
    
* Application deployment (like Docker containers)
    
* Network automation (routers, switches)
    
* Security patches and compliance
    
* CI/CD pipelines integration
    

## Key Components of Ansible

| Component | Description |
| --- | --- |
| **Inventory** | List of servers Ansible will manage (usually in a `.ini` or `.yaml` file) |
| **Playbook** | YAML file containing instructions for tasks to perform |
| **Module** | A unit of work (e.g., copy a file, install software) |
| **Role** | A way to organize play books and make them reusable |
| **Task** | A single action in a playbook |

## Example: A Simple Ansible Playbook

```bash
- name: Install Nginx on Ubuntu
  hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```

This playbook installs **Nginx** on all servers in the **webservers** group.

## Why Use Ansible in DevOps?

Ansible is a great choice for DevOps because:

* It automates repetitive tasks, reducing human error.
    
* It works well with CI/CD tools like Jenkins or GitLab CI.
    
* It helps ensure consistent configuration across all environments.
    
* It speeds up infrastructure deployment.
    

## Ansible vs Other Tools

| Feature | Ansible | Chef | Puppet |
| --- | --- | --- | --- |
| Agentless | Yes | No | No |
| Language | YAML | Ruby | DSL |
| Learning Curve | Easy | Medium | Medium |
| OS Support | Linux, some Windows | Linux | Linux |
| GUI Support | Tower/AWX | Yes | Yes |

# Ansible vs Chef – A Simple Comparison

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1747708998326/3666f3f7-c6d8-4f51-8fd7-4280b0a21bf2.jpeg align="center")

| Feature | **Ansible** | **Chef** |
| --- | --- | --- |
| **Developed By** | Michael DeHaan (Red Hat) | Opscode (now Progress Chef) |
| **First Released** | 2012 | 2009 |
| **Language Used** | YAML (Ansible Playbook) | Ruby DSL (Domain-Specific Language) |
| **Agentless** | Yes | No (requires agent `chef-client`) |
| **Installation** | Only on control node | Requires Chef Server, Chef Client, Workstation |
| **Ease of Use** | Very simple (suitable for beginners) | More complex (needs Ruby knowledge) |
| **Configuration Style** | Declarative | Declarative + Procedural |
| **Architecture** | Agentless, uses SSH | Requires a Chef Server and agents |
| **Community Support** | Strong open-source and Red Hat support | Strong, but fewer users than Ansible |
| **Push vs Pull Model** | **Push** (control node pushes configs) | **Pull** (nodes pull from Chef server) |
| **GUI** | AWX/Tower (optional) | Chef Manage (optional) |
| **Use Case** | Quick automation, simple deployments | Large, complex infrastructures |