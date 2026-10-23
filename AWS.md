# Comprehensive AWS & Cloud Engineering Study Notes

An in-depth, structured reference guide covering cloud computing fundamentals, AWS core infrastructure, networking, storage, databases, container orchestration, monitoring, CI/CD, and Infrastructure as Code (IaC) with Terraform.

---

## Table of Contents

- [1. Cloud Computing Fundamentals & Global Infrastructure](#1-cloud-computing-fundamentals--global-infrastructure)
  - [1.1 Cloud Service Models (IaaS, PaaS, SaaS)](#11-cloud-service-models-iaas-paas-saas)
  - [1.2 AWS Global Infrastructure (Regions, AZs, Edge Locations)](#12-aws-global-infrastructure-regions-azs-edge-locations)
  - [1.3 The AWS Root User](#13-the-aws-root-user)
- [2. AWS Identity and Access Management (IAM) & Governance](#2-aws-identity-and-access-management-iam--governance)
  - [2.1 IAM Core Components (User, Group, Policy, Role)](#21-iam-core-components-user-group-policy-role)
  - [2.2 AWS Organizations & Service Control Policies (SCPs)](#22-aws-organizations--service-control-policies-scps)
  - [2.3 AWS IAM Identity Center (AWS SSO)](#23-aws-iam-identity-center-aws-sso)
- [3. Amazon EC2 (Elastic Compute Cloud) & Compute Architecture](#3-amazon-ec2-elastic-compute-cloud--compute-architecture)
  - [3.1 EC2 Fundamentals & Elasticity](#31-ec2-fundamentals--elasticity)
  - [3.2 EC2 Launch Components](#32-ec2-launch-components)
  - [3.3 Firewalls & IP Addressing (Public, Private, Elastic IP)](#33-firewalls--ip-addressing-public-private-elastic-ip)
  - [3.4 EC2 User Data (Bootstrap Scripts)](#34-ec2-user-data-bootstrap-scripts)
- [4. Amazon EBS (Elastic Block Store)](#4-amazon-ebs-elastic-block-store)
  - [4.1 EBS Volumes & Availability Zone Scoping](#41-ebs-volumes--availability-zone-scoping)
  - [4.2 EBS Snapshots & Region Scoping](#42-ebs-snapshots--region-scoping)
  - [4.3 Volume ↔ Snapshot Lifecycle Flow](#43-volume--snapshot-lifecycle-flow)
- [5. Elastic Load Balancing (ELB) & Auto Scaling](#5-elastic-load-balancing-elb--auto-scaling)
  - [5.1 Load Balancer Concepts & Benefits](#51-load-balancer-concepts--benefits)
  - [5.2 Types of AWS Load Balancers (ALB, NLB, GWLB)](#52-types-of-aws-load-balancers-alb-nlb-gwlb)
  - [5.3 Routing Algorithms & Target Groups](#53-routing-algorithms--target-groups)
  - [5.4 AWS Auto Scaling Groups (ASG)](#54-aws-auto-scaling-groups-asg)
- [6. Amazon S3 (Simple Storage Service)](#6-amazon-s3-simple-storage-service)
  - [6.1 S3 Architecture & Core Properties](#61-s3-architecture--core-properties)
  - [6.2 Block Storage vs. Object Storage](#62-block-storage-vs-object-storage)
  - [6.3 S3 Key Features (Versioning, Object Lock, Transfer Acceleration)](#63-s3-key-features-versioning-object-lock-transfer-acceleration)
  - [6.4 S3 Static Website Hosting](#64-s3-static-website-hosting)
- [7. Amazon S3 Storage Classes & Intelligent-Tiering Deep Dive](#7-amazon-s3-storage-classes--intelligent-tiering-deep-dive)
  - [7.1 Storage Class Selection Criteria](#71-storage-class-selection-criteria)
  - [7.2 S3 Storage Classes Breakdown](#72-s3-storage-classes-breakdown)
  - [7.3 S3 Intelligent-Tiering & Its 5 Access Tiers](#73-s3-intelligent-tiering--its-5-access-tiers)
  - [7.4 Critical Technical Rules (128 KB Rule, Costs, Durations)](#74-critical-technical-rules-128-kb-rule-costs-durations)
  - [7.5 Storage Classes Comparison Matrix & Mental Models](#75-storage-classes-comparison-matrix--mental-models)
- [8. Amazon RDS & Amazon Aurora](#8-amazon-rds--amazon-aurora)
  - [8.1 Amazon Relational Database Service (RDS)](#81-amazon-relational-database-service-rds)
  - [8.2 Amazon Aurora](#82-amazon-aurora)
  - [8.3 Architectural Relationship: RDS vs. Aurora vs. EC2](#83-architectural-relationship-rds-vs-aurora-vs-ec2)
- [9. Amazon VPC (Virtual Private Cloud) & Networking](#9-amazon-vpc-virtual-private-cloud--networking)
  - [9.1 VPC & Subnet Architecture](#91-vpc--subnet-architecture)
  - [9.2 Internet Gateway (IGW) & Route Tables](#92-internet-gateway-igw--route-tables)
  - [9.3 NAT Gateway & Outbound Traffic Patterns](#93-nat-gateway--outbound-traffic-patterns)
  - [9.4 End-to-End VPC Traffic Architecture Flow](#94-end-to-end-vpc-traffic-architecture-flow)
  - [9.5 Network Security: Security Groups vs. Network ACLs (NACLs)](#95-network-security-security-groups-vs-network-acls-nacls)
  - [9.6 CIDR (Classless Inter-Domain Routing)](#96-cidr-classless-inter-domain-routing)
  - [9.7 VPC Peering](#97-vpc-peering)
- [10. Amazon Route 53 (DNS & Traffic Management)](#10-amazon-route-53-dns--traffic-management)
  - [10.1 Route 53 Fundamentals & Resolution Workflow](#101-route-53-fundamentals--resolution-workflow)
  - [10.2 Routing Policies, Health Checks & DNS Record Types](#102-routing-policies-health-checks--dns-record-types)
- [11. AWS CloudWatch & Amazon SNS (Monitoring & Alerts)](#11-aws-cloudwatch--amazon-sns-monitoring--alerts)
  - [11.1 Amazon CloudWatch (Metrics, Logs, Alarms, Dashboards)](#111-amazon-cloudwatch-metrics-logs-alarms-dashboards)
  - [11.2 Amazon Simple Notification Service (SNS)](#112-amazon-simple-notification-service-sns)
  - [11.3 CloudWatch + SNS Integration Architecture](#113-cloudwatch--sns-integration-architecture)
- [12. AWS CodeBuild & CI/CD Pipelines](#12-aws-codebuild--cicd-pipelines)
  - [12.1 AWS CodeBuild Overview & Workflow](#121-aws-codebuild-overview--workflow)
  - [12.2 The `buildspec.yml` Lifecycle & S3 Artifact Storage](#122-the-buildspecyml-lifecycle--s3-artifact-storage)
  - [12.3 Full CI/CD Orchestration with AWS CodePipeline](#123-full-cicd-orchestration-with-aws-codepipeline)
- [13. Infrastructure as Code (IaC) & AWS CloudFormation](#13-infrastructure-as-code-iac--aws-cloudformation)
  - [13.1 IaC Concepts, Advantages & Tooling](#131-iac-concepts-advantages--tooling)
  - [13.2 AWS CloudFormation (Templates & Stacks)](#132-aws-cloudformation-templates--stacks)
  - [13.3 AWS Elastic Beanstalk (PaaS Application Deployment)](#133-aws-elastic-beanstalk-paas-application-deployment)
  - [13.4 Architectural Comparison: EC2 vs. Beanstalk vs. CloudFormation](#134-architectural-comparison-ec2-vs-beanstalk-vs-cloudformation)
- [14. Containers & Container Orchestration (ECS, EKS, ECR & Fargate)](#14-containers--container-orchestration-ecs-eks-ecr--fargate)
  - [14.1 Amazon ECS (Elastic Container Service)](#141-amazon-ecs-elastic-container-service)
  - [14.2 Amazon EKS (Elastic Kubernetes Service)](#142-amazon-eks-elastic-kubernetes-service)
  - [14.3 ECS vs. EKS Comparison & Compute Options (EC2 vs. Fargate)](#143-ecs-vs-eks-comparison--compute-options-ec2-vs-fargate)
  - [14.4 Amazon ECR (Elastic Container Registry)](#144-amazon-ecr-elastic-container-registry)
- [15. HashiCorp Terraform Deep Dive](#15-hashicorp-terraform-deep-dive)
  - [15.1 Terraform Fundamentals & CLI Workflow](#151-terraform-fundamentals--cli-workflow)
  - [15.2 Terraform State (`terraform.tfstate` & `terraform.tfstate.backup`)](#152-terraform-state-terraformtfstate--terraformtfstatebackup)
  - [15.3 Remote State Storage, Security & Locking](#153-remote-state-storage-security--locking)
  - [15.4 Input Variables & Output Variables](#154-input-variables--output-variables)
  - [15.5 Terraform Resource Replacement: Taint vs. Modern `-replace`](#155-terraform-resource-replacement-taint-vs-modern--replace)
  - [15.6 Terraform Modules (Architecture, Root vs. Child, Registry)](#156-terraform-modules-architecture-root-vs-child-registry)

---

## 1. Cloud Computing Fundamentals & Global Infrastructure

### 1.1 Cloud Service Models (IaaS, PaaS, SaaS)

Cloud computing resources are categorized into three primary service models based on the level of management shared between the cloud provider and the user:

```
┌─────────────────────────────────────────────────────────────┐
│                       SaaS (Software)                       │
│    Applications, Data, Runtime, OS, Servers, Storage        │  <-- Managed by Provider
├─────────────────────────────────────────────────────────────┤
│                       PaaS (Platform)                       │
│    Applications, Data  [User Managed]                       │
│    Runtime, OS, Servers, Storage  [Provider Managed]        │
├─────────────────────────────────────────────────────────────┤
│                    IaaS (Infrastructure)                    │
│    Applications, Data, Runtime, Middleware, OS [User]       │
│    Virtualization, Servers, Storage, Networking [Provider]  │
└─────────────────────────────────────────────────────────────┘
```

| Service Model | Description | What You Manage | What Cloud Provider Manages | Real-World Example |
| :--- | :--- | :--- | :--- | :--- |
| **IaaS** *(Infrastructure as a Service)* | Provides virtualized compute infrastructure, storage, and networking. Maximum control over the OS and environment. | OS, runtime, middleware, dependencies, applications, security configurations, patches. | Physical servers, virtualization layer, storage hardware, physical network. | **AWS EC2**, Google Compute Engine |
| **PaaS** *(Platform as a Service)* | Provides a ready-to-use platform for developing, testing, and deploying applications without managing underlying infrastructure. | Application code, application data, environment configuration variables. | Operating system, server provisioning, runtime engines, updates, maintenance. | **AWS Elastic Beanstalk**, Heroku, Google App Engine |
| **SaaS** *(Software as a Service)* | Delivers a complete, fully functional software application accessed directly over the internet. | User access, basic preferences, personal settings. | Everything: application code, updates, runtime, OS, infrastructure, databases. | **Gmail**, Google Docs, Microsoft 365, Slack |

---

### 1.2 AWS Global Infrastructure (Regions, AZs, Edge Locations)

AWS global infrastructure is built on three core geographic abstractions:

1. **AWS Region:**
   - A physical geographic location in the world (e.g., `ap-south-1` Mumbai, `us-east-1` N. Virginia).
   - Each Region contains multiple, physically separated and isolated Availability Zones.
2. **Availability Zone (AZ):**
   - An isolated data center (or cluster of discrete data centers) located within an AWS Region.
   - Each AZ has independent power feeds, redundant networking, and separate cooling systems.
   - Connected to other AZs in the same Region via low-latency, high-throughput private fiber links.
   - **Primary Goal:** Provides **High Availability (HA)** and **Fault Tolerance** by deploying workloads across multiple AZs.
3. **Edge Location:**
   - A globally distributed endpoint placed close to end users.
   - Primarily used by **Amazon CloudFront** (Content Delivery Network - CDN) and **AWS Global Accelerator** to cache static and dynamic web content.
   - **Primary Goal:** Drastically reduces **latency** by serving requests from the geographic location nearest to the client rather than routing all traffic back to the origin Region.

---

### 1.3 The AWS Root User

- **Definition:** The single highest-privileged identity created automatically when an AWS account is first established.
- **Access Level:** Complete, unrestricted access to all AWS resources, billing data, and administrative account settings.
- **Security Best Practices:**
  - **Never use the root user for everyday administrative tasks or routine operations.**
  - Lock away the root user credentials and enable Multi-Factor Authentication (MFA).
  - Use the root user **only** for specific tasks that strictly require root privileges, such as:
    - Changing the AWS account name, root email, or root password.
    - Closing the AWS account.
    - Changing or canceling AWS Support plans.
    - Viewing certain tax invoices or specific billing preferences.

---

## 2. AWS Identity and Access Management (IAM) & Governance

### 2.1 IAM Core Components (User, Group, Policy, Role)

AWS IAM controls authentication (*who you are*) and authorization (*what you are allowed to do*) within an AWS account.

```text
       ┌───────────────┐
       │   IAM Group   │
       │ (Collection)  │
       └───────┬───────┘
               │ contains
       ┌───────▼───────┐             ┌────────────────┐
       │   IAM User    ├────────────►│   IAM Policy   │
       │   (Person)    │  attaches   │ (JSON Document)│
       └───────────────┘             └────────▲───────┘
                                              │ attaches
       ┌───────────────┐                      │
       │   IAM Role    ├──────────────────────┘
       │  (Assumable)  │
       └───────────────┘
```

- **IAM User:** Represents a specific human person (e.g., `Veeraj`, a developer) or a service/application that requires persistent credentials to access AWS resources. Permissions can be attached directly or inherited from groups.
- **IAM Group:** A collection of IAM users (e.g., `Developers`, `Admins`). Attaching a policy to a group automatically grants those permissions to every user currently in or added to the group.
- **IAM Policy:** A structured **JSON document** defining formal permissions. Explicitly specifies which AWS API `Actions` are `Allowed` or `Denied` on specific `Resources` under optional `Conditions`.
- **IAM Role:** An identity with attached permissions that is **assumed temporarily** by users, AWS services, or external applications. Roles do not have permanent long-term credentials (passwords or access keys); instead, AWS Security Token Service (STS) provides temporary session tokens.
  - *Common Use Cases:* An EC2 instance assuming a role to read files from S3; cross-account resource access.

> [!TIP]
> **Core IAM Memory Formula:**
> - **User** = A specific person or persistent application
> - **Group** = A collection of users sharing permissions
> - **Policy** = The JSON document defining permissions (Allow / Deny)
> - **Role** = An assumable identity granting temporary permissions

---

### 2.2 AWS Organizations & Service Control Policies (SCPs)

- **AWS Organizations:** An account management service that allows organizations to centrally consolidate and manage multiple AWS accounts under a single hierarchical structure.
  - Enables centralized consolidated billing, programmatic account creation, and organizational grouping into Organizational Units (OUs) (e.g., *Dev*, *Test*, *Prod*, *Security*, *Staging*).
- **Service Control Policies (SCPs):**
  - Organizational guardrails that define the **maximum permissible permissions** for accounts or OUs.
  - **Crucial Rule:** An SCP **does not grant permissions** by itself; it acts as an absolute boundary/filter. If an SCP denies an action or does not allow a service, no IAM user or role within that account can execute that action—even if they have `AdministratorAccess`.
  - *Example:* An organization-wide SCP can restrict an entire AWS account so resources can only be provisioned in the Mumbai (`ap-south-1`) Region.

---

### 2.3 AWS IAM Identity Center (AWS SSO)

- **Purpose:** Centrally manages human workforce identities and their single-sign-on (SSO) access to multiple AWS accounts and third-party business cloud applications (e.g., Salesforce, Microsoft 365).
- **Eliminates IAM User Sprawl:** Instead of manually creating separate IAM users inside every individual AWS account, users log in once through a centralized portal.
- **Permission Sets:** AWS IAM Identity Center uses *permission sets* (collections of IAM policies assigned centrally) to determine what actions a user or group is authorized to perform when federating into a given target AWS account.

---

## 3. Amazon EC2 (Elastic Compute Cloud) & Compute Architecture

### 3.1 EC2 Fundamentals & Elasticity

- **Definition:** Amazon EC2 provides scalable, on-demand virtual compute servers (called **EC2 Instances**) in the cloud.
- **Shared Responsibility:** EC2 is **IaaS**, not serverless. AWS manages physical host hardware, power, physical networking, and virtualization hypervisors. You are responsible for the guest operating system, runtime libraries, application code, security patching, and firewall configurations.
- **What "Elastic" Means:** Compute capacity can be scaled up or down vertically (changing instance size, e.g., `t3.micro` to `t3.large`) or horizontally (adding/removing instance counts via Auto Scaling) based on real-time traffic demand.
- **Decoupled Architecture:** Commands executed and files created inside a connected EC2 Linux instance run on the remote virtual server in the cloud, completely independent of the local computer running the SSH terminal.

---

### 3.2 EC2 Launch Components

When launching an EC2 virtual instance, several core parameters must be defined:

1. **AMI (Amazon Machine Image):**
   - A pre-configured template containing the OS (e.g., Amazon Linux 2023, Ubuntu 22.04, Windows Server) and optional pre-installed software, dependencies, and configuration.
2. **Instance Type:**
   - Defines the compute hardware allocation: CPU cores, RAM size, storage IOPS characteristics, and network performance (e.g., `t3.micro`, `c5.xlarge`, `m5.large`).
3. **Key Pair:**
   - Used for cryptographic authentication when connecting remotely.
   - For Linux instances via **SSH (Port 22)**: The private key (`.pem`) is retained locally on your computer, while AWS injects the public key into the instance's `~/.ssh/authorized_keys`.
   - For Windows instances via **RDP (Port 3389)**: The private key is used to decrypt the initial Administrator password.
4. **Security Group:**
   - Acts as a virtual firewall for the instance, regulating inbound and outbound network traffic at the protocol, port, and IP CIDR level.
   - *Example:* Allowing inbound SSH on port 22 only from your corporate IP, and HTTP on port 80 / HTTPS on port 443 from all origins (`0.0.0.0/0`).
5. **Storage Configuration:**
   - Boot volume and optional data volumes attached using **Amazon EBS (Elastic Block Store)**.

---

### 3.3 Firewalls & IP Addressing (Public, Private, Elastic IP)

#### Firewalls in AWS
A firewall is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules. In AWS, an **EC2 Security Group acts as a virtual firewall** protecting compute instances.

#### IP Address Types in Amazon EC2

| IP Type | Scope | Persistence Across Stop/Start | Internet Reachability | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Private IP** | Internal to VPC | **Permanent** for the lifetime of the instance's network interface (ENI) | No (Internal only) | Communication between EC2 servers, backend microservices, and databases within the VPC. |
| **Public IP** | Public Internet | **Ephemeral** (Changes whenever the instance is stopped and started) | Yes | Direct internet communication for temporary, test, or non-static web servers. |
| **Elastic IP (EIP)** | Public Internet | **Static** (Remains identical across stops, starts, and reboots) | Yes | Production workloads requiring a constant public IPv4 address (e.g., DNS A-records, whitelisting). |

> [!NOTE]
> **Summary Rule:**
> - `Private IP` $\rightarrow$ Internal AWS VPC routing (Never exposed to public internet)
> - `Public IP` $\rightarrow$ Internet accessible, but changes on instance stop/start
> - `Elastic IP` $\rightarrow$ Fixed, persistent public IPv4 address allocated to your AWS account

---

### 3.4 EC2 User Data (Bootstrap Scripts)

- **Definition:** A shell script (or cloud-init directive) provided during the instance launch wizard.
- **Execution Lifecycle:** AWS automatically executes the User Data script with root privileges **only once during the instance's very first initial boot cycle**.
- **Use Case:** Automating software installation, updating system packages (`yum update -y`), cloning Git repositories, configuring environment variables, and starting web servers (`systemctl start nginx`) without manual SSH intervention.

---

## 4. Amazon EBS (Elastic Block Store)

### 4.1 EBS Volumes & Availability Zone Scoping

- **Definition:** Amazon EBS provides high-performance, persistent **block-level storage** designed for use with EC2 instances. It functions like a virtual raw hard disk drive (SSD or HDD).
- **Availability Zone Scoping Rule:**
  - An EBS volume is strictly **Availability-Zone-specific**.
  - A volume created in `ap-south-1a` can **only** be attached to an EC2 instance physically running inside `ap-south-1a`. It cannot be attached directly to an instance in `ap-south-1b`.
- **Filesystem Requirement:**
  - When attaching a new raw EBS data volume to an EC2 Linux instance, it must first be formatted with a filesystem (such as `ext4` or `xfs`) and mounted to a directory (such as `/data`) before files can be stored.

---

### 4.2 EBS Snapshots & Region Scoping

- **Definition:** Point-in-time incremental backups of the data residing on an EBS volume.
- **Storage & Scoping Rule:**
  - EBS Snapshots are stored redundantly across multiple AZs within **Amazon S3 infrastructure** managed by AWS.
  - Snapshots are **Region-scoped**, not AZ-specific. A snapshot created from a volume in `ap-south-1a` is available across the entire `ap-south-1` Region.
- **Cross-AZ Migration:** Snapshots are the primary mechanism used to move an EBS volume from one Availability Zone to another, or to duplicate data across an entire Region.

---

### 4.3 Volume ↔ Snapshot Lifecycle Flow

```text
       ┌────────────────────────┐
       │       EBS Volume       │  (Scoped to AZ: ap-south-1a)
       └───────────┬────────────┘
                   │
                   │ Create Snapshot (Point-in-time backup)
                   ▼
       ┌────────────────────────┐
       │      EBS Snapshot      │  (Region-scoped: ap-south-1 / Stored in S3)
       └───────────┬────────────┘
                   │
                   │ Restore / Create New Volume
                   ▼
       ┌────────────────────────┐
       │     New EBS Volume     │  (Can choose ANY AZ, e.g., ap-south-1b)
       └───────────┬────────────┘
                   │
                   │ Attach Volume
                   ▼
       ┌────────────────────────┐
       │      EC2 Instance      │  (Running in ap-south-1b)
       └───────────┬────────────┘
                   │
                   │ Format (ext4) + Mount (/data)
                   ▼
       ┌────────────────────────┐
       │ Active Operating System│
       │  File Storage / DB     │
       └────────────────────────┘
```

---

## 5. Elastic Load Balancing (ELB) & Auto Scaling

### 5.1 Load Balancer Concepts & Benefits

A Load Balancer acts as a single point of contact that automatically distributes incoming application traffic across a pool of backend targets (such as EC2 instances, containers, or IP addresses).

**Key Architectural Benefits:**
- **High Availability & Fault Tolerance:** Reroutes traffic away from unhealthy targets to healthy targets.
- **Scalability:** Easily accommodates increases or decreases in traffic demand.
- **Decoupling:** Clients connect to the Load Balancer DNS name rather than individual server IP addresses.

---

### 5.2 Types of AWS Load Balancers (ALB, NLB, GWLB)

```
        Application Load Balancer (ALB)      Network Load Balancer (NLB)       Gateway Load Balancer (GWLB)
        ───────────────────────────────      ───────────────────────────       ────────────────────────────
OSI:    Layer 7 (Application)                Layer 4 (Transport)               Layer 3/4 (Network/Transport)
Traffic:HTTP, HTTPS, gRPC                    TCP, UDP, TLS                     IP Packets
Routing:URL Paths (/api, /users), Hostnames  Ultra-low latency, Static IP      Third-party Firewalls, IDS/IPS
```

1. **Application Load Balancer (ALB) — Layer 7:**
   - Understands HTTP/HTTPS protocol features: request paths, host headers, HTTP methods, and query parameters.
   - Ideal for web applications, REST APIs, and microservices (e.g., routing `/api/v1/orders` to Order instances and `/api/v1/users` to User instances).
2. **Network Load Balancer (NLB) — Layer 4:**
   - Operates at the transport layer for TCP, UDP, and TLS connections.
   - Optimized for ultra-high throughput, extreme spiky traffic, and ultra-low latency (sub-millisecond). Can assign static/Elastic IPs per AZ.
3. **Gateway Load Balancer (GWLB) — Layer 3/4:**
   - Used to deploy, scale, and manage fleets of third-party virtual network security appliances (firewalls, deep packet inspection, intrusion detection systems - IDS/IPS).

---

### 5.3 Routing Algorithms & Target Groups

#### Target Groups
- A **Target Group** is a logical grouping of backend endpoints (EC2 instances, microservice containers, private IP addresses, or Lambda functions) registered with a load balancer.
- **Health Checks:** The load balancer continuously queries each target's health check path (e.g., `GET /health` returning HTTP `200`). Unhealthy targets are immediately removed from active routing until they recover.

#### Traffic Routing Algorithms
- **Round Robin:** Distributes requests sequentially across registered targets (Target 1 $\rightarrow$ Target 2 $\rightarrow$ Target 3 $\rightarrow$ Target 1).
- **Least Outstanding Requests (LOR):** Routes traffic to the target currently processing the lowest number of concurrent uncompleted HTTP requests (supported on ALB).
- **Connection-Based:** NLB uses flow hash algorithms based on IP protocols, source/destination IPs, and port numbers to maintain persistent connections.

---

### 5.4 AWS Auto Scaling Groups (ASG)

- **Purpose:** Automatically adjusts the number of running EC2 instances to match real-time application demand.
- **Core Parameters:**
  - **Minimum Size:** The lowest number of instances that must run at all times.
  - **Desired Capacity:** The target number of active instances running under normal conditions.
  - **Maximum Size:** The upper limit of instances allowed during high-traffic spikes to prevent runaway costs.
- **Self-Healing (Health Replacement):** If an instance in the ASG fails an EC2 status check or an ELB health check, the ASG automatically terminates the unhealthy instance and launches a fresh replacement.
- **Elastic Scaling:** Scales out (adds servers) when traffic increases and scales in (terminates surplus servers) when traffic normalizes.

---

## 6. Amazon S3 (Simple Storage Service)

### 6.1 S3 Architecture & Core Properties

Amazon S3 is a highly durable, internet-accessible **object storage service**. Rather than treating data as blocks or files on a disk, S3 stores data as **objects inside containers called buckets**.

1. **Object Structure:** Each object consists of raw data, a unique Key (its path/name), metadata (key-value pairs describing the file), and a unique Version ID.
2. **Globally Unique Bucket Names:** S3 bucket names share a single global namespace across all AWS accounts worldwide. No two AWS customers can have the same bucket name.
3. **Regionally Scoped:** Although bucket names are globally unique, every bucket is physically deployed and stored within a specific AWS Region (e.g., `ap-south-1`).
4. **Object Size Limits:** Single objects can range from 0 bytes up to **5 TB**. Individual file uploads exceeding **100 MB** should use **Multipart Upload** (mandatory for files larger than 5 GB).
5. **Extreme Durability:** Designed for **11 nines (99.999999999%) of data durability** by automatically replicating data across a minimum of three physical Availability Zones within the Region.

---

### 6.2 Block Storage vs. Object Storage

```text
      BLOCK STORAGE (Amazon EBS)                    OBJECT STORAGE (Amazon S3)
┌──────────────────────────────────────┐     ┌──────────────────────────────────────┐
│  - Raw block device (like a HDD/SSD) │     │  - Flat object namespace             │
│  - Attached to a single EC2 instance │     │  - Independent internet access (API) │
│  - OS formats filesystem (ext4/NTFS) │     │  - Data + Metadata + Unique Key      │
│  - Scoped to a single AZ             │     │  - Regional, Multi-AZ durability     │
│  - Best for OS, fast DB operations   │     │  - Best for images, media, backups   │
└──────────────────────────────────────┘     └──────────────────────────────────────┘
```

| Dimension | Block Storage (Amazon EBS) | Object Storage (Amazon S3) |
| :--- | :--- | :--- |
| **Data Format** | Raw data blocks without intrinsic metadata. | Entire objects comprising data, metadata, and a unique key. |
| **Filesystem** | Managed directly by the operating system (e.g., `ext4`, `NTFS`, `xfs`). | No operating system filesystem; accessed via REST APIs, HTTP, or SDKs. |
| **Connectivity** | Attached to a virtual server via high-speed storage network bus. | Accessed directly over HTTP/HTTPS from anywhere on the internet or within AWS. |
| **Scope** | Restricted to a single Availability Zone. | Replicated across at least 3 AZs across an entire Region. |
| **Primary Use** | OS boot drives, database transaction logs, low-latency disk operations. | Media assets, unstructured big data, static web hosting, long-term backups. |

---

### 6.3 S3 Key Features (Versioning, Object Lock, Transfer Acceleration)

- **S3 Bucket Versioning:**
  - Maintains multiple historical versions of an object under the exact same object key name.
  - S3 does not inspect file contents; every upload with an identical key creates a new version with a distinct Version ID.
  - Deleting an object places a *Delete Marker*; previous versions can still be restored, protecting against accidental overwrites or malicious deletions.
- **S3 Object Lock (WORM - Write Once, Read Many):**
  - Prevents an object version from being deleted or overwritten for a fixed retention period or indefinite legal hold.
  - Complies with financial and regulatory governance standards.
  - **Prerequisite:** S3 Bucket Versioning **must be enabled**.
- **S3 Transfer Acceleration:**
  - Accelerates long-distance uploads and downloads between client machines and S3 buckets.
  - Traffic routes from the client into the nearest **AWS Edge Location**, and then traverses AWS's optimized private global fiber backbone to reach the bucket's home Region.

---

### 6.4 S3 Static Website Hosting

- S3 can serve client-side static web applications (HTML, CSS, JavaScript, client-side React/Vue builds, images) without running an EC2 web server.
- S3 **cannot execute server-side code** (such as Node.js, PHP, Python, or Ruby).
- **Two Deployment Approaches:**
  1. **Direct S3 Website Endpoint:**
     - Enable Static Website Hosting in the bucket properties.
     - Specify the index document (e.g., `index.html`) and error document.
     - Disable "Block Public Access" and apply an S3 Bucket Policy granting public `s3:GetObject` permissions to all principals (`"Principal": "*"`).
  2. **Modern Secure Approach (CloudFront + OAC):**
     - Keep the S3 bucket completely **private**.
     - Place an Amazon CloudFront distribution in front of S3 and configure **Origin Access Control (OAC)** so only CloudFront can read files from the bucket.

---

## 7. Amazon S3 Storage Classes & Intelligent-Tiering Deep Dive

### 7.1 Storage Class Selection Criteria

S3 offers distinct storage classes to balance cost, access patterns, latency, and retention requirements:

```
Frequently Accessed        ────────────────► S3 Standard / S3 Express One Zone
Unknown / Dynamic Patterns ────────────────► S3 Intelligent-Tiering
Infrequent Access          ────────────────► S3 Standard-IA / S3 One Zone-IA
Archived (Immediate)       ────────────────► S3 Glacier Instant Retrieval
Archived (Hours)           ────────────────► S3 Glacier Flexible Retrieval
Long-term Archive (Days)   ────────────────► S3 Glacier Deep Archive
```

---

### 7.2 S3 Storage Classes Breakdown

1. **S3 Standard:**
   - Default general-purpose storage for active, frequently accessed data.
   - Millisecond retrieval latency; replicated across $\ge 3$ AZs. No minimum retention period.
2. **S3 Express One Zone:**
   - High-performance storage class providing consistent **single-digit millisecond data access**.
   - Data is stored redundantly within a **single Availability Zone**. Ideal for compute-intensive AI/ML training and big data analytics.
3. **S3 Standard-IA (Infrequent Access):**
   - For data accessed less frequently but requiring immediate millisecond access when requested.
   - Stored across $\ge 3$ AZs. Features lower storage pricing than S3 Standard, but incurs a per-GB retrieval fee. **30-day minimum storage duration**.
4. **S3 One Zone-IA:**
   - Infrequent access storage residing inside a **single Availability Zone**.
   - Storage cost is 20% less than Standard-IA. Data is lost if that specific AZ is destroyed. Suitable for easily reproducible, non-critical backup data. **30-day minimum duration**.
5. **S3 Glacier Instant Retrieval:**
   - Archive class for rarely accessed records requiring **immediate millisecond retrieval** during audits or medical reviews. **90-day minimum duration**.
6. **S3 Glacier Flexible Retrieval (formerly S3 Glacier):**
   - Cost-effective archive storage for data that does not require immediate access.
   - Requires an asynchronous **restore operation**: Expedited (1–5 min), Standard (3–5 hours), or Bulk (5–12 hours). **90-day minimum duration**.
7. **S3 Glacier Deep Archive:**
   - The lowest-cost storage in AWS for long-term digital preservation and regulatory compliance.
   - Retrieval takes **within 12 hours** (Standard) or **within 48 hours** (Bulk). **180-day minimum duration**.
8. **S3 on Outposts:**
   - Brings S3 object storage capabilities directly to on-premises AWS Outposts hardware.

---

### 7.3 S3 Intelligent-Tiering & Its 5 Access Tiers

> [!IMPORTANT]
> **Fundamental Architectural Rule:**
> **S3 Intelligent-Tiering is a Storage Class.** The tiers within it are **Access Tiers**, not separate storage classes.
> For example: An object's Storage Class remains `S3 Intelligent-Tiering`, while its internal access tier changes dynamically between Frequent, Infrequent, and Archive.

```text
                            NEW OBJECT UPLOADED
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Frequent Access Tier │
                         └──────────┬───────────┘
                                    │ 30 consecutive days without access
                                    ▼
                         ┌──────────────────────┐
                         │Infrequent Access Tier│
                         └──────────┬───────────┘
                                    │ 90 consecutive days without access
                                    ▼
                         ┌──────────────────────┐
                         │Archive Instant Access│  <-- Millisecond access!
                         └──────────┬───────────┘
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         │ (Optional Opt-in)                                   │ (Optional Opt-in)
         ▼                                                     ▼
┌──────────────────┐                                  ┌──────────────────┐
│  Archive Access  │ (90-730 days; 3-5h restore)      │Deep Archive Access (180-730 days; <12h)
└──────────────────┘                                  └──────────────────┘
```

- **How it Works:** S3 continuously tracks object access patterns and moves objects among tiers automatically without manual lifecycle policies or operational overhead.
- **Cost Structure:**
  - Charges a small monthly monitoring and automation fee per eligible object.
  - **There are zero retrieval fees** and zero tier-transition penalties.
- **The 5 Access Tiers:**
  1. **Frequent Access Tier (Default):** For actively used data; standard millisecond latency.
  2. **Infrequent Access Tier:** Automatically activated after **30 consecutive days** of zero access.
  3. **Archive Instant Access Tier:** Automatically activated after **90 consecutive days** of zero access; retains immediate millisecond access speeds.
  4. **Archive Access Tier (Optional):** Asynchronous retrieval (3–5 hours restore); configurable between 90 and 730 days of inactivity.
  5. **Deep Archive Access Tier (Optional):** Lowest archive cost (within 12 hours restore); configurable between 180 and 730 days of inactivity.

---

### 7.4 Critical Technical Rules (128 KB Rule, Costs, Durations)

- **The 128 KB Intelligent-Tiering Rule:**
  - Objects smaller than **128 KB** can be stored in S3 Intelligent-Tiering, but they are **not eligible for automatic tiering**.
  - They are always retained and billed at the Frequent Access tier rates, without incurring monitoring fees.
- **Minimum Storage Duration Matrix:**
  - Placing short-lived data into long-term archive classes incurs early-deletion fees if deleted before the minimum threshold:
    - `S3 Standard` $\rightarrow$ No minimum
    - `S3 Intelligent-Tiering` $\rightarrow$ No minimum
    - `S3 Standard-IA` $\rightarrow$ **30 days**
    - `S3 One Zone-IA` $\rightarrow$ **30 days**
    - `S3 Glacier Instant Retrieval` $\rightarrow$ **90 days**
    - `S3 Glacier Flexible Retrieval` $\rightarrow$ **90 days**
    - `S3 Glacier Deep Archive` $\rightarrow$ **180 days**

---

### 7.5 Storage Classes Comparison Matrix & Mental Models

| Storage Class | Durability | Availability | AZ Redundancy | Retrieval Latency | Min Storage Duration | Retrieval Fee? |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **S3 Standard** | 99.999999999% | 99.99% | $\ge 3$ AZs | Milliseconds | None | None |
| **S3 Express One Zone** | 99.999999999% | 99.95% | 1 AZ | Single-digit ms | None | None |
| **S3 Intelligent-Tiering** | 99.999999999% | 99.9% | $\ge 3$ AZs | Milliseconds* | None | **None** |
| **S3 Standard-IA** | 99.999999999% | 99.9% | $\ge 3$ AZs | Milliseconds | 30 Days | Yes (per GB) |
| **S3 One Zone-IA** | 99.999999999% | 99.5% | 1 AZ | Milliseconds | 30 Days | Yes (per GB) |
| **Glacier Instant** | 99.999999999% | 99.9% | $\ge 3$ AZs | Milliseconds | 90 Days | Yes (per GB) |
| **Glacier Flexible** | 99.999999999% | 99.9% | $\ge 3$ AZs | Minutes to Hours | 90 Days | Yes (per GB) |
| **Glacier Deep Archive**| 99.999999999% | 99.9% | $\ge 3$ AZs | Hours ($\le 12$h) | 180 Days | Yes (per GB) |

*\*Note: The optional Archive Access and Deep Archive Access tiers inside Intelligent-Tiering require asynchronous restore.*

---

## 8. Amazon RDS & Amazon Aurora

### 8.1 Amazon Relational Database Service (RDS)

Amazon RDS is a fully managed cloud service for provisioning, operating, and scaling relational SQL databases.

```text
React Web App ───► Node.js / Express API ───► Amazon RDS for MySQL
(Client Frontend)     (Backend Compute)         (Managed Database Service)
```

- **Supported Engines:** MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, and Amazon Aurora.
- **What AWS Manages:** Hardware provisioning, OS installation and patching, database engine software updates, automated daily backups, point-in-time recovery, Multi-AZ synchronous replication, and hardware failover.
- **What You Manage:** Database schemas, table creation, indexing strategies, query optimization, database user credentials/permissions, and application connection logic.

---

### 8.2 Amazon Aurora

- **Definition:** A cloud-native, high-performance relational database engine developed by AWS, fully compatible with **MySQL** and **PostgreSQL**.
- **Architectural Innovation:**
  - Decouples database **compute** from **storage**.
  - Storage is distributed across a shared, self-healing SSD cluster that automatically expands up to **128 TB**.
  - Automatically replicates **6 copies of your data across 3 Availability Zones** (2 copies per AZ).
  - Delivers up to $5\times$ the throughput of standard MySQL and up to $3\times$ the throughput of standard PostgreSQL on identical hardware.

---

### 8.3 Architectural Relationship: RDS vs. Aurora vs. EC2

```text
                            Amazon RDS (Managed Platform)
                             │
       ┌─────────────────────┼─────────────────────┬──────────────────┐
       ▼                     ▼                     ▼                  ▼
  RDS for MySQL       RDS for PostgreSQL      RDS for Oracle    Amazon Aurora
                                                                      │
                                                ┌─────────────────────┴─────────────────────┐
                                                ▼                                           ▼
                                      Aurora MySQL-Compatible                   Aurora PostgreSQL-Compatible
```

- **RDS is the managed service platform; Aurora is a high-performance engine within RDS.** Stating *"I use RDS"* is incomplete without identifying the underlying engine.
- **EC2 Database vs. RDS Managed Database:**
  - **Self-Hosted Database on EC2:** Maximum OS-level control, but you must manually configure backups, replication, failover, OS patches, and storage scaling.
  - **Managed RDS Database:** AWS manages administrative maintenance, backups, and high availability, allowing developers to focus entirely on application logic.

---

## 9. Amazon VPC (Virtual Private Cloud) & Networking

### 9.1 VPC & Subnet Architecture

- **Amazon VPC:** A logically isolated virtual network dedicated to your AWS account within a specific Region. You have complete control over IP address ranges, subnets, route tables, and gateways.
- **Subnet:** A subdivision of a VPC's IP CIDR block tied to a specific Availability Zone. Subnets isolate and categorize infrastructure tiers:
  - **Public Subnet:** Its associated route table contains a direct route to an **Internet Gateway (IGW)** (`0.0.0.0/0` $\rightarrow$ `igw-xxxx`).
  - **Private Subnet:** Does **not** have a direct route to an Internet Gateway. Protected from inbound internet traffic.

---

### 9.2 Internet Gateway (IGW) & Route Tables

- **Internet Gateway (IGW):** A horizontally scaled, redundant VPC component that enables communication between resources in public subnets and the internet (supporting both inbound and outbound traffic).
- **Route Table:** A set of routing rules (destination CIDR $\rightarrow$ target) that directs network traffic exiting subnets:
  - `10.0.0.0/16` $\rightarrow$ `local` (Keeps internal VPC traffic within the network).
  - `0.0.0.0/0` $\rightarrow$ `igw-xxxx` (Routes all other outbound IPv4 traffic to the Internet Gateway).

---

### 9.3 NAT Gateway & Outbound Traffic Patterns

- **Why Private Resources Need Internet:** Backend EC2 instances and databases in private subnets must remain inaccessible from direct public inbound connections, yet frequently require outbound internet access to download OS security patches, install npm/pip libraries, pull Docker images, or invoke third-party payment APIs.
- **NAT Gateway (Network Address Translation):**
  - Placed inside a **public subnet** and assigned an **Elastic IP address**.
  - Enables instances in private subnets to initiate outbound connections to the internet while preventing external internet entities from initiating inbound connections to those private instances.

---

### 9.4 End-to-End VPC Traffic Architecture Flow

```text
                                 PUBLIC INTERNET
                                       ▲
                                       │ Inbound Web Traffic / Outbound Responses
                                       ▼
                            ┌─────────────────────┐
                            │  Internet Gateway   │
                            └──────────┬──────────┘
                                       │
     ══════════════════════════════════╪═════════════════════════════════
     VPC Boundary (CIDR: 10.0.0.0/16)  │
                                       ▼
     ┌─────────────────────────────────────────────────────────────────┐
     │ PUBLIC SUBNET (CIDR: 10.0.1.0/24)                               │
     │ Route Table: 0.0.0.0/0 ──► Internet Gateway                     │
     │                                                                 │
     │   ┌────────────────────────┐      ┌─────────────────────────┐   │
     │   │ Application Load       │      │ NAT Gateway             │   │
     │   │ Balancer (ALB)         │      │ (Assigned Elastic IP)   │   │
     │   └───────────┬────────────┘      └────────────▲────────────┘   │
     └───────────────┼────────────────────────────────┼────────────────┘
                     │ Inbound Internal Forwarding    │ Outbound Patch/API
                     ▼                                │ Traffic
     ┌────────────────────────────────────────────────┼────────────────┐
     │ PRIVATE SUBNET (CIDR: 10.0.2.0/24)             │                │
     │ Route Table: 0.0.0.0/0 ──► NAT Gateway         │                │
     │                                                │                │
     │   ┌────────────────────────┐                   │                │
     │   │ Backend EC2 Instances  ├───────────────────┘                │
     │   │ (App / API Servers)    │                                    │
     │   └───────────┬────────────┘                                    │
     │               │ Internal DB Traffic                             │
     │               ▼                                                 │
     │   ┌────────────────────────┐                                    │
     │   │ Database Tier          │                                    │
     │   │ (Amazon RDS / Aurora)  │                                    │
     │   └────────────────────────┘                                    │
     └─────────────────────────────────────────────────────────────────┘
```

---

### 9.5 Network Security: Security Groups vs. Network ACLs (NACLs)

AWS implements a defense-in-depth model utilizing two complementary firewall layers:

| Feature / Dimension | Security Group (SG) | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Operational Level** | **Resource/Instance Level** (ENI) | **Subnet Level** |
| **Scope of Protection** | Specific instances assigned to the SG | All traffic entering/leaving the subnet |
| **State Nature** | **Stateful:** Inbound reply traffic is automatically allowed back out regardless of outbound rules. | **Stateless:** Return traffic must be explicitly allowed by rules in both directions. |
| **Rule Capabilities** | **Allow rules only** (No explicit deny rules) | Supports both **Allow and Deny rules** |
| **Rule Processing** | Evaluates all rules simultaneously | Evaluates rules sequentially by **rule number** |
| **Evaluation Order** | First line of defense for **outgoing** responses | First line of defense for **incoming** traffic |
| **Capacity** | Up to 50 inbound and 50 outbound rules | Numbered rules (e.g., 100, 200, 300) |

---

### 9.6 CIDR (Classless Inter-Domain Routing)

- **Definition:** Standard notation used to represent contiguous IPv4 IP address blocks (`<Base IP>/<Prefix Length>`).
- **Bit Allocation:** An IPv4 address comprises 32 bits. The prefix (e.g., `/16` or `/24`) defines the fixed network bits; the remaining bits identify individual host interfaces.
  - `/16` $\rightarrow 32 - 16 = 16$ host bits $\rightarrow 2^{16} = 65,536$ total IP addresses.
  - `/24` $\rightarrow 32 - 24 = 8$ host bits $\rightarrow 2^8 = 256$ total IP addresses.
- **AWS Reserved IPs:** In every VPC subnet, **AWS reserves 5 IP addresses** (first 4 and last 1) for network routing, DNS, and broadcast. A `/24` subnet provides $256 - 5 = 251$ usable IP addresses.

---

### 9.7 VPC Peering

- **Definition:** A direct, private networking connection between two VPCs that enables resource communication using private IP addresses.
- **Traffic Routing:** Peered traffic never traverses the public internet, reducing exposure and latency.
- **Core Constraints:**
  1. **Non-Overlapping CIDRs:** The two VPCs cannot have overlapping IP ranges (e.g., `10.0.0.0/16` and `10.1.0.0/16` can peer, but two `10.0.0.0/16` VPCs cannot).
  2. **Explicit Routing Required:** Peering connections require manual routing entries in the route tables of both VPCs.
  3. **Non-Transitive:** Peering is strictly point-to-point. If VPC A is peered with VPC B, and VPC B is peered with VPC C, **VPC A cannot communicate with VPC C through VPC B**. Direct communication requires a dedicated peering link or an **AWS Transit Gateway**.

---

## 10. Amazon Route 53 (DNS & Traffic Management)

### 10.1 Route 53 Fundamentals & Resolution Workflow

Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service running on standard **port 53**.

```text
[User Browser]
      │
      │ 1. Resolves "www.example.com"
      ▼
[Amazon Route 53 DNS Servers]
      │
      │ 2. Returns Alias IP / DNS Record (e.g., ALB or CloudFront Endpoint)
      ▼
[User Browser]
      │
      │ 3. Direct HTTP/HTTPS connection established to resolved endpoint
      ▼
[Target Application: ALB / CloudFront / S3 Website / EC2]
```

> [!NOTE]
> **Key Architecture Rule:** Route 53 **does not carry application payload traffic**. It resolves domain names to destination endpoints during the initial DNS handshake; the client browser then connects directly to the returned endpoint.

---

### 10.2 Routing Policies, Health Checks & DNS Record Types

#### Common Routing Policies
- **Simple Routing:** Maps a domain to a single resource destination.
- **Weighted Routing:** Distributes traffic across multiple endpoints according to assigned percentage weights (e.g., 80% to v1, 20% to v2 for canary testing).
- **Latency-Based Routing:** Routes user traffic to the AWS Region that provides the lowest network latency.
- **Failover Routing:** Directs traffic to a primary resource when healthy, and fails over to a secondary disaster recovery site when the primary fails a health check.

#### DNS Record Types
- **A Record:** Maps a domain name directly to an IPv4 address.
- **AAAA Record:** Maps a domain name to an IPv6 address.
- **CNAME Record:** Points a hostname to another hostname (canonical name).
- **Alias Record (AWS-Specific):** An Amazon-specific extension that points a root domain (zone apex, e.g., `example.com`) directly to internal AWS resources (such as CloudFront distributions or ALBs).

---

## 11. AWS CloudWatch & Amazon SNS (Monitoring & Alerts)

### 11.1 Amazon CloudWatch (Metrics, Logs, Alarms, Dashboards)

Amazon CloudWatch is an observability and monitoring service providing visibility across AWS resources and applications.

- **Metrics:** Continuous numeric time-series measurements (e.g., EC2 `CPUUtilization`, `DiskReadBytes`, `NetworkIn`).
- **Logs:** Aggregates and stores log records emitted by operating systems, containers, Lambda functions, and application frameworks.
- **Alarms:** Monitors metric thresholds (e.g., `CPUUtilization > 80% for 5 minutes`) and triggers automated actions, such as publishing messages to Amazon SNS or invoking Auto Scaling policies.
- **Dashboards:** Customized web console views aggregating real-time metrics and diagnostic graphs.

---

### 11.2 Amazon Simple Notification Service (SNS)

Amazon SNS is a fully managed **publish-subscribe (Pub/Sub) messaging service** for high-throughput, fan-out event distribution.

- **Publisher:** Systems that produce and emit messages (e.g., CloudWatch Alarms, custom microservices).
- **Topic:** A logical communication channel and message broker.
- **Subscribers:** Endpoints subscribed to a topic that receive forwarded messages asynchronously:
  - Email notifications
  - SMS messages
  - AWS Lambda functions
  - Amazon SQS queues
  - HTTP/HTTPS webhook endpoints

---

### 11.3 CloudWatch + SNS Integration Architecture

```text
 ┌────────────────┐
 │  EC2 Instance  │
 └───────┬────────┘
         │ Metric Data (CPUUtilization)
         ▼
 ┌────────────────────────┐
 │    Amazon CloudWatch   │
 │   Alarm: CPU > 80%     │
 └───────┬────────────────┘
         │ Triggers Alarm Action
         ▼
 ┌────────────────────────┐
 │     Amazon SNS Topic   │
 └───────┬────────────────┘
         │ Fans out message to all subscribers
         ├────────────────────────┬────────────────────────┐
         ▼                        ▼                        ▼
 ┌───────────────┐        ┌───────────────┐        ┌───────────────┐
 │ Email Alert   │        │ SMS to On-Call│        │ Lambda Remed- │
 │ to DevOps Team│        │ Engineer      │        │ iation Action │
 └───────────────┘        └───────────────┘        └───────────────┘
```

> [!TIP]
> **CloudWatch vs. SNS Mental Model:**
> - **CloudWatch** answers: *"What is currently happening inside my infrastructure?"*
> - **SNS** answers: *"Who needs to be notified, and where should this alert be sent?"*

---

## 12. AWS CodeBuild & CI/CD Pipelines

### 12.1 AWS CodeBuild Overview & Workflow

- **Definition:** A fully managed continuous integration service that compiles source code, executes test suites, installs build dependencies, and packages software into deployable build artifacts.
- **Serverless Compute:** Eliminates the operational overhead of provisioning and patching dedicated build servers (such as Jenkins controller/worker nodes). CodeBuild provisions isolated, temporary compute containers for the build duration and terminates them immediately upon completion.
- **Source Integrations:** Natively fetches code from GitHub, GitHub Enterprise, Bitbucket, and AWS CodeCommit.

---

### 12.2 The `buildspec.yml` Lifecycle & S3 Artifact Storage

CodeBuild is driven by a YAML configuration file named **`buildspec.yml`** located in the root of the source repository:

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - echo "Installing dependencies..."
      - npm install
  pre_build:
    commands:
      - echo "Running unit tests and linters..."
      - npm test
  build:
    commands:
      - echo "Compiling application..."
      - npm run build
  post_build:
    commands:
      - echo "Build completed successfully on $(date)"
artifacts:
  files:
    - dist/**/*
    - package.json
```

- **S3 Artifact Destination:** Compiled outputs (production bundles, ZIP distributions, static assets) are packaged and exported directly to an **Amazon S3 bucket** for storage and subsequent deployment.

---

### 12.3 Full CI/CD Orchestration with AWS CodePipeline

```text
[Developer Push] ──► [GitHub Repository]
                           │
                           ▼
               [AWS CodePipeline: Source Stage]
                           │
                           ▼
               [AWS CodePipeline: Build Stage (AWS CodeBuild)]
                           │ ├── Install dependencies
                           │ ├── Compile code
                           │ └── Execute unit tests
                           ▼
               [Amazon S3: Deployable Artifact Storage]
                           │
                           ▼
               [AWS CodePipeline: Deploy Stage]
                           │
                           ▼
          [Target Infrastructure: EC2 / ECS / Lambda / S3]
```

- **Architectural Distinction:** **CodeBuild is exclusively a build-and-test execution engine**; it is not an end-to-end continuous delivery orchestrator. **AWS CodePipeline** provides the overall orchestration that connects Source, Build, Test, Manual Approval, and Deployment stages.

---

## 13. Infrastructure as Code (IaC) & AWS CloudFormation

### 13.1 IaC Concepts, Advantages & Tooling

- **Infrastructure as Code (IaC):** The architectural discipline of provisioning, configuring, and managing IT infrastructure resources through declarative configuration files rather than manual point-and-click operations in web consoles.
- **Key Advantages:**
  - **Repeatability & Consistency:** Eliminates configuration drift across Dev, Test, Staging, and Production environments.
  - **Version Control & Auditability:** Infrastructure code is stored in Git, enabling code reviews, pull requests, and audit histories.
  - **Automation & Velocity:** Enables automated CI/CD pipeline deployments.

---

### 13.2 AWS CloudFormation (Templates & Stacks)

- **Definition:** AWS's native Infrastructure as Code service that models and provisions cloud resources using declarative templates formatted in **YAML or JSON**.
- **CloudFormation Stacks:** Resources specified within a template are instantiated, updated, and deleted together as an atomic operational unit called a **Stack**.
- **State & Drift Detection:** CloudFormation tracks the real-world operational status of resources against the template definition and can detect manual configuration drift.

---

### 13.3 AWS Elastic Beanstalk (PaaS Application Deployment)

- **Definition:** An AWS **Platform as a Service (PaaS)** designed to quickly deploy, scale, and manage web applications and API services without manually assembling infrastructure components.
- **Supported Platforms:** Node.js, Python, Java, Docker, Go, .NET, and PHP.
- **Underlying Automation:** The developer uploads application code; Elastic Beanstalk automatically provisions and configures the required EC2 instances, Application Load Balancers, Auto Scaling groups, security groups, and CloudWatch alarms behind the scenes.

---

### 13.4 Architectural Comparison: EC2 vs. Beanstalk vs. CloudFormation

```
                  LEVEL OF ABSTRACTION & CONTROL
Low Abstraction ◄────────────────────────────────► High Abstraction
(Full OS Control)                                (Code-Centric PaaS)

    [Amazon EC2]        [AWS CloudFormation]      [Elastic Beanstalk]
  Manual management     Declarative automation    Automated application
  of virtual servers     of entire AWS stacks      platform (Upload code)
```

| Dimension | Amazon EC2 (IaaS) | AWS CloudFormation (IaC) | AWS Elastic Beanstalk (PaaS) |
| :--- | :--- | :--- | :--- |
| **Model** | Infrastructure as a Service | Infrastructure as Code | Platform as a Service |
| **Primary Focus** | Managing virtual servers directly | Declaratively orchestrating any AWS resource | Rapidly deploying application code |
| **User Input** | Manual configuration and OS setup | JSON / YAML template defining architecture | Application code package / Docker image |
| **Infrastructure Management** | Manual patching, scaling, and networking | Automated stack creation based on code | Fully automated management by AWS |

---

## 14. Containers & Container Orchestration (ECS, EKS, ECR & Fargate)

### 14.1 Amazon ECS (Elastic Container Service)

- **Definition:** An AWS-native, highly scalable **container orchestration service** engineered to deploy, manage, and scale Docker containers across compute clusters.
- **Simplicity:** Natively integrates with AWS IAM, CloudWatch, ALBs, and VPCs without the operational overhead of running a Kubernetes control plane.

---

### 14.2 Amazon EKS (Elastic Kubernetes Service)

- **Definition:** A managed Kubernetes service that runs upstream open-source **Kubernetes** clusters in the AWS cloud.
- **Kubernetes Ecosystem:** Compatible with standard Kubernetes tools (such as `kubectl`, Helm, and Prometheus). Ideal for enterprises seeking multi-cloud portability and standardized Kubernetes workloads.

---

### 14.3 ECS vs. EKS Comparison & Compute Options (EC2 vs. Fargate)

```text
                     CONTAINER ORCHESTRATION SERVICE
                      ┌──────────────┬──────────────┐
                      │  Amazon ECS  │  Amazon EKS  │
                      └──────┬───────┴──────┬───────┘
                             │              │
                             ▼              ▼
                     UNDERLYING COMPUTE INFRASTRUCTURE
                      ┌─────────────────────────────┐
                      │    EC2 Launch Type          │  <-- You manage host virtual machines
                      ├─────────────────────────────┤
                      │    AWS Fargate              │  <-- Serverless compute; zero server management
                      └─────────────────────────────┘
```

| Dimension | Amazon ECS | Amazon EKS |
| :--- | :--- | :--- |
| **Full Name** | Elastic Container Service | Elastic Kubernetes Service |
| **Orchestrator** | AWS proprietary container orchestrator | Upstream Open-Source Kubernetes |
| **Complexity** | Lower; minimal learning curve | Higher; requires Kubernetes domain expertise |
| **Portability** | AWS-centric | Universal across AWS, Azure, GCP, on-premises |
| **Serverless Engine**| AWS Fargate | AWS Fargate |

- **Compute Launch Types:**
  - **EC2 Launch Type:** You provision, manage, scale, and patch the underlying EC2 instances hosting the containers.
  - **AWS Fargate (Serverless):** You configure CPU and memory requirements at the container level; AWS provisions, manages, and scales the underlying compute infrastructure automatically.

---

### 14.4 Amazon ECR (Elastic Container Registry)

- **Definition:** A fully managed, highly available private Docker container registry used to store, manage, scan, and distribute container images.
- **Workflow Pipeline:**
  1. Build Docker image locally (`docker build -t app:v1 .`).
  2. Authenticate and push image to ECR repository (`docker push <account>.dkr.ecr.<region>.amazonaws.com/app:v1`).
  3. ECS or EKS pulls the image from ECR to instantiate running containers across clusters.

> [!NOTE]
> **Container Trinity:**
> - **ECR** $\rightarrow$ Stores container images (The Registry)
> - **ECS / EKS** $\rightarrow$ Orchestrates and coordinates container workloads (The Brain)
> - **EC2 / Fargate** $\rightarrow$ Provides the physical compute hardware (The Muscle)

---

## 15. HashiCorp Terraform Deep Dive

### 15.1 Terraform Fundamentals & CLI Workflow

- **Definition:** An open-source, multi-cloud Infrastructure as Code provisioning tool created by HashiCorp. Written in **HCL (HashiCorp Configuration Language)**.
- **Multi-Cloud Reach:** Unlike AWS CloudFormation (which is AWS-specific), Terraform manages infrastructure across hundreds of cloud providers (including AWS, Azure, GCP, Kubernetes, and GitHub) through extensible provider plugins.

```text
 ┌──────────────────────┐
 │ Write HCL (.tf) Code │
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │    terraform init    │  (Downloads required provider plugins & modules)
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │   terraform plan     │  (Calculates execution diff: Desired vs. Actual State)
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │   terraform apply    │  (Invokes provider APIs to provision live infrastructure)
 └──────────┬───────────┘
            │
            ▼
 ┌──────────────────────┐
 │ Infrastructure Live  │
 └──────────────────────┘
```

---

### 15.2 Terraform State (`terraform.tfstate` & `terraform.tfstate.backup`)

- **`terraform.tfstate`:**
  - A structured JSON document tracking every infrastructure resource managed by Terraform, including its cloud provider IDs, attributes, dependencies, and metadata.
  - Acts as the single source of truth mapping your declarative `.tf` configuration files to real-world cloud resources.
  - During `terraform plan` and `terraform apply`, Terraform compares configuration files against the current state file and refreshed provider APIs to determine necessary additions, updates, or deletions.
- **`terraform.tfstate.backup`:**
  - An automatic backup copy of the state generated immediately prior to applying state modifications, serving as a safety fallback.

> [!CAUTION]
> **State File Security Warning:**
> The `terraform.tfstate` file can store sensitive plaintext data (such as initial database passwords, private keys, and environment secrets). **Never commit `terraform.tfstate` or `terraform.tfstate.backup` into public Git repositories.**

---

### 15.3 Remote State Storage, Security & Locking

In multi-engineer team environments, maintaining state locally on individual laptops leads to state divergence and race-condition write collisions.

- **Remote Backend (AWS Production Standard):**
  - **Storage:** State is stored centrally in a private, encrypted **Amazon S3 bucket** with Versioning enabled.
  - **State Locking:** Uses an **Amazon DynamoDB table** (with a `LockID` primary key) or native S3 locking to lock the state during write operations. This prevents concurrent execution of `terraform apply` by multiple engineers.

```text
Engineer 1 ──┐
             │
Engineer 2 ──┼──► Remote State (Amazon S3) + State Locking (DynamoDB)
             │
Engineer 3 ──┘
```

---

### 15.4 Input Variables & Output Variables

```text
Values passed IN                 Terraform Engine                Values exposed OUT
──────────────────► ┌────────────────────────────────────────┐ ───────────────────►
- instance_type     │  resource "aws_instance" "web" {       │ - public_ip
- vpc_cidr          │    instance_type = var.instance_type   │ - database_endpoint
- environment_name  │  }                                     │ - load_balancer_dns
                    └────────────────────────────────────────┘
```

#### Input Variables (`variables.tf`)
Parameterize configurations to make code modular, reusable, and dynamic:

```hcl
variable "instance_type" {
  description = "The compute size of the virtual server"
  type        = string
  default     = "t3.micro"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
}
```

#### Output Variables (`outputs.tf`)
Expose useful computed values returned by cloud providers after resources are provisioned:

```hcl
output "web_public_ip" {
  description = "The assigned public IPv4 address"
  value       = aws_instance.web.public_ip
}
```

---

### 15.5 Terraform Resource Replacement: Taint vs. Modern `-replace`

#### The Legacy Concept (`terraform taint`)
- In older Terraform releases, marking a resource as "tainted" told Terraform that the object had become degraded and needed to be destroyed and recreated on the subsequent apply:
  ```bash
  terraform taint aws_instance.web
  terraform untaint aws_instance.web  # Removed tainted flag
  ```

#### Modern Standard (`-replace` Flag)
- `terraform taint` is **deprecated**.
- The modern, recommended pattern instructs Terraform directly during execution planning without mutating state files prematurely:
  ```bash
  # Plan the replacement:
  terraform plan -replace="aws_instance.web"

  # Apply the replacement directly:
  terraform apply -replace="aws_instance.web"
  ```

---

### 15.6 Terraform Modules (Architecture, Root vs. Child, Registry)

A **Terraform Module** is a container for multiple resources that are used together. Modules make infrastructure reusable, modular, and maintainable across environments (analogous to writing a reusable function in programming).

#### Standard Module File Structure
```text
my-terraform-project/
├── main.tf              <-- Root Module
├── variables.tf
├── outputs.tf
└── modules/
    ├── vpc/             <-- Child Module: VPC
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    └── ec2/             <-- Child Module: EC2 Server
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

#### Module Invocation Syntax
```hcl
module "production_web_server" {
  source        = "./modules/ec2"
  instance_type = "t3.large"
  subnet_id     = module.vpc.public_subnet_id
}
```

- **Root Module:** The working directory where `terraform apply` is executed.
- **Child Module:** An external module called from within another module.
- **Terraform Registry:** A public directory of community and verified vendor modules (e.g., official AWS VPC modules) that can be imported directly to avoid writing infrastructure from scratch.

---
*Document compiled and structured from AWS technical study notes.*
