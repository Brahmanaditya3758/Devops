---
title: "(Day 17) Task : What is CI/CD Pipeline & Jenkins :-"
datePublished: Sat May 24 2025 10:37:41 GMT+0000 (Coordinated Universal Time)
cuid: cmb23ir49000s09kzc6zw7kpx
slug: day-17-task-what-is-cicd-pipeline-and-jenkins
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1748082604404/950bdba3-466a-4472-9716-73e5bcc3f79b.jpeg
tags: jenkins, cicd, jenkins-devops, cicd-pipelines, jenkins-pipeline, twsbashblazechallenge-trainwithshubham

---

**A Quick Note Before We Dive In :**

While I’ve been actively exploring and writing about Ansible and its use in configuration management, I’ve decided to momentarily pause my Ansible series. This is not due to a lack of interest—in fact, quite the opposite. Ansible forms a critical piece of many DevOps workflows.

However, as I dive deeper into the DevOps lifecycle, I realised that understanding **CI/CD pipelines** is essential before continuing further. Since CI/CD acts as the backbone of automation in modern DevOps, grasping its concepts will provide a better context for how tools like Ansible fit into the broader picture.

So, in this post (and a few upcoming ones), I’ll be focusing on CI/CD pipelines and Jenkins what they are, how they work, and how to set one up using tools like GitHub Actions, Jenkins, and more.

Once we’ve built a solid understanding of CI/CD, I’ll return to Ansible with a more comprehensive perspective.

## Before Understanding CI/CD Concepts Let’s Discuss about History Of CI/CD :-

### **Before CI/CD (Traditional Software Development) :**

#### Manual Processes :

* Developers wrote code and stored it locally or on shared folders.
    
* Code merging was manual and painful, especially in large teams.
    
* Testing was often done after development was complete.
    
* Builds and deployments were done manually by developers or operations teams.
    
* Bugs were usually found late, during staging or even after release.
    

#### Slow and Risky Releases :

* New features were released every few months.
    
* Releases were big, complex, and stressful.
    
* If something broke in production, rollback was manual and time-consuming.
    

#### Siloed Teams :

* Developers wrote the code.
    
* Testers tested it.
    
* Ops deployed it.
    
* Teams worked in isolation (no collaboration = more errors).
    

***<mark>Note</mark>* :- पहले के टाइम जब अलग अलग डेवलपर्स कोड लिखा करते थे तब (मान के चलो के डेवलपर्स ने पचास हज़ार लाइन का कोड लिखा जो की पूरा कोड हो) और डेवलपर्स जब कोड पुश होते थे डिफरेंट डिफरेंट डेवलपर्स द्वारा Github पीआर होते थे और फिर सारे कोड्स को इंटीग्रेट किया जाता था जब एसबी कोड integrate हो जाते थे तब code टेस्टिंग की जाती थी और अगर टेस्टिंग के बाद कोई फेलियर आता था तो डेवलपर के पास फेलियर नोटिफिकेशन आटा था जिसे debug करने के लिए डेवलपर को वो लिखा हुआ कोड दोबारा सही करना पड़ता था जिसमे भूत टाइम लगता था /**

**So to save from this issue CI Comes into play.**

### **After CI/CD (Modern DevOps with Automation) :**

#### Automated Pipelines :

* Every code push triggers a CI/CD pipeline: test → build → deploy.
    
* Integration and testing happen continuously (daily or hourly).
    
* Developers get instant feedback if something breaks.
    

#### Fast and Safe Releases :

* Features can be released in **days or hours**, not months.
    
* Small changes are easier to test and fix.
    
* Rollbacks and re-deployments are automated and fast.
    

#### Collaborative Teams (Dev + Ops + QA = DevOps) :

* Developers, testers, and ops work together from the start.
    
* Everyone owns the quality and reliability of the code.
    
* Shared responsibility improves speed and stability.
    

## **CI/CD: From Basics to Advanced Concepts :-**

### **What Is CI/CD?**

CI/CD stands for:

* **CI (Continuous Integration):** Automatically integrating code changes into a shared repository several times a day, ensuring each change is tested and verified.
    
* **CD (Continuous Delivery / Continuous Deployment):** Automatically delivering or deploying code to production or staging environments after it passes all checks.
    

Together, CI/CD is the pipeline or workflow that automates the process from code commit → build → test → deploy.

### **Foundational Concepts :**

#### 1\. **Source Code Repository :**

* A central place (e.g., GitHub, GitLab, Bitbucket) where developers push their code.
    
* Common branches: `main`, `develop`, `feature/*`.
    

#### 2\. **Version Control System (VCS) :**

* Git is the most used VCS.
    
* It tracks changes, helps in collaboration, and acts as the trigger for CI.
    

### **Continuous Integration (CI) – In Depth :-**

#### 1\. **Code Commit Triggers CI :**

* Every code commit triggers an automated process.
    
* Tools like Jenkins, GitHub Actions, CircleCI, etc., detect the change and start the pipeline.
    

#### 2\. **Build Stage :**

* Converts source code into executable artifacts.
    
* Example: Java code is compiled into `.jar` files; Node.js app is bundled.
    

#### 3\. **Automated Testing :**

* **Unit tests:** Test small units (functions/classes).
    
* **Integration tests:** Test interactions between modules.
    
* **Static code analysis:** Code quality, linting, security checks (e.g., SonarQube).
    

#### 4\. **Artifact Management :**

* Build outputs are stored in artifact repositories (e.g., Nexus, Artifactory).
    
* Ensures versioning and traceability.
    

### **Continuous Delivery (CD) – Controlled Delivery :-**

#### 1\. **Staging Environment Deployment :**

* The build is deployed to a staging (pre-production) environment.
    
* Used for further testing (manual, smoke, UAT).
    

#### 2\. **Approval Workflow :**

* Manual approval or business rule to push to production.
    
* Ensures human verification before final release.
    

### **Continuous Deployment (CD) – Full Automation**

* Fully automated push to production after tests pass.
    
* No human intervention.
    
* Companies like Netflix, Facebook, and Amazon use this model.
    

#### Key Requirements:

* Extremely reliable test suite
    
* Monitoring & rollback strategies
    
* Canary or blue-green deployment
    

## CI/CD Pipeline with Key Components :-

Here’s how a modern CI/CD pipeline flows step by step:

### 1\. Version Control (Git) :

* **Tool:** GitHub, GitLab, Bitbucket
    
* **What happens:**  
    Developers push code to a shared repository (usually to branches like `main`, `develop`, or `feature/*`).
    
* ✅ **Trigger**: This push triggers the CI/CD pipeline.
    

### 2\. Build :

* **Tool:** Jenkins, GitHub Actions, GitLab CI
    
* **What happens:** The code is compiled or packaged into an executable format (e.g., `.jar`, `.zip`, `.docker image`).
    
* **Goal**: Make sure code can be built without errors.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748082573512/dad02ab8-d131-4558-9fed-12871277a941.jpeg align="center")

### 3\. Unit Test :

* **Tool:** JUnit, PyTest, Mocha, etc.
    
* **What happens:**  
    Small tests check individual functions or components.
    
* Fail here = Stop pipeline.
    

### 4\. Deploy to Staging (Pre-Prod) :

* **Tool:** Kubernetes, Docker, AWS/GCP/Azure, Ansible
    
* **What happens:** Built application is deployed to a staging environment that mimics production.
    
* Used for further testing.
    

### 5\. Automated Testing (Integration, UI, Regression) :

* **Tool:** Selenium, Cypress, Postman, TestNG.
    
* **What happens:**  
    Run more advanced tests on the staging app.  
    These test how components work together and simulate real user behavior.
    
* Failing tests stop the pipeline.
    

### 6\. Deploy to Production Environment :

* **Tool:** ArgoCD, Spinnaker, Helm, Terraform.
    
* **What happens:** If all tests pass, code is automatically or manually deployed to the production environment.
    
* Optional manual approval step for Continuous Delivery, or fully automatic for Continuous Deployment.
    

### 7\. Measure & Validate (Monitoring and Feedback) :

* **Tool:** Prometheus, Grafana, New Relic, Datadog.
    
* **What happens:**
    
    * Monitor performance, uptime, and errors.
        
    * Collect metrics like response time, CPU, memory.
        
    * Trigger alerts if anything goes wrong.
        
    * Feedback will be given at every phase.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748080794862/3e0b87d8-7e71-450c-94be-cce8c754d21b.jpeg align="center")

## What is Jenkins Automation?

Jenkins automation means using Jenkins to automatically perform tasks like:

* Building your code,
    
* Testing it,
    
* Deploying it,
    
* And even monitoring results…
    
* it’s a CI tool and it’s an open source project written in java.
    
* Runs on every System.
    
* Automate the entire SDLC.
    
* Whenever developer write Code , we integrate the code of all developers and we build , test and deploy to client This Process is called CI/CD.
    
    ...without manual intervention, every time there’s a change in the source code.
    

## How Jenkins Automation Works (Step-by-Step) :-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748082351536/66d38ec8-f1cd-4960-841d-998ebd8dd6c4.jpeg align="center")

### 1\. **Developer Pushes Code :**

* Code is pushed to a version control system like GitHub or GitLab.
    

### 2\. **Trigger Starts the Jenkins Job :**

* Jenkins is configured to watch the repo using a webhook or polling.
    
* When code changes, Jenkins automatically triggers a pipeline or job.
    

### 3\. **Jenkins Builds the Project :**

* Code is compiled or built using tools like Maven, Gradle, or npm.
    
* If the build fails, Jenkins stops the pipeline and notifies the team.
    

### 4\. **Automated Testing :**

* Runs unit, integration, or UI tests using tools like JUnit, PyTest, or Selenium.
    
* Jenkins collects test results and displays reports.
    

### 5\. **Artifacts and Packaging :**

* If tests pass, Jenkins can package the application (e.g., `.jar`, `.war`, Docker image).
    
* Artifacts are uploaded to a repository like JFrog Artifactory or Nexus.
    

### 6\. **Deployment :**

* Jenkins automatically deploys to:
    
    * A staging server for further testing
        
    * Or directly to production using tools like Ansible, Docker, Kubernetes, etc.
        

### 7\. **Notifications and Reports :**

* Jenkins sends build/test/deploy results to Slack, Email, or Dashboards.
    
* Teams stay updated without manual checks.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748081998571/ef4b0edd-e691-4525-b569-cdb576815f05.webp align="center")