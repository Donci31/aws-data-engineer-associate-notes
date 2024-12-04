# Amazon Aurora

Aurora is a fully managed relational database that combines the speed and availability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. It is compatible with MySQL and PostgreSQL. Storage and compute are separated.

Aurora features a distributed, fault-tolerant, and self-healing storage system that is decoupled from compute resources and auto-scales up to 128 TiB per database instance. It delivers high performance and availability with up to 15 low-latency read replicas, point-in-time recovery, continuous backup to Amazon Simple Storage Service (Amazon S3), and automatic replication across three Availability Zones (AZs).

## Data Replication

Aurora stores copies of the data in a DB cluster across three Availability Zones in a single AWS Region by default. When data is written to the primary DB instance, Aurora synchronously replicates the data across three Availability Zones (AZs) to six storage nodes associated with your cluster volume. Doing so provides data redundancy, eliminates I/O freezes, and minimizes latency spikes during system backups. Even if some or all DB instances become unavailable, the data remains safe due to six node storages spread in 3 AZ.

Unlike RDS Multi-AZ deployments, where a primary DB instance has a standby instance in a different AZ, Aurora uses Read Replicas that have access to the same underlying data as the primary instance. There are no standby instances in Aurora.

## Aurora Read Replicas

Aurora Read Replicas are optional independent endpoints in an Aurora DB cluster, best used for scaling read operations. Up to 15 Aurora Replicas can be distributed across the Availability Zones (AZs) that a DB cluster spans within an AWS Region. You can also set up two Aurora MySQL DB clusters in different AWS Regions, by creating an Aurora Read Replica of an Amazon Aurora MySQL DB cluster in a different AWS Region. In this way, Aurora Read Replicas can be deployed globally. The replication is asynchronous. While read replicas access the same underlying storage volume as the primary instance, the visibility of the new data is slightly delayed.

Aurora Read Replicas have two main purposes:

1. **Scale read operations**: You can issue queries to them to scale the read operations for your application. You typically do so by connecting to the reader endpoint of the cluster. That way, Aurora can spread the load for read-only connections across as many Aurora Replicas as you have in the cluster.
2. **Increase availability**: If the writer instance in a cluster becomes unavailable, Aurora automatically promotes one of the reader instances to take its place as the new writer.

For Amazon Aurora, each Read Replica is associated with a priority tier (0-15). In the event of a failover, Amazon Aurora will promote the Read Replica that has the highest priority (the lowest numbered tier). If two or more Aurora Replicas share the same priority, then Amazon RDS promotes the replica that is largest in size. If two or more Aurora Replicas share the same priority and size, then Amazon Aurora promotes an arbitrary replica in the same promotion tier.

## Aurora Node and Storage

A node or instance (compute) refers to an individual instance within an Aurora DB cluster. There’s always one primary node that handles all write operations, but it can also serve read queries, and then read replicas that have a reading purpose only. All nodes in a cluster share the same underlying storage volume. Be careful, the six storage nodes aren’t compute nodes, they work at the distributed storage layer.

## Aurora Serverless

Aurora Serverless is an on-demand, auto-scaling configuration for Amazon Aurora (MySQL-compatible and PostgreSQL-compatible editions), where the database will automatically start-up, shut down, and scale capacity up or down based on your application’s needs. It enables you to run your database in the cloud without managing any database instances. It’s a simple, cost-effective option for infrequent, intermittent, or unpredictable workloads. You pay on a per-second basis for the database capacity you use when the database is active and migrate between standard and serverless configurations with a few clicks in the Amazon RDS Management Console.

## Aurora Global Database

An Aurora global database provides more comprehensive failover capabilities than the failover provided by a default Aurora DB cluster. By using an Aurora global database, you can plan for and recover from disasters fairly quickly.

 **Recovery Time Objective** (the time it takes a system to return to a working state after a disaster), for an Aurora global database can be in the order of minutes. **Recovery Point Objective** is typically measured in seconds.

With an Aurora global database, you can choose from two different approaches to failover:

- **Managed planned failover**: This feature is intended for controlled environments, such as disaster recovery (DR) testing scenarios, operational maintenance, and other planned operational procedures. Managed planned failover allows you to relocate the primary DB cluster of your Aurora global database to one of the secondary Regions. Because this feature synchronizes secondary DB clusters with the primary before making any other changes, RPO is 0 (no data loss).
- **Unplanned failover**: To recover from an unplanned outage, you can perform a cross-Region failover to one of the secondaries in your Aurora global database. The RTO for this manual process depends on how quickly you can perform the tasks listed in Recovering an Amazon Aurora global database from an unplanned outage. The RPO is typically measured in seconds, but this depends on the Aurora storage replication lag across the network at the time of the failure.

## Aurora Cloning and Backtracking

You can quickly create clones of an Aurora DB by using the database cloning feature. In addition, database cloning uses a copy-on-write protocol, in which data is copied only at the time the data changes, either on the source database or the clone database. Cloning is much faster than a manual snapshot of the DB cluster. You cannot clone databases across AWS regions. The clone databases must be created in the same region as the source databases. Currently, you are limited to 15 clones based on a copy, including clones based on other clones.

Using **Backtracking**, you can "rewind" the DB cluster to any time you specify. One of the major advantages of backtracking is that it can rewind the DB cluster much faster compared to restoring a DB cluster via point-in-time restore (PITR) or via a manual DB cluster snapshot, which can take hours. Backtracking a DB cluster doesn’t require a new DB cluster and rewinds the DB cluster in minutes.

## Aurora Backup and Restore

Aurora backs up your cluster volume automatically and retains restore data for the length of the backup retention period. Aurora backups are continuous and incremental so you can quickly restore to any point within the backup retention period. No performance impact or interruption of database service occurs as backup data is being written.

Automated backups occur daily during the preferred backup window. If the backup requires more time than allotted to the backup window, the backup continues after the window ends, until it finishes. The backup window can't overlap with the weekly maintenance window for the DB cluster. Aurora backups are continuous and incremental, but the backup window is used to create a daily system backup that is preserved within the backup retention period. The latest restorable time for a DB cluster is the most recent point at which you can restore your DB cluster, typically within 5 minutes of the current time.

## Aurora Auto Scaling

Aurora Auto Scaling is particularly useful for businesses that have fluctuating workloads. It ensures that your database cluster scales up or down as needed without manual intervention. This feature saves time and resources, allowing businesses to focus on other aspects of their operations. Aurora Auto Scaling is also cost-effective, as it helps minimize unnecessary expenses associated with overprovisioning or underprovisioning database resources.

## Aurora Endpoints

Using endpoints, you can map each connection to the appropriate instance or group of instances based on your use case. For example, to perform DDL statements, you can connect to whichever instance is the primary instance. To perform queries, you can connect to the reader endpoint, with Aurora automatically performing load-balancing among all the Aurora Replicas. For clusters with DB instances of different capacities or configurations, you can connect to custom endpoints associated with different subsets of DB instances. For diagnosis or tuning, you can connect to a specific instance endpoint to examine details about a specific DB instance.

## Aurora Limitations and Integrations

While Amazon Aurora offers many advanced features, there are some limitations and considerations when it comes to specific use cases:

### Row-Level Security and Amazon Cognito Integration

Amazon Aurora supports row-level security; however, it does not natively integrate with Amazon Cognito for authentication with Amazon accounts. This means that Aurora may not be the best option if you're looking for tight integration with Amazon Cognito for managing user authentication at a granular level.

### Zero-ETL Integration with Amazon Redshift

Aurora offers a zero-ETL integration with Amazon Redshift, allowing direct analysis of data without the need for traditional ETL jobs. This integration enables near real-time analytics and machine learning on large volumes of transactional data.

With this integration, transactional data from Aurora is made available in Amazon Redshift within seconds of being written into Aurora, making it easier for businesses to perform real-time analytics on massive data sets without the overhead of maintaining separate ETL pipelines. This integration provides a streamlined, cost-effective solution for businesses requiring quick access to large amounts of data for analytics.
