---
title: "(Day 41) Task : Terraform Advanced Concepts – Multicloud, Multiregion, Providers, Variables, and Conditions :-"
datePublished: Thu Jul 24 2025 03:01:13 GMT+0000 (Coordinated Universal Time)
cuid: cmdgt2opt000702jn2lytes4s
slug: day-41-task-terraform-advanced-concepts-multicloud-multiregion-providers-variables-and-conditions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753325936475/3a82144b-b296-4ff5-8f91-765104b6470e.jpeg
tags: variables, aws, azure, terraform, provider, 2articles1week, multi-region, multicloud, abhishek-veeramalla, conditions-cloudformation

---

### A Note Before We Begin...

Hlo Everyone

Before jumping into today’s topic, I wanted to share why I haven’t been consistent lately in writing my Hashnode articles as part of my #90DaysOfDevOps journey. I’ve recently not been keeping well and had to travel back to my hometown for rest and recovery. Unfortunately, I didn’t have my personal PC with me and also struggled with poor internet connectivity. This break was unplanned, and I appreciate your patience and support as I get back into my rhythm. Now that I’ve regained some energy and access to better resources, I’m back with an in-depth blog on Terraform’s powerful capabilities, focusing on real-world infrastructure management use cases like multi-region and multi-cloud deployments, conditional logic, variables, and more. Let's dive in!

## What is Terraform?

* Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to provision, manage, and version cloud infrastructure using a declarative configuration language called HCL (HashiCorp Configuration Language).
    
* Using Terraform, you can define infrastructure — VPCs, subnets, servers, databases, and even DNS — in human-readable .tf files, and it will automatically create/update/destroy resources as needed, across multiple cloud providers like AWS, Azure, GCP, and many others.
    

## Terraform Providers :-

### What is a Provider?

A provider in Terraform is a plugin that allows Terraform to interact with APIs of external platforms like AWS, GCP, Azure, GitHub, Kubernetes, etc. Providers are responsible for understanding API authentication, managing resource creation, and maintaining state.

```bash
provider "aws" {
  region = "us-west-2"
}
```

You can also configure multiple providers (multi-region or multi-cloud) in the same Terraform project.

### Multiregion Deployments in Terraform :-

Managing infrastructure in multiple regions is often necessary for:

* High availability.
    
* Disaster recovery.
    
* Lower latency for users in different parts of the world.
    

Terraform allows you to define multiple providers with aliasing for different regions.

```bash
provider "aws" {
  region = "us-east-1"
}

provider "aws" {
  alias  = "us-west"
  region = "us-west-2"
}

resource "aws_instance" "east_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

resource "aws_instance" "west_server" {
  provider      = aws.us-west
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

Here, east\_server runs in `us-east-1`, and west\_server runs in `us-west-2`, both defined in the same config file.

### Multicloud Infrastructure Using Terraform :-

Why restrict yourself to a single cloud? Enterprises now use multiple clouds for redundancy, compliance, or to leverage unique services from each provider.

Terraform makes multicloud infrastructure straightforward:

```bash
# AWS Provider
provider "aws" {
  region = "us-east-1"
}

# Google Cloud Provider
provider "google" {
  project = "my-gcp-project"
  region  = "us-central1"
}

resource "aws_s3_bucket" "bucket" {
  bucket = "my-multicloud-bucket"
}

resource "google_storage_bucket" "gcp_bucket" {
  name     = "my-gcp-bucket"
  location = "US"
}
```

With a single terraform apply, both AWS and GCP resources are provisioned, showing the true power of Terraform’s provider-agnostic model.

### Variables in Terraform :-

Terraform uses variables to make configurations dynamic and reusable. This is helpful when working in teams or across different environments like dev, stage, and prod.

**Define a variable :-**

```bash
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}
```

**Use it in a resource :-**

```bash
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
}
```

You can also pass variables via:

* terraform.tfvars.
    
* CLI: terraform apply -var="instance\_type=t2.small".
    
* Environment variables: TF\_VAR\_instance\_type.
    

### Conditional Logic in Terraform :-

Terraform supports conditions using ternary operators and dynamic blocks. These help in controlling which resources should be created or how they should be configured based on input.

**Example:**

**Condition-based instance size :**

```bash
variable "environment" {
  type    = string
  default = "dev"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.environment == "prod" ? "t2.large" : "t2.micro"
}
```

In this case, production uses a larger instance, while other environments use a smaller one.

**Dynamic Blocks :**

You can dynamically generate nested configuration blocks using dynamic.

```bash
variable "enable_ebs" {
  type    = bool
  default = true
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  dynamic "ebs_block_device" {
    for_each = var.enable_ebs ? [1] : []
    content {
      device_name = "/dev/sdh"
      volume_size = 10
    }
  }
}
```