---
title: "(Day 09) Task : Chef Methodology , Cookbook  Generation :-"
datePublished: Sun Apr 13 2025 13:14:06 GMT+0000 (Coordinated Universal Time)
cuid: cm9fo1ys5000m09k17kx2d2io
slug: day-09-task-chef-methodology-cookbook-generation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1744549971736/cf9c4f23-a5b4-4fbb-8edd-7205776d5a46.webp
tags: 90daysofdevops, tws, twsbashblazechallenge, twsbashblazechallenge-trainwithshubham, trainwithshubham-tws-devops-devopscommunity-shubhamlondhe-automation-awswithtws-7daysofaws-aws-cli, twscommunity

---

​Chef is a powerful configuration management tool in the DevOps ecosystem that automates the deployment and management of infrastructure. It uses a declarative language to define system configurations, ensuring consistency and scalability across environments.

**Chef Workstation** is the place where all the coding, testing, and configuration happens before anything is pushed to the Chef server or nodes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744546931452/a8df252d-27d1-4981-8a44-85ed4e2c86f6.jpeg align="center")

## ***What is Chef Workstation?***

* It is the local system (our laptop or desktop) where we:
    
    * Write and test Chef code (recipes/cookbooks).
        
    * Interact with the Chef server using CLI tools like `knife`.
        
    * Use tools like Test Kitchen, InSpec, and ChefSpec for testing.
        
    * Upload and manage cookbooks on the Chef server.
        

## ***Working of Chef Workstation :-***

1. **Authoring Configuration:**
    
    * You write **recipes** and **cookbooks** using Ruby DSL.
        
2. **Testing Code:**
    
    * Use **Test Kitchen** to run cookbooks in local virtual machines.
        
    * Validate code using tools like **Foodcritic** and **Cookstyle**.
        
3. **Interaction with Chef Server:**
    
    * Use `knife` to upload cookbooks, manage nodes, roles, environments, etc.
        
4. **Push to Chef Server:**
    
    * Once tested, code is pushed to the Chef server which distributes it to the nodes.
        
5. **Nodes Execute:**
    
    * Nodes pull configurations from the Chef server and apply them during the chef-client run.
        

## **What is a Chef Cookbook?**

A **Cookbook** is the **basic unit of configuration and policy distribution** in Chef.

### Why Use a Cookbook?

* To **automate** tasks like installing software, creating users, or configuring services.
    
* To keep your infrastructure **consistent** across multiple servers.
    
* To **reuse** configuration code by organizing it neatly.
    

## What's Inside a Cookbook?

A Cookbook is a **directory with a specific structure**. Each folder/file has a purpose:

### ✅ Common Components:

* `recipes/`
    
    * Contains `.rb` files written in Ruby.
        
    * Each recipe defines a series of configuration steps.
        
* `attributes/`
    
    * Stores default values (e.g., software versions).
        
    * Used inside recipes for dynamic configuration.
        
* `templates/`
    
    * Contains `.erb` files (Embedded Ruby).
        
    * Used for generating dynamic config files.
        
* `files/`
    
    * Contains static files (like images, scripts, etc.).
        
    * These are copied directly to nodes.
        
* `metadata.rb`
    
    * Describes cookbook name, version, dependencies.
        
* [`README.md`](http://README.md)
    
    * Provides human-readable documentation about the cookbook.
        
* `libraries/` (optional)
    
    * Custom Ruby code to extend Chef resources.
        
* `resources/` and `providers/` (optional)
    
    * For writing custom Chef resources (advanced usage).
        

## ***Creating Cookbook in Linux Machine :-***

1. Create a Linux Machine(Vm) using any platform (like - AWS , GCP).
    
2. Go to Google & Search www.chef.io
    
    * Go to Downloads ( `Ctrl + J` ) & Copy address url.
        
3. Open and access machine using putty
    
    * First login as ec2-user.
        
    * ```bash
        ec2-user
        sudo su
        yum update -y
        ```
        
4. Now in Vm paste url in wget section which helps in staging the chef file & use ls which shows chef package then use that package using yum install &lt;chef workstation(package)&gt; -y
    
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744548739315/1ae4907a-6dc2-4501-97a8-1b12658d4476.png align="left")
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744549173679/7a95f974-ddeb-491c-9038-c9618a7d1e52.png align="center")
        
5. Now create a directory / cookbooks(name of directory) inside which all cookbooks will be present and will be generated.
    
    * ```bash
        mkdir cookbooks                         # This is used to create a directory naming cookbooks.  
        ls                                      # This helps in showing all content present.
        cd cookbooks                            # Change directory. 
        chef generate cookbook test-cookbook    # To Generate Cookbook naming test-cookbook inside directory cookbooks
        ```
        
    
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744549650918/d0105621-94df-4596-a33f-0d48b1674fc9.png align="center")
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1744549789470/74829169-f780-4bfc-87be-c1ae9e27c2d8.png align="left")
        

## **What is a Chef Recipe :-**

* A **Recipe** is a collection of resources that describes how a part of the system should be configured.
    
* A **Recipe** is a **script written in Ruby** that tells Chef what to do on a system.
    
* Written in Ruby DSL.
    
* Example: Installing Nginx, starting a service, creating a file.
    

### Features of Recipes :

* Declarative: Define *what* the system should look like.
    
* Reusable: Can be used across multiple nodes.
    

## **Conclusion :-**

* Creation of a recipe will be described in Tomorrow’s Task .