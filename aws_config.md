# AWS Config

AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. With AWS Config, you can track the configuration history of AWS resources and assess their compliance with internal guidelines and best practices. It provides a detailed history of resource configurations and can evaluate whether resources meet compliance standards. AWS Config helps answer questions such as, *What did my AWS resource look like at a specific point in time?* and *How has the configuration of my resources changed over time?*

> While AWS Config focuses on resource-specific history, audit, and compliance, it does not offer direct feedback on architectural best practices, unlike services such as AWS Well-Architected Tool.

## Key Features of AWS Config
- **Resource Configuration History**: AWS Config records the configurations of your AWS resources and tracks how they change over time. You can review the historical configurations and relationships between resources.
- **Compliance Evaluation**: AWS Config helps evaluate whether your AWS resources comply with predefined rules, and it allows you to create custom rules tailored to your compliance standards.
- **Managed Rules**: AWS Config provides over 75 predefined, AWS-managed rules that you can use to assess resource compliance. For example, you can check if ACM certificates are expiring, or if security groups allow unrestricted SSH access.
- **Custom Rules**: You can create custom AWS Config rules, defined in AWS Lambda, to evaluate specific conditions in your environment, such as checking whether EC2 instances are of a specific instance type (e.g., `t2.micro`) or if an EBS disk is of a certain type (e.g., `gp2`).
- **Change Tracking and Notifications**: AWS Config tracks changes to resource configurations and can send alerts (SNS notifications) when a change occurs.
- **Region-specific**: AWS Config is a per-region service, but it supports aggregation across multiple regions and accounts for centralized management.
- **Storage and Analysis**: Configuration data can be stored in Amazon S3, and you can analyze this data using Amazon Athena.

## Config Rules and Remediations
- **Evaluation Triggers**: Config rules can be triggered when a configuration change occurs or on a scheduled interval (e.g., daily, weekly). This allows you to continuously monitor the compliance status of your AWS resources.
- **Remediations**: You can automate remediation actions for non-compliant resources by invoking SSM Automation Documents or Lambda functions. If a resource is non-compliant after an automatic remediation attempt, you can set remediation retries.
- **No Preventative Measures**: AWS Config rules do not prevent actions from being taken on resources (i.e., they don’t "deny" actions); they simply provide insights into compliance status.

## AWS Config Pricing
- **Pricing Model**: AWS Config pricing is based on the number of configuration items recorded per region and the number of rule evaluations per region. The cost is:
  - $0.003 per configuration item recorded per region
  - $0.001 per config rule evaluation per region
- **No Free Tier**: AWS Config does not offer a free tier for its service.

## CloudWatch vs CloudTrail vs AWS Config

| Feature | **AWS CloudWatch** | **AWS CloudTrail** | **AWS Config** |
| --- | --- | --- | --- |
| **Primary Function** | Monitor and log AWS resource performance, including metrics and logs | Track user activity, API usage, and changes in AWS resources | Assess, audit, and track AWS resource configurations and compliance |
| **Focus Area** | Metrics, Logs, Alarms, Dashboards | Event logs for actions on AWS resources | Configuration history, compliance, and audits of resources |
| **Event Logging** | Logs and metrics for AWS resources and applications | Logs of AWS API calls and resource activities | Tracks configuration changes and relationships between resources |
| **Visibility** | Resource performance and operational health | User activities and resource changes (e.g., API calls, sign-ins) | Historical configuration of resources and compliance over time |
| **Data Storage** | Logs and metrics stored in CloudWatch, can be archived in S3 | Logs stored in S3 or CloudWatch Logs | Configuration history stored in S3, can be queried with Athena |
| **Automated Actions** | Alarms, Auto Scaling, automated remediation (via Lambda) | Insights events for anomaly detection, can trigger alarms | Remediation via SSM Automation, Lambda, retries for non-compliant resources |
| **Rule Management** | Can create custom alarms and metric filters | No rules (focused on logging) | Predefined AWS-managed rules and customizable Lambda-based rules for compliance |
| **Default Behavior** | Always on, collects resource data and metrics | Always on, logs all API calls and activities by default | Not always on, must be explicitly configured to track resources |
| **Retention Period** | Configurable based on logs (e.g., 7 days, or archived in S3) | Retention up to 7 years | Retention up to 7 years |
| **Integration with Other Services** | Integrates with Lambda, EC2, SNS, S3, etc. | Integrates with CloudWatch, S3, EventBridge, and security tools | Integrates with Lambda, S3, Athena, and Security Hub |
| **Use Case** | Monitoring, alerting, operational troubleshooting | Auditing, security monitoring, and tracking user activity | Compliance auditing, resource configuration tracking, and historical analysis |

- **AWS CloudWatch** is focused on performance monitoring, logs, and metrics for AWS resources and applications, providing real-time monitoring and alerts.
- **AWS CloudTrail** focuses on tracking user and API activity across AWS resources, offering logs of API calls and management events for auditing and security purposes.
- **AWS Config** is specialized in tracking and auditing the configurations of AWS resources, providing a detailed historical record for compliance and resource relationship analysis. 
