---
title: "Linux Shell Scripting (Day 04): Basic Linux Shell Scripting for DevOps Engineers 🚀"
datePublished: Mon Mar 31 2025 05:33:39 GMT+0000 (Coordinated Universal Time)
cuid: cm8wmvrgi000209l8610ph8kg
slug: linux-shell-scripting-day-04-basic-linux-shell-scripting-for-devops-engineers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1743399035234/dbb42cf0-060a-46f3-9b39-24b0e4286260.jpeg
tags: devops, shell-scripting, shell-script, 90daysofdevops, 90daysofdevops-chanllenge, tws, shellscripting-devops, trainwithshubham-tws-devops-devopscommunity-shubhamlondhe-automation-awswithtws-7daysofaws-aws-cli

---

Mastering shell scripting is essential for DevOps engineers to automate tasks, manage servers, and streamline workflows. Here’s a structured breakdown of today's task:

Before Understanding about shell scripting let’s discuss some aspects related to linux-based OS archi.

## Linux OS System Components & Functions:

### Hardware Layer 🖥️ :-

* Includes physical components like:
    
    * **CPU (Processor)** – Executes instructions.
        
    * **RAM (Memory)** – Stores temporary data for running programs.
        
    * **Disk (Storage: HDD/SSD)** – Stores OS files, applications, and user data.
        
    * **Network Interface Card (NIC)** – Handles communication over the internet.
        
    * I\*\*-Network Interface Card (NIC)\*\* – Handles communication over the internet.
        
* ***Note :-*** The hardware interacts with the OS only though kernel.
    
* **Linux =** Kernel (the core system software). (As mentioned in Day02)
    
* **Linux-Based OS =** Kernel + Tools + UI + Applications.
    

### Kernel Layer (Core of Linux) ⚙️ :-

* The Linux Kernel is the heart of the OS. It directly manages hardware and system resources.
    
    🛠️ **Kernel Responsibilities:**
    
    * **Process Management** – Schedules and controls running processes.
        
    * **Memory Management** – Allocates and deallocates RAM.
        
    * **File System Management** – Handles data storage and retrieval.
        
    * **Device Drivers** – Enables communication with hardware (e.g., printers, disks).
        
    * **Networking** – Manages internet connections and traffic routing.
        
    * **Security & Access Control** – Enforces user permissions.
        

### Shell & Command-Line Interface (CLI) **🖱️** :-

* The Shell is a command-line interface (CLI) that allows user to interact with OS.
    
* A shell is special user program which provide an interface to user to use operating system services. Shell accept human readable commands from user and convert them into something which kernel can understand. It is a command language interpreter that execute commands read from input devices such as keyboards or from files. The shell gets started when the user logs in or start the terminal.
    
    🛠️ **Shell Functions:**
    
    * Takes Users command and sends them to the kernel.
        
    * Supports scripting to automate tasks.
        
        * **Example Shell Commands:**
            
            * `ls` → List files.
                
            * `pwd` → Show current directory.
                
            * `echo “text”` → Print text.
                
            * `bash script.sh` → Run a shell script.
                
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1743399122995/1a14a3c0-678d-4eec-a1d6-1ed34f921bbc.jpeg align="center")

### System Libraries & Utilities 📚 :-

* These are pre-installed software components that help the OS run efficiently.
    
    🛠️ **Key Functions:**
    
    * Provides standard function for applications (I/O, memory, networking).
        
    * Acts as a bridge between user applications and the kernel.
        
    * Includes essential tools (compilers, debugging utilities, performance monitoring).
        
        * **Example Commands to Work with Libraries:**
            
            * `ldd /bin/ls` → Show shared libraries used by a command.
                
            * `dpkg -L libc6` → List files inside the libc6 package.
                

### User Applications & Services 📦 :-

* This is the topmost layer where users interact with the system via applications.
    
    🛠️ **Key Functions:**
    
    * Provides a GUI or CLI interface for users.
        
    * Manages installed software.
        
    * Allows networking, file management, and programming.
        
        * **Example Commands to Manage Applications:**
            
            * `sudo apt install <package>` → Install a package.
                
            * `rpm -q <package>` → Check if a package is installed.
                

### How These Parts Work Together? 🤝 :-

* **Example :** Running a Bash Command.
    
* **Step 1** – You type `ls -l /home/user/` in the terminal
    
* **Step 2** – The **Shell (CLI)** sends the command to the **Kernel**
    
* **Step 3** – The **Kernel** interacts with the **File System** to fetch directory details.
    
* **Step 4** –The result is displayed on your terminal screen
    

### What is Shell Scripting? :-

* A Shell Script is a file containing a series of Linux commands executed sequentially.
    
* A shell script is a computer program designed to be run by a linux shell, a command-line interpreter. The various dialects of shell scripts are considered to be scripting languages. Typical operations performed by shell scripts include file manipulation, program execution, and printing text.
    
* It helps automate repetitive tasks, manage servers, and optimize workflows.
    
* Written using shell interpreters like Bash, Zsh, Fish, etc.
    
    * 💡 **Example Use Cases:**
        
        * Automating system updates 🔄.
            
        * Managing users & permissions 👤.
            
        * Monitoring system performance 📊.
            
        * Deploying applications 🚀.
            
        * Creating backups & logs 📂.
            

### **Why Use Shell Scripting? :-**

* **Saves Time** – No need to run commands manually 🕒.
    
* **Reduces Errors** – Automates repetitive tasks 🔄.
    
* **Increases Efficiency** – Speeds up DevOps & system tasks ⚡.
    
* **Customisable** – Modify as per project needs 🛠️.
    
* **Lightweight** – No need for extra software 🎯.
    

## Day 4 Tasks :-

### ***Ques* : What is** `#!/bin/bash?` **can we write** `#!/bin/sh` **as well?**

The line `#!/bin/bash` at the beginning of a shell script is called a shebang (or "hashbang"). It tells the system which interpreter (shell) to use for executing the script.

💡 **Breaking it down:**

* `#!` → This is a special character sequence called shebang (🤷‍♂️ be professional 😂) , which tells the OS to use the specified interpreter.
    
* `/bin/bash` → This is the path to the Bash shell (Bourne Again Shell).
    

**Can we use** `#!/bin/sh` **instead? 🤔**

Yes, you can write `#!/bin/sh`, but there are differences:

| Shebang | Shell Used | Differences |
| --- | --- | --- |
| `#!/bin/bash` | Bash (Bourne Again Shell) | Supports advanced scripting features like arrays, `[[` for conditions, `==` operator, and string manipulation. |
| `#!/bin/sh` | POSIX Shell (Often a symlink to Bash, Dash, or other shell) | More portable but lacks many advanced features of Bash. |

**When to use** `#!/bin/bash` **vs** `#!/bin/sh`**?**

✅ Use `#!/bin/bash` when:

* You need Bash-specific features like arrays, string operations, or advanced condition handling.
    
* Your script is meant for Linux systems that support Bash.
    

✅ Use `#!/bin/sh` when:

* You want maximum portability (e.g., scripts for UNIX, BSD, macOS, and Linux).
    
* You don’t need Bash-specific features.
    

### ***Ques* :** Write a Shell Script which prints I will complete #90DaysOofDevOps challenge?

Here's a simple shell script that prints your DevOps goal! 💪

#### **Steps to Create & Run the Script:**

* **Step 1 :-**
    
    * Create the script file:
        
        vim devops\_challenge.sh
        
        ```bash
        vim devops_challenge.sh
        ```
        
* **Step 2 :-**
    
    * Add the following script inside the vim editor opened after Step 1 :
        
        ```bash
        #!/bin/bash
        echo "I will complete #90DaysOfDevOps challenge 💪🔥"
        ```
        
* **Step 3 :-**
    
    * Save & exit `(CTRL + X`, `then Y`, then `ENTER`)
        
* **Step 4 :-**
    
    * Give execution permission:
        
    * as we know there are three options ( `r → read` , `w→ write` , `x → execute`)
        
        ```bash
        chmod +x devops_challenge.sh
        ```
        
* **Step 5 :-**
    
    * Run the script:
        
        ```bash
        ./devops_challenge.sh
        ```
        
* **Output :-**
    
    * ```bash
        echo "I will complete #90DaysOfDevOps challenge 💪🔥"
        ```