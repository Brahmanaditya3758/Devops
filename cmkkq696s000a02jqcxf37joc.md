---
title: "(Day 45) Task : Backend Setup with Node.js, Express & First Working Server :-"
datePublished: Mon Jan 19 2026 05:29:02 GMT+0000 (Coordinated Universal Time)
cuid: cmkkq696s000a02jqcxf37joc
slug: day-45-task-backend-setup-with-nodejs-express-and-first-working-server
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1768800406427/d785095c-696b-48e3-9702-ed10ca6ee764.png
tags: projects, backend, devops, full-stack-development, blogger, 2articles1week

---

## Introduction :

After completing Day 1 (planning, project structure, and Git setup), Day 2 was the day where real development started.

The goal of Day 2 was simple but very important:  
Set up the backend using Node.js and Express and run the first working server.

This step creates the foundation of the entire system because all future features like authentication, events, and resources will be built on top of this backend.

## Why Backend Setup Is Important?

The backend is responsible for:

* Handling requests from frontend.
    
* Connecting to the database.
    
* Applying business logic.
    
* Securing the system.
    
* Managing users, events, and resources
    
    ❝ <mark>If the backend foundation is weak, the whole project becomes unstable.</mark> ❞
    

So Day 2 was focused on building a clean and scalable backend base.

## Tech Used on Day 2 :-

* **Node.js** – JavaScript runtime.
    
* **Express.js** – Backend framework.
    
* **MongoDB** – (Will be connected in upcoming days).
    
* **Cors & Dotenv** – For configuration and security.
    

## Backend Folder Structure

Inside the main project folder:

```bash
college-event-system/
 ├── backend/
 └── frontend/
```

Inside `backend/` after Day 2:

```nginx
backend/
 ├── models/
 ├── routes/
 ├── controllers/
 ├── middleware/
 ├── config/
 ├── server.js
 └── package.json
```

This structure is industry-style and helps in keeping the project clean and scalable.

## Step 1: Initializing Node Project :-

Inside the `backend` folder, I initialized a Node.js project:

```nginx
npm init -y
```

This created the `package.json` file, which manages:

* Project info
    
* Dependencies
    
* Scripts
    

## Step 2: Installing Required Packages :-

I installed the core dependencies:

```bash
npm install express mongoose cors dotenv
```

* **express** → For creating APIs.
    
* **mongoose** → For MongoDB (used later).
    
* **cors** → For frontend-backend communication.
    
* **dotenv** → For environment variables.
    

## Step 3: Creating Proper Folder Structure :-

I created these folders:

* `models/` → For database schemas.
    
* `routes/` → For API routes.
    
* `controllers/` → For logic.
    
* `middleware/` → For auth & security.
    
* `config/` → For DB and config files.
    

And a main file:

* `server.js` → Entry point of backend.
    

## Step 4: Creating First Express Server :-

In `server.js`, I created a basic Express server with:

* JSON middleware
    
* CORS enabled
    
* A test route
    
* Server listening on port 5000
    

## Step 5: Running and Testing the Server

I started the server using:

```nginx
node server.js
```

And tested it in browser:

```nginx
http://localhost:5000
```

When I saw the message:

“<mark>Backend is running</mark> ”

That confirmed:

***Server is working.  
Express setup is correct.  
Project is ready for real APIs.***

## Why This Setup Matters for Future?

This backend base will now be used to:

* Build authentication system.
    
* Create event APIs.
    
* Create resource APIs.
    
* Add role-based access.
    
* Connect MongoDB.
    
* Secure the application.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799279920/7e312392-ed18-45a8-833c-5882f51ac212.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799308911/7594ce3f-9449-49cd-82a3-2b4145c9e60d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799404215/aba4fdab-b983-41e0-a221-aa084a93c0cd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799352222/d756ef50-4abb-4a13-a33a-7207038d3f85.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799546402/bd3fc23a-471f-4658-94af-bdaf20eb745e.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768799556995/2862bbdd-7fe1-4cd3-87ad-ff784fac24b6.png align="center")

## ***Why I Started This Project With Backend Instead of Frontend :-***

In most student projects, people usually start by designing the UI first. But for this College Event & Resource Management System, I intentionally decided to start with the backend.The backend is the foundation of any real-world application. It is responsible for handling authentication, database operations, business logic, security, and API communication. If the backend architecture is weak, the entire project becomes unstable, no matter how good the frontend looks.

Since this project is meant to be a production-level and placement-oriented project, I wanted to first:

* Design a scalable backend structure
    
* Setup proper API architecture
    
* Prepare database models and authentication flow
    
* Ensure that the system can handle real data and real users
    

Another important reason is that the frontend depends completely on backend APIs. If APIs are not ready, frontend developers usually end up writing fake data (dummy JSON), which later causes rework.

By building the backend first:

* My frontend will be clean and API-driven
    
* There will be no rework
    
* The project structure will be more professional
    
* Deployment and DevOps integration will be much easier later
    

This is exactly how real companies build software: backend and architecture first, UI second.

## Problems I Faced While Setting Up Backend (Day 2) **:-**

While setting up the backend, I faced multiple real-world issues which actually taught me more than a smooth setup would have.

### Problem 1: <mark>Express 5 Causing 403 Forbidden Error</mark>

initially, I installed the latest version of Express (v5). Even though my server was running, when I tried to open [`http://localhost:5000`](http://localhost:5000), the browser kept showing:

* “Access Denied – HTTP 403”
    
* This was confusing because the code was correct. After debugging, I learned that:
    
    * Express 5 is still experimental
        
    * It has stricter default security rules
        
    * It blocks requests unless configured properly
        

**<mark>Solution:</mark>** I downgraded Express to the stable version (Express 4), which is used in production by most companies.

### Problem 2: Port 5000 Already in Use Error

After fixing Express, I got another error:

* `EADDRINUSE: address already in use :::5000`
    

This happened because my old server process was still running in the background.

**<mark>Solution:</mark>**

* I learned how to find running processes using the port.
    
* Killed the old process.
    
* Restarted the server properly.
    

### What I Learned From These Problems

These issues taught me:

* How real backend debugging works.
    
* How version mismatches create real production bugs.
    
* How to debug ports, processes, and Node.js configuration.
    
* That building backend is not just coding — it’s also system-level problem solving
    

## SAVE DAY 2 WORK (Git) :-

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1768800382685/a50d3fc5-d185-4791-aa9a-44012e6675c5.png align="center")