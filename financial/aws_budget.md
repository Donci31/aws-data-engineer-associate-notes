# AWS Budgets

AWS Budgets is a cost management tool that allows you to set custom budgets for your AWS usage and costs, and receive notifications when those budgets are exceeded. It provides you with the flexibility to track and control your AWS spending more effectively, ensuring that your costs stay within acceptable limits. Whether you're managing a single account or multiple accounts, AWS Budgets can help you keep your cloud expenses in check and avoid unexpected charges.

## Budget Creation and Alarms

With AWS Budgets, you can easily create custom budgets for your usage, costs, or reserved instances and set alarms to alert you when spending exceeds the defined threshold. These alerts can be sent via **Amazon Simple Notification Service (SNS)**, ensuring you stay informed when your usage or costs reach predefined levels. This helps you stay proactive in managing your AWS resources and avoid unexpected charges at the end of the month.

## Types of Budgets

AWS Budgets offers four primary types of budgets, each designed to track different aspects of your AWS resources:

1. **Usage Budgets**: This type of budget tracks the usage of AWS resources (e.g., EC2 instances, storage, etc.) and helps you ensure that you are not exceeding your expected usage. It can be configured to monitor a variety of usage metrics, such as the number of hours an EC2 instance runs or the amount of data stored in Amazon S3.

2. **Cost Budgets**: This budget type helps you track your AWS spending over time. It monitors your total cost, giving you visibility into your spending patterns and allowing you to set thresholds for spending to ensure you do not exceed your budget. You can set up alerts to notify you when your costs surpass the predefined budget.

3. **Reservation Budgets**: For organizations using **Reserved Instances (RIs)** or other reserved services, this type of budget helps track the utilization of those resources. You can use it to monitor the performance of your RIs and ensure they are being used efficiently. This is particularly useful for EC2, ElastiCache, RDS, and Redshift resources, which can be reserved for cost savings over time.

4. **Savings Plans Budgets**: This budget helps you track your **Savings Plans**, which offer discounted rates in exchange for committing to a certain level of usage. You can track how much you are saving with your Savings Plans, and set alerts to monitor if you are on track to meet the expected savings.

## Filtering Options

When creating a budget, AWS Budgets provides flexible filtering options, allowing you to narrow down your budget scope based on specific criteria. You can filter by:
- **Service** (e.g., EC2, S3, Lambda)
- **Linked Account** (for organizations with multiple AWS accounts)
- **Tag** (for cost allocation tags)
- **Purchase Option** (e.g., On-demand or Reserved instances)
- **Instance Type** (e.g., EC2 instance types)
- **Region** (geographical regions where your resources are deployed)
- **Availability Zone**
- **API Operation**
- And many more, including options available in AWS Cost Explorer.

## Cost and Usage Management

With AWS Budgets, you can control your spending and usage across different AWS services, accounts, regions, and resources. By combining the powerful tracking and alerting features with customizable filters, you can effectively manage and optimize costs at scale.

## Notification Flexibility

AWS Budgets supports up to **5 SNS topics per budget**, allowing you to send alerts to different recipients or take action based on your budget thresholds. For example, you can set up notifications for different team members to be alerted when the budget exceeds a certain percentage, or you can automate actions like stopping underutilized resources.

## Pricing

AWS Budgets allows you to create up to **2 free budgets** per account. After that, each additional budget costs **$0.02 per day**. This makes it an affordable option for organizations looking to implement cost controls without significant overhead.
