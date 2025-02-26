# Amazon EventBridge

Amazon EventBridge is a **serverless event bus service** designed to help applications react to events from **AWS services**, **custom applications**, or **SaaS applications**. It enables **event-driven architecture** by simplifying the ingestion, filtering, and routing of events to appropriate targets, facilitating decoupled microservices and system integration.

## Key Features and Capabilities

### Event Sources

1. **AWS Services**:
    - Natively integrates with over **90 AWS services**.
    - Developers don’t need to configure additional resources for ingestion.
2. **SaaS Applications**:
    - Direct integration with **third-party SaaS providers**, making it unique among AWS event-driven services.
3. **Custom Applications**:
    - Supports **custom events** from user applications using the AWS SDK.

### Event Targeting and Routing

- **JSON-Based Event Structure**:
    - Events follow a defined JSON structure.
    - Rules can be created to filter events based on attributes in the event body.
- **Targets**:
    - Supports over **15 AWS services** as event targets, including:
    - **AWS Lambda**
    - **Amazon SQS**
    - **Amazon SNS**
    - **Amazon Kinesis Data Streams**
    - **Amazon Kinesis Data Firehose**
- **Flexible Event Rules**:
    - **Event Patterns**:
    - Define rules that respond to specific events (e.g., "an EC2 instance changes state").
    - **Scheduled Events**:
    - Define cron-like schedules for recurring events, such as invoking a Lambda function every hour.

## Advanced Capabilities

1. **Event Buses**:
    - **Event Bus Types**:
        - **Default Event Bus**: For AWS service events.
        - **Custom Event Buses**: For custom applications.
        - **SaaS Event Buses**: For third-party SaaS applications.
    - **Cross-Account Access**:
        - Allows other AWS accounts to access your EventBus using **resource-based policies**.
    - **Use Case**:
        - Aggregate all events from multiple AWS accounts into a single account or region for centralized management.

2. **Event Archiving and Replay**:
    - Archive events (all or filtered) sent to an event bus for:
        - **Indefinite storage** or for a **specified retention period**.
    - **Replay Events**:
        - Reprocess archived events, useful for troubleshooting or testing new workflows.

3. **Schema Discovery and Registry**:
    - **Schema Discovery**:
        - Automatically infer the structure of events on your bus.
    - **Schema Registry**:
        - Enables developers to:
        - Store and manage event schemas.
        - Generate code bindings for applications to work seamlessly with event data.
        - Schemas can be **versioned** to support evolving event structures.

4. **Permissions Management**:
    - Manage **event bus permissions** with fine-grained controls.
    - Examples:
        - Allow/deny events from another AWS account or region.
    - Use Case:
        - Aggregate events across an **AWS Organization** in a central AWS account or region.

## Performance and Limits

- **Latency**:
    - Typical latency is around **0.5 seconds**.
- **Throughput**:
    - Limited by default but can be increased upon request.
- **Service Limits**:
    - Predefined limits for event buses, rules, and targets, adjustable via AWS support.

## Security

- **IAM Policies**:
    - Control which users and services can publish or subscribe to event buses.
- **Cross-Account Access**:
    - Resource-based policies enable secure sharing of event buses between accounts.
- **Encryption**:
    - Events are encrypted in transit using HTTPS.

## Example Use Cases

1. **Event-Driven Applications**:
    - React to **EC2 instance state changes** by triggering automated workflows using AWS Lambda.
2. **Centralized Event Aggregation**:
    - Aggregate events from all accounts in an **AWS Organization** to a single monitoring account for unified event processing.
3. **SaaS Integration**:
    - Automate workflows based on events from third-party tools like **Zendesk** or **Stripe**.
4. **Operational Monitoring**:
    - Forward AWS CloudTrail events to **Amazon Kinesis Data Streams** for real-time log analysis.
5. **Scheduled Events**:
    - Use cron-like scheduling to trigger periodic tasks, such as running maintenance scripts.

## Summary Table

| **Feature**                  | **Description**                                                                                             |
|-------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Event Sources**             | AWS services, SaaS applications, and custom applications.                                                  |
| **Event Targets**             | Supports over 15 AWS services, including Lambda, SQS, SNS, and Kinesis.                                    |
| **Event Structure**           | JSON-based events with rules for attribute-based filtering.                                                |
| **Event Buses**               | Default, custom, and SaaS event buses. Can be shared across accounts and regions.                          |
| **Archiving and Replay**      | Archive events indefinitely or for a defined period. Replay events for testing and debugging.              |
| **Schema Registry**           | Automatically discover, version, and store schemas. Generate code bindings for application integration.    |
| **Scheduling**                | Cron-like functionality for periodic tasks.                                                                |
| **Latency**                   | ~0.5 seconds.                                                                                             |
| **Throughput**                | Limited by default, adjustable via AWS support.                                                            |
| **Cross-Account Access**      | Manage permissions to share event buses across AWS accounts or regions.                                    |
| **Security**                  | IAM policies and encryption ensure secure event publishing and delivery.                                   |
