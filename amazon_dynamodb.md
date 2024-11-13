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

 While DynamoDB Auto Scaling is effective for gradually adjusting capacity in response to changing access patterns, it might not always scale up quickly enough to accommodate sudden, massive spikes in traffic, such as those expected during a major sale event. Auto Scaling adjusts capacity units based on predefined utilization metrics and thresholds, which can introduce a lag between the onset of increased demand and the scaling action, potentially leading to Provisioned Throughput Exceptions during periods of rapid traffic increase.

Application Auto Scaling with pre-defined schedules is ideal for this scenario, as it allows the company to automatically scale up DynamoDB capacity during known periods of high traffic and scale down during low-traffic times.

## Caching with DynamoDB Accelerator (DAX)

As datasets grow, the primary key design remains crucial, but for read-intensive workloads, the implementation of an in-memory cache can be a game-changer in maintaining high performance. 

DAX is an in-memory cache that reduces read times to microseconds for high-performance applications. It supports:

- **Item Cache:** Caches items based on primary key values for GetItem requests.
- **Query Cache:** Stores result sets for Query and Scan operations.

A DAX cluster consists of one or more nodes. Each node runs its own instance of the DAX caching software. One of the nodes serves as the primary node for the cluster. Additional nodes (if present) serve as read replicas. 

In a DAX cluster, throttling can occur if the request rate exceeds the capacity of the DAX cluster itself. Adding more nodes to the cluster increases its capacity and can help mitigate this issue or addin retry logic.

## Global Tables

DynamoDB’s global tables provide multi-Region, multi-active replication, making it ideal for globally distributed applications requiring low-latency access. Data changes propagate to all specified AWS regions, allowing users to access data with minimal delay from anywhere in the world.

DynamoDB global tables are ideal for massively scaled applications with globally dispersed users. In such an environment, users expect very fast application performance. Global tables provide automatic multi-active replication to AWS Regions worldwide. They enable you to deliver low-latency data access to your users no matter where they are located.

- **Monitoring:** You can use CloudWatch to observe the metric `ReplicationLatency`. This tracks the elapsed time between when an item is written to a replica table, and when that item appears in another replica in the global table. It’s expressed in milliseconds and is emitted for every source-Region and destination-Region pair. This metric is kept at the source Region. This is the only CloudWatch metric provided by Global Tables v2

- **Time To Live (TTL):** With global tables you configure TTL in one Region, and that setting is auto replicated to the other Region(s). When an item is deleted via a TTL rule, that work is performed without consuming Write Units on the source table - but the target table(s) will incur Replicated Write Unit costs.

- **Consistency:** Global tables employ a last-writer-wins reconciliation method to handle write conflicts, which can occur when multiple applications update the same item in different regions at the same time. This approach simplifies the design for developers by automatically resolving conflicts without the need for custom conflict resolution logic.

- **Streams:** Each global table maintains its own DynamoDB Stream that captures all write operations performed on the table. This stream includes write activities from every region that forms part of the global table. If you want processed local writes but not replicated writes, you can add your own Region attribute to each item. Then you can use a Lambda event filter to invoke only the Lambda for writes in the local Region.

## Streams

Amazon DynamoDB stream is an ordered flow of information about changes to items in Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table. Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attributes of the items that were modified. A stream record contains information about a data modification to a **single** item in a DynamoDB table. It can be chained with an AWS Lambda function that will be triggered to react to these changes.

A stream consists of stream records. Each stream record represents a single data modification in the DynamoDB table to which the stream belongs. Each stream record is assigned a sequence number, reflecting the order in which the record was published to the stream.

Stream records are organized into groups, or shards. Each shard acts as a container for multiple stream records, and contains information required for accessing and iterating through these records. The stream records within a shard are removed automatically **after 24 hours**.

You can enable a stream on a new table when you create it using the AWS CLI or one of the AWS SDKs. You can also enable or disable a stream on an existing table, or change the settings of a stream. DynamoDB Streams operates asynchronously, so there is no performance impact on a table if you enable a stream.

AWS maintains separate endpoints for DynamoDB and DynamoDB Streams. To work with database tables and indexes, your application must access a DynamoDB endpoint. To read and process DynamoDB Streams records, your application must access a DynamoDB Streams endpoint in the same Region. To read and process a stream, your application must connect to a DynamoDB Streams endpoint and issue API requests.

## PartiQL Support

PartiQL, a SQL-compatible query language, provides a familiar SQL syntax for managing data in DynamoDB. Available through AWS Management Console, NoSQL Workbench, CLI, and DynamoDB APIs, PartiQL simplifies querying, updating, and deleting data for developers accustomed to SQL.
