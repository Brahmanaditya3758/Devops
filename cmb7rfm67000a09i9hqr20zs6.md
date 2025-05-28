---
title: "(Day 20) Task : Jenkins- Linked Projects, Views, User Management & Master-Slave Concept :-"
datePublished: Wed May 28 2025 09:45:57 GMT+0000 (Coordinated Universal Time)
cuid: cmb7rfm67000a09i9hqr20zs6
slug: day-20-task-jenkins-linked-projects-views-user-management-and-master-slave-concept
tags: maven, devops, jenkins, user-management, maven-job

---

## Linked Projects in Jenkins

### What Are Linked Projects?

Linked projects in Jenkins refer to jobs that are dependent or related to one another. Jenkins allows you to link multiple jobs through build triggers, post-build actions, or parameterized builds.

### Why Link Projects?

* Modularize complex workflows: Break down large pipelines into smaller jobs.
    
* Reuse jobs: A single job (e.g., testing) can be reused in multiple pipelines.
    
* Control execution order: Ensure one job starts only after another completes successfully.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748424768996/ae281ff0-d5d3-4e4d-9c98-7c003c05f2eb.png align="center")

### Ways to Link Projects

1. **Post-build Actions**:
    
    * Go to Configure &gt; Post-build Actions.
        
    * Select Build other projects.
        
    * Enter the downstream job names and define conditions like “Trigger only if build is stable”.
        
2. **Parameterized Trigger Plugin**:
    
    * Allows passing parameters from one job to another.
        
    * Enables more dynamic and flexible job linking.
        
3. **Pipeline Syntax**:
    
    ```clojure
    stage('Build') {
        steps {
            build job: 'Job-A'
        }
    }
    ```
    
4. **Upstream/Downstream Jobs View**:
    
    * Jenkins shows the relationship graphically under “Project Relationship”.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748424863832/75d5336f-941a-4803-b651-8ce5fa50b3ee.png align="center")

## Upstream and Downstream Projects in Jenkins

### What Do "Upstream" and "Downstream" Mean?

In Jenkins, Upstream and Downstream refer to the sequence of job executions and their dependencies.

* **Upstream Project**: A job that triggers another job after its execution.
    
* **Downstream Project**: A job that is triggered by another job.
    

Think of it like a chain reaction:

* Job A (upstream) triggers Job B (downstream) when it finishes.
    
* Job B can in turn be upstream for Job C, and so on.
    

### Real-World Analogy

Let’s say you're building a Java web application:

1. Job A compiles the code (Upstream).
    
2. Job B runs tests only if Job A is successful (Downstream of A).
    
3. Job C deploys the app only if Job B passes (Downstream of B).
    

Here, the full chain is:  
Job A → Job B → Job C

### How to Set Up Upstream and Downstream Jobs

#### **Using Post-build Actions (GUI)**

1. Go to Job A.
    
2. Click Configure &gt; Post-build Actions.
    
3. Choose Build other projects.
    
4. Enter Job B as the project to trigger.
    

This makes:

* Job A the upstream.
    
* Job B the downstream.
    

#### **Using the "Build Trigger" Option in Job B**

1. Go to Job B.
    
2. Click Configure &gt; Build Triggers.
    
3. Check Build after other projects are built.
    
4. Enter Job A.
    

This makes:

* Job A the upstream of Job B.
    
* Job B the downstream of Job A.
    

### Visualizing Upstream/Downstream Relationships

Jenkins offers a visual representation of project relationships:

1. Open any job (e.g., Job B).
    
2. In the left panel, click Pipeline Steps or Project Relationship (if plugins like Build Pipeline or Delivery Pipeline are installed).
    
3. You'll see arrows or a graph showing the flow between upstream and downstream jobs.
    

### Why Use This?

* **Organize build flow**: Define clear sequences.
    
* **Error handling**: Only proceed if previous steps are successful.
    
* **Improve modularity**: Separate compile, test, deploy, etc., into manageable units.
    

### Example Scenario

Let’s take a CI/CD scenario with four jobs:

* **Job 1**: Checkout & Build
    
* **Job 2**: Unit Tests
    
* **Job 3**: Integration Tests
    
* **Job 4**: Deploy to Staging
    

Relationships:

* Job 1 → Job 2 → Job 3 → Job 4
    

This means:

* Job 1 is upstream for all others.
    
* Job 4 is downstream of all others.
    
* Failures in upstream jobs will block the execution of downstream ones.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748425000836/5e14fbfe-c48e-4ecc-a040-ff634c3da3ff.png align="center")

## Views in Jenkins

### What Are Views?

Views help organize and group jobs within the Jenkins UI. This is useful when you have dozens or hundreds of jobs and need better visibility.

### Types of Views

1. **List View** (default):
    
    * Create a custom list of jobs.
        
    * Filter by name, status, or regex.
        
    * Shows job status, last build, and last success/failure.
        
2. **My View**:
    
    * Personalized view for each user.
        
    * Can include jobs triggered or configured by that user.
        
3. **Nested Views Plugin**:
    
    * Allows creating folders and sub-views within those folders.
        
4. **Dashboard View Plugin**:
    
    * Rich UI with widgets like build statistics, job health, weather reports, etc.
        

### How to Create a View

* Navigate to Jenkins Dashboard &gt; + New View.
    
* Choose the type (e.g., List View).
    
* Name the view and select jobs to include.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748425056933/73543f4e-cf38-4bea-8940-f626dde9195d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748424768996/ae281ff0-d5d3-4e4d-9c98-7c003c05f2eb.png align="center")

## Jenkins User Management

### Why User Management Matters

In multi-user environments, especially in large organizations, controlling who can do what is essential for security and traceability.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748425204651/97c3c56d-43a5-4cdf-b6c0-3a345ba2f771.png align="center")

### Enabling User Security

1. Go to Manage Jenkins &gt; Configure Global Security.
    
2. Check Enable Security.
    
3. Choose a Security Realm (e.g., Jenkins’ own database or LDAP).
    
4. Select an Authorization Strategy:
    
    * Matrix-based security: Fine-grained control.
        
    * Project-based Matrix Authorization: Define permissions per job.
        
    * Role-based Strategy Plugin: Best for enterprise-grade access control.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748425238128/767234f3-8a4a-4926-bd32-2b02bb3616dc.png align="center")

### Adding Users

* Go to Manage Jenkins &gt; Manage Users &gt; Create User.
    
* Enter username, password, full name, and email.
    

### Recommended Plugins

* Role-based Authorization Strategy
    
* Matrix Authorization Strategy
    
* LDAP Plugin (for enterprise SSO)
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748425251109/840be85c-0296-4807-8000-dcb3b386bdd7.png align="center")

## Master-Slave (Now Controller-Agent) Concept

### What Is the Master-Slave Architecture?

Jenkins uses a **controller-agent** model (previously master-slave) to distribute the workload of building, testing, and deploying projects across multiple machines.

### Jenkins Controller

* Hosts the Jenkins UI.
    
* Manages job scheduling, user management, and job configuration.
    
* Should not be overloaded with build tasks.
    

### Jenkins Agent

* Executes the build tasks.
    
* Can be physical, virtual, or cloud-based machines.
    
* Useful for running builds on different OS, hardware, or isolated environments.
    

### Why Use Agents?

* **Load distribution**: Avoid overloading the controller.
    
* **Parallel execution**: Multiple builds can run at the same time.
    
* **Environment-specific builds**: E.g., test on Linux, Windows, and macOS.
    

### How to Add an Agent

1. Go to Manage Jenkins &gt; Manage Nodes and Clouds &gt; New Node.
    
2. Enter the node name, choose Permanent Agent, and click OK.
    
3. Configure:
    
    * Remote root directory
        
    * Labels (to filter jobs)
        
    * Launch method (e.g., SSH, JNLP, or cloud provider)
        
4. Save and connect the agent.
    

### Pipeline Example Using Labels

```clojure
pipeline {
    agent { label 'linux-agent' }
    stages {
        stage('Test') {
            steps {
                sh 'pytest test_app.py'
            }
        }
    }
}
```