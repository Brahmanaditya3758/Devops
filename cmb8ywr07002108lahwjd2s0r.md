---
title: "(Day 21) Task : How to Install Jenkins on Ubuntu (with Commands) :-"
datePublished: Thu May 29 2025 06:03:00 GMT+0000 (Coordinated Universal Time)
cuid: cmb8ywr07002108lahwjd2s0r
slug: day-21-task-how-to-install-jenkins-on-ubuntu-with-commands
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1748498514823/4bf30192-ca1e-4a4d-a4a9-b233844d5277.png
tags: jenkins, jenkins-devops, jenkins-ubuntu-linux-devops-cicd-automation-softwaredevelopment-sysadmin-devopsengineer-techblog-linkedinpost

---

## System Requirements & Prerequisites

Before we begin, make sure:

| Requirement | Description |
| --- | --- |
| OS | Ubuntu 20.04+ (or any supported LTS version) |
| User Access | Root user or a user with sudo privileges |
| Internet | Required to download Jenkins packages |
| Shell | Terminal/SSH access to the Ubuntu system |

## Step-by-Step Jenkins Installation on Ubuntu

### Step 1: Update the System :

We begin by updating the system package index to ensure all existing packages are up to date.

```clojure
sudo apt update
sudo apt upgrade -y
```

This command does two things:

* `apt update`: Updates the list of available packages.
    
* `apt upgrade`: Upgrades the existing installed packages.
    

### Step 2: Install Java :

Jenkins runs on Java, so it must be installed first.

```clojure
sudo apt install openjdk-11-jdk -y
```

You can verify the installation using:

```clojure
java -version
```

### Step 3: Add Jenkins Repository Key :

Jenkins packages are hosted on its official repository. First, we need to add the GPG key used to sign the packages:

```clojure
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
```

### Step 4: Add Jenkins Repository :

Now, add the official Jenkins repository to your system:

```clojure
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

Then update your system again:

```clojure
sudo apt update
```

This makes Jenkins packages available through `apt`.

### Step 5: Install Jenkins :

Now that we’ve added the repo, we can install Jenkins:

```clojure
sudo apt install jenkins -y
```

This installs the Jenkins service and related files.

### Step 6: Check Status Of Jenkins :

Check service status:

```clojure
sudo service jenkins status
```

### Step 7: Access Jenkins in a Browser :

Now open Jenkins using your server’s IP or domain name:

```clojure
<your-server-publicip>:8080
```

If you're using a local system:

```clojure
localhost:8080
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748498370427/310cdd43-0800-4907-a484-81407f8a4075.png align="center")

### Step 8: Unlock Jenkins :

On the first access, Jenkins will ask for an <mark>initial admin password</mark>.

Get the password with this command:

```clojure
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748498392008/30a4a88d-5d0a-4979-ae0e-07647f47a44a.png align="center")

Copy and paste the password shown in the terminal into the browser prompt.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1748498437411/dc7e8060-db3a-42a8-8854-dc7ea6f9a343.png align="center")

### Step 9: Install Plugins

* After unlocking, Jenkins will ask to install plugins. Choose:
    
* Install Suggested Plugins (Recommended)
    
* This will install the most commonly used plugins (Git, Pipeline, Docker, etc.).
    
* You can also install plugins manually later.
    

🎉 Congratulations! Jenkins is now ready to use.