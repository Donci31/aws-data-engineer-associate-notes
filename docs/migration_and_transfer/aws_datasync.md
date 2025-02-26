# AWS DataSync

AWS DataSync is a fully managed online data transfer service that simplifies, automates, and accelerates the process of copying large datasets to and from AWS storage services. Designed for scalability and efficiency, DataSync can transfer data up to 10 times faster than traditional command-line tools by leveraging a purpose-built network protocol and scale-out architecture.

## Versatility in Data Transfers

- **On-Premises/Other Cloud to AWS**: Transfers data from Network File System (NFS), Server Message Block (SMB), Hadoop Distributed File Systems (HDFS), or self-managed object storage to AWS storage services. This process requires deploying an agent in the source environment.
- **AWS to AWS Transfers**: Migrates data between different AWS storage services (e.g., Amazon S3 to Amazon EFS) without the need for an agent.
- **Cross-Service Support**: Supports data synchronization across:
    - **Amazon S3**: All storage classes, including Glacier and Glacier Deep Archive.
    - **Amazon EFS**: For scalable and elastic file storage.
    - **Amazon FSx**: Includes FSx for Windows File Server, FSx for Lustre, FSx for OpenZFS, and FSx for NetApp ONTAP.

## Reliability and Data Integrity

- **Network Optimizations**: Built-in retry and resiliency mechanisms to handle transient network issues.
- **Data Integrity Verification**: Ensures data consistency during and after transfer by verifying integrity.
- **File Metadata Preservation**: Retains file permissions and metadata, such as NFS POSIX and SMB attributes.

## Scalability and Performance

- A single DataSync agent can saturate a 10 Gbps network link, ensuring high-speed transfers.
- Bandwidth can be limited to manage network utilization effectively.

## Automation and Scheduling

- **Task Scheduling**: Automates repetitive tasks with schedules on an hourly, daily, or weekly basis.
- **Monitoring and Metrics**: Offers detailed visibility through:
    - **Amazon CloudWatch**: Provides metrics, events, and logs for granular monitoring.
    - **AWS DataSync API and Console**: Facilitates task creation and management.

## Long-Term Storage and Cost Optimization

- **Cold Data Management**: Transfers infrequently accessed on-premises data to cost-effective AWS storage solutions such as Amazon S3 Glacier and Glacier Deep Archive.
- **Integration with AWS Storage Gateway**: For ongoing access and updates to migrated data.

## Security

- **Encrypted Transfers**: Ensures secure data movement between source and target.
- **Native Integrations**: Works seamlessly with Amazon CloudWatch and AWS CloudTrail for security auditing and monitoring.

## Use Cases

1. **Large-Scale Migrations**: Moving datasets from on-premises or other clouds to AWS storage services for cloud adoption or hybrid architectures.
2. **AWS-to-AWS Transfers**: Migrating or synchronizing data between AWS storage services for compliance, performance optimization, or disaster recovery purposes.
3. **Data Archival**: Offloading infrequently accessed on-premises data to S3 Glacier or Glacier Deep Archive to reduce costs.
4. **Hybrid Cloud Storage**: Retaining access to migrated data using AWS Storage Gateway File Gateway.

## Limitations

- **Database Migration**: AWS DataSync is primarily designed for file-based data transfers. It lacks database-specific features such as schema handling, ongoing replication (Change Data Capture), and JDBC/ODBC integration. For database migrations, AWS Database Migration Service (DMS) is recommended.
- **On-Premises Agent Requirement**: For transfers from on-premises or non-AWS cloud environments, an agent must be deployed.

## Alternative Solutions

- **AWS Snowcone**: For environments with limited connectivity or when a physical device-based migration is more practical.
- **AWS Storage Gateway**: For hybrid storage solutions that enable continued on-premises access to AWS-stored data.

AWS recommends that you should use AWS DataSync to migrate existing data from on-premises to Amazon S3, and subsequently use the File Gateway configuration of AWS Storage Gateway to retain access to the migrated data and for ongoing updates from your on-premises file-based applications.
