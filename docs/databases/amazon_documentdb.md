# Amazon DocumentDB

**Amazon DocumentDB** (with MongoDB compatibility) is a fully managed, fast, and reliable database service that enables easy setup, operation, and scaling of MongoDB-compatible databases in the cloud. With Amazon DocumentDB, you can use the same application code, drivers, and tools commonly used with MongoDB, providing a seamless transition to a managed, cloud-native service.

## Components of Amazon DocumentDB

### Instances

An Amazon DocumentDB instance represents an isolated database environment within the cloud, capable of hosting multiple user-defined databases. Instances are manageable through the AWS Management Console or the AWS CLI, providing flexibility and control in database administration.

### Clusters

DocumentDB clusters can contain up to 16 instances within the same AWS region. Before creating Amazon DocumentDB instances, you must set up a cluster to contain and organize them. Within a cluster:

- **Primary Instance**: Handles all read-write operations.
- **Read Replicas**: You can provision up to 15 read replica instances for read-only operations, enhancing both read capacity and failover support.

### Instance Classes

Each instance’s memory and compute capacity are determined by its **instance class**, allowing users to select the class that best fits their workload. As requirements evolve, the instance class can be modified to adjust resources accordingly. However, not all instance classes are available in every region.

> **Note**: Amazon DocumentDB operates as a regional service.

### Storage and Compute Architecture

Amazon DocumentDB is built on AWS’s custom Aurora platform, which **decouples storage and compute** to optimize flexibility and scalability. The storage layer, known as the **cluster volume**, ensures data durability by replicating data six times across three AWS Availability Zones. This multi-zone redundancy reduces data loss risks and minimizes downtime in the event of disruptions in any single zone.

### Endpoints

Each DocumentDB cluster comes with unique endpoints:

- **Cluster Endpoint**: Directs read-write traffic to the primary instance.
- **Reader Endpoint**: Provides a load-balanced connection for read operations across the replica instances within the cluster.

## Features of Amazon DocumentDB

Amazon DocumentDB offers a variety of features that make it a robust solution for managing MongoDB-compatible databases:

### High Availability and Automated Failover

In case of an instance failure, DocumentDB automates the failover process:

- If one instance fails, DocumentDB automatically fails over to one of the up to 15 replicas in other Availability Zones.
- If no replicas are available, DocumentDB will attempt to create a new instance to maintain cluster availability and performance.

### Seamless Scaling

DocumentDB scales storage automatically as data storage requirements grow:

- Storage scales in **10 GB increments**, up to a maximum of 128 TiB, without the need for manual intervention.
- Users can also adjust compute and memory resources as needed, with scaling operations typically completing within minutes.

### Security Features

Amazon DocumentDB provides robust, multi-layered security features to protect your data:

- **Network Isolation**: Uses Amazon VPC for secure network isolation.
- **Encryption**: Supports encryption at rest (managed by AWS Key Management Service) and in transit (using MongoDB wire protocol encryption).
- **Backup and Snapshots**: Automated backups, snapshots, and replicas are encrypted within the same cluster, providing a secure backup and data redundancy solution.

### Continuous Backup and Point-in-Time Recovery

DocumentDB continuously backs up cluster data to **Amazon S3**:

- Point-in-time recovery enables restoration to any second within the configured retention period, up to the last five minutes.
- The backup retention period can be configured for up to **35 days**.

### Time to Live (TTL)

The Time to Live (TTL) feature allows users to specify an expiration time for data items, based on an attribute’s value:

- TTL is expressed as seconds since the Unix epoch.
- Expired items can be automatically deleted without incurring write costs, keeping the database lean and efficient by removing outdated data.
