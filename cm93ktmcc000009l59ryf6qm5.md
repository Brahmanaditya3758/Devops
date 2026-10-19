---
title: "(Day 06) Task: Basic Git & GitHub for DevOps Engineers."
datePublished: 2025-04-05T02:10:23.964Z
cuid: cm93ktmcc000009l59ryf6qm5
slug: day-06-task-basic-git-and-github-for-devops-engineers
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1743819005759/93715624-1751-4fb6-9ae0-29405c6366ae.jpeg
tags: github, git, 90daysofdevops, version-control-systems, 90daysofdevops-chanllenge, 90daysofdevopschallenge, tws, trainwithshubham-tws-devops-devopscommunity-shubhamlondhe-automation-awswithtws-7daysofaws-aws-cli

---

Let’s learn the foundations of Git and GitHub, which are essential for DevOps work! 💻🔧

## *What is Source Code Management :-*

* It helps to store the Source code’s every version.
    
* **Source Code Management (SCM)** is a system or process used to track, manage, and control changes to source code in software development. It ensures that multiple developers can work on code together efficiently, safely, and without conflicts.
    

**In Simple Words:**

SCM helps you to:

* 🗂️ Save and organize code properly.
    
* 🔄 Track every change made to the code.
    
* 👨‍💻 Allow multiple people to work on the same project without overwriting each other’s work.
    
* ⏪ Roll back to previous versions if something breaks.
    
* SCM = VCS + collaboration + code history + branching + merging + access control.
    

## **What is Version Control?**

Version Control is a system that helps track changes to files (especially code) over time, so you can:

* ⏪ Go back to earlier versions if needed.
    
* 👨‍💻 Collaborate with a team without overwriting each other’s work.
    
* 📂 Organize and manage your project’s entire history.
    
* Version control is like a **"time machine"** for your code. It remembers every change, who made it, and when.
    

## **Types of Version Control Systems :**

There are mainly two types of SCM systems:

### **1\. Centralized Version Control System (CVCS):-**

* All code is stored on one central server.
    
* Developers pull (download) and push (upload) code to that central server.
    
* If the server goes down, no one can access or update code.
    

#### ***Examples:***

* Subversion (SVN)
    
* CVS (Concurrent Versions System).
    

#### ***Pros:***

* Simple to set up.
    
* Easy to understand for beginners.
    
* Central place for all code.
    

#### ***Cons:***

* High risk: single point of failure.
    
* Cannot work without internet or server access.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743816803877/04b5a73f-1096-446e-bb67-384bfdb7a1e4.jpeg align="center")

### **2\. Distributed Version Control System (DVCS):-**

* Every developer has a full copy of the repository, including history.
    
* Developers can work offline.
    
* Supports faster branching, merging, and better collaboration.
    

#### ***Examples:***

* Git
    
* Mercurial
    
* Bazaar
    

#### ***Pros:***

* Work offline anytime.
    
* Faster and more scalable.
    
* Safe → every user has a backup of the code.
    

#### ***Cons:***

* Slightly more complex to learn at first.
    
* Uses more local storage.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743817673209/ebbf02df-7c9a-48cd-9a3c-d7f2dbdd4ec6.jpeg align="center")

### **Why we use distributed version control over centralized version control?**

**1\. Work Offline:-**

* In DVCS, you can commit, view history, and create branches without internet access.
    
* In CVCS, you must be connected to the central server to do anything.
    

**2\. Faster Operations:-**

* DVCS is faster because most tasks happen locally (on your own computer).
    
* CVCS depends on the network/server speed, which can be slow.
    

**3\. Better Collaboration:-**

* Developers can work independently on their local copies, and merge later.
    
* Makes collaboration smoother and reduces conflicts.
    

**4\. Safer (No Single Point of Failure):-**

* In DVCS, everyone has a full backup of the repository.
    
* In CVCS, if the central server crashes, all code and history could be lost.
    

**5\. Full Change History for Everyone:-**

* In DVCS, every developer has the entire history of the project on their machine.
    
* In CVCS, the server stores the history, and you must query it every time.
    

## **What is Git?**

Git is a free and open-source version control system that helps you track changes in your code, work with a team, and manage different versions of your project easily.

Git is like a time machine for your code. It keeps track of every change, helps you undo mistakes, and lets you work with others without messing up each other’s work.

### **Why Use Git?**

* **Tracks Changes** – Keeps a full history of your code.
    
* **Undo Mistakes** – Go back to an earlier version anytime.
    
* **Team Collaboration** – Multiple people can work on the same project.
    
* **Branches** – Try new features without affecting the main project.
    
* **Merge Easily** – Combine changes from different people/branches smoothly.
    
* **Free & Open Source** – Anyone can use it.
    

### **Key Concepts in Git:**

* **Repository (Repo)**: A project folder where Git tracks everything.
    
* **Commit**: A snapshot of your changes (like a save point).
    
* **Branch**: A separate line of development (like a copy to experiment on).
    
* **Merge**: Combine changes from one branch into another.
    
* **Push**: Send your code to a remote server (e.g., GitHub).
    
* **Pull**: Get the latest code from the server.
    

## **What is GitHub?**

GitHub is a web-based platform that helps developers store, share, and collaborate on code using Git. Think of GitHub as an online home for your Git projects where you and your team can work together from anywhere.

**Why Use GitHub?**

* Makes working in teams super smooth.
    
* Work on open-source projects with developers worldwide.
    
* Manage all your code, documentation, and discussions in one place.
    
* Track progress with issues, projects, and milestones.
    
* Integrates with tools like Jenkins, Docker, Terraform, and Kubernetes.
    

**Key Features of GitHub:**

* Cloud Storage for Code – You can host your Git repositories online.
    
* Team Collaboration – Multiple people can work on the same project.
    
* Version Control – Keeps track of every change with Git.
    
* Branches & Merging – Try new features and merge them easily.
    
* Issue Tracking – Report and manage bugs or tasks.
    
* CI/CD Integration – Automate testing and deployment.
    
* Access Control – Choose who can see or edit your code (private or public).
    

### **Git vs GitHub — What’s the Difference?**

| Feature | Git 🛠️ | GitHub ☁️ |
| --- | --- | --- |
| What it is | Version control tool | Hosting & collaboration platform |
| Works on | Your computer (locally) | Online (web-based) |
| Use case | Track changes in code | Share and collaborate on code |