# Amazon Elastic File System (EFS)

Amazon Elastic File System (EFS) is a highly scalable, serverless, and cloud-native file storage service designed by AWS. It enables you to create a shared file system in the cloud that can be accessed by multiple Amazon EC2 instances and other AWS services, providing seamless integration for applications requiring shared file access. EFS is built to scale automatically, with high availability, durability, and multi-AZ (Availability Zone) resilience, making it an ideal solution for growing applications in the cloud. 

## Key Features

### **Elastic Scalability and Cost Efficiency**
EFS automatically scales up or down based on the amount of data stored. There is no need for pre-provisioning or manual adjustments, which means you can avoid over-provisioning costs. EFS offers pay-as-you-go pricing, where you only pay for the actual storage consumed, making it highly cost-efficient for variable workloads.

### **High Availability and Durability**
EFS is designed to be multi-AZ resilient, meaning that it automatically replicates data across multiple Availability Zones within a region. This ensures high availability and fault tolerance in case of an Availability Zone failure. EFS provides high durability with 99.999999999% (11 nines) durability for stored data, making it a reliable solution for critical applications.

### **Multiple Throughput Modes**
EFS offers two throughput modes, which allow you to select the most appropriate performance level for your application:

- **Bursting Throughput**: This mode is the default setting for EFS and automatically scales throughput as the file system grows. Bursting Throughput is ideal for applications with variable throughput requirements and is designed to handle spiky workloads, providing high throughput during peak usage times.
  
- **Provisioned Throughput**: This mode allows you to provision a fixed throughput (measured in MiB/s) for your file system, independent of the amount of data stored. Provisioned Throughput is recommended for workloads with high throughput needs relative to their data storage size, such as applications requiring high IOPS, low-latency, or constant throughput for data processing.

- **Elastic Troughput**: Automatically scales throughput based on workload demands, up to 3 GiB/s for reads and 1 GiB/s for writes; ideal for unpredictable workloads

In **Provisioned Throughput**, you can modify throughput at any time, while in **Bursting Throughput**, throughput scales automatically as the storage size increases.

### **Storage Classes for Cost Optimization**
EFS offers two storage classes, each optimized for different access patterns:

- **EFS Standard**: This is the default storage class, optimized for frequent access to stored files. It’s suitable for applications requiring low-latency access to data and high throughput.
  
- **EFS Infrequent Access (IA)**: This storage class is designed for files that are not frequently accessed, providing up to 92% lower storage costs compared to the standard storage class. Files in the IA tier are cheaper to store, but retrieval may incur additional charges. To transition data into the IA tier, you must enable **EFS Lifecycle Management**, which automatically moves files to IA based on predefined policies (e.g., if the files haven’t been accessed in 30 days).

- **Archive**: storage option designed for data that is rarely accessed but must be retained for long periods, such as backup data, infrequently accessed files, or cold storage.

### **Compatibility and Platform Restrictions**
- **Linux-Only Support**: EFS is compatible only with Linux-based instances, and it uses the NFS protocol for file sharing. This makes it an excellent choice for applications running on Linux and other Unix-based systems.
- **Windows Incompatibility**: EFS is not supported for Windows instances, so Windows workloads must explore other storage options, such as Amazon FSx for Windows File Server or Amazon EBS.

### **Security and Access Control**
EFS integrates with several AWS security and access management features to ensure secure access and proper file permissions:

- **Network Access Control**: You can control access to your EFS file system using **VPC security groups** to regulate network traffic between EC2 instances and the EFS file system. Additionally, you can configure **IAM policies** to control which EC2 instances and users can mount and access the file system.
  
- **POSIX Permissions**: EFS supports POSIX-compliant user and group permissions, which provide fine-grained access control. When mounting the file system, the user IDs (UIDs) and group IDs (GIDs) specified by NFS clients are used to enforce access permissions. This ensures that only authorized users or applications can read/write to specific files or directories within the file system.

- **EFS Access Points**: These provide a way to define and manage access permissions at a more granular level. You can use access points to enforce specific user and group IDs for different applications or workloads accessing the file system, helping manage access control at the application level.

### **Concurrent Connections and Performance**
EFS supports thousands of concurrent NFS connections, making it suitable for large-scale distributed applications that require shared file access across multiple instances or services. It is ideal for workloads such as web serving, content management, development environments, and big data applications, where multiple compute resources need simultaneous access to the same files.

### **File Lifecycle Management**
EFS supports file lifecycle management through **EFS Lifecycle Management**, which automates the transition of infrequently accessed files to the **Infrequent Access (IA)** storage class. You can configure lifecycle policies to determine when files should be moved to the IA tier based on access patterns. This is useful for optimizing storage costs by ensuring that infrequently accessed data is stored at a lower price without manual intervention.

Note: **EFS Lifecycle Management** does not delete data but simply moves files between the Standard and IA tiers.

### **Access Control and Permissions with IAM and Security Groups**
You can manage access to EFS file systems through a combination of VPC security groups and IAM policies. These allow you to:
- Use **VPC security groups** to control network traffic to and from the file system.
- Use **IAM policies** to control which EC2 instances and users can mount the file system and what permissions they have.
- **EFS Access Points** provide more fine-grained control over file system access, allowing for specific user ID and group ID overrides for each application or service.

## Use Cases and Applications

EFS is suitable for a wide range of applications, including:

- **Web Serving and Content Management**: EFS can handle shared storage for dynamic web content, images, videos, and user-generated content. Its scalability ensures that as your application grows, your file system can grow with it without manual intervention.
  
- **Big Data and Analytics**: For big data applications that require high throughput and shared access to large datasets, EFS provides the performance and scalability needed to handle such workloads effectively.
  
- **Development Tools and Environments**: EFS serves as a shared file storage platform for software development environments where multiple developers or CI/CD pipelines need access to the same code repositories or build environments.
  
- **Media and Entertainment**: EFS is ideal for workflows that require collaboration on large media files, such as video editing, rendering, and post-production tasks. It provides fast, low-latency access to media files across a distributed team.

## Comparison Between Amazon EBS and Amazon EFS

| Feature                          | **Amazon EBS**                                          | **Amazon EFS**                                            |
|-----------------------------------|---------------------------------------------------------|----------------------------------------------------------|
| **Type of Service**               | Block storage                                          | File storage                                              |
| **Use Case**                      | Suitable for EC2 instance storage, databases, and boot volumes | Suitable for shared file storage across multiple EC2 instances and services |
| **Access**                         | Attached to a single EC2 instance at a time            | Shared access across multiple EC2 instances and services |
| **Performance Mode**              | General Purpose (gp3), Provisioned IOPS (io2), HDD-based (st1, sc1) | Bursting Throughput, Provisioned Throughput, Elastic Throughput |
| **Storage Class**                 | Standard, Throughput Optimized, Cold HDD               | EFS Standard, EFS Infrequent Access (IA)                 |
| **Scale**                          | Scales up to 16 TiB per volume                        | Scales to petabyte size without pre-provisioning          |
| **Data Durability**               | Replicated within an Availability Zone                | Multi-AZ replication for high durability and availability |
| **Throughput Scaling**            | Dependent on volume type (up to 256,000 IOPS)         | Bursting Throughput scales with data size; Provisioned Throughput offers independent scaling; Elastic Throughput scales automatically with workload |
| **Supported Platforms**           | Linux and Windows instances                           | Linux-based instances (Windows not supported)            |
| **Cost**                          | Pay-per-GB storage + IOPS and throughput costs         | Pay-per-GB storage, with additional costs for Provisioned Throughput and Infrequent Access storage |
| **Encryption**                    | Supports encryption at rest and in transit             | Supports encryption at rest and in transit               |
| **Backup**                        | Snapshots for backup                                   | Supports automated backup with Lifecycle Management for IA tier |
| **Network Connectivity**          | Attached directly to EC2 instance in the same AZ       | Mounted via NFS protocol, accessible across multiple EC2 instances in different AZs |
| **Data Retention**                | Data is retained as long as the volume is attached to an instance | Data persists even after EC2 instance termination       |
| **Availability**                  | Available in a single AZ (multi-AZ requires replication setup) | Multi-AZ availability for high resilience and fault tolerance |
| **Access Control**                | Attached via EC2 instance, controlled with IAM and Security Groups | Managed using VPC security groups, IAM policies, and EFS Access Points |
| **Ideal for**                     | Databases, boot volumes, transactional workloads, and single-instance applications | Shared storage, content management, web hosting, big data, and applications requiring shared access |


