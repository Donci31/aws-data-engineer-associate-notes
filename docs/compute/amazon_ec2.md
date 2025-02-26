# Amazon EC2

**Amazon Elastic Compute Cloud (EC2)** is a fundamental service within the Amazon Web Services (AWS) ecosystem, providing scalable compute capacity in the cloud. It allows businesses and developers to run applications on virtual servers without investing in or maintaining physical infrastructure. EC2 is part of AWS's IaaS (Infrastructure-as-a-Service) offerings and provides extensive flexibility, control, and cost optimization options.

## 1. **EC2 Instances**

At the core of Amazon EC2 is the **EC2 instance** – a virtual server running in AWS's data center that can host applications, databases, and services. The instance is fully customizable, offering a variety of configurations tailored to meet different computational needs.

### Instance Types

EC2 instances are categorized based on their resource profile and optimized use case. The major families include:

- **General Purpose Instances**: Balanced compute, memory, and network resources for a variety of diverse workloads.
    - **T-series** (e.g., `t4g`, `t3`): Burstable performance instances.
    - **M-series** (e.g., `m6i`, `m5`): Balanced resources for many types of workloads.

- **Compute Optimized**: Instances designed for compute-heavy workloads such as batch processing, video encoding, etc.
    - **C-series** (e.g., `c7g`, `c5`): High-performance processors.

- **Memory Optimized**: For workloads requiring high memory capacity, such as in-memory caches and real-time big data analytics.
    - **R-series** (e.g., `r6g`, `r5`): Memory-intensive tasks.

- **Storage Optimized**: For workloads that require high, sequential read and write access to very large datasets.
    - **I-series** (e.g., `i3`, `i3en`): Local storage-heavy tasks.

- **Accelerated Computing**: These instances provide specialized hardware accelerators like GPUs or FPGAs, useful for machine learning, artificial intelligence, or scientific computation.
    - **P-series** (e.g., `p4d`): GPU-powered instances for ML and high-performance computing.
    - **Inf1**: Instances designed for deep learning inference.

## 2. **Auto Scaling**

**Auto Scaling** is a feature that automatically adjusts the number of EC2 instances in response to demand, ensuring high availability and cost efficiency.

- **Scaling Policies**: Auto Scaling can be configured to add or remove instances based on specific metrics (e.g., CPU utilization, memory usage, etc.).
- **Health Checks**: You can leverage both Amazon EC2 instance status checks and ELB health checks to make scaling and health decisions about instances within the group. By default, it uses EC2 status check. When an instance is reported as impaired status, Amazon EC2 Auto Scaling waits a few minutes for the instance to recover and otherwise terminates it. Amazon EC2 Auto Scaling doesn’t terminate an instance in based on Amazon EC2 status checks and Elastic Load Balancing (ELB) health checks until the health check grace period expires.

By integrating **Auto Scaling** with **Elastic Load Balancing (ELB)**, you can ensure that traffic is always distributed across healthy, scalable instances, optimizing application performance.

Amazon EC2 Auto Scaling doesn’t terminate an instance in based on Amazon EC2 status checks and Elastic Load Balancing (ELB) health checks until the health check grace period expires.

### Launch Template

A launch template specifies instance configuration information. It includes the ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and other parameters used to launch EC2 instances.

When you create a Launch Template, the default value for the instance tenancy is shared and the instance tenancy is controlled by the tenancy attribute of the VPC. If you set the Launch Template Tenancy to shared (default) and the VPC Tenancy is set to dedicated, then the instances have dedicated tenancy. If you set the Launch Template Tenancy to dedicated and the VPC Tenancy is set to default, then again the instances have dedicated tenancy.

You can only change the tenancy of an instance from dedicated to host, or from host to dedicated after you’ve launched it.

## 3. **Networking in EC2**

### a. **Virtual Private Cloud (VPC)**

An EC2 instance runs within a **Virtual Private Cloud (VPC)**, a logically isolated network that allows you to define your IP address range, subnets, and route tables. Each VPC is isolated from other VPCs in AWS, but you can connect them using VPC Peering or Transit Gateway.

- **Subnets**: Segments of a VPC's IP address range that can house instances. You can have public and private subnets.
- **Security Groups**: Virtual firewalls that control inbound and outbound traffic to instances.
- **Network ACLs**: Provide an additional layer of security at the subnet level, filtering traffic entering and leaving subnets.

### b. **Elastic Load Balancer (ELB)**

**Elastic Load Balancing (ELB)** automatically distributes incoming traffic across multiple EC2 instances to ensure high availability and fault tolerance. ELB helps protect against a sudden surge in traffic by dynamically scaling as needed. There are three types of ELBs:

- **Classic Load Balancer**: Provides basic load balancing for EC2 instances.
- **Application Load Balancer (ALB)**: Best for HTTP and HTTPS traffic, routing requests based on URL path or hostname.
- **Network Load Balancer (NLB)**: Designed for handling TCP traffic at very high throughput with low latency.

### c. **Elastic IP (EIP)**

An **Elastic IP** is a static IPv4 address designed for dynamic cloud computing. Unlike a regular public IP address, an Elastic IP can be reassigned to any EC2 instance within the same region, enabling resilience and flexibility in case of failure. This ensures that even if your instance fails, the IP can be remapped to another instance, maintaining uptime and continuity.

## 4. **Amazon EC2 Storage Options**

### a. **Amazon EBS (Elastic Block Store)**

EC2 instances often use **Amazon Elastic Block Store (EBS)** for persistent storage. EBS volumes provide high-availability block-level storage that persists even when the instance is stopped or terminated. Types of EBS volumes include:

- **General Purpose SSD (gp3)**: Balanced price and performance for most workloads.
- **Provisioned IOPS SSD (io2)**: High-performance volumes for I/O-intensive applications.
- **Magnetic**: Low-cost, low-performance option for archival data.

### b. **Instance Store**

**Instance Store** provides temporary block-level storage that is physically attached to the host server. Data is lost when the instance is stopped, so it is only suitable for non-persistent storage.

### c. **Amazon S3 (Simple Storage Service)**

While S3 is not directly attached to EC2 instances, it is frequently used alongside EC2 for storing large amounts of unstructured data, backups, or logs. S3 provides a scalable, durable, and low-cost storage solution for use cases that require large-scale data storage and retrieval.

## 5. **Security Features in EC2**

### a. **Security Groups and Key Pairs**

- **Security Groups**: Act as a virtual firewall for EC2 instances. They define the allowed inbound and outbound traffic, functioning at the instance level. Security Groups are stateful, meaning that if you allow inbound traffic, the response traffic is automatically allowed, regardless of outbound rules.
- **Key Pairs**: Key pairs are used for secure SSH access to EC2 instances. When launching an EC2 instance, you must associate it with an existing key pair or create a new one. The private key must be kept safe to ensure secure access.

### b. **IAM Roles and Policies**

**AWS Identity and Access Management (IAM)** is used to define granular permissions for users, groups, and roles to control access to EC2 instances and other AWS services. IAM roles can be assigned to EC2 instances to allow them to interact with other AWS services, such as S3 or DynamoDB, without embedding access credentials in your application.

### c. **VPC endpoint**

Data in transit can be secured by enabling VPC endpoint encryption, ensuring that data is protected during transmission between EC2 and EBS.

## 6. **Pricing Models**

### a. **On-Demand Instances**

On-demand instances allow you to pay for compute capacity by the hour with no long-term commitments. This model is ideal for short-term workloads or unpredictable traffic patterns.

### b. **Reserved Instances**

Reserved instances require a commitment to a specific instance type and region for a one- or three-year term. In return, AWS offers significant discounts (up to 75%) compared to on-demand instance pricing. This is ideal for stable, predictable workloads.

- Reserved Instance Marketplace: A platform for selling unused Reserved Instances, catering to changing business needs such as region shifting, instance type updates, or unused capacity.

### c. **Spot Instances**

**Spot Instances** allow you to purchase unused EC2 capacity at a significantly reduced rate compared to on-demand prices. Spot Instances can be terminated by AWS at any time with a two-minute warning, so they are suitable for flexible, fault-tolerant applications.

- **Spot Fleet**: A set of Spot Instances that can be managed collectively to meet a specified target capacity.
- **Spot Blocks**: A way to request Spot Instances for a fixed duration (1 to 6 hours), guaranteeing that the instance will run uninterrupted during the block period.

### d. **Savings Plans**

**EC2 Savings Plans** offer flexible pricing for EC2 usage in exchange for a commitment to consistent usage for a one- or three-year term. There are two types of Savings Plans:

- **Compute Savings Plans**: Applies to any EC2 instance, regardless of region or instance type.
- **EC2 Instance Savings Plans**: Applies to a specific instance family within a region

## 7. **AWS Graviton**

AWS Graviton refers to a family of custom ARM-based processors developed by Amazon Web Services (AWS) to deliver high performance, cost efficiency, and power savings for cloud workloads. These processors are designed to provide superior price/performance compared to traditional x86-based processors, enabling organizations to run a variety of workloads with enhanced efficiency and lower operational costs.

Graviton processors are built using the ARM architecture, which is known for its energy efficiency, and they are optimized for a wide range of EC2 instances. This includes general-purpose, compute-optimized, memory-optimized, and storage-optimized instances. The Graviton2 and the more recent Graviton3 processors are available in multiple EC2 instance types, offering significant performance improvements over previous generations of AWS's general-purpose processors.

For data processing tasks that are both compute-intensive and require cost-effective solutions, the c6g.2xlarge instances, powered by AWS Graviton2 processors, are the optimal choice. AWS Graviton2 processors provide up to 7x the performance of the first-generation Graviton processors, offering superior price-performance benefits for a wide range of workloads, including the compute and memory-intensive operations involved in preparing data for machine learning.

## 8. **Tenancy Options**

Amazon EC2 provides three options for the tenancy (way in which compute resources are allocated in physical servers) of your EC2 instances:

1. **Shared:** Default, shared hardware among AWS accounts.
2. **Dedicated:** Exclusive single-tenant hardware.
3. **Dedicated Host:** Full control over server resources, beneficial for legacy software licensing and compliance.

## 9. **AMI**

An Amazon Machine Image (AMI) provides the information required to launch an instance. An AMI includes the following:

- One or more Amazon EBS snapshots, or, for instance-store-backed AMIs, a template for the root volume of the instance.
- Launch permissions that control which AWS accounts can use the AMI to launch instances.
- A block device mapping that specifies the volumes to attach to the instance when it’s launched
