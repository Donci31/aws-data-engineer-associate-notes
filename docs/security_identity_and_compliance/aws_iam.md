# AWS Identity and Access Management (IAM)

AWS Identity and Access Management (IAM) is a foundational service that provides secure, fine-grained control over access to AWS services and resources. By defining permissions and policies, IAM ensures that users and applications only access what they are explicitly allowed to.

## IAM Identities

### Users

- Represent individuals or applications that need long-term credentials.
- Typically configured with **access keys** for API or CLI access.

### Groups

- Simplify permissions management by grouping users and assigning shared policies.

### Roles

- Provide temporary security credentials.
- Common use cases:
    - **Service Roles**: Allow AWS services to perform actions on your behalf (e.g., Lambda accessing S3).
    - **Cross-Account Roles**: Enable secure interactions between AWS accounts.
    - **Instance Profiles**: Attach a role to an EC2 instance, granting it permissions to access AWS resources securely.

### Service Roles

- IAM roles used by AWS services (e.g., Lambda, EC2, RDS).
- Limited to actions within your account.
- Must be stored in **instance profiles** for use with Amazon EC2.
- **Note**: Each instance profile can contain only one role, and this limit cannot be increased.

## IAM Policy Structure (JSON)

IAM policies are JSON documents that contain statements defining permissions. Key fields include:

1. **Version**:
    - Specifies the policy language version. Default is `"2012-10-17"`.
2. **Statement**:
    - Contains one or more individual permissions.

### Statement Fields

- **Effect**:
    - Specifies whether to `Allow` or `Deny` the action.
- **Action**:
    - Specifies the AWS service operations (e.g., `s3:PutObject`, `ec2:StartInstances`).
    - Use `*` as a wildcard for all actions.
- **Resource**:
    - Specifies the resources the policy applies to (e.g., `arn:aws:s3:::my-bucket/*`).
    - Use `*` for all resources.
- **Condition** (Optional):
    - Adds conditional logic (e.g., time-based restrictions, IP address limits).
    - Example: `{ "IpAddress": { "aws:SourceIp": "203.0.113.0/24" } }`

### Example Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
    {
        "Effect": "Allow",
        "Action": "s3:ListBucket",
        "Resource": "arn:aws:s3:::example-bucket",
        "Condition": {
        "StringEquals": {
            "aws:RequestTag/Environment": "Production"
        }
        }
    }
    ]
}
```

## Access Control through Policies

Policies define what actions are permitted or denied for IAM identities or AWS resources. AWS evaluates these policies when a principal (user, role, or service) makes a request.

### Types of Policies

1. **Identity-Based Policies**:
    - Attach to users, groups, or roles.
    - Enable granting permissions for multiple resources and services.
2. **Resource-Based Policies**:
    - Directly attach to resources like S3 buckets.
    - Allow cross-account access.
3. **Permissions Boundaries**:
    - Define a maximum permission level for IAM users and roles (not groups).
    - Effective permissions are the intersection of the permissions boundary and the attached policies.
4. **Service Control Policies (SCPs)**:
    - Apply at the organization or organizational unit (OU) level.
    - Restrict actions for all accounts under an organization.
    - SCPs override even the root user’s permissions in member accounts but do not impact service-linked roles.
    - Example: Prevent creating or restoring backups in non-compliant AWS Regions to enforce geographical compliance.
5. **Access Control Lists (ACLs)**:
    - Provide network-layer permissions for resources such as S3 and VPCs.
6. **Session Policies**:
    - Temporary policies that apply to specific sessions.

## IAM Database Authentication

- Supported for **MySQL** and **PostgreSQL** on RDS.
- Authenticate using IAM-generated tokens instead of static passwords.
- Tokens:
    - Generated using **AWS Signature Version 4**.
    - Valid for 15 minutes, eliminating the need for long-term credential storage.
- Managed externally, ensuring no user credentials are stored in the database.

## IAM Credential Report and Access Analyzer

### Credential Report

- Provides a complete overview of all IAM users in your account.
- Includes:
    - Password status.
    - Access key age.
    - Multi-Factor Authentication (MFA) usage.

### Access Analyzer

- Helps identify unintended access to your resources.
- Scans resource policies for vulnerabilities or misconfigurations.
- Supports integration with AWS Organizations for centralized analysis.

## Restrictions and Unique Requirements

### Root User-Only Tasks

Some critical operations require root account access:

- Modifying the AWS account name, root password, or email address.
- Changing AWS support plans.
- Enabling MFA delete on S3 buckets.
- Creating CloudFront key pairs.
- Registering for AWS GovCloud.

### Availability Zone (AZ) IDs

- AZ IDs ensure consistency across accounts:
    - Example: `use2-az1` is a globally consistent identifier for an Availability Zone.
- Useful for cross-account coordination where names like `us-west-2a` might vary.

## Use Cases and Best Practices

### Use Cases

1. **Temporary Access via Roles**:
    - Attach roles to EC2 instances for secure, temporary access to resources like S3 or DynamoDB.
2. **Database Access**:
    - Use IAM authentication to manage access to RDS instances without passwords.
3. **Enforce Compliance**:
    - Leverage SCPs to ensure all accounts adhere to organizational security policies.

### Account Migration Between Organizations

- To migrate accounts:
    1. Remove the member account from the old organization.
    2. Send an invitation to the member account from the new organization.
    3. Accept the invitation to the new organization.

### Best Practices

1. **Follow the Principle of Least Privilege**:
    - Grant only the permissions required to perform specific tasks.
2. **Enable Multi-Factor Authentication (MFA)**:
    - Mandatory for privileged users, especially root users.
3. **Rotate Access Keys Regularly**:
    - Minimize the risk of compromised credentials.
4. **Centralize Governance with AWS Organizations**:
    - Use SCPs to enforce policies across all accounts.
5. **Audit Regularly**:
    - Generate IAM Credential Reports and use Access Analyzer to identify and mitigate risks.
