# Amazon Kinesis Data Streams

Amazon Kinesis Data Streams (KDS) is a powerful, scalable service for capturing, processing, and analyzing large volumes of streaming data in real-time. It is designed to handle gigabytes of data per second from sources such as clickstreams, social media feeds, and application logs, enabling applications to process data almost immediately for analytics or decision-making. This section delves into essential components, operational modes, producer and consumer integrations, and best practices for scaling Kinesis Data Streams.

## Architecture

- **Shards and Partitioning**
  - **Shards** are the fundamental unit of capacity in Kinesis Data Streams, each capable of handling 1 MB/second of data input and 2 MB/second of data output. A stream consists of one or more shards, and the total throughput capacity of a stream is determined by the number of shards it contains.
  - Data is partitioned across shards using a **partition key** assigned when writing records to the stream. Records with the same partition key are assigned to the same shard, preserving the order of records within each shard.

- **Service Modes**
  - **Provisioned Mode**: Ideal for predictable workloads, allowing users to define the number of shards upfront. This mode supports auto-scaling within predefined limits and requires regular monitoring and management to ensure optimal performance without over-provisioning or throttling.
  - **On-Demand Mode**: Best for unpredictable workloads, dynamically adjusting capacity based on incoming data volume, freeing users from managing shard counts. This mode is convenient for applications with variable or unknown data traffic patterns.

## Producers

When integrating with Amazon Kinesis Data Streams, AWS provides several tools and libraries, including the AWS SDK, the Kinesis Producer Library (KPL), and the Kinesis Agent.

- **AWS Software Development Kit (SDK)**:  
  The AWS SDK is a set of libraries and tools available for various programming languages and platforms, allowing developers to interact with AWS services programmatically. For Amazon Kinesis Data Streams, the AWS SDK enables direct creation, configuration, and management of streams, as well as sending and receiving data records from applications.

  To insert data into a stream, you typically use the `PutRecord` or `PutRecords` API operations. **PutRecords** supports batching, which improves throughput and reduces data ingestion costs by allowing up to 500 records or 5MB of data (whichever comes first) to be sent in a single API call.

- **Kinesis Producer Library (KPL)**:  
  The Kinesis Producer Library (KPL) is a high-level, easy-to-use library designed specifically for efficient batch insertion of large volumes of data records into a Kinesis Data Stream. Written in Java (with a native C++ core for performance), KPL is best suited for applications that require high-throughput data ingestion, where manually managing batching, buffering, and retry logic would be inefficient.

  The KPL simplifies the data-sending process to Kinesis Data Streams by offering built-in capabilities for efficient data batching, asynchronous operations for enhanced throughput, and automatic retry mechanisms to handle transmission failures. This makes it ideal for streaming clickstream data and server logs from Java applications, ensuring reliable, efficient data collection with minimal development effort.

  - **Synchronous and Asynchronous Publishing**:  
    In KPL, data can be published either synchronously or asynchronously, with asynchronous publishing being the default and recommended mode. The **RecordMaxBufferedTime** parameter controls how long a record will be buffered (in milliseconds) before transmission to the Kinesis Data Stream. This buffering allows KPL to aggregate records into larger requests, optimizing network and throughput performance.

    - For applications sensitive to latency, a lower buffered time may be preferred to ensure near real-time data processing.
    - For applications prioritizing maximum throughput, increasing the buffered time can allow more records to aggregate per request, reducing API calls and potentially decreasing costs.

- **Kinesis Agent**:  
  The Kinesis Agent is a pre-built, standalone Java application for Linux-based systems, providing a simple way to collect and send data to Kinesis Data Streams (and Kinesis Firehose) from files. The agent monitors files for changes, parses log entries, and automatically sends them to Kinesis Data Streams, handling tasks like file rotation, checkpointing, and retries. This setup simplifies the process of sending log data and metrics to Kinesis without requiring custom logging code.

## Consumers

Each data record in Kinesis Data Streams can be up to 1 MB in size. Each shard supports a read throughput of up to 2 MB per second, which is shared among all consumers accessing that shard. When multiple consumers access the same shard concurrently, they share this bandwidth, impacting data retrieval speed.

When retrieving data using the `GetRecords` API, a consumer can retrieve up to 10 MB of data in a single request from a shard. If a shard is polled less frequently, data accumulates in the shard, allowing larger batch retrievals (up to the 10 MB limit) per call. The **GetRecords call limit** is 5 calls per second per shard. Exceeding this rate triggers throttling and returns a `ProvisionedThroughputExceededException`. To avoid this, consider implementing a **backoff strategy**, where consumers pause and retry requests, especially during high-throughput periods.

### Kinesis Client Library (KCL)

The **Kinesis Client Library (KCL)** is designed for distributed stream processing, simplifying data retrieval from multiple shards and enabling effective load balancing and failure recovery.

- **Distributed Processing**: KCL automatically manages the coordination of data records across multiple shards. Each shard is processed in parallel by separate processors, referred to as “workers,” which run on consumer instances (e.g., EC2 instances or containers). KCL ensures that each shard is assigned to a single worker at any given time, maintaining efficient and balanced data processing.

- **Checkpointing**: One of KCL’s key features is checkpointing, which allows applications to track progress in processing the stream. By setting successful processing points, or “checkpoints,” within a shard, applications can resume from the last checkpoint in the event of a failure or restart. KCL uses DynamoDB to store checkpoints.
  - **Cost and Capacity Management**: Frequent checkpointing increases DynamoDB write operations, consuming more write capacity units (WCUs). Developers must balance the need for accurate checkpointing with the costs of DynamoDB usage to avoid exceeding provisioned throughput. Tuning the checkpoint frequency based on application needs helps control costs and maintain optimal performance.

### AWS Lambda Integration

AWS Lambda offers a serverless solution for consuming streaming data from Kinesis Data Streams. In this integration, Lambda automatically polls the Kinesis stream and processes new data as it becomes available.

- **Polling and Scaling**: Lambda continuously checks the stream and retrieves new records. It scales automatically based on the number of shards, with each Lambda invocation handling data from a single shard.
- **Parallel Processing**: The **Parallelization Factor** in Lambda allows multiple concurrent processes per shard, enhancing throughput for high-volume applications.
- **Event Source Mapping**: Lambda can be configured to trigger automatically when new records arrive in the Kinesis Data Stream using an event source mapping.
- **Dead-Letter Queue (DLQ)**: Configuring a DLQ in Lambda helps manage unprocessed events by storing them for later review, providing a failsafe for error handling in data processing.

These components and strategies allow consumers to retrieve and process data from Kinesis Data Streams efficiently, enabling applications to handle high-throughput and distributed workloads effectively.


## Scaling

- **Shard Splitting and Merging**
  - Shards in Kinesis Data Streams can be split to increase capacity or merged to reduce costs. Splitting a shard doubles the capacity for handling write and read operations, while merging combines two adjacent shards to save on resources when data volume decreases. Managing shard scaling effectively requires applications to handle data ordering across split and merged shards by processing all data from parent shards before the children.

  - One implication of splitting and merging shards is the potential impact on data ordering. Kinesis Data Streams guarantees the order of records within a shard, but this order can become complicated when shards are split or merged. When a shard is split, records that used to go to the parent shard based on their partition key will now be distributed to one of the two new child shards. To maintain data sequence integrity, applications should process all records from the parent shard before processing records from the child shards.

- **Enhanced Fan-Out**
  - By default, the 2MB/second/shard output is shared between all applications consuming data from the stream. **Enhanced Fan-Out** provides a dedicated 2 MB/sec read throughput per shard per registered consumer, avoiding throughput-sharing limits. This feature is critical for low-latency, high-throughput scenarios, enabling each consumer (e.g., Lambda) to process data independently.

- **Resharding**
  - When data throughput exceeds allocated shard capacity, Kinesis may return a **ProvisionedThroughputExceededException**. **Resharding** (adding more shards), **backoff strategies** (pausing and retrying requests), and an appropriate **partition key** help manage read rates during high-traffic periods. These tactics are essential for maintaining steady data flow and handling throughput bottlenecks.

- **Manual and Automated Scaling**
  - While Kinesis Data Streams does not automatically scale shard numbers based on traffic, AWS provides APIs and tools for monitoring and manually adjusting shard counts. Scaling operations can be time-consuming, sometimes taking up to several hours, so proactive monitoring and timely adjustments are necessary to avoid throttling and ensure smooth operations.

## Data Duplication and Reliability

Data duplicates in Amazon Kinesis Data Streams can arise on both the producer and consumer sides. On the **producer side**, duplicates often occur due to network issues or service errors where acknowledgments for submitted records are lost, causing producers to resend data that may have already been successfully received. On the **consumer side**, duplicates may result from processing retries following failures, especially if checkpointing (marking a record as processed) is not correctly managed.

To mitigate these challenges, developers can employ several strategies:

- **Idempotent Writes**: Ensuring idempotent writes, where submitting the same data multiple times does not result in duplicates, often involves assigning unique identifiers for each record. 
- **Unique Keys in Target Data Stores**: Ensuring idempotency in target data stores or databases can automatically resolve duplicates by using unique keys derived from the data itself.

## Best Practices for Optimizing Performance

1. **Partition Key Design:**
Choosing a partition key that distributes records evenly across shards is essential for optimal throughput. Poorly distributed partition keys can lead to hot shards, where one shard becomes a bottleneck while others are underutilized.

2. **Enhanced Fan-Out for Multiple Consumers:**
Enhanced Fan-Out is beneficial in scenarios where multiple applications need to consume data from the same shard. This feature provides dedicated throughput for each consumer, improving performance and reducing data retrieval latency.

3. **Configuring Lambda for High-Volume Data:**
Adjusting the Parallelization Factor in AWS Lambda improves concurrency and throughput, particularly in high-volume streaming applications. Combining this with Enhanced Fan-Out can ensure faster data processing and minimize the IteratorAge (time records wait in the stream before processing).
