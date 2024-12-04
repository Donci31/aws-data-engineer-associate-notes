# AWS CloudFormation

**AWS CloudFormation** is a service that allows you to model and set up your entire AWS infrastructure using declarative code, known as **Infrastructure as Code (IaC)**. With CloudFormation, you define your desired AWS resources (e.g., EC2 instances, S3 buckets, security groups, and more) in a template file, which is then used by CloudFormation to automatically create, update, and manage those resources in the correct order.

## What is a Stack?

- A **stack** is the simplest form of organizing your CloudFormation resources. It's essentially the set of AWS resources that are created and managed by a single CloudFormation template. Once a stack is created, you can manage the entire infrastructure as a single entity.
- **Stack Creation**: When you create a stack, CloudFormation provisions all the resources specified in the template (e.g., EC2 instances, S3 buckets, security groups).
- **Stack Updates**: When changes are made to the CloudFormation template (like adding a new EC2 instance or modifying a security group), you can update the stack to apply those changes automatically, and CloudFormation handles the dependency management.
- **Stack Deletion**: When you delete a stack, all the resources within that stack are automatically deleted, helping you manage the lifecycle of your infrastructure.

> StackSets allow you to create, update, or delete CloudFormation stacks across multiple AWS accounts and regions with a single CloudFormation template.

## Key Features of CloudFormation

1. **Declarative Infrastructure as Code**:
   - CloudFormation enables you to declare the infrastructure you need using templates in **JSON** or **YAML** format. You simply specify the desired state of your infrastructure, and CloudFormation will take care of the execution.
   - For example, in a template, you can specify that:
     - You want a **security group**.
     - You want **two EC2 instances** that use this security group.
     - You want an **S3 bucket**.
     - You want a **load balancer (ELB)** in front of these EC2 instances.
   - CloudFormation ensures that these resources are created in the right order and with the exact configurations you define.

2. **No Manual Resource Creation**:
   - CloudFormation eliminates the need to manually create and configure resources, reducing the potential for human errors and increasing infrastructure control.
   - Resources within the template are automatically created, updated, or deleted according to the changes made to the template.

3. **Cost Management**:
   - CloudFormation allows you to easily manage costs by tagging resources within a stack. You can easily track the costs of individual stacks and determine how much each resource is contributing to the total cost.
   - You can also **estimate the cost** of your resources directly from the CloudFormation template before provisioning them, helping you make informed decisions regarding infrastructure costs.

4. **Productivity and Automation**:
   - CloudFormation improves productivity by allowing you to create and destroy infrastructure quickly. The ability to destroy and recreate your infrastructure on the fly is crucial for development, testing, and troubleshooting environments.
   - You can automate tasks such as deleting and recreating environments at specific times (e.g., automatically deleting templates in the evening and recreating them in the morning), which can help optimize costs and resource management in dev environments.

5. **Declarative Programming**:
   - CloudFormation simplifies the orchestration of complex infrastructure setups by automating resource ordering and dependencies. Unlike imperative programming, where you must define step-by-step processes, CloudFormation declaratively allows you to define the desired outcome without specifying how to achieve it.
   - CloudFormation automatically handles dependencies between resources and ensures they are created or updated in the correct order.

6. **Templates and Reusability**:
   - You don’t have to reinvent the wheel. CloudFormation enables you to use existing templates available from the AWS documentation, community, or other sources.
   - These templates can be customized or extended to fit specific needs, saving time and effort in architecture creation.

7. **Support for AWS Resources**:
   - CloudFormation supports **almost all AWS resources**, and you can create templates for resources like EC2, S3, VPC, Lambda, RDS, and more.
   - For resources that are not supported by default, you can use **custom resources**, which allow you to extend CloudFormation to manage other resources or external services not natively supported.

8. **Mappings in CloudFormation**:
   - **Mappings** are key-value pairs used to define environment-specific configurations.
   - This allows you to adapt a single template to multiple environments (such as **dev**, **staging**, or **prod**) without needing separate templates for each. For instance, an EMR cluster’s configuration could vary based on the environment, and you can use mappings to dynamically set those configurations within the same template.
