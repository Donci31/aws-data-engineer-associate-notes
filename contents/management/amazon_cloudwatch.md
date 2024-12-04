# Amazon CloudWatch

Amazon CloudWatch is a powerful monitoring service designed to provide insights into the performance and health of your AWS resources. It collects data across various AWS services, enabling monitoring of metrics, logs, and events.

## Amazon CloudWatch Metrics

CloudWatch provides a variety of metrics for monitoring AWS services, including EC2, ECS, and others. These metrics allow you to track the performance of individual resources and services in real-time.

### EC2 Instance Metrics

Some standard EC2 metrics include:

- **CPU Utilization**: Indicates the processing power required to run an application on an EC2 instance.
- **Network Utilization**: Measures the incoming and outgoing network traffic to a specific instance.
- **Disk Read and Write Metrics**: Tracks the amount of data read from and written to the instance's disk, which can provide insights into application performance.

### Custom Metrics

While CloudWatch provides a wide range of default metrics, certain system-level metrics like **memory utilization** and **disk space utilization** are not available out-of-the-box. You can create **custom metrics** to track these values by using CloudWatch Monitoring Scripts or installing the **CloudWatch Agent**.

### Metric Visualization

You can create CloudWatch Dashboards to visualize your metrics in real-time, allowing you to track performance and operational health across your AWS infrastructure. Dashboards provide a centralized view of all your metrics and alarms in one place.

## CloudWatch Alarms

CloudWatch Alarms are used to monitor AWS resources by setting conditions on various metrics. Once an alarm condition is met, it can trigger predefined actions to help automate management tasks. Some common actions include:

- **Stopping** or **terminating** EC2 instances to save costs when they're no longer needed.
- **Rebooting** EC2 instances to resolve certain system impairments.
- **Recovering** EC2 instances onto new hardware in the event of system health failures.

### Reboot and Recover Actions

The **reboot alarm action** is particularly useful for EC2 instance health check failures. This action is typically favored over the **recover alarm action**, which is better suited for system health check failures. For EC2 instance rebooting, using CloudWatch's native **Reboot Alarm Action** is recommended over using AWS Lambda or EventBridge triggers, which could be less efficient in terms of resource usage.

### Alarm States

CloudWatch alarms monitor resources through various states:

- **OK**: The monitored metric is within the defined threshold.
- **INSUFFICIENT_DATA**: There isn't enough data to evaluate the metric.
- **ALARM**: The metric is outside the defined threshold.

### Alarm Configuration

CloudWatch alarms can be configured based on metrics and include several options for evaluation:

- **Period**: The length of time (in seconds) to evaluate the metric.
- **Sampling options**: This can include evaluating the metric at different granularities (e.g., max, min, %, etc.).
- **High-Resolution Metrics**: CloudWatch allows custom metrics with resolutions as low as 10 seconds or multiples of 60 seconds.

### CloudWatch Composite Alarms

CloudWatch composite alarms allow you to monitor the state of multiple alarms together, combining them with **AND** or **OR** conditions. This helps reduce alarm noise, making it easier to monitor critical issues without being overwhelmed by minor fluctuations.

Composite alarms are especially useful when you need to track more complex situations. For example, you may want to trigger an alarm only if two different services are both showing problems, or if the system is experiencing issues but only in certain regions.

## CloudWatch Logs

### CloudWatch Logs Sources

CloudWatch Logs can be sourced from various AWS services and external systems:

- **EC2 Instances**: Logs from applications running on EC2.
- **AWS Lambda**: Function logs.
- **Elastic Beanstalk**: Collect logs from your application’s environment.
- **ECS Containers**: Capture logs from containers.
- **VPC Flow Logs**: Collect logs related to your VPC's network traffic.
- **CloudTrail**: Log API calls to AWS resources.
- **Route 53**: Log DNS queries.

### CloudWatch Logs Agent and Unified Agent

To enable log collection, you must install and configure the CloudWatch Logs Agent on your EC2 instances or on-premises servers. By default, EC2 instances do not send logs to CloudWatch unless configured.

- The **CloudWatch Logs Agent** can send logs to CloudWatch Logs but does not collect system metrics.
- The **CloudWatch Unified Agent** collects both logs and system-level metrics, such as CPU, memory, and disk utilization.

### Log Groups and Streams

CloudWatch Logs are organized into **Log Groups** and **Log Streams**:

- **Log Groups**: Represent a logical grouping of log data, often associated with a specific application or service.
- **Log Streams**: Contain individual logs, such as logs from specific EC2 instances, containers, or servers.

### Log Retention and Expiration

You can set retention policies for logs, allowing you to define when logs should expire (ranging from 1 day to 10 years). CloudWatch Logs also supports encryption by default, with the option to use **KMS-based encryption** for added security.

### Log Subscription Filters

CloudWatch Logs allows you to stream log data in real-time to destinations such as:

- **Kinesis Data Streams**
- **Kinesis Data Firehose**
- **AWS Lambda**
- **OpenSearch Service**

This enables you to perform advanced analysis or forward logs to other systems for further processing.

## CloudWatch Logs Insights

CloudWatch Logs Insights provides a powerful query language for analyzing log data stored in CloudWatch Logs. You can perform detailed searches to find specific log events, count occurrences of specific errors, and calculate aggregate statistics.

### Features of Logs Insights

- **Querying Multiple Log Groups**: You can query logs across multiple AWS accounts and log groups.
- **Search Flexibility**: Logs Insights allows you to filter logs, sort events, and aggregate data.
- **Dashboards**: You can save your queries and display the results in custom CloudWatch Dashboards.

### Real-Time vs Historical Data

Note that CloudWatch Logs Insights is designed for **log analysis** and **historical data**. It’s not meant for real-time log processing, which is better suited for solutions like **Kinesis** or **Lambda**.

## Integrating CloudWatch with AWS Services

### CloudTrail and CloudWatch Integration

AWS **CloudTrail** provides a detailed log of all API calls made in your AWS environment, which can be ingested into CloudWatch for monitoring and analysis. By analyzing CloudTrail logs in CloudWatch, you can identify potential security threats, track account activity, and create compliance reports.

CloudTrail log data can be transformed into CloudWatch metrics using **metric filters**, allowing you to create alarms based on specific API calls or events.
