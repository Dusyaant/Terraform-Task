```
              +-----------------------------------+
              |      Terraform Core Engine        |
              |     (Local Workspace State)       |
              +-----------------------------------+
                                |
        +-----------------------+-----------------------+
        |                                               |
        v                                               v
+-----------------------+                       +-----------------------+
|  Provider: Default    |                       |   Provider Alias:     |
|  (aws / us-east-1)    |                       |   (aws.oregon /       |
|                       |                       |    us-west-2)         |
+-----------------------+                       +-----------------------+
|                                               |
v                                               v
+-----------------------+                       +-----------------------+
|  Data Source Lookup:  |                       |  Data Source Lookup:  |
|  Latest Ubuntu Core   |                       |  Latest Ubuntu Core   |
|  noble-24.04-amd64    |                       |  noble-24.04-amd64    |
+-----------------------+                       +-----------------------+
|                                               |
v                                               v
+-----------------------+                       +-----------------------+
|     EC2 Instance      |                       |     EC2 Instance      |
| "ec2_region_one"      |                       | "ec2_region_two"      |
|   (t3.micro size)     |                       |   (t3.micro size)     |
+-----------------------+                       +-----------------------+
|                                               |
v                                               v
[Region: US-East-1 (VA)]                       [Region: US-West-2 (OR)]

```

---

## 🛠️ Infrastructure Component Breakdown

1. **Multi-Region Providers:**
   * Configures the primary global cloud provider interface defaulting orchestration hooks to `us-east-1`.
   * Establishes a secondary provider path via `alias = "oregon"` targeting `us-west-2` to support concurrent resource instantiation across distinct geographical domains.
2. **Automated Dynamic AMIs:**
   * Eliminates hardcoded, fragile Amazon Machine Image (AMI) references that cause cross-account lookup failures.
   * Utilizes `data "aws_ami"` filters targeting Canonical's official AWS account ID (`099720109477`) to fetch the latest stable Hardware Virtual Machine (HVM) SSD-backed General Purpose 3 (`gp3`) **Ubuntu 24.04 LTS (Noble Numbat)** 64-bit architecture image dynamically at execution time.
3. **Compute Node Layer:**
   * Provisions optimized **`t3.micro`** compute virtual machines (Intel Xeon Scalable processors). 
   * Utilizes `t3.micro` to align completely with modern sandboxed AWS account access constraints while staying fully covered within the standard AWS Free Tier guidelines.

---

## 🚀 Step-by-Step Deployment Guide

### 📋 Prerequisites
* **Terraform CLI** installed on your local machine (v1.5.0+ recommended).
* **AWS CLI v2** installed and verified.
* An active **AWS Account** with administrative permissions.

### 🏁 1. Authenticate Local Environment
Open your local terminal or Windows Command Prompt and map your active IAM credentials to the infrastructure engine:
```cmd
aws configure

```

Input your account vectors when prompted:

* **AWS Access Key ID:** `AKIAXXXXXXXXXXXXXXXX`
* **AWS Secret Access Key:** `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`
* **Default region name:** `us-east-1`
* **Default output format:** `json`

Verify endpoint connectivity and identity verification:

```cmd
aws sts get-caller-identity

```

### ⚙️ 2. Initialize the Workspace

Navigate into your local task directory containing your configuration files and initialize the backend state and download dependencies:

```cmd
cd %USERPROFILE%\\Desktop\\Terraform-MultiRegion-Task
terraform init

```

### 🔍 3. Execute Execution Preview (Plan)

Generate the spec blueprint to audit resource shifts prior to provisioning against cloud APIs. The output must indicate exactly 2 items to add:

```cmd
terraform plan

```

### ⚡ 4. Live Provisioning (Apply)

Apply the state transformations live to AWS. When the orchestration engine prompts you for explicit authorization confirmation, type `yes` and hit Enter:

```cmd
terraform apply

```

*Upon completion, capture your terminal execution context for submission documentation.*

---

## 🗂️ Proof of Deployment Deliverables

The successful completion of this deployment task requires three visual verification checkpoints to ensure accurate multi-region state mapping:

### 1. Terminal Execution Success

Capture the successful completion statement displaying resource allocation statistics at the base of the standard shell interface:

```text
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

```

### 2. Primary Node: US-East-1 (N. Virginia)

Log into the AWS Management Console, adjust the upper-right regional index parameter to **N. Virginia**, navigate to the EC2 panel, and verify the following running node:

* **Name Tag:** `EC2-Virginia-Region`
* **Instance Size:** `t3.micro`
* **Operating System:** Ubuntu 24.04 LTS

### 3. Secondary Node: US-West-2 (Oregon)

Switch the upper-right regional index parameter to **Oregon**, refresh the dashboard view, and capture the secondary active compute resource:

* **Name Tag:** `EC2-Oregon-Region`
* **Instance Size:** `t3.micro`
* **Operating System:** Ubuntu 24.04 LTS

---
