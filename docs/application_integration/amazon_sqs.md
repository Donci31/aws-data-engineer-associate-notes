# Amazon Simple Queue Service (SQS)

Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

## Queue Types

### Standard Queue

- The **standard queue** is the original offering of Amazon SQS and is designed for applications requiring high throughput.
- **Features**:
    - Scales automatically, supporting up to **15,000 messages per second**.
    - Guarantees **at-least-once delivery**, meaning messages may be delivered more than once.
    - Messages may arrive **out of order** (best-effort ordering).
    - Maximum message size: **256 KB**.
    - Default retention period of **4 days**, extendable up to **14 days**.

### FIFO Queue

- The **First-In-First-Out (FIFO) queue** is designed for applications requiring strict ordering and exactly-once message delivery.
- **Features**:
    - Guarantees messages are delivered in the exact order they are sent.
    - Ensures each message is processed exactly once.
    - Supports up to **3,000 messages per second** with batching or **300 messages per second** without batching.
    - FIFO queues must have names ending with the `.fifo` suffix. The suffix counts towards
the 80-character queue name limit. To determine whether a queue is FIFO, you can check
whether the queue name ends with the suffix.
    - **Deduplication**: Messages with the same deduplication ID sent within **5 minutes** are treated as duplicates and are not processed again.

> Note: You can’t convert an existing standard queue into a FIFO queue. To make the move, you must either create a new FIFO queue for your application or delete your existing standard queue and recreate it as a FIFO queue.

## Decoupling Applications

SQS serves as a virtual queue between application components, allowing each component to operate independently. For example:

- **File Uploads and Processing**: Files uploaded to Amazon S3 can trigger notifications sent to an SQS queue. These messages can then be processed by an AWS Lambda function, separating upload operations from downstream processing tasks.
- **Scaling**: Decoupling applications helps distribute workloads across multiple consumers, improving fault tolerance and enabling horizontal scaling.

## Message Retention and Visibility Timeout

### Message Retention

- Messages are stored in a queue for a default retention period of **4 days**. This can be configured up to a maximum of **14 days**.
- Retention ensures that messages are available for processing even if a consumer application faces downtime.

### Visibility Timeout

- The **visibility timeout** is the period during which a message is temporarily hidden from other consumers after being received.
- The default visibility timeout is **30 seconds**, with a configurable range between **0 seconds** and **12 hours**.
- If the message is not deleted within the timeout period, it becomes available for processing by other consumers. This prevents message duplication and ensures reliable processing.

## Polling Mechanisms

### Short Polling

- Immediately returns a response, even if no messages are available in the queue.
- Best suited for applications requiring rapid responses but may result in higher costs due to empty responses.

### Long Polling

- Waits for messages to become available or until the polling timeout expires.
- Reduces the number of empty responses, making it more cost-efficient for low-traffic queues.
- Configured using the `ReceiveMessageWaitTimeSeconds` parameter, with a maximum wait time of **20 seconds**.
- Using long polling can reduce the cost of using SQS because you can reduce the number of empty receives.

## Delay Queues and Message Timers

### Delay Queues

- Delay queues allow the postponement of message delivery for up to **15 minutes**.
- Useful when the consumer application needs additional time to prepare before processing messages.
- Configured using the `DelaySeconds` setting at the queue level.

### Message Timers

You can use message timers to set an initial invisibility period for a message added
to a queue. So, if you send a message with a 60-second timer, the message isn’t visible to consumers for its first 60 seconds in the queue. The default (minimum) delay for a message is 0 seconds. The maximum is 15 minutes.

## Dead-Letter Queues (DLQs) and Redrive Policies

- **Dead-Letter Queues** are used to isolate and handle messages that fail processing. Messages are moved to a DLQ after exceeding a configurable `MaxReceiveCount`.
- **Benefits**:
    - Debugging: DLQs help identify problematic messages and issues in the processing pipeline.
    - Redrive Policy: Configured at the source queue, specifying the `MaxReceiveCount` and the DLQ to target failed messages. MaxReceiveCount refers to the number of times a message can be received from the queue before being deleted. When the limit is reached, the message will be sent to the dead letter queue. The default Maximum received is 10.

> Note: You cannot use dead-letter queues to postpone the delivery of new messages to the queue for a few seconds.

---

## Security Features

### Encryption

- **In-Transit**: Messages are encrypted using HTTPS endpoints.
- **At Rest**: Enable **Server-Side Encryption (SSE)** with AWS Key Management Service (KMS) for encrypting message bodies.
- SSE encrypts only the message body, not metadata (e.g., message ID, timestamps, attributes).

### Access Control

- Managed using **IAM policies** and **queue access policies**, allowing fine-grained control over who can interact with the queue.
- **Private Access**: AWS customers can access Amazon Simple Queue Service (Amazon SQS) from their Amazon Virtual Private Cloud (Amazon VPC) using VPC endpoints, without using public IPs, and without needing to traverse the public internet. VPC endpoints for Amazon SQS are powered by AWS PrivateLink, a highly available, scalable technology that enables you to privately connect your VPC to supported AWS services.

## Backlog Metrics for Auto Scaling

If you use a target tracking scaling policy in an Auto Scaling Group of EC2 instances based on a custom Amazon SQS queue metric, you may use an existing CloudWatch Amazon SQS metric like `ApproximateNumberOfMessagesVisible` for target tracking but you could still face an issue so that the number of messages in the queue might not change proportionally to the size of the Auto Scaling group that processes messages from the queue. The solution is to use a backlog per instance metric with the target value being the acceptable backlog per instance to maintain. To calculate your backlog per instance, divide the ApproximateNumberOfMessages queue attribute by the number of instances in the InService state for the Auto Scaling group. Then set a target value for the Acceptable backlog per instance.

- Example:
    - If an Auto Scaling group has 10 EC2 instances and a queue has 1,500 messages, each instance processes 150 messages.
    - If each instance takes 0.1 seconds per message, processing 150 messages takes **15 seconds**.
    - To reduce processing time to **10 seconds**, increase the instance count to **15**.

## Use Cases

1. **Decoupling Applications**: For example, asynchronously handling payment processing in e-commerce systems.
2. **Buffering Writes to Databases**: Managing spikes in incoming data, such as in a voting system.
3. **Scaling Workloads**: Integrating SQS with Auto Scaling and CloudWatch to dynamically scale resources based on queue size.
4. **Prioritization**: Use separate queues for tasks with different priorities, such as pro users versus lite users in photo processing applications.

## Advanced Features

### Temporary Queues

- **Temporary queues** are designed for use cases like request-response messaging patterns. They are application-managed and cost-effective.

### Message Batching

- FIFO queues support batching, which increases throughput to **3,000 messages per second**. Without batching, the throughput is limited to **300 messages per second**.

## Best Practices

1. **Message Deletion**: Always remember that the messages in the SQS queue will continue to exist even after the EC2 instance has processed it, until you delete that message. You have to ensure that you delete the message after processing to prevent the message from being received and processed again once the visibility timeout expires.
2. **Queue Separation**: Use different queues for tasks with varying priorities or requirements.
3. **Efficient Polling**: Opt for long polling to minimize costs and avoid excessive empty responses.
4. **Security**: Implement encryption and fine-grained access controls to protect sensitive data.

## SQS vs Kinesis Data Streams

| Feature                        | Amazon SQS                                  | Amazon Kinesis Streams                     |
|--------------------------------|---------------------------------------------|--------------------------------------------|
| **Use Case**                   | Decouple and scale microservices, distributed systems, or serverless applications. | Real-time streaming and analytics of data. |
| **Message Ordering**           | FIFO queues guarantee ordering; Standard queues do not. | Maintains strict order of records.         |
| **Message Delivery**           | At-least-once (Standard), exactly-once (FIFO). | Exactly-once delivery for consumer applications. |
| **Retention Period**           | 4 days (default), configurable up to 14 days. | 24 hours (default), configurable up to 365 days. |
| **Max Message Size**           | 256 KB per message.                         | Up to 1 MB per record.                     |
| **Message Processing**         | Polling-based; consumers retrieve messages. | Real-time streaming via shards.            |
| **Concurrency**                | Horizontal scaling of consumers; handles thousands of messages per second. | Each shard supports 1 MB/s input and 2 MB/s output. |
| **Use of Dead Letter Queues**  | Supports Dead Letter Queues for failed messages. | No direct support for DLQs; errors handled in application logic. |
| **Latency**                    | Low latency (<10 ms for Standard).          | Millisecond latency for real-time data.    |
| **Data Persistence**           | Limited to retention period.                | Streamed data is stored for replay within retention period. |
| **Scalability**                | Fully managed, scales automatically.        | Requires manual shard management for scaling. |
| **Integration with AWS Services** | Easy integration with Lambda, EC2, Auto Scaling, and S3. | Integrated with Lambda, analytics tools (e.g., Kinesis Data Analytics), and S3. |
| **Security**                   | Supports HTTPS in transit and KMS encryption for message bodies. | Supports HTTPS in transit, KMS encryption, and fine-grained IAM policies. |
| **Cost Model**                 | Pay per request and data transfer.          | Pay per shard-hour and data throughput.    |
| **Common Use Cases**           | Job queues, asynchronous processing, decoupling application components. | Streaming logs, metrics, IoT data, and analytics in real-time. |
