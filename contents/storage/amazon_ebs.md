# Amazon Elastic Block Store (EBS) Overview

Amazon Elastic Block Store (EBS) is a high-performance, durable block storage service designed to work seamlessly with Amazon EC2 for a variety of workload requirements, including both throughput-intensive and transaction-intensive applications. EBS offers persistent, scalable storage that can be tailored to meet specific cost, performance, and data resiliency needs, making it suitable for a wide range of enterprise applications.

EBS provides block-level storage that offers performance suitable for large-scale applications such as databases, data warehousing, and logs.

## Key Features and Benefits

1. **Storage Volume Types**:
   EBS offers a range of volume types that provide flexibility to optimize for performance, cost, or both, based on workload requirements. Options include:
   - **SSD-backed Volumes**: Suitable for transactional applications that require low latency and consistent IOPS, such as databases or OS boot volumes.
     - **Provisioned IOPS SSD (io2)**: Delivers the highest IOPS consistency for mission-critical workloads.
     - **General Purpose SSD (gp3)**: Balanced for most applications requiring SSD performance but at a lower cost.
   - **HDD-backed Volumes**: Ideal for sequential workloads with high throughput needs, such as big data processing.
     - **Throughput Optimized HDD (st1)**: For high-throughput, low-cost storage.
     - **Cold HDD (sc1)**: The most economical choice, suited for infrequently accessed data.

2. **Instance Boot and Storage Configuration**:
   When launching an EC2 instance, you can choose between two types of root volumes:
   - **EBS-backed AMIs**: Provide persistent storage that remains after the instance stops. Remember that EBS volumes are not phsycally attached. They use the network to communicate, which means more latency than instance store.
   - **Instance Store-backed AMIs**: Offer temporary storage located on physical disks, which is cleared when the instance stops. This makes it suitable for cache data, temporary logs, or ephemeral workloads.

    By default, the root volume for an AMI backed by Amazon EBS is deleted when the instance terminates. You can change the default behavior to ensure that the volume persists after the instance terminates. Non-root EBS volumes remain available even after you terminate an instance to which the volumes were attached.
3. **Automatic Data Replication**:
   Each EBS volume is automatically replicated within its Availability Zone (AZ) to protect against data loss due to hardware failures. Volumes can be attached to EC2 instances within the same AZ, ensuring data resilience and high availability.

   You can attach an Amazon EBS volume to an Amazon EC2 instance in the same Availability Zone (AZ). EBS volumes are linked to the AZ!

4. **Data Encryption with AWS Key Management Service (KMS)**:
   EBS volumes can be encrypted to secure data at rest, data in transit between the instance and volume, and data in associated snapshots. Encryption is achieved through AWS KMS, ensuring that sensitive information remains protected.

   When you create an encrypted Amazon EBS volume and attach it to a supported instance type, data stored at rest on the volume, data moving between the volume and the instance, snapshots created from the volume and volumes created from those snapshots are all encrypted. It uses AWS Key Management Service (AWS KMS) customer master keys (CMK) when creating encrypted volumes and snapshots. Encryption operations occur on the servers that host Amazon EC2 instances, ensuring the security of both data-at-rest and data-in-transit between an instance and its attached Amazon EBS storage.

5. **Elastic Volumes**:
   Elastic Volumes enable on-the-fly changes to EBS volume attributes—such as size, performance, and volume type—without detaching the volume or stopping the instance. This flexibility supports real-time workload adjustments, making it easier to scale storage as needs evolve without incurring downtime.

## EBS Multi-Attach and Nitro Instances

Amazon EBS Multi-Attach allows a single **Provisioned IOPS SSD (io1 or io2)** volume to be attached to multiple **Linux-based Nitro EC2 instances** within the same AZ. Multi-Attach provides read and write access to each connected instance, enhancing high availability for clustered applications. However, this feature is exclusive to Provisioned IOPS SSD volumes.

## Enhanced Data Recovery with Amazon CloudWatch

You can set up Amazon CloudWatch alarms to automatically recover impaired EC2 instances that encounter hardware issues or other impairments. A recovered instance is identical to the original instance, including the instance ID, private IP addresses, Elastic IP addresses, and all instance metadata. If the impaired instance is in a placement group, the recovered instance runs in the placement group. If your instance has a public IPv4 address, it retains the public IPv4 address after recovery. During instance recovery, the instance is migrated during an instance reboot, and any data that is in-memory is lost.

## RAID Configurations for Improved Performance and Redundancy

A RAID array uses multiple EBS volumes to improve performance or redundancy. There are two types:

- **RAID 1** (Mirroring): Creates a copy of the data on another volume, improving fault tolerance.
- **RAID 0** (Striping): Distributes data across multiple volumes to enhance performance but does not provide redundancy.

When fault tolerance is more important than I/O performance a RAID 1 array should be used which creates a mirror of your data for extra redundancy:

## Amazon Data Lifecycle Manager (DLM) for Automated Snapshots

Amazon DLM automates the creation, retention, and deletion of EBS snapshots, reducing manual management overhead and supporting regulatory compliance. DLM allows administrators to:

- Enforce regular backup schedules to protect critical data.
- Retain snapshots for compliance.
- Reduce storage costs by deleting old snapshots.

## EBS Volume Snapshots and Backup Retention

EBS volumes can be backed up through snapshots, which are incremental and stored in Amazon S3, providing a durable and scalable backup solution. Snapshots enable point-in-time recovery, but the retention period for automated backup is only 35 days.
