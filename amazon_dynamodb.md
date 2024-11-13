# Amazon DynamoDB

Amazon DynamoDB is a fully managed, serverless, NoSQL database designed for high-performance applications. DynamoDB provides single-digit millisecond latency at any scale, supporting key-value and document data models for a wide range of use cases. Its serverless architecture frees developers from provisioning or managing servers, allowing them to focus on application development rather than database maintenance. 

In addition to high performance, DynamoDB offers built-in security, cross-region replication through global tables, and flexible pricing models that adapt to varying throughput needs. It integrates broadly with other AWS services like AWS CloudFormation, Amazon CloudWatch, Amazon S3, IAM, and AWS Auto Scaling, streamlining scalability and operational ease. 

DynamoDB Streams further extends DynamoDB’s functionality by enabling real-time data processing, as it captures a continuous, ordered flow of information about changes to table items.

## Capacity Modes

Amazon DynamoDB offers two capacity modes to handle table throughput:

### On-Demand Capacity

With on-demand capacity mode, DynamoDB automatically scales based on workload demands without requiring prior capacity planning. This pay-per-request model is ideal for applications with unpredictable or variable traffic.

### Provisioned Capacity

In provisioned capacity mode, you specify the read and write throughput required. Best suited for applications with predictable workloads, this mode allows cost optimization for stable traffic patterns. Throughput is measured in capacity units:

- **Read Capacity Units (RCUs):** One RCU provides one strongly consistent read per second (or two eventually consistent reads) for items up to 4 KB in size.
- **Write Capacity Units (WCUs):** One WCU supports one write request per second for items up to 1 KB in size.

#### Read Consistency

- **Strongly Consistent Reads:** Returns the most recent version of data, ensuring that a read immediately reflects any preceding writes.
- **Eventually Consistent Reads:** Provides a more cost-effective read option, though recent writes may not be immediately reflected.

Example calculations:
1. 9 strongly consistent reads per second for 4 KB items require 9 RCUs.
2. 16 eventually consistent reads per second for 12 KB items require 24 RCUs.
3. Writing 10 items per second at 2 KB each requires 20 WCUs.
4. Writing 6 items per second at 4.5 KB each requires 30 WCUs.

The AWS Management Console includes a Capacity Calculator to help estimate RCUs and WCUs based on application requirements.

## Secondary Indexes

Each DynamoDB table contains uniquely identifiable items with a primary key. This primary key can be:

- **Simple:** A single partition key.
- **Composite:** A partition key and a sort key, allowing multiple items with the same partition key to be stored together.

### Global Secondary Indexes (GSIs)

GSIs offer flexible access patterns by allowing queries on non-primary key attributes. They operate with independent provisioned capacity and can be added at any time, even to existing tables.

#### Example: BlogPosts Table

**Primary Key:**
- **Partition Key:** `AuthorID` (unique identifier for each author).
- **Sort Key:** `PostID` (unique identifier for each post).

**Attributes:** 
- `Title`, `Content`, `Category`, `PublishDate`

**Global Secondary Index:** Allows queries based on `Category` and `PublishDate` to retrieve posts by category, sorted by publish date.

### Local Secondary Indexes (LSIs)

LSIs enhance query flexibility within partitions by enabling additional sort key attributes while keeping the same partition key. Unlike GSIs, LSIs must be defined at table creation and share the partition key with the base table. LSIs allow up to five per table, offering access patterns by projecting additional non-key attributes for efficient querying. Important, LSI shares the same read and write capacity as the base table.

## Data Distribution

Data in DynamoDB is distributed across multiple partitions based on the **partition key**. This distribution can lead to "hot partitions" if a small subset of partition keys receives the majority of traffic, potentially causing throttling issues. Optimizing partition key design, such as using **high-cardinality keys**, helps distribute load evenly across partitions to prevent hot partition problems.

## Auto Scaling and Performance

DynamoDB Auto Scaling adjusts capacity units to meet changing demands based on predefined utilization metrics. However, for traffic spikes (e.g., during events), Application Auto Scaling with predefined schedules can better accommodate predictable load changes.

## Caching with DynamoDB Accelerator (DAX)

DAX is an in-memory cache that reduces read times to microseconds for high-performance applications. It supports:

- **Item Cache:** Caches items based on primary key values for GetItem requests.
- **Query Cache:** Stores result sets for Query and Scan operations.

Caching helps reduce the database load for read-intensive workloads, especially in scenarios where the dataset is too large to fit in memory on a single machine.

## Global Tables

DynamoDB’s global tables provide multi-Region, multi-active replication, making it ideal for globally distributed applications requiring low-latency access. Data changes propagate to all specified AWS regions, allowing users to access data with minimal delay from anywhere in the world.

### Monitoring and Consistency

- **ReplicationLatency** metric (CloudWatch) tracks data synchronization delay across regions.
- **Time To Live (TTL):** Global tables allow TTL configuration in one region, automatically replicating the setting to other regions.

## Streams

DynamoDB Streams captures all changes to table items, storing them in shards organized by sequence number, enabling real-time data processing. Stream records, each representing a single data modification, are automatically removed after 24 hours. Streams can be paired with AWS Lambda for event-driven processing, allowing integration with external systems for real-time synchronization or analytics.

## PartiQL Support

PartiQL, a SQL-compatible query language, provides a familiar SQL syntax for managing data in DynamoDB. Available through AWS Management Console, NoSQL Workbench, CLI, and DynamoDB APIs, PartiQL simplifies querying, updating, and deleting data for developers accustomed to SQL.
