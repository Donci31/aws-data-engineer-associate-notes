# AWS Lambda

AWS Lambda is a **serverless computing service** that allows you to run code without provisioning or managing servers. Lambda automatically scales your application by running code in response to triggers such as HTTP requests through Amazon API Gateway, modifications in Amazon S3 buckets, updates in Amazon DynamoDB tables, and more. You pay only for the compute time you consume, with no charge when the code is not running.

With AWS Lambda, you can run code for virtually any type of application or backend service - all with zero administration. Just upload your code and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to automatically trigger from other AWS services (important S3, DynamoDB and Kinesis) or call it directly from any web or mobile app.

## **Support for Multiple Programming Languages**
   AWS Lambda supports multiple programming languages, including:
   - **Node.js**
   - **Python**
   - **Java**
   - **Go**
   - **Ruby**
   - **C#**

   This flexibility makes Lambda a suitable choice for different application types and languages.

## **VPC Integration**
   AWS Lambda functions are, by default, associated with an **AWS-owned VPC**, enabling them to access public AWS APIs (e.g., DynamoDB, S3). However, when interacting with private resources like an Amazon RDS instance, you can configure your Lambda function to interact with a **private VPC**.

   - **VPC-Enabled Lambda Functions**: When Lambda functions are configured for VPC access, the network traffic is subject to the routing rules of the VPC and subnet.
   - **NAT Gateway**: To enable access to public resources from within a VPC, you must configure a **NAT gateway** in a public subnet.
   - **Dedicated Tenancy**: Lambda functions cannot connect directly to a VPC with dedicated instance tenancy. To access resources in such a VPC, you must peer it with a second VPC with **default tenancy**.

   Once your function is VPC-enabled, all network traffic from your function is subject to the routing rules of your VPC/Subnet. If your function needs to interact with a public resource, you will need a route through a NAT gateway in a public subnet.

   - **VPC ENI (Elastic Network Interface) Considerations**
    - **ENI Capacity**: Lambda functions need sufficient ENI capacity in your VPC to scale properly. If your VPC doesn’t have enough **IP addresses** or ENIs, Lambda functions will not scale as expected, leading to errors such as **EC2ThrottledException**.
    - It's essential to ensure that your function has access to at least one subnet in each **Availability Zone** to improve fault tolerance. If one Availability Zone runs out of IP addresses or becomes unavailable, Lambda can continue executing functions in other zones.

## **Concurrency and Throttling**
   - **Concurrency**: AWS Lambda allows **1,000 concurrent executions per AWS account per region** by default. When the concurrency limit is reached, incoming requests are throttled. For heavy workloads, you can request an increase in concurrency limits from AWS support.
    - Provisioned Concurrency allows for the function to maintain a specified number of "warm" instances that are always ready to respond instantly. This eliminates cold starts and provides consistent performance, especially for functions that need to respond to traffic with low latency.

   - **Throttling**: If your Lambda invocations exceed the concurrency limits or your Amazon SNS messages result in excessive concurrent invocations, throttling will occur.

> Note: SnapStart is designed to improve cold start performance primarily for functions that are not invoked frequently. But, SnapStart may not provide the same level of readiness and performance consistency as provisioned concurrency, which is explicitly designed to keep functions warm and ready to execute.
 

## **Cost Management**
   You are billed only for the compute time you use, with the option to set the execution timeout between 1 second and **15 minutes**. Lambda functions can be invoked using **standard rate expressions** or **cron expressions** for tasks such as scheduling jobs up to once per minute.

## **Automatic Scaling & Event-Driven Execution**
   AWS Lambda automatically runs your code in response to various AWS services events, such as:
   - **Amazon S3**: Trigger Lambda when objects are created, modified, or deleted.
   - **Amazon DynamoDB**: Trigger Lambda on table updates like insertions, updates, or deletions.
   - **Amazon SNS**: Trigger Lambda functions from SNS notifications.
   - **API Gateway**: Handle HTTP requests sent through API Gateway.

   Lambda scales automatically, handling individual requests in parallel. It charges only for the compute time consumed, which means you are billed based on the execution duration and the memory allocated to the function.

## **CloudWatch Alarms and Budgeting**
   Lambda functions can scale rapidly, so it's recommended to set up **Amazon CloudWatch Alarms** to notify you when function metrics like **ConcurrentExecutions** or **Invocations** exceed predefined thresholds. 
   
   - **AWS Budgets**: It's also crucial to create AWS Budgets to monitor costs on a daily basis to avoid unexpected charges.
   
## **Lambda Layers**
   AWS Lambda Layers allow you to include additional code, libraries, or runtime environments in your Lambda functions. Layers are **ZIP archives** that contain dependencies or custom runtimes, which can be shared across multiple functions to reduce deployment package size and improve efficiency.

   - **Layer Management**: Layers reduce the size of the function deployment package and **enable the sharing of common code across multiple Lambda functions**. A Lambda function can use up to **5 layers** simultaneously.
   - **Layer Permissions**: Layers can have resource-based policies, granting access permissions to specific AWS accounts, AWS Organizations, or all AWS accounts.
   - **Size Limit**: The unzipped size of the Lambda function and all layers must be **less than 250 MB**.

When the function is invoked, layers are installed in /opt in the order you provided. Order is important because layers are all extracted under the same path, so each layer can potentially overwrite the previous one. This approach can be used to customize the environment. For example, the first layer can be a runtime, and the second layer can add specific versions of the libraries you need.

   Layers can be versioned to manage updates, each version is immutable. When a version is deleted or permissions to use it are revoked, functions that used it previously will continue to work, but you won’t be able to create new ones.

## **Memory and Timeout Management**
   - **Memory Allocation**: Lambda allocates compute power proportionally to the memory you allocate to your function. Over-provisioning memory can lead to faster execution but higher costs. AWS recommends setting a reasonable memory value based on your function's requirements.
   - **Timeout Settings**: Functions can be configured to run up to **15 minutes** per execution, and it’s essential to adjust the timeout based on the expected execution duration to avoid unnecessary charges.
   
   > **Over-provisioning timeout**: It’s important to avoid over-provisioning the function's timeout, as this can result in functions running longer than needed and leading to unexpected costs.

## **Security & Permissions**
- **IAM Roles**: Lambda functions are assigned IAM roles to grant them the necessary permissions to interact with other AWS services (e.g., DynamoDB, S3, SNS). It's important to follow the principle of least privilege when defining IAM roles for Lambda functions.
- **Private Subnets**: When Lambda functions need to interact with private resources (e.g., an Amazon RDS instance), they must be configured with private subnets in your VPC.

## **Error Handling & Retries**
- **Automatic Retries**: Lambda automatically retries failed invocations for certain event sources, such as Kinesis and DynamoDB Streams. This ensures that records are successfully processed, but also means you need to ensure your Lambda functions are idempotent to avoid duplicate processing.
- **Timeout Handling**: If a Lambda function times out, the event source will typically retry the invocation based on predefined retry policies.

## **Integration with EFS (Elastic File System)**
AWS Lambda functions can be integrated with **Amazon EFS**, which allows them to access a shared file system for persistent storage. This is particularly useful when dealing with data that needs to be retained across multiple Lambda invocations.

- **Mounting EFS**: To use EFS with Lambda, your Lambda function must be deployed within a VPC and configured to access an EFS file system.
- **Latency Considerations**: While EFS is highly scalable, it can introduce additional latency during cold starts, so careful consideration is necessary for time-sensitive applications.

 