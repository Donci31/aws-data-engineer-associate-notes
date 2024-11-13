# Amazon RDS (Relational Database Service)

Amazon RDS (Relational Database Service) is a fully managed database service that simplifies the process of setting up, operating, and scaling relational databases in the cloud. AWS offers six familiar database engines for RDS, including:
- **Amazon Aurora**
- **PostgreSQL**
- **MySQL**
- **MariaDB**
- **Oracle Database**
- **SQL Server**

RDS handles routine database tasks such as hardware provisioning, database setup, patching, backups, and scaling, allowing users to focus on their applications rather than on database management. Amazon RDS is designed for applications with traditional relational database needs but **is not meant for Big Data** scenarios.

## High Availability and Durability

Amazon RDS provides several features to ensure the availability and durability of your database deployments.

### Multi-AZ Deployments

RDS Multi-AZ deployments enhance availability and durability by automatically replicating data across different Availability Zones (AZs). When you enable Multi-AZ, Amazon RDS creates a primary DB instance and synchronously replicates the data to a standby instance in a different AZ. Each AZ operates on distinct, independent infrastructure to ensure high reliability. 

- In the event of a failure, RDS automatically fails over to the standby DB instance in another AZ. 
- Failover times typically range from **60 to 120 seconds**.
- The standby takes over the primary’s DNS name.
- The standby replica, while highly available, cannot be used for reads or writes during normal operations; it only becomes active during a failover.
- **Cross-region Multi-AZ deployments are not supported**, but you can configure **Cross-Region Read Replicas** for disaster recovery.

### Cross-Region Automated Backups

RDS supports **Cross-Region Automated Backups**, **Manual Snapshots**, and **Read Replicas** across regions, which enable enhanced disaster recovery and high availability solutions. Automated backups can be copied to a different region, helping to protect against regional failures.

Read Replicas allow you to offload read traffic from your primary DB instance. These replicas can be asynchronously copied from the source instance and even promoted to become a standalone source in case of failure. This feature is valuable for **disaster recovery (DR)** and can be implemented across multiple AWS regions for additional resilience.

- The maximum number of read replicas depends on the database engine but generally supports up to **5 replicas**.
- Read replicas can be in the same region or in a different region.

## Security

RDS provides various layers of security to protect your data.

### Data Encryption

RDS supports **encryption at rest** using **AWS Key Management Service (KMS)**, as well as **encryption in transit** with **SSL/TLS**. You can encrypt your database connections, ensuring that sensitive data is securely transmitted between your applications and your database.

- **SSL certificates** are automatically created and installed when you provision an RDS instance.
- For **MySQL**, you launch the MySQL client using the `–ssl` parameter to reference the public key to encrypt connections.
- For **PostgreSQL**, you can force all connections to your PostgreSQL DB instance to use SSL.

Amazon RDS does not support enabling encryption for an existing database instance directly. To encrypt an existing unencrypted RDS database, the recommended approach is to take a snapshot of the unencrypted database, copy the snapshot with encryption, and then restore the RDS instance from this encrypted snapshot.

### Networking and Access Control

RDS supports Virtual Private Cloud (VPC) configurations, allowing you to isolate your database instances in your own network environment. **VPC Security Groups** and **IAM roles** are used to control access to your RDS instances.

## Backup and Recovery

RDS offers automated backups, point-in-time recovery, and manual snapshots, which help protect your data and ensure business continuity.

### Automated Backups

Amazon RDS allows you to configure automated backups to ensure that you can restore your database to any point in time within the retention period. The backups are stored in **Amazon S3**, and you can restore your database to any second during the backup retention period.

- Automated backups are enabled by default and provide point-in-time recovery.
- You can specify a backup window for RDS to minimize disruption during backups.

### Manual Snapshots

You can take manual snapshots of your DB instances at any time. These snapshots are stored in **Amazon S3** and can be restored as new DB instances. Unlike automated backups, manual snapshots are retained until you delete them.

- Snapshots can be shared across AWS accounts.
- Manual snapshots are ideal for retaining consistent backups of your database.

## Caching with ElastiCache

Amazon ElastiCache is an ideal front-end for data stores such as Amazon RDS, providing a high-performance middle tier for applications with extremely high request rates and/or low latency requirements. The best part of caching is that it’s minimally invasive to implement and by doing so, your application performance regarding both scale and speed is dramatically improved.

## Performance and Scaling

Amazon RDS is designed to scale easily with your application. Scaling can be achieved both vertically (by changing instance types) and horizontally (via read replicas).

### Vertical Scaling

You can vertically scale your database by modifying the instance type of your DB instance. This allows you to increase CPU, RAM, and storage capacity to handle more load or larger datasets. However, vertical scaling can involve some downtime.

### Horizontal Scaling

**Read Replicas** allow for horizontal scaling by offloading read traffic from the primary DB instance. This increases read throughput and provides higher availability for applications that require read-heavy workloads. Read replicas can be used to:

- Distribute read traffic.
- Serve as a disaster recovery mechanism by promoting a read replica to the primary DB instance in case of a failure.

### Aurora vs. RDS

While Amazon RDS supports multiple engines, **Amazon Aurora** is specifically optimized for high performance and scalability. Aurora offers **five times the throughput of MySQL** and **three times the throughput of PostgreSQL**. 

- Aurora replicates data **six times** across three Availability Zones, offering higher durability than traditional RDS.
- Failover in Aurora happens automatically via **read replicas**, whereas in RDS Multi-AZ, the failover is typically to a standby replica.
- Aurora scales quickly as it separates **compute** and **storage**, enabling automatic storage scaling without downtime.

## Upgrades and Maintenance

### Engine Version Upgrades

Upgrades to the database engine level require downtime. Even if your Amazon RDS DB instance uses a Multi-AZ deployment, both the primary and standby DB instances are upgraded at the same time. This causes downtime until the upgrade is complete, and the duration of the downtime varies based on the size of your database instance.

- To minimize downtime, RDS applies operating system updates in a staged process:
  1. Perform maintenance on the standby.
  2. Promote the standby to primary.
  3. Perform maintenance on the old primary, which becomes the new standby.

### Warm Standby

The term **warm standby** is used to describe a **disaster recovery (DR)** scenario in which a scaled-down version of a fully functional environment is always running in the cloud. A warm standby solution extends the **pilot light** concept and ensures that essential services are always operational, which decreases recovery time.

- A warm standby solution involves duplicating business-critical systems in AWS, ensuring they are always available for rapid recovery.

### Pilot Light

The **pilot light** DR strategy is akin to a backup-and-restore approach. A minimal version of the critical system is always running in AWS, and full-scale recovery is enabled once a failure is detected. This is beneficial for maintaining **core elements** of your system in AWS.

- The pilot light approach enables rapid scaling of the environment once a disaster occurs.

### Multi-Site

A **multi-site** solution operates across both AWS and your on-premises infrastructure in an **active-active configuration**. Data replication methods will depend on the recovery point objectives (RPO) that you define for your environment.

## RDS Storage Auto Scaling

Amazon RDS provides **storage auto-scaling** to automatically adjust your storage capacity in response to growing database workloads. With storage auto-scaling, there is no downtime involved, making it an efficient and seamless way to handle increasing storage demands.

- When your database nears its storage capacity, RDS automatically increases the allocated storage size to accommodate the additional data.
  
## Best Practices

Here are some best practices for managing Amazon RDS:

- **Monitor System Metrics**: Use **Amazon CloudWatch** to monitor memory, CPU usage, replica lag, and storage. Set up alarms to notify you when these metrics exceed thresholds to ensure system performance and availability.
  
- **Scale Storage as Needed**: Scale your DB instance when you're nearing storage capacity limits. Always maintain some buffer in storage and memory to handle unforeseen demand spikes.

- **Automate Backups**: Enable automatic backups and schedule them during periods of low database activity to minimize disruption. Set your backup window to occur during the daily low in write IOPS.

- **DNS Caching**: If your application is caching the **Domain Name System (DNS)** data of your DB instances, set a **time-to-live (TTL)** value of less than 30 seconds to ensure clients always get the most recent information about your DB instance.

- **Enhanced Monitoring**: Enable **Enhanced Monitoring** to get real-time metrics for your DB instance’s operating system, providing deeper insights into its performance.

- **Optimize MySQL and MariaDB Tables**: Avoid tables growing too large. If your table sizes approach the 16 TiB limit for MySQL or MariaDB, partition large tables. Ensure fewer than 10,000 tables are present across all databases in your instance.

- **Autovacuum for PostgreSQL**: Use **autovacuum** to maintain the health of your PostgreSQL DB instance. Autovacuum automates the execution of **VACUUM** and **ANALYZE** commands, which help reclaim storage occupied by dead tuples and ensure that the database statistics are up-to-date. 

> **Note:** Important to know about PostgreSQL, you can explicitly use the LOCK command in your application code to control the level. PostGIS extensions to handle spatial and geographic data.
