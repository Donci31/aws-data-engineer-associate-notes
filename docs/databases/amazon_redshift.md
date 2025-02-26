# Amazon Redshift

Amazon Redshift is a fully managed, petabyte-scale cloud data warehouse service designed for fast and scalable storage and analysis of large datasets. It is built for performing complex queries and analytics on vast amounts of structured and semi-structured data.

Amazon Redshift is a relational database management system (RDBMS) that provides the same functionality as typical RDBMS platforms, including online transaction processing (OLTP) features such as inserting and deleting data. However, Redshift is optimized for high-performance data analysis and reporting, especially for large datasets.

A Redshift data warehouse consists of a cluster of computing resources called **nodes**. These nodes are grouped together into a **cluster**, and if the cluster is provisioned with two or more compute nodes, an additional **leader node** is used to coordinate the compute nodes and handle communication with external systems.

## Node and Cluster Architecture

- **Leader Node**: The leader node is responsible for managing communications between client applications and the compute nodes, parsing and developing execution plans, and distributing workloads. The leader node processes queries that don’t require compute nodes, and it allocates data to the compute nodes when necessary.

- **Compute Nodes**: These nodes carry out the heavy lifting of processing data. Each compute node has its own dedicated CPU and memory, based on its node type. Compute nodes are partitioned into **slices**, which are responsible for processing a portion of the workload.

- **Slices**: Each slice is allocated a portion of a compute node's memory and disk space, allowing the parallel processing of workloads across nodes. This architecture helps speed up data loading and query execution.

As workloads grow, you can scale your Redshift cluster by adding more nodes, upgrading node types, or both.

## Data Distribution and Storage

- **Distribution Keys**: When creating tables, you can specify a **distribution key** that helps distribute data across slices and nodes. This enables parallel processing and efficient query execution.

- **Storage Decoupling**: Traditionally, in data warehousing, storage and compute resources are tightly coupled. Redshift decouples these resources, leveraging **Amazon S3** for scalable, petabyte-scale storage. Frequently accessed (hot) data is stored in **SSD-based local storage** for fast access. When local storage fills up, Redshift automatically overflows data to Amazon S3 without manual intervention. This offers virtually unlimited storage capacity while maintaining performance.

## Data Loading and Performance

- **Parallel Processing**: Redshift utilizes parallel processing to handle large data sets more efficiently. By dividing the data into smaller chunks and processing these chunks in parallel across compute nodes, Redshift speeds up both data loading and query performance.

- **Compression**: Data compression in Redshift reduces storage costs and increases the speed of data loading and querying. You can select from several compression encodings to best suit your data.

## Federated Queries

Amazon Redshift supports **federated queries**, allowing you to query data from external databases (e.g., PostgreSQL on Amazon RDS) directly from Redshift. This capability eliminates the need for data movement or duplication, enabling cross-database queries without leaving Redshift. This is particularly useful for running analyses across datasets stored in multiple locations.

Remember, Amazon Timestream is not currently supported as a source since Timestream is a specialized time-series database requiring specific query capabilities not addressed by Redshift's federated queryingfeatures.

## Redshift Spectrum

**Redshift Spectrum** allows you to run SQL queries directly against data stored in **Amazon S3** without having to load it into Redshift tables. This enables you to query both structured and semi-structured data (e.g., JSON, Parquet) without having to move the data, saving time and resources.

Redshift Spectrum leverages **massive parallel processing (MPP)**, ensuring fast query performance even on large datasets. By offloading much of the compute-intensive processing to the Spectrum layer, it reduces the burden on your Redshift cluster.

You can create **Redshift Spectrum tables** by registering data stored in Amazon S3 and defining the table structure in an external data catalog (e.g., AWS Glue, Amazon Athena, or your own Apache Hive metastore). Once registered, you can query and join these external tables in the same way you would query native Redshift tables. However, note that **Redshift Spectrum does not support update operations** on external tables.

## Data Sharing

Redshift supports **data sharing**, allowing live data to be seamlessly shared between different Redshift clusters or Redshift Serverless endpoints. This eliminates the need for data replication, reduces storage costs, and simplifies data sharing between business units or organizations.

## Redshift Serverless

**Redshift Serverless** optimizes capacity by automatically scaling compute resources up or down based on usage. It charges only for the compute resources used and incurs no charges during idle periods. This is ideal for environments with unpredictable or sporadic workloads. For instance, if your cluster operates only for a few hours every couple of weeks, Redshift Serverless provides a cost-effective solution without the overhead of managing clusters.

When Redshift needs to be operational every X time during the week, using Redshift Serverless is a prudent choice to minimize compute costs.

## Performance Insights

Amazon Redshift provides powerful tools to monitor and optimize query performance:

- **Query Performance Insights**: This tool offers a comprehensive view of query performance, allowing data engineers to quickly identify long-running or problematic queries and optimize them for better performance.

- **Amazon Redshift Advisor**: Redshift Advisor offers automated recommendations to optimize the performance of Redshift clusters, such as distribution style changes, sort key additions, and more.

Together, **Query Performance Insights** and **Amazon Redshift Advisor** provide an effective solution for monitoring and optimizing query performance.

## System Tables and View

Amazon Redshift has many system tables and views that contain information about how the system is functioning. You can query these system tables and views the same way that you would query any other database tables.

There are several types of system tables and views:

- **SVV** views contain information about database objects with references to transient STV tables.
- **SYS** views are used to monitor query and workload usage for provisioned clusters and serverless workgroups.
- **STL** views are generated from logs that have been persisted to disk to provide a history of the system.
- **STV** tables are virtual system tables that contain snapshots of the current system data. They are based on transient in-memory data and are not persisted to disk-based logs or regular tables.
- **SVCS** views provide details about queries on both the main and concurrency scaling clusters.
- **SVL** views provide details about queries on main clusters.

Important tables to remember:

- **VACUUM SORT ONLY**: This option is used to optimize the physical layout of data within tables. It is particularly useful for improving query performance in cases where frequent updates and insertions have caused data to become unsorted. This command does not delete any data, ensuring that it aligns with requirements to maintain datasets while improving efficiency.

- **STL_ALERT_EVENT_LOG**: The `STL_ALERT_EVENT_LOG` table is tailored to identify queries where the query optimizer has detected potential performance issues, such as inefficient joins or excessive data scanning. This helps users address and resolve performance bottlenecks.

- **STL_WLM_QUERY**: This table primarily records information about query execution such as CPU time, number of rows read, and disk space used for spills. While useful for performance tuning, it does not directly provide alerts or event information that would indicate problematic database operations.

- **STL_USAGE_CONTROL**: This table only logs changes in WLM configuration and when queries are paused or canceled due to WLM rules. It is helpful for understanding WLM configuration impacts.

- **STL_QUERY_METRICS**: This view provides detailed information about query execution, including the query text, start time, end time, and execution duration. This system view is designed specifically for monitoring and analyzing query performance, making it an invaluable resource for data engineers looking to optimize query execution times and evaluate the effectiveness of various optimization strategies.

- **STV_BLOCKLIST**: is a system table that provides information about blocks that are currently marked for deletion in the cluster. It is useful for understanding vacuum operations but not for analyzing query performance.

- **STV_TBL_PERM**: shows snapshot data about the current state of permanent tables, including information on disk space usage. While it can help assess storage utilization, it does not offer insights into query execution times.

## Importing/Exporting Data

Amazon Redshift offers powerful tools like the COPY and UNLOAD commands for efficiently managing data transfers between Redshift clusters and external storage solutions, such as Amazon S3.

### COPY

The **COPY** command facilitates the import of data into Amazon Redshift from sources including Amazon S3, Amazon EMR, DynamoDB, or remote hosts via SSH. It's optimized for loading extensive volumes of data rapidly and efficiently, making it the preferred choice for bulk data loading due to its speed and parallel processing capabilities.
The COPY command supports various data formats, including JSON, CSV, Avro, Parquet, and ORC. To execute a COPY operation, you merely need three key parameters: a table name, a data source, and the authorization to access the data.

    COPY your_table
    FROM 's3://your-bucket/data-files/'
    IAM_ROLE 'arn:aws:iam::123456789012:role/MyRedshiftRole'
    FORMAT AS JSON 'auto';

Authorization is necessary to access data in other AWS resources, such as Amazon S3, Amazon EMR, Amazon DynamoDB, and Amazon EC2. This can be accomplished through referring to an IAM role attached to your cluster or providing an access key ID and secret access key for an IAM user.

### UNLOAD

The **UNLOAD** command exports query results to one or more text, JSON, or Apache Parquet files on Amazon S3, with options for Amazon S3 server-side encryption (SSE-S3), AWS Key Management Service key (SSE-KMS) encryption, or client-side encryption using a customer managed key. This is useful for archiving data, analyzing data externally, or transferring data to other databases.

Unloading Redshift queries to your Amazon S3 data lake in Apache Parquet format is efficient for analytics, as Parquet is faster to unload and conserves more storage in Amazon S3 compared to text formats. This allows you to retain data transformations and enrichments done in Redshift into your S3 data lake in an open format and analyze through Redshift Spectrum or other AWS services like Amazon Athena, Amazon EMR, and Amazon SageMaker.

To execute the UNLOAD command, SELECT privileges on the database data and permission to write to the Amazon S3 location are required.

    UNLOAD ('SELECT * FROM your_table')
    TO 's3://your-bucket/unload/'
    IAM_ROLE 'arn:aws:iam::123456789012:role/MyRedshiftRole'
    PARALLEL OFF
    FORMAT AS JSON;

## Amazon Redshift Integration

### **ODBC and JDBC Connections**

Amazon Redshift supports **ODBC** and **JDBC** connections, allowing you to connect to your Redshift cluster from many third-party SQL client tools and applications. You can set up these connections from your client computer or Amazon EC2 instance.

While **ODBC** connections are widely supported, you may prefer to use **JDBC** due to its easier configuration and better support for Java-based applications. If your client tool supports JDBC, it is generally the preferred option for connecting to Redshift.

### **Using the Data API**

The **Amazon Redshift Data API** is ideal for running SQL queries on Redshift clusters without needing to manage database connections. This is especially beneficial for diverse applications, such as microservices architectures, where database connection management can be a challenge.

The Data API executes queries asynchronously, allowing for a more scalable and efficient way to retrieve results, especially for use cases that require integration with external applications and services.

## Internal Management

### **Implementing Sorted Keys**

Sorted keys in Amazon Redshift can significantly improve query performance by reducing the amount of data scanned during query execution.

When setting up sorted keys, focus on columns that are frequently used in filtering, joining, or as part of the `WHERE` clause (e.g., `date`, `product_category`). This helps Redshift organize data efficiently and can greatly speed up query performance.

### **Handling Locks and DDL Statements**

**Locking** is a protection mechanism that controls how many sessions can access a table at the same time. Locking also determines which operations can be performed in those sessions. Most relational databases use row-level locks. However, Amazon Redshift uses table-level locks. You might experience locking conflicts if you perform frequent DDL statements on user tables or DML queries.

Amazon Redshift has three lock modes:

- **AccessExclusiveLock**: Acquired primarily during DDL operations, such as ALTER TABLE, DROP, or TRUNCATE. AccessExclusiveLock blocks all other locking attempts.
- **AccessShareLock**: Acquired during UNLOAD, SELECT, UPDATE, or DELETE operations. AccessShareLock blocks only AccessExclusiveLock attempts. AccessShareLock doesn’t block other sessions that are trying to read or write on the table.
- **ShareRowExclusiveLock**: Acquired during COPY, INSERT, UPDATE, or DELETE operations. ShareRowExclusiveLock blocks AccessExclusiveLock and other ShareRowExclusiveLock attempts but doesn’t block AccessShareLock attempts.

When DDL statements like **TRUNCATE** are executed, they require an `AccessExclusiveLock` on the table. This lock can conflict with other queries that hold `AccessShareLock`, potentially causing blocking or delays.

If a **TRUNCATE** operation is hanging due to locked tables, you can identify and terminate the session (PID) that is holding the lock using the `pg_terminate_backend` function. If this does not resolve the issue, consider rebooting the cluster.

#### **Lock command**

The Lock command restricts access to the database table. This command enables the acquisition of a table-level lock in `ACCESS EXCLUSIVE` mode. It waits, if necessary, for any conflicting locks to be released. Explicitly locking a table in this way causes other transactions or sessions to wait when attempting to read or write to the table.

### **Procedures**

In Amazon Redshift, stored procedures serve as a mechanism to encapsulate logic for various operations, such as data transformation and validation, as well as business-specific logic. This encapsulation allows for a more streamlined and efficient process, particularly when dealing with multiple SQL statements. By bundling these statements into a single stored procedure, the number of round trips between the custom-built application and the database can be significantly reduced, thereby simplifying the operation and enhancing network traffic efficiency.

Stored procedures are stored directly in the database, making them accessible to any user with the appropriate privileges. They offer a level of flexibility not found in user-defined functions (UDFs), as they can incorporate not only SELECT queries but also data definition language (DDL) and data manipulation language (DML). Unlike UDFs, stored procedures do not necessarily need to return a value.

### **Query Editor**

The query editor v2 in Amazon Redshift can execute SQL commands directly within the Redshift console, including commands to refresh materialized views. These commands can be scheduled to execute automatically and can be set up directly in the Redshift environment. This provides a straightforward method to automate materialized view updates without additional infrastructure or services, thereby minimizing operational overhead.

### **Query Performance**

Amazon Redshift offers several features to enhance query performance, such as result caching, concurrency scaling, and **materialized views**.

Materialized views in Redshift provide a powerful way to optimize query performance for repeated and predictable query workloads. They precompute and store the results of a query, which improves the speed of data retrieval for complex queries that are run frequently. This makes them particularly useful for scenarios where data from historical records stored in Amazon S3 is regularly queried alongside newer data. Redshift can use materialized views to serve query results from precomputed data, reducing the compute resources required and accelerating query performance, thus enabling near real-time analytics on large datasets.

Amazon Redshift Concurrency Scaling allows the system to handle a varying number of incoming queries. As concurrency increases, Amazon Redshift automatically adds query processing power in seconds to process queries without any delays. Once the workload demand subsides, this extra processing power is automatically removed, so you pay only for the time when Concurrency Scaling clusters are in use.

### **VACUUM Command**

In Amazon Redshift, the **VACUUM** command is crucial for optimizing query performance and reclaiming disk space. It consolidates blocks of data marked for deletion through updates and deletes.

Over time, inserting, updating, or deleting rows leads to logically marked rows for deletion, consuming space until VACUUM is executed.

The primary VACUUM commands are:

- **VACUUM FULL**: Reclaims disk space, rebuilds indexes, and re-sorts all rows.
- **VACUUM DELETE ONLY**: Reclaims disk space from deleted rows without re-sorting, faster than FULL.
- **VACUUM REINDEX**: Analyzes interleaved sort key column distributions and performs a full VACUUM with re-sorting.
- **VACUUM SORT ONLY**: Sorts the table without reclaiming disk space, useful when rows are unsorted but space is adequate.

> Note: Redshift automatically runs VACUUM DELETE ONLY to reclaim space from deleted rows.

## Redshift Workload Management

Amazon Redshift workload management (WLM) enables flexible management priorities within workloads so that short, fast-running queries don't get stuck in queues behind long-running queries. Amazon Redshift creates query queues at runtime according to service classes, which define the configuration parameters for various types of queues, including internal system queues and user-accessible queues.

Redshift offers automatic workload management, called automatic WLM, which is tuned to handle varying workloads and is the recommended default. With automatic WLM, Redshift determines resource utilization as queries arrive and dynamically determines whether to run them on the main cluster, on a currency-scaling cluster, or to send each to a queue. When queries are queued, automatic WLM prioritizes shorter-duration queries. Automatic WLM maximizes total throughput and enables you to maintain efficient data-warehouse resources.

An important metric to measure the success of workload management configuration is system throughput, which in other words is how many queries are completed successfully. System throughput is measured in queries per second.

Each queue can be configured to run a certain number of queries concurrently. The maximum number depends on your Redshift cluster's size and configuration. Queries can be prioritized within queues based on criteria such as user groups or query groups, ensuring critical reports and dashboards have resources prioritized over less critical jobs.

## **Amazon Redshift Logging and Auditing**

    Amazon Redshift provides **database auditing** functionality, logging connections, user activities, and query execution details. These logs are stored in Amazon S3 for easy access and security.

- **Connection Log**: Records authentication attempts, connections, and disconnections.
- **User Log**: Tracks changes to database user definitions.
- **User Activity Log**: Records the queries users run, providing insight into query patterns and system usage.

These logs are helpful for security audits, troubleshooting, and monitoring database operations. You can query these logs directly from Amazon S3.

While the connection and user log contain the same information as the system tables, the log files provide an easier way to retrieve and review the information. You need database permissions to query the system tables, while the log files rely on Amazon S3 permissions. Viewing the information in log files instead of querying system tables also helps reduce the impact of interacting with the database.

### CloudTrail

AWS CloudTrail and Amazon Redshift integration offers detailed records of Redshift API calls, including API caller identity, time, source IP address, request parameters, and response elements. This integration provides a comprehensive audit trail that includes the actions taken within the Redshift cluster and API interactions that affect the cluster’s configuration or data query. The built-in audit logging and CloudTrail integration create a robust monitoring solution that secures and audits a Redshift environment, ensuring compliance obligations are met, and data is safeguarded. This dual approach simplifies audit log management and enhances the visibility of operations within and around the Redshift clusters, making it easier to take a proactive stance on security and compliance.

## Access

### **Managing User Access with Database Groups**

- Amazon Redshift allows you to create **database groups** to manage user access efficiently. By assigning users to groups based on their roles and responsibilities, you can streamline permission management.
- Permissions are granted at the group level, allowing for easier access control across multiple users. This approach helps with scaling and managing large teams by reducing the need for individual user permission modifications.

### **Granting and Revoking User Permissions**

- Use the **GRANT** and **REVOKE** commands to manage user permissions in Amazon Redshift. These commands allow you to specify which actions a user or role can perform on specific tables, views, and schemas, ensuring proper access control.

### **Row-Level Security**

- Implement **row-level security** to control access to specific rows within a table based on user roles. This ensures that users (such as analysts) only have access to the data that is relevant to their tasks.
- With row-level security, users can be granted access to an entire table but restricted to specific rows based on predefined conditions, ensuring data privacy and compliance.
- Additionally, integrating Amazon Cognito provides a seamless way to authenticate users using their Amazon.com accounts, ensuring a secure and scalable solution for managing user identities and access.

### **Role-Based Access Control (RBAC)**

- Amazon Redshift supports **Role-Based Access Control (RBAC)**, which allows you to define hierarchical roles and assign permissions to these roles. This simplifies permission management, as a role can inherit the privileges of other roles.
- With RBAC, Redshift ensures that users only have access to the data they are authorized to view and interact with, helping maintain data security and integrity.

## Reliability

- Amazon Redshift is launching data warehouse availability improvements by introducing a **Multi-AZ** deployment that supports running a Redshift data warehouse in multiple AWS Availability Zones (AZ) simultaneously to continue operating in unforeseen failure scenarios.

- The **cluster relocation** feature moves a cluster to another AZ in one step without requiring application changes. You can invoke the relocation function in cases where resource constraints in a given AZ are disrupting cluster operations such as the ability to resume or resize a cluster. This feature is available for use on clusters leveraging the RA3 instance family and is offered at no additional cost.

- Amazon Redshift offers **automated snapshots** and **recovery points** to customers at no charge. These snapshots and recovery points can be used to recover an entire cluster or table from a previous point in time to recover from failures. You can configure Amazon Redshift to automatically copy snapshots (automated or manual) for a cluster to another AWS Region. When a snapshot is created in the cluster's primary AWS Region, it's copied to a secondary AWS Region. If you store a copy of your snapshots in another AWS Region, you can restore your cluster from recent data if anything affects the primary AWS Region.

## Scaling

Amazon Redshift offers two main methods for scaling, allowing accommodation for both storage and computing needs: **Elastic Resize** and **Concurrency Scaling**. Additionally, Redshift supports managed storage to automatically scale storage capacity.

- **Elastic Resize** is used to change the number of nodes in a Redshift cluster to adjust the compute resources available, or by switching to nodes of different types or sizes within the cluster. This method is ideal for scenarios where you need more (or fewer) compute resources due to changes in demand, such as end-of-month reporting or seasonal data analysis increases. Elastic Resize usually completes within a few minutes, minimizing downtime. It's particularly useful for workload spikes that can be anticipated and planned for.

- **Concurrency Scaling** enables Redshift to support thousands of concurrent users and concurrent queries, with consistently fast query performance. When you turn on concurrency scaling, Amazon Redshift automatically adds additional cluster capacity to process an increase in both read and write queries. Users see the most current data, whether the queries run on the main cluster or a concurrency-scaling cluster. Elastic Resize changes the cluster's actual size; Concurrency Scaling temporarily augments processing capability for queries without altering the cluster's physical configuration.

### **RA3 Nodes and Managed Storage**

**RA3 nodes** use **Managed Storage**, which automatically offloads infrequently accessed (cold) data to Amazon S3 while keeping frequently accessed (hot) data on SSDs. This helps in cost-effective scaling of storage, as compute and storage can be scaled independently.

This feature helps optimize both storage costs and query performance by using high-performance SSDs for hot data and utilizing Amazon S3's scalability for cold data.

## Distribution

Distribution styles determine how data is allocated across the nodes in a cluster. Choosing an optimal distribution style is crucial for query performance, as it affects both the storage and the speed of your query executions.

- **Auto**: When you choose AUTO, Redshift automatically manages the data distribution based on the size of the tables and the query workload. For large tables, Redshift might use the **KEY** distribution style, while for smaller tables, it might choose the **ALL** distribution style.
- **Even**: Redshift distributes the rows of the table evenly across all slices in the cluster. It doesn't use any specific column as the distribution key. Suitable for tables without a natural join key or those not frequently joined. It prevents skew but can lead to more cross-node traffic during queries that involve table joins.
- **Key**: You specify a column as the distribution key. Redshift distributes the rows of the table across the nodes based on the values in the distribution key column. Rows with the same key value are stored on the same slice. Best for tables frequently joined on the distribution key column. It minimizes data movement across nodes during joins, leading to faster query execution. Choose a key that distributes data evenly to avoid data skew.
- **All**: The ALL distribution style copies the entire table to every node. This results in faster query performance for small tables due to local joins (no network traffic) but uses more storage.

## Redshift Lambda UDF

Amazon Redshift can use custom functions defined in AWS Lambda as part of SQL queries. You can write scalar Lambda UDFs in any programming languages supported by Lambda, such as Java, Go, PowerShell, Node.js, C#, Python, and Ruby. Or you can use a custom runtime. You do so by creating an EXTERNAL FUNCTION linking to the lambda.

The CREATE EXTERNAL FUNCTION command requires authorization to invoke Lambda functions in AWS Lambda. To start authorization, specify an AWS Identity and Access Management (IAM) role when you run the CREATE EXTERNAL FUNCTION command with enough permissions.

## Extras for the exam

- A star schema, ideal for data warehousing with Redshift, consists of a central fact table linked to dimension tables (e.g., customers, products).
- Redshift Streaming Ingestion allows near real-time data ingestion from Kinesis data streams, handling high volumes with low latency.
- The **MERGE** operation efficiently ingests data by performing combined INSERT and UPDATE operations based on specified conditions, minimizing separate operations and performance impacts.
