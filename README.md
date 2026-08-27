# AWS Infrastructure Automation Using Terraform

📌 Project Overview

**AWS Infrastructure Automation using Terraform** is an Infrastructure as Code (IaC) project designed to automate the provisioning, configuration, and management of AWS cloud infrastructure.

Instead of manually creating and configuring AWS resources through the AWS Management Console, this project uses **Terraform** to define the required infrastructure as reusable and version-controlled configuration files.

The project provisions a secure and scalable AWS environment consisting of a **VPC, public subnet, Internet Gateway, route table, security group, EC2 instance, and S3 bucket**. Terraform manages the complete infrastructure lifecycle, from creation and modification to controlled destruction.

The project demonstrates how cloud infrastructure can be made **repeatable, consistent, automated, maintainable, and easier to manage** using Infrastructure as Code.

---

## 🎯 Project Objectives

The primary objectives of this project are:

* Automate AWS infrastructure provisioning using Terraform.
* Implement Infrastructure as Code principles.
* Create a structured and isolated AWS networking environment.
* Deploy an EC2 instance within the custom VPC.
* Configure secure network access using Security Groups.
* Provide internet connectivity through an Internet Gateway.
* Configure routing using Route Tables.
* Provision S3 for cloud object storage.
* Use Terraform variables and outputs to make the infrastructure reusable.
* Demonstrate Terraform's infrastructure lifecycle management.
* Maintain infrastructure configuration in GitHub for version control.
* Reduce manual configuration and deployment effort.
* Provide a foundation that can be extended to CI/CD and production-grade cloud infrastructure.

---

# 🏗️ Architecture

The infrastructure follows a layered AWS architecture:

```text
                         Developer
                             |
                             |
                         GitHub
                             |
                             |
                       Terraform Code
                             |
                    +--------+--------+
                    |                 |
               terraform init    terraform plan
                    |                 |
                    +--------+--------+
                             |
                       terraform apply
                             |
                             v
                    +------------------+
                    |       AWS        |
                    +------------------+
                             |
                            VPC
                      CIDR: 10.0.0.0/16
                             |
                +------------+------------+
                |                         |
         Public Subnet              Private Subnet
          10.0.1.0/24                10.0.2.0/24
                |
                |
        Internet Gateway
                |
          Route Table
                |
        Security Group
                |
              EC2
                |
          Application /
          Web Server

                             +
                             |
                            S3
                       Object Storage
```

---

# 🔄 Complete Project Flow

The complete workflow of the project is:

```text
1. Developer writes Terraform configuration
                    ↓
2. Terraform AWS Provider is configured
                    ↓
3. AWS credentials are configured securely
                    ↓
4. terraform init
                    ↓
5. Terraform downloads and initializes providers
                    ↓
6. terraform validate
                    ↓
7. Terraform configuration is checked
                    ↓
8. terraform plan
                    ↓
9. Desired infrastructure is compared with
   existing Terraform state
                    ↓
10. Execution plan is generated
                    ↓
11. terraform apply
                    ↓
12. Terraform communicates with AWS APIs
                    ↓
13. VPC and networking resources are created
                    ↓
14. Security Group is configured
                    ↓
15. EC2 instance is provisioned
                    ↓
16. S3 bucket is provisioned
                    ↓
17. Terraform records infrastructure state
                    ↓
18. Outputs display important resource information
                    ↓
19. Infrastructure can be modified through code
                    ↓
20. terraform plan detects changes
                    ↓
21. terraform apply updates only required resources
                    ↓
22. terraform destroy removes managed infrastructure
```

---

# ☁️ AWS Infrastructure Components

## 1. Amazon VPC

A custom **Virtual Private Cloud (VPC)** is created to provide an isolated networking environment for the project.

Example:

```text
VPC CIDR: 10.0.0.0/16
```

The VPC acts as the primary networking boundary for the infrastructure.

### Purpose

* Provides network isolation.
* Defines the IP address range.
* Contains subnets and networking components.
* Controls communication between AWS resources.

---

## 2. Public Subnet

A public subnet is created inside the VPC.

Example:

The subnet is associated with a route table containing a route to the Internet Gateway.

The EC2 instance can therefore be placed inside the public subnet when internet accessibility is required.

---

## 3. Internet Gateway

The Internet Gateway provides communication between the VPC and the public internet.

The traffic flow is:

EC2
 ↓
Public Subnet
 ↓
Route Table
 ↓
Internet Gateway
 ↓
Internet

---

## 4. Route Table

The route table controls where network traffic from the subnet should be directed.

A default route is configured:

This means traffic destined for external networks is forwarded through the Internet Gateway.

---

## 5. Security Group

A Security Group acts as a virtual firewall for the EC2 instance.

Typical rules include:

```text
Inbound:
SSH  → Port 22
HTTP → Port 80

Outbound:
Allow required outbound traffic
```

For security, SSH access should preferably be restricted to the administrator's IP address instead of allowing unrestricted access from the internet.

---

## 6. Amazon EC2

An EC2 instance is provisioned automatically using Terraform.

The instance is configured with:

* AMI
* Instance type
* Subnet
* Security Group
* Public IP configuration
* Resource tags

Example:

```text
VPC
 ↓
Public Subnet
 ↓
Security Group
 ↓
EC2 Instance
```

The EC2 instance can be used to host a web server, backend application, or other workloads.

---

## 7. Amazon S3

An S3 bucket is provisioned for object storage.

It can be used for:

* Application files
* Static assets
* Logs
* Backups
* Data storage

Additional security features such as encryption, versioning, and appropriate bucket policies can be enabled depending on the project requirements.

---

# 🛠️ Technology Stack

| Technology           | Purpose                                        |
| -------------------- | ---------------------------------------------- |
| **Terraform**        | Infrastructure provisioning and automation     |
| **AWS Provider**     | Allows Terraform to communicate with AWS       |
| **Amazon VPC**       | Network isolation                              |
| **Subnets**          | Network segmentation                           |
| **Internet Gateway** | Internet connectivity                          |
| **Route Tables**     | Traffic routing                                |
| **Security Groups**  | Network-level access control                   |
| **Amazon EC2**       | Compute infrastructure                         |
| **Amazon S3**        | Object storage                                 |
| **AWS CLI**          | AWS authentication and command-line management |
| **Git**              | Version control                                |
| **GitHub**           | Source code and infrastructure versioning      |

---

# ⚙️ Terraform Workflow

## Step 1 — Configure AWS

AWS CLI is configured with the appropriate credentials and region.

```bash
aws configure
```

The configuration can then be verified using:

```bash
aws sts get-caller-identity
```

---

## Step 2 — Initialize Terraform

```bash
terraform init
```

This initializes the Terraform working directory and downloads the required AWS provider.

---

## Step 3 — Format Configuration

```bash
terraform fmt
```

This formats Terraform configuration files according to Terraform's standard formatting conventions.

---

## Step 4 — Validate Configuration

```bash
terraform validate
```

This checks whether the Terraform configuration is syntactically valid and internally consistent.

---

## Step 5 — Generate Execution Plan

```bash
terraform plan
```

Terraform analyzes the configuration and determines what infrastructure needs to be:

```text
Created
Modified
Destroyed
```

The plan allows the developer to review changes before they are applied.

---

## Step 6 — Provision Infrastructure

```bash
terraform apply
```

Terraform communicates with AWS and creates the infrastructure defined in the configuration.

After successful execution:

```text
Apply complete!
```

Terraform outputs important resource information.

---

The state allows Terraform to understand which AWS resources correspond to the infrastructure defined in the configuration.

The Terraform state file is **not committed to GitHub** because it may contain sensitive infrastructure information.

---

# 🔐 Security Practices

The project follows basic infrastructure security practices:

* AWS credentials are not hardcoded in Terraform files.
* Sensitive files are excluded using `.gitignore`.
* Terraform state is excluded from version control.
* EC2 access is controlled using Security Groups.
* SSH access should be restricted to trusted IP addresses.
* S3 access should follow the principle of least privilege.
* IAM permissions should be limited to required operations.
* AWS resources should be tagged for easier management and cost tracking.

---

# 🚀 Future Enhancements

The project can be extended into a more production-oriented infrastructure platform by adding:

* Terraform Modules
* Private Subnets
* NAT Gateway
* IAM Roles and Policies
* S3 Versioning
* S3 Encryption
* CloudWatch Monitoring
* Application Load Balancer
* Auto Scaling Groups
* Development/Staging/Production environments


# 📊 Key Benefits

### Infrastructure as Code
### Automation
### Repeatability
### Version Control
### Scalability
### Maintainability
### Reduced Manual Errors
---

# 🏁 Conclusion

This project demonstrates how **Terraform can be used to automate AWS infrastructure provisioning through Infrastructure as Code**.

By defining AWS resources in Terraform configuration files, the project eliminates repetitive manual infrastructure setup and provides a consistent, version-controlled approach to cloud infrastructure management.

The implementation establishes a foundation for more advanced DevOps and cloud engineering practices such as **modular Terraform architecture, remote state management, CI/CD automation, monitoring, security, and multi-environment deployments**.

---

## 👩‍💻 Author

**Sri Pooja K.A.**

CSE | Cloud & DevOps Enthusiast


