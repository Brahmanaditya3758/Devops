---
title: "Linux Basics (Day 02): Mastering Essential Terminal Commands 🚀💻"
datePublished: Fri Mar 28 2025 06:16:58 GMT+0000 (Coordinated Universal Time)
cuid: cm8se3wtf001609jv5sly0qpo
slug: linux-basics-day-02-mastering-essential-terminal-commands
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1743142520075/5ce13d09-2f26-413d-a0dd-09b04a949358.png

---

## What is Linux?

Linux is an **open-source, Unix-like operating system (OS)** based on the **Linux kernel**, created by **Linus Torvalds** in 1991.Before **Linux** was created in **1991** by **Linus Torvalds**, most operating systems were:

* **Expensive** – UNIX was powerful but costly.
    
* **Proprietary** – Windows and macOS restricted user control.
    
* **Limited** – No free, customisable alternative for developers and businesses.
    

**Linux is a kernel, not a full operating system.** However, it is often referred to as an **OS** because it is the core component of **Linux-based operating systems** (also called Linux distributions).

* **Linux = Kernel** (the core system software).
    
* **Linux-Based OS = Kernel + Tools + UI + Applications.**
    
    When people say “Linux,” they usually mean a **Linux-based OS** , but technically, Linux itself is just the **kernel**.
    

# Basic Commands:

## Check your present working directory.

In Linux, you can check your **present working directory (PWD)** by using the following command in the terminal:

### Input:

```apache
pwd
```

## Explanation:

* `pwd` stands for **Print Working Directory**.
    
* It shows the **absolute path** of the current directory you are in.
    

### Output:

```apache
/home/user/Documents
```

## List all the files or directories including hidden files.

To list **all files and directories**, including **hidden files**, in Linux, use ls-a.

### Input:

```apache
ls-a
```

## Explanation:

* `ls` → Lists files and directories.
    
* `-a` → Shows **all** files, including **hidden files** (files starting with `.`).
    

### Output:

```apache
.  ..  .bashrc  .profile  Documents  Downloads  Pictures
```

* `.` → Represents the **current directory**.
    
* `..` → Represents the **parent directory**.
    
* Files like `.bashrc`, `.profile` are **hidden files**.
    

## Create a nested directory A/B/C/D/E.

To create a **nested directory structure** (`A/B/C/D/E`) in Linux, we use mkdir -p A/B/C/D/E.

### Input:

```apache
mkdir -p A/B/C/D/E
```

## Explanation:

* `mkdir` → Creates a directory.
    
* `-p` (**\--parents**) → Creates **all parent directories** if they don’t exist.